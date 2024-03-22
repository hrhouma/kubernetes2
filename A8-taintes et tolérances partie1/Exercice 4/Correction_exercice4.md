Utilisation de Node Selector et de Node Affinity dans Kubernetes. 

### Partie 1: Utilisation de Node Selector

#### Étape 1: Étiqueter un Nœud

1. **Identifier un Nœud à Étiqueter**:
   - Ouvrez un terminal.
   - Listez tous les nœuds disponibles dans votre cluster avec la commande :
     ```bash
     kubectl get nodes
     ```
   - Choisissez un nœud sur lequel vous voulez déployer le pod nginx. Supposons que ce soit `demo-worker2`.

2. **Appliquer une Étiquette au Nœud**:
   - Utilisez la commande suivante pour ajouter l'étiquette `color=red` au nœud choisi :
     ```bash
     kubectl label nodes demo-worker2 color=red
     ```

#### Étape 2: Créer et Déployer le Pod nginx

1. **Créer un Fichier de Manifeste**:
   - Ouvrez un éditeur de texte.
   - Copiez et collez le YAML suivant dans l'éditeur :
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
   - Sauvegardez le fichier sous le nom `pod-nodeselector.yaml`.

2. **Déployer le Pod**:
   - Dans le terminal, naviguez jusqu'au répertoire contenant le fichier `pod-nodeselector.yaml`.
   - Exécutez la commande suivante pour créer le pod :
     ```bash
     kubectl apply -f pod-nodeselector.yaml
     ```

#### Étape 3: Vérification

1. **Vérifiez le Placement du Pod**:
   - Utilisez cette commande pour voir si le pod a été correctement placé sur le nœud avec l'étiquette `color=red` :
     ```bash
     kubectl get pods -o wide
     ```

### Partie 2: Utilisation de Node Affinity

#### Étape 1: Créer le Manifeste du Pod avec Node Affinity

1. **Créer un Nouveau Fichier de Manifeste**:
   - Ouvrez un nouvel éditeur de texte.
   - Copiez et collez le YAML suivant :
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
               - key: color
                 operator: In
                 values:
                   - red
               - key: env
                 operator: In
                 values:
                   - aws
       containers:
       - name: nginx
         image: nginx
     ```
   - Sauvegardez ce fichier sous `pod-nodeaffinity.yaml`.

#### Étape 2: Déployer le Pod avec Node Affinity

1. **Appliquer le Manifeste**:
   - Retournez à votre terminal.
   - Assurez-vous d'être dans le même répertoire que `pod-nodeaffinity.yaml`.
   - Exécutez la commande suivante pour créer le pod :
     ```bash
     kubectl apply -f pod-nodeaffinity.yaml
     ```

#### Étape 3: Vérification

1. **Vérifiez le Placement du Pod**:
   - Comme précédemment, utilisez `kubectl get pods -o wide` pour vérifier le placement du pod. Il devrait être placé sur un nœud qui satisfait à la fois les critères `color=red` et `env=aws`.

### Nettoyage (Pour les Deux Parties)

- Pour garder votre cluster propre, pensez à supprimer les pods et à retirer les étiquettes des nœuds :
  ```bash
  kubectl delete pod nginx nginx-affinity
  kubectl label nodes demo-worker2 color- env-
  ```

Félicitations ! Vous avez maintenant une compréhension pratique de comment diriger les pods vers des nœuds spécifiques dans votre cluster Kubernetes en utilisant Node Selector et Node Affinity, permettant un contrôle plus fin du placement des ressources selon les exigences de vos applications.


### Commandes


Scheduling in k8s — 
Node Selector, 

k label no demo-worker2 color=red
vi pod-nodeselector.yaml

root@k8s-demo:/home/ubuntu# cat pod-nodeselector.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  nodeSelector:
    color: red
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

k get po
k apply -f pod-nodeselector.yaml
k get po
k get po -o wide
k get no --show-labels

NodeSelector —> 
20 nodes in the cluster

Labels —> only Key and value

5 nodes on AWS and only 2 has ssd.. out of two, only one is blue node.
Out of 3 non-ssd nodes on AWS, you have 1 blue node

color=blue and ssd node

NodeAffinity —> 
We can have complex queries
hdd=ssd & color=blue


root@k8s-demo:/home/ubuntu# cat pod-nodeaffinity.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
            - key: color
              operator: In
              values:
                - red
          - matchExpressions:
            - key: env
              operator: In
              values:
                - aws
  containers:
  - image: nginx
    name: nginx
    
    
