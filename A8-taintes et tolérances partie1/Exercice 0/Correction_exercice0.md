Correction détaillée étape par étape pour l'exercice sur les taints et tolérances dans Kubernetes.

### Partie A: Appliquer un Taint à un Nœud

**Objectif:** Marquer un nœud pour qu'il ne puisse accepter que certains pods.

1. **Trouver le nom de votre nœud:**
   - Ouvrez votre terminal.
   - Tapez la commande suivante pour lister tous les nœuds de votre cluster Kubernetes:
     ```bash
     kubectl get nodes
     ```
   - Repérez le nom du nœud sur lequel vous souhaitez appliquer le taint. Pour cet exemple, supposons que le nœud s'appelle `node1`.

2. **Appliquer un taint au nœud:**
   - Dans le terminal, tapez la commande suivante pour appliquer un taint au nœud `node1`:
     ```bash
     kubectl taint nodes node1 key1=value1:NoSchedule
     ```
   - Cette commande indique que seul un pod ayant une tolérance correspondante à ce taint pourra être planifié sur `node1`.

### Partie B: Créer un Pod avec Tolérance

**Objectif:** Créer un pod qui peut être déployé sur le nœud tainté.

1. **Préparer le fichier YAML du pod:**
   - Ouvrez un éditeur de texte de votre choix (comme Notepad ou TextEdit).
   - Copiez et collez le contenu suivant dans l'éditeur:
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: mypod
     spec:
       containers:
       - name: mycontainer
         image: nginx
       tolerations:
       - key: "key1"
         operator: "Equal"
         value: "value1"
         effect: "NoSchedule"
     ```
   - Sauvegardez le fichier sous le nom `pod-with-toleration.yaml` sur votre bureau.

2. **Créer le pod:**
   - Retournez à votre terminal.
   - Changez le répertoire courant vers l'endroit où vous avez enregistré le fichier `pod-with-toleration.yaml`. Si vous l'avez enregistré sur le bureau, la commande pourrait ressembler à ceci (sur Windows utilisez `cd Desktop`, sur MacOS ou Linux utilisez `cd ~/Desktop`).
   - Tapez la commande suivante pour créer le pod dans Kubernetes:
     ```bash
     kubectl apply -f pod-with-toleration.yaml
     ```
   - Cette commande crée un pod avec une tolérance qui correspond au taint appliqué sur `node1`, permettant au pod d'être planifié sur ce nœud.

### Partie C: Vérification et Nettoyage

1. **Vérifier le placement du pod:**
   - Pour vérifier si le pod a été correctement déployé sur le nœud tainté, tapez:
     ```bash
     kubectl get pods -o wide
     ```
   - Regardez la colonne `NODE` pour le nom de votre pod (`mypod`). Elle devrait indiquer `node1`, montrant que le pod a été déployé sur le nœud tainté.

2. **Nettoyer:**
   - Pour supprimer le taint du nœud, exécutez:
     ```bash
     kubectl taint nodes node1 key1=value1:NoSchedule-
     ```
   - Pour supprimer le pod, tapez:
     ```bash
     kubectl delete pod mypod
     ```

Félicitations! Vous avez réussi à appliquer un taint à un nœud et à déployer un pod avec une tolérance correspondante dans votre cluster Kubernetes. Ces étapes montrent comment contrôler le placement des pods sur les nœuds dans un environnement Kubernetes.