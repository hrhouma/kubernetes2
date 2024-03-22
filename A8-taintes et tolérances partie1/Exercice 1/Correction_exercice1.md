Allons-y étape par étape pour le premier exercice concernant `requiredDuringSchedulingIgnoredDuringExecution`.

### Objectif
Comprendre comment utiliser l'affinité de nœuds dans Kubernetes pour s'assurer qu'un pod soit programmé sur un nœud spécifique en respectant certaines conditions lors de sa création.

### Étape 1: Vérifier les Nœuds Disponibles
1. **Ouvrir le terminal.** Cela peut être votre terminal local si vous avez configuré `kubectl` pour communiquer avec votre cluster Kubernetes, ou le terminal d'une interface web si vous utilisez un service cloud.

2. **Lister les nœuds disponibles.**
   - Tapez la commande suivante et appuyez sur Entrée:
     ```bash
     kubectl get nodes
     ```
   - Notez le nom d'un nœud sur lequel vous souhaitez que votre pod soit planifié. Par exemple, `mon-noeud`.

### Étape 2: Ajouter une Étiquette au Nœud
1. **Choisir une étiquette pour le nœud.** Par exemple, nous utiliserons `type=front-end`.

2. **Appliquer l'étiquette au nœud.**
   - Remplacez `<nom-du-nœud>` par le nom de votre nœud et exécutez :
     ```bash
     kubectl label nodes <nom-du-nœud> type=front-end
     ```
   - Cette étape marque le nœud avec une étiquette pour l'identifier comme un nœud front-end.

### Étape 3: Créer un Manifeste de Pod avec Affinité de Nœuds
1. **Ouvrir un éditeur de texte.** N'importe quel éditeur fera l'affaire, comme Notepad sur Windows, TextEdit sur Mac, ou Nano/Vim dans le terminal.

2. **Écrire le manifeste du pod.** Copiez et collez le texte suivant dans l'éditeur :
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: mon-pod-front-end
   spec:
     containers:
     - name: nginx-container
       image: nginx
     affinity:
       nodeAffinity:
         requiredDuringSchedulingIgnoredDuringExecution:
           nodeSelectorTerms:
           - matchExpressions:
             - key: type
               operator: In
               values:
               - front-end
   ```
   - Ce manifeste demande à Kubernetes de placer le pod sur un nœud ayant l'étiquette `type=front-end`.

3. **Sauvegarder le fichier.** Nommez-le `mon-pod.yaml` et sauvegardez-le sur votre ordinateur.

### Étape 4: Appliquer le Manifeste pour Créer le Pod
1. **Retour au terminal.** Assurez-vous que vous êtes dans le même répertoire que le fichier `mon-pod.yaml`.

2. **Exécuter la commande pour créer le pod.**
   - Tapez et exécutez :
     ```bash
     kubectl apply -f mon-pod.yaml
     ```
   - Cette commande demande à Kubernetes de créer le pod selon les spécifications du fichier YAML.

### Étape 5: Vérifier le Placement du Pod
1. **Observer où le pod a été planifié.**
   - Tapez :
     ```bash
     kubectl get pods -o wide
     ```
   - Regardez la colonne `NODE` pour voir si `mon-pod-front-end` a été placé sur le nœud avec l'étiquette `type=front-end`.

### Étape 6: Nettoyage (Optionnel)
1. **Supprimer le pod et retirer l'étiquette du nœud si nécessaire.**
   - Pour le pod :
     ```bash
     kubectl delete -f mon-pod.yaml
     ```
   - Pour l'étiquette du nœud :
     ```bash
     kubectl label nodes <nom-du-nœud> type-
     ```

Félicitations! Vous avez appris à utiliser l'affinité de nœuds dans Kubernetes pour contrôler le placement des pods en fonction des étiquettes des nœuds.