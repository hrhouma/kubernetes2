Correction de l'exercice sur la création et la gestion de jobs dans Kubernetes. 

### Étape 1: Création d'un Job

#### Instruction

- **Commande**: 
  ```bash
  kubectl create job myjob1 --image=busybox -- /bin/sh -c "sleep 30"
  ```

#### Explication

- **`kubectl create job myjob1`**: Cette partie de la commande indique à Kubernetes que vous souhaitez créer un nouveau job nommé `myjob1`.
- **`--image=busybox`**: Ici, vous spécifiez que le conteneur exécuté par ce job utilisera l'image `busybox`. `BusyBox` est une image Docker très légère qui contient une variété d'outils UNIX standards dans un seul exécutable. Elle est souvent utilisée pour des tâches de débogage ou d'administration dans un environnement Kubernetes.
- **`-- /bin/sh -c "sleep 30"`**: Cette partie spécifie la commande à exécuter dans le conteneur. Dans ce cas, il exécute `sleep 30`, ce qui met simplement le conteneur en pause pendant 30 secondes.

### Étape 2: Observer le Job

#### Instruction

- **Utiliser `kubectl get jobs` pour voir l'état du job**:
  - Tapez cette commande dans votre terminal pour lister tous les jobs. Vous devriez voir `myjob1` avec un statut qui indique s'il a été exécuté avec succès.
  
- **Utiliser `kubectl describe job myjob1` pour voir les détails du job**:
  - Cette commande vous donne des informations détaillées sur le job, y compris le nombre de pods qu'il a créés, leur état, et les événements associés au job.

### Étape 3: Supprimer le Job

#### Instruction

- **Commande**: 
  ```bash
  kubectl delete job myjob1
  ```
- **Explication**: Cette commande supprime le job `myjob1` de votre cluster Kubernetes. Cela est utile pour nettoyer les ressources une fois que le job a accompli sa tâche ou pour terminer un job qui ne s'est pas exécuté comme prévu.

### Bonus: Création d'un Job via un Fichier YAML

#### Instruction

1. **Créer un Fichier YAML** (`myjob2.yaml`):
   - Ouvrez un éditeur de texte et copiez-y le contenu suivant :
     ```yaml
     apiVersion: batch/v1
     kind: Job
     metadata:
       name: myjob2
     spec:
       template:
         spec:
           containers:
           - name: hello-kubernetes
             image: busybox
             command: ["/bin/sh", "-c", "echo Hello Kubernetes"]
           restartPolicy: Never
     ```
   - Ce fichier définit un job nommé `myjob2` qui exécute un conteneur `busybox` pour afficher le message "Hello Kubernetes".
   - Enregistrez et fermez le fichier.

2. **Déployer le Job**:
   - Dans le terminal, naviguez jusqu'au répertoire contenant `myjob2.yaml`.
   - Exécutez la commande suivante pour créer le job :
     ```bash
     kubectl apply -f myjob2.yaml
     ```

3. **Observer l'Exécution du Job**:
   - Comme précédemment, utilisez `kubectl get jobs` et `kubectl describe job myjob2` pour observer l'état et les détails du job.

### Conclusion

Ces étapes vous ont guidé à travers le processus de création, d'observation, et de suppression de jobs dans Kubernetes, ainsi que la création d'un job via un fichier YAML. Ces compétences sont fondamentales pour gérer les tâches par lots ou ponctuelles dans un environnement Kubernetes.

### Commandes

cronjob
root@k8s-demo:/home/ubuntu# k create cj mycj1 --image=busybox --schedule="55 13 * * *" --dry-run=client -o yaml -- sleep 20 > mycj.yaml^C
root@k8s-demo:/home/ubuntu# cat mycj.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  creationTimestamp: null
  name: mycj1
spec:
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: mycj1
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - command:
            - sleep
            - "20"
            image: busybox
            name: mycj1
            resources: {}
          restartPolicy: OnFailure
  schedule: 55 13 * * *
status: {}

DaemonSet (ds)
root@k8s-demo:/home/ubuntu# cat ds.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx
  labels:
    k8s-app: nginx
spec:
  selector:
    matchLabels:
      name: nginx
  template:
    metadata:
      labels:
        name: nginx
    spec:
      tolerations:
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: nginx
        image: nginx
