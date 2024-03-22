Correction avec une version guidée pas à pas:

---

## Guide Simplifié pour la Gestion de Base de Données Mongo avec Kubernetes

### Création et Gestion d'un Pod Mongo

1. **Création d'un Pod Mongo** : Ouvrez un éditeur de texte et créez un fichier nommé `mongo-pod.yaml` avec le contenu suivant :

   ```yaml
   apiVersion: v1             
   kind: Pod                  
   metadata:
     name: db
     labels:
       app: db
   spec:
     containers:
     - name: mongo
       image: mongo:4.0
   ```

   Sauvegardez le fichier, puis exécutez cette commande dans votre terminal :

   ```bash
   kubectl apply -f mongo-pod.yaml
   ```

   Ou, pour une méthode plus directe, tapez :

   ```bash
   kubectl run db --image=mongo:4.0
   ```

2. **Exposition de la Base Mongo** : Créez un fichier nommé `mongo-svc.yaml` avec le contenu suivant :

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: db
   spec:
     selector:
       app: db
     type: ClusterIP
     ports:
     - port: 27017
   ```

   Sauvegardez et appliquez avec :

   ```bash
   kubectl apply -f mongo-svc.yaml
   ```

   Ou utilisez cette commande pour un raccourci :

   ```bash
   kubectl expose pod/db --port=27017 --target-port=27017
   ```

### Sauvegarde et Planification des Sauvegardes

3. **Ajout d'un Label à un Node** : Identifiez le nom d'un node dans votre cluster et exécutez :

   ```bash
   kubectl label node <NOM_DU_NODE> app=dump
   ```

   Remplacez `<NOM_DU_NODE>` par le nom de votre node.

4. **Création d'un Job pour le Dump de la Base** : Créez un fichier `mongo-dump-job.yaml` avec le contenu suivant :

   ```yaml
   apiVersion: batch/v1
   kind: Job
   metadata:
     name: dump
   spec:
     template:
       spec:
         restartPolicy: Never
         nodeSelector:
           app: dump
         containers:
         - name: mongo
           image: mongo:4.0
           command:
           - /bin/bash
           - -c
           - mongodump --gzip --host db --archive=/dump/db.gz
           volumeMounts:
           - name: dump
             mountPath: /dump
         volumes:
         - name: dump
           hostPath:
             path: /dump
   ```

   Appliquez avec :

   ```bash
   kubectl apply -f mongo-dump-job.yaml
   ```

5. **Planification de Sauvegardes Régulières avec un CronJob** : Créez `mongo-dump-cronjob.yaml` avec :

   ```yaml
   apiVersion: batch/v1
   kind: CronJob
   metadata:
     name: dump
   spec:
     schedule: "* * * * *"
     jobTemplate:
       spec:
         template:
           spec:
             nodeSelector:
               app: dump
             containers:
             - name: mongo
               image: mongo:4.0
               command:
               - /bin/bash
               - -c
               - mongodump --gzip --host db --archive=/dump/$(date +"%Y%m%dT%H%M%S")-db.gz
               volumeMounts:
               - name: dump
                 mountPath: /dump
             restartPolicy: OnFailure
             volumes:
             - name: dump
               hostPath:
                 path: /dump
   ```

   Lancez avec :

   ```bash
   kubectl apply -f mongo-dump-cronjob.yaml
   ```

### Vérification et Nettoyage

6. **Vérification des Sauvegardes** : Utilisez un Pod de test ou la commande `kubectl debug` pour vérifier les dumps.

7. **Nettoyage** : Supprimez toutes les ressources créées avec :

   ```bash
   kubectl delete job/dump cj/dump po/test po/db svc/db
   ```

---

Ce guide

 est conçu pour que vous puissiez suivre les étapes une par une, sans nécessiter de comprendre en profondeur chaque commande ou spécification. N'hésitez pas à exécuter ces commandes dans l'ordre pour mener à bien votre exercice sur Kubernetes.
