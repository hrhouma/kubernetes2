### Exercice 4: Pratique de Node Selector et Node Affinity

**Objectif:** Comprendre et appliquer les concepts de Node Selector et Node Affinity pour contrôler le placement des pods sur des nœuds spécifiques dans un cluster Kubernetes.

### Contexte
Kubernetes offre plusieurs mécanismes pour influencer où les pods sont déployés dans le cluster, parmi lesquels le Node Selector simple et la Node Affinity plus complexe et flexible.

#### Partie 1: Utilisation de Node Selector

**Scénario:** Vous avez un cluster de 20 nœuds sur AWS, et vous voulez déployer un pod nginx uniquement sur les nœuds ayant l'étiquette `color=red`.

1. **Étiqueter un Nœud:**
   - Choisissez un nœud (par exemple, `demo-worker2`) et appliquez-lui une étiquette `color=red`.
     ```bash
     kubectl label nodes demo-worker2 color=red
     ```

2. **Créer le Manifeste du Pod:**
   - Ouvrez un éditeur de texte et saisissez le YAML suivant pour un pod nginx utilisant un Node Selector.
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: nginx
     spec:
       nodeSelector:
         color: red
       containers:
       - name: nginx
         image: nginx
     ```
   - Enregistrez le fichier sous le nom `pod-nodeselector.yaml`.

3. **Déployer le Pod:**
   - Appliquez le manifeste avec la commande :
     ```bash
     kubectl apply -f pod-nodeselector.yaml
     ```
   - Vérifiez que le pod est bien déployé sur le nœud avec `color=red`.

#### Partie 2: Pratique de Node Affinity

**Scénario:** Vous voulez cette fois déployer un pod nginx sur un nœud qui est à la fois dans l'environnement AWS (`env=aws`) et qui possède un SSD (`hdd=ssd`).

1. **Étiqueter les Nœuds:**
   - Supposons que vos nœuds soient déjà étiquetés correctement pour cet exercice. Sinon, appliquez des étiquettes correspondantes aux nœuds de votre choix.
     ```bash
     kubectl label nodes <nom-du-nœud> env=aws hdd=ssd
     ```

2. **Créer le Manifeste du Pod avec Node Affinity:**
   - Dans votre éditeur de texte, créez un nouveau fichier YAML pour le pod nginx avec les conditions d'affinité spécifiées.
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: nginx-affinity
     spec:
       affinity:
         nodeAffinity:
           requiredDuringSchedulingIgnoredDuringExecution:
             nodeSelectorTerms:
             - matchExpressions:
               - key: env
                 operator: In
                 values:
                   - aws
               - key: hdd
                 operator: In
                 values:
                   - ssd
       containers:
       - name: nginx
         image: nginx
     ```
   - Enregistrez le fichier sous `pod-nodeaffinity.yaml`.

3. **Déployer le Pod avec Node Affinity:**
   - Appliquez ce manifeste pour déployer le pod.
     ```bash
     kubectl apply -f pod-nodeaffinity.yaml
     ```
   - Vérifiez le placement du pod pour confirmer qu'il est déployé sur un nœud satisfaisant les deux conditions (`env=aws` et `hdd=ssd`).

### Nettoyage

- **Supprimer les Pods et les Étiquettes des Nœuds:**
  Pour garder votre cluster propre, pensez à supprimer les pods créés et à retirer les étiquettes si nécessaire.
  ```bash
  kubectl delete pod nginx
  kubectl delete pod nginx-affinity
  kubectl label nodes <nom-du-nœud> color- env- hdd-
  ```

### Conclusion

Ces exercices vous ont permis de pratiquer le contrôle précis du placement des pods dans un cluster Kubernetes, utilisant à la fois la méthode simple du Node Selector et la méthode plus flexible de la Node Affinity. Ces compétences sont essentielles pour optimiser l'utilisation des ressources et respecter les contraintes opérationnelles spécifiques dans un environnement Kubernetes.