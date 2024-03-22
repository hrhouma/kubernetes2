### Exercice 2: Mise en Pratique de `preferredDuringSchedulingIgnoredDuringExecution`

**Objectif:** Créer un pod qui préfère, mais ne requiert pas obligatoirement, être placé sur un nœud spécifique en se basant sur une étiquette, lors de sa planification.

### Contexte

Dans un environnement Kubernetes, il est parfois préférable de guider le scheduler pour optimiser le placement des pods en fonction de certaines préférences sans rendre ces conditions obligatoires. Cela peut aider à optimiser la performance tout en conservant une certaine flexibilité dans la planification des ressources.

### Étapes Détaillées

#### Étape 1: Préparation de l'Environnement

1. **Ajouter des Étiquettes aux Nœuds:**
   - Supposons que vous ayez deux zones dans votre cluster: `primaire` et `secondaire`. Vous souhaitez que vos pods soient de préférence placés dans la zone primaire.
   - Identifiez vos nœuds et ajoutez-leur des étiquettes correspondantes:
     ```bash
     kubectl label nodes <nom-du-nœud-primaire> zone=primaire
     kubectl label nodes <nom-du-nœud-secondaire> zone=secondaire
     ```

#### Étape 2: Création d'un Pod avec Préférence d'Affinité de Nœud

1. **Rédiger le Manifeste du Pod:**
   - Ouvrez un éditeur de texte et créez un fichier nommé `pod-preferred-affinity.yaml`.
   - Copiez et collez le contenu suivant:
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
   - Ce manifeste indique que le scheduler doit essayer de placer ce pod sur un nœud avec l'étiquette `zone=primaire` en priorité, mais ce n'est pas strict. Le poids de 100 maximise cette préférence.

2. **Déployer le Pod:**
   - Appliquez le manifeste avec la commande suivante:
     ```bash
     kubectl apply -f pod-preferred-affinity.yaml
     ```
   - Cette action demande au scheduler de Kubernetes de préférer les nœuds étiquetés avec `zone=primaire` pour le placement du pod, sans en faire une exigence absolue.

#### Étape 3: Vérification et Analyse

1. **Vérifier le Placement du Pod:**
   - Utilisez la commande suivante pour voir les détails du placement du pod:
     ```bash
     kubectl get pods -o wide
     ```
   - Notez sur quel nœud le pod `pod-preference` a été placé. Si un nœud avec l'étiquette `zone=primaire` était disponible, le scheduler aura probablement choisi ce nœud en raison de la préférence spécifiée.

#### Étape 4: Nettoyage

- Pour maintenir votre cluster propre, vous pouvez supprimer le pod et retirer les étiquettes des nœuds:
  ```bash
  kubectl delete pod pod-preference
  kubectl label nodes <nom-du-nœud-primaire> zone-
  kubectl label nodes <nom-du-nœud-secondaire> zone-
  ```

### Conseils pour les Débutants

- **Préférences vs. Exigences:** Dans cet exercice, vous avez vu comment exprimer une préférence sans imposer d'exigences strictes. Cela permet d'avoir des placements de pods plus flexibles et adaptatifs selon les ressources disponibles.
- **Poids des Préférences:** Le poids que vous attribuez à vos préférences influence la décision du scheduler. Un poids plus élevé donne plus d'importance à la préférence.

Cet exercice vous a permis de comprendre comment utiliser les préférences d'affinité pour guider le placement des pods, une compétence utile pour optimiser les déploiements Kubernetes tout en gardant une certaine souplesse dans la gestion des ressources.