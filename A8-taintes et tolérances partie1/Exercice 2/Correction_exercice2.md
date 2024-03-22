Voici une correction détaillée, étape par étape, pour l'exercice 2 sur l'utilisation de `preferredDuringSchedulingIgnoredDuringExecution`

### Correction Détaillée de l'Exercice 2

#### Étape 1: Ajout d'Étiquettes aux Nœuds

1. **Ouvrez votre terminal**. Cela peut être le terminal de votre système d'exploitation ou un terminal dans un environnement cloud, selon l'endroit où votre cluster Kubernetes est hébergé.

2. **Identifiez vos nœuds**. Tapez la commande suivante pour lister tous les nœuds de votre cluster :
   ```bash
   kubectl get nodes
   ```
   Prenez note des noms des nœuds que vous souhaitez étiqueter comme `primaire` et `secondaire`.

3. **Ajoutez les étiquettes**. Exécutez les commandes suivantes pour ajouter les étiquettes correspondantes à vos nœuds. Remplacez `<nom-du-nœud-primaire>` et `<nom-du-nœud-secondaire>` par les noms réels de vos nœuds.
   ```bash
   kubectl label nodes <nom-du-nœud-primaire> zone=primaire
   kubectl label nodes <nom-du-nœud-secondaire> zone=secondaire
   ```
   Ces commandes ajoutent une étiquette `zone=primaire` à votre nœud primaire et `zone=secondaire` à votre nœud secondaire.

#### Étape 2: Création d'un Pod avec Préférence d'Affinité de Nœud

1. **Créez un fichier de manifeste pour votre pod**. Ouvrez un éditeur de texte simple (comme Notepad sur Windows, TextEdit sur Mac, ou Nano sur Linux/Unix) et copiez-collez le contenu suivant :
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pod-preference
   spec:
     containers:
     - name: nginx
       image: nginx
     affinity:
       nodeAffinity:
         preferredDuringSchedulingIgnoredDuringExecution:
         - weight: 100
           preference:
             matchExpressions:
             - key: zone
               operator: In
               values:
               - primaire
   ```
   Enregistrez ce fichier sous le nom `pod-preferred-affinity.yaml`.

2. **Déployez le pod**. Revenez à votre terminal, assurez-vous d'être dans le répertoire où vous avez enregistré le fichier `pod-preferred-affinity.yaml`, et exécutez la commande suivante :
   ```bash
   kubectl apply -f pod-preferred-affinity.yaml
   ```
   Cette commande demande à Kubernetes de tenter de placer le pod sur un nœud avec l'étiquette `zone=primaire`.

#### Étape 3: Vérification et Analyse

1. **Vérifiez où le pod a été placé**. Après quelques instants, exécutez :
   ```bash
   kubectl get pods -o wide
   ```
   Regardez la colonne `NODE` pour le pod `pod-preference`. Si un nœud avec l'étiquette `zone=primaire` était disponible, il est probable que le pod y soit placé. Sinon, il pourrait être placé sur un nœud différent.

#### Étape 4: Nettoyage

1. **Supprimez le pod** pour nettoyer votre cluster :
   ```bash
   kubectl delete pod pod-preference
   ```
2. **Retirez les étiquettes des nœuds** si vous ne prévoyez plus de les utiliser :
   ```bash
   kubectl label nodes <nom-du-nœud-primaire> zone-
   kubectl label nodes <nom-du-nœud-secondaire> zone-
   ```

### Conclusion

Félicitations ! Vous avez appris comment utiliser `preferredDuringSchedulingIgnoredDuringExecution` pour influencer, mais pas forcer, le placement des pods dans Kubernetes. Cette fonctionnalité vous permet de donner des instructions préférentielles au scheduler, offrant un équilibre entre vos souhaits de déploiement et la flexibilité nécessaire pour une gestion efficace du cluster.


### Un autre exercice
 `preferredDuringSchedulingIgnoredDuringExecution`. Cet exercice est conçu pour être suivi étape par étape, même par quelqu'un qui débute totalement dans le domaine technique.

### Objectif

Mettre en pratique la stratégie `preferredDuringSchedulingIgnoredDuringExecution` pour créer un pod qui préfère être déployé sur un nœud spécifique, mais peut être déployé ailleurs si nécessaire.

### Étape 1: Comprendre la Stratégie

1. **Explication Simple** : `preferredDuringSchedulingIgnoredDuringExecution` signifie que, lors de la création du pod, Kubernetes va essayer de respecter vos préférences (par exemple, placer le pod sur un certain type de nœud), mais si cela n'est pas possible, le pod sera quand même déployé sur un autre nœud disponible. Une fois le pod en cours d'exécution, si les conditions préférées changent, cela n'affecte pas le placement du pod.

### Étape 2: Ajouter une Étiquette à un Nœud

Pour cet exercice, nous allons marquer un nœud avec une étiquette qui sera notre préférence pour le déploiement du pod.

1. **Lister les Nœuds Disponibles** :
   - Tapez `kubectl get nodes` pour voir tous vos nœuds.
2. **Ajouter une Étiquette** :
   - Choisissez un nœud et ajoutez-lui une étiquette, par exemple `zone=préférentielle`.
   - Commande : `kubectl label nodes <nom-du-nœud> zone=préférentielle`

### Étape 3: Créer un Manifeste de Pod avec une Préférence d'Affinité

1. **Ouvrir un Éditeur de Texte** :
   - N'importe quel éditeur fera l'affaire. Créez un nouveau fichier.
2. **Rédiger le Manifeste** :
   - Copiez et collez le texte suivant, qui définit un pod simple avec une préférence d'affinité pour le nœud marqué précédemment.
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pod-preferentiel
   spec:
     containers:
     - name: conteneur-simple
       image: nginx
     affinity:
       nodeAffinity:
         preferredDuringSchedulingIgnoredDuringExecution:
         - weight: 1
           preference:
             matchExpressions:
             - key: zone
               operator: In
               values:
               - préférentielle
   ```
   - **Explication** : Ce manifeste demande à Kubernetes de tenter de placer ce pod sur un nœud avec l'étiquette `zone=préférentielle`, mais il n'est pas strict quant à cette exigence.

3. **Sauvegarder le Fichier** :
   - Nommez-le `pod-preferentiel.yaml`.

### Étape 4: Déployer le Pod

1. **Appliquer le Manifeste** :
   - Dans votre terminal, naviguez jusqu'au répertoire où se trouve le fichier `pod-preferentiel.yaml`.
   - Tapez `kubectl apply -f pod-preferentiel.yaml` pour créer le pod.

### Étape 5: Vérifier le Placement du Pod

1. **Observer le Placement** :
   - Pour vérifier où le pod a été déployé, utilisez `kubectl get pods -o wide`.
   - Vérifiez la colonne `NODE` pour voir si le pod est sur le nœud préféré.

### Étape 6: Nettoyage (Facultatif)

1. **Supprimer le Pod** :
   - Tapez `kubectl delete -f pod-preferentiel.yaml` pour nettoyer.
2. **Retirer l'Étiquette du Nœud** (si souhaité) :
   - Utilisez `kubectl label nodes <nom-du-nœud> zone-` pour retirer l'étiquette.

### Conclusion

Félicitations ! Vous avez appris comment guider le placement d'un pod en utilisant des préférences d'affinité de nœuds avec `preferredDuringSchedulingIgnoredDuringExecution`, offrant ainsi une flexibilité dans le déploiement tout en respectant les préférences lorsque cela est possible.