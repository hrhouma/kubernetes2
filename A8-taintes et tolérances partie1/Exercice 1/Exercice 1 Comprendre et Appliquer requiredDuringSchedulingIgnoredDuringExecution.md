### Exercice 1: Comprendre et Appliquer `requiredDuringSchedulingIgnoredDuringExecution`

**Objectif:** Créer un pod qui nécessite un nœud spécifique basé sur une étiquette (label) lors de sa planification, mais ignore cette contrainte une fois exécuté.

1. **Étape Préparatoire: Ajout d'une Étiquette à un Nœud**
   - **Objectif:** Identifier un nœud et lui ajouter une étiquette pour l'utiliser dans notre règle d'affinité.
   - **Commande:**
     ```bash
     kubectl label nodes <nom-du-nœud> zone=primaire
     ```
     Remplacez `<nom-du-nœud>` par le nom de votre nœud cible. Cette étiquette sert à identifier le nœud comme faisant partie de la "zone primaire".

2. **Création d'un Pod avec Affinité de Nœud**
   - **Objectif:** Définir un manifeste de pod qui spécifie que le pod doit être exécuté sur un nœud avec l'étiquette `zone=primaire`.
   - **Fichier Manifeste (pod-affinity.yaml):**
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: pod-affinite
     spec:
       containers:
       - name: nginx
         image: nginx
       affinity:
         nodeAffinity:
           requiredDuringSchedulingIgnoredDuringExecution:
             nodeSelectorTerms:
             - matchExpressions:
               - key: zone
                 operator: In
                 values:
                 - primaire
     ```
   - **Explication:** Ce manifeste crée un pod qui doit être placé sur un nœud marqué avec `zone=primaire` lors de la planification. Après le déploiement, si l'étiquette du nœud change, le pod continue de fonctionner sans être affecté.

3. **Déploiement du Pod**
   - **Commande:**
     ```bash
     kubectl apply -f pod-affinity.yaml
     ```
   - **Vérification:**
     - Pour confirmer que le pod a été correctement planifié, utilisez :
       ```bash
       kubectl get pods -o wide
       ```
     - Observez la colonne `NODE` pour vérifier que le pod est sur le nœd avec l'étiquette `zone=primaire`.

4. **Nettoyage:**
   - Supprimez le pod et retirez l'étiquette du nœud pour revenir à la configuration initiale.
     ```bash
     kubectl delete -f pod-affinity.yaml
     kubectl label nodes <nom-du-nœud> zone-
     ```

### Conseils pour les Débutants:

- **Étiquettes (Labels) et Sélecteurs:** Les étiquettes sont des paires clé-valeur attachées aux objets pour organiser et sélectionner des sous-ensembles d'objets. Dans cet exercice, nous utilisons une étiquette pour identifier un groupe de nœuds.
- **Affinité de Nœud:** L'affinité de nœud est une règle utilisée pour spécifier sur quels nœuds un pod peut être planifié, basée sur les étiquettes du nœud.
- **Pourquoi `requiredDuringSchedulingIgnoredDuringExecution` ?** Cette stratégie garantit que vos besoins sont strictement respectés lors du déploiement initial, sans affecter la stabilité des pods en cours d'exécution en cas de changement d'environnement.

Cet exercice illustre comment contrôler le placement des pods dans un cluster Kubernetes en utilisant l'affinité de nœud, permettant une gestion plus fine des ressources et une meilleure adaptation aux besoins spécifiques de vos applications.