**"Maîtrise des Métriques Kubernetes: Déploiement et Surveillance avec Metrics Server"**

Exercice:
**Objectif:**
Déployer le Metrics Server dans votre cluster Kubernetes et analyser les métriques de performance des nœuds et des pods.

**Instructions:**

1. **Déploiement du Metrics Server:**
   - Clonez le dépôt contenant les fichiers de déploiement du Metrics Server.
   - Appliquez les fichiers YAML pour déployer le Metrics Server dans votre cluster.
   - Vérifiez que le Metrics Server est actif et opérationnel.

2. **Collecte des Métriques:**
   - Utilisez la commande `kubectl top nodes` pour afficher les métriques des nœuds.
   - Utilisez la commande `kubectl top pods` pour afficher les métriques des pods.

3. **Analyse des Métriques:**
   - Identifiez le nœud qui consomme le plus de CPU.
   - Identifiez le nœud qui consomme le plus de mémoire.
   - Identifiez le pod qui consomme le plus de mémoire dans l'espace de noms par défaut.
   - Identifiez le pod qui consomme le moins de CPU dans l'espace de noms par défaut.

**Exercice Pratique:**

En supposant que vous ayez un cluster avec les nœuds suivants: `node01`, `node02`, `node03`, et un espace de noms par défaut contenant les pods: `tiger`, `elephant`, `lion`, `rabbit` :

1. Exécutez la commande pour afficher les métriques de tous les nœuds.
2. Déterminez quel nœud a la plus haute consommation de CPU et de mémoire.
3. Exécutez la commande pour afficher les métriques de tous les pods dans l'espace de noms par défaut.
4. Identifiez quel pod a la plus haute consommation de mémoire et lequel a la plus basse consommation de CPU.
5. Documentez vos résultats et les commandes que vous avez utilisées dans un fichier `metrics-analysis.txt`.

**Critères de Réussite:**

- Le Metrics Server est correctement déployé et actif.
- Les métriques des nœuds et des pods sont correctement collectées et affichées.
- L'analyse des métriques est correctement réalisée et documentée.

**Ressources Fournies:**

- Accès au cluster Kubernetes.
- Documentation officielle de Kubernetes pour les commandes `kubectl`.
- Accès au dépôt Git du Metrics Server.

**Livraison:**

- Un fichier `metrics-analysis.txt` contenant toutes les commandes exécutées et les analyses des métriques.

Prenez le temps de compléter cet exercice pour renforcer votre compréhension de la surveillance des ressources dans Kubernetes.