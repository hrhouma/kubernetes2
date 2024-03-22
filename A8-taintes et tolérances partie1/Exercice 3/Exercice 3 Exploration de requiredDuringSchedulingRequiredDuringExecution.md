### Exercice 3: Exploration de `requiredDuringSchedulingRequiredDuringExecution`

**Note Importante:** Au moment de la rédaction, `requiredDuringSchedulingRequiredDuringExecution` est une fonctionnalité en alpha ou pas encore implémentée dans toutes les versions de Kubernetes. Cet exercice est donc plus théorique et vise à comprendre le concept et l'intention derrière cette stratégie d'affinité de nœuds, en préparation pour son utilisation future.

**Objectif:** Comprendre le concept de `requiredDuringSchedulingRequiredDuringExecution` pour maintenir les exigences d'affinité non seulement lors de la planification initiale du pod mais aussi pendant toute sa durée de vie.

### Contexte

Dans certains cas, il est crucial que les pods restent sur des nœuds qui respectent certaines contraintes tout au long de leur exécution. Par exemple, pour des raisons de conformité réglementaire ou de dépendances matérielles spécifiques, il peut être nécessaire de garantir que les pods ne s'exécutent que sur des nœuds autorisés.

### Étapes Théoriques

#### Étape 1: Compréhension du Concept

1. **Théorie:**
   - `requiredDuringSchedulingRequiredDuringExecution` est conçu pour s'assurer que les pods restent sur des nœuds qui continuent de satisfaire leurs contraintes d'affinité tout au long de leur cycle de vie. Si un nœud cesse de respecter ces contraintes, le système doit réagir pour rester conforme, potentiellement en redémarrant ou en déplaçant les pods vers un nœud approprié.

#### Étape 2: Création d'une Affinité de Nœud Hypothétique

1. **Conception d'un Manifeste de Pod:**
   - Imaginez et rédigez un manifeste de pod (hypothétique) qui utilise cette stratégie pour une application nécessitant un GPU spécifique. Utilisez une étiquette fictive `gpu=typeA` pour démontrer cette exigence.
   - **Exemple de Manifeste:**
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: pod-gpu-exigeant
     spec:
       containers:
       - name: application-gpu
         image: application-gpu:latest
       affinity:
         nodeAffinity:
           requiredDuringSchedulingRequiredDuringExecution:
             nodeSelectorTerms:
             - matchExpressions:
               - key: gpu
                 operator: In
                 values:
                 - typeA
     ```

#### Étape 3: Discussion sur l'Application Pratique

1. **Analyse de Scénario:**
   - Discutez de l'importance d'une telle stratégie pour les applications critiques dont les exigences ne doivent pas être compromises (par exemple, une application de traitement médical en temps réel nécessitant un type spécifique de traitement GPU pour fonctionner correctement).

#### Étape 4: Réflexion sur les Défis Potentiels

1. **Considérations:**
   - Réfléchissez aux défis que pourrait poser l'implémentation de cette stratégie, comme la nécessité de redémarrer fréquemment les pods, l'impact sur la disponibilité des applications, et les stratégies de gestion des nœuds pour maintenir la conformité aux exigences d'affinité.

### Activité Complémentaire

- **Recherche et Présentation:**
   - Encouragez les participants à rechercher si et comment `requiredDuringSchedulingRequiredDuringExecution` a été implémenté dans les versions récentes de Kubernetes et à partager leurs découvertes, ainsi que les meilleures pratiques proposées pour son utilisation.

### Conclusion

Bien que `requiredDuringSchedulingRequiredDuringExecution` ne soit pas encore une fonctionnalité largement disponible ou stable, comprendre son intention et son application potentielle prépare les administrateurs et les développeurs à tirer parti de niveaux de contrôle plus granulaires sur le placement et la gestion des pods dans un cluster Kubernetes, en vue de répondre à des exigences strictes de conformité, de performance ou de sécurité.