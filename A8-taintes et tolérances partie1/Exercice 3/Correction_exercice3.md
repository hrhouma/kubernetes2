Comme mentionné précédemment, `requiredDuringSchedulingRequiredDuringExecution` est une fonctionnalité plus théorique dans Kubernetes et peut ne pas être disponible dans toutes les versions. Pour cet exercice, nous allons simuler une situation qui explique le concept de manière à préparer pour son utilisation future, en restant conscients que l'exécution pratique exacte de cette fonctionnalité pourrait varier.

### Objectif
Comprendre le principe de maintien des exigences d’affinité de nœuds non seulement lors de la planification initiale d’un pod mais aussi pendant toute sa durée de vie.

### Préparation: Compréhension du Concept

1. **Comprendre le concept**: `requiredDuringSchedulingRequiredDuringExecution` signifie que les conditions spécifiées pour l'affinité de nœud doivent être respectées au moment de la planification du pod ET continuer à être respectées pendant toute la durée d'exécution du pod. Si les conditions ne sont plus remplies, des actions sont prises pour corriger cela.

### Étape 1: Recherche de la Fonctionnalité

1. **Documentation**: Vu que c'est une fonctionnalité en développement, commencez par vérifier la documentation de Kubernetes ou les notes de version pour voir si `requiredDuringSchedulingRequiredDuringExecution` est disponible dans votre version de Kubernetes.
   - **Action**: Ouvrez votre navigateur et recherchez "Kubernetes requiredDuringSchedulingRequiredDuringExecution".

### Étape 2: Simulation Théorique

Comme la mise en œuvre pratique peut ne pas être possible, effectuons une simulation théorique à travers la rédaction d'un manifeste de pod.

1. **Écrire le manifeste**: Ouvrez un éditeur de texte simple et tapez le YAML suivant (ceci est hypothétique et vise à illustrer le concept).

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: mon-pod-critique
   spec:
     containers:
     - name: mon-container
       image: nginx
     affinity:
       nodeAffinity:
         requiredDuringSchedulingRequiredDuringExecution:
           nodeSelectorTerms:
           - matchExpressions:
             - key: hardware
               operator: In
               values:
               - gpu-haute-performance
   ```
   **Note**: Ce manifeste imagine un pod nécessitant un GPU de haute performance pour fonctionner correctement. Il s'agit d'une illustration; la syntaxe et les champs spécifiques peuvent varier selon l'implémentation finale de Kubernetes.

### Étape 3: Discussion et Réflexion

1. **Discussion**: Imaginez que ce pod soit crucial pour votre application, nécessitant constamment l'accès à un GPU de haute performance. Réfléchissez à la manière dont la stratégie `requiredDuringSchedulingRequiredDuringExecution` pourrait affecter la gestion de ce pod. Qu'arriverait-il si le nœud perdait sa capacité GPU ?

### Étape 4: Nettoyage (Théorique)

Dans un environnement réel, si vous aviez pu appliquer ce manifeste et que la fonctionnalité était activée, le nettoyage serait crucial pour éviter les coûts inutiles ou l'utilisation de ressources.

1. **Suppression du Pod**: Pour supprimer le pod (si cela était appliqué), vous utiliseriez la commande suivante:
   ```bash
   kubectl delete pod mon-pod-critique
   ```
2. **Réaffectation ou Mise à Jour des Nœuds**: Si des nœuds ne répondent plus aux exigences, ils devraient être réévalués ou mis à jour pour assurer la conformité continue avec les besoins du pod.

### Conclusion

Bien que `requiredDuringSchedulingRequiredDuringExecution` ne soit pas encore une pratique courante ou disponible, comprendre son intention vous prépare à utiliser Kubernetes de manière plus avancée. Cela souligne l'importance de planifier non seulement le déploiement initial des applications mais aussi leur gestion continue pour répondre aux exigences critiques.