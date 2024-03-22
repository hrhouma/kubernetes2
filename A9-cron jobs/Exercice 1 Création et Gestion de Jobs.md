### Exercice 1: Création et Gestion de Jobs

**Objectif:** Apprendre à créer, observer et gérer des jobs dans Kubernetes.

**Contexte:** Les jobs dans Kubernetes permettent d'exécuter des tâches devant se terminer après avoir exécuté une certaine charge de travail. Ils sont utiles pour les traitements par lots, les tâches de maintenance ou les tâches ponctuelles dans un cluster.

**Instructions:**

1. **Créer un job:** Utilisez la commande suivante pour créer un job qui exécute une tâche simple (`sleep 30`).
   ```bash
   kubectl create job myjob1 --image=busybox -- /bin/sh -c "sleep 30"
   ```
   Expliquez ce que fait cette commande et pourquoi on utilise `busybox`.

2. **Observer le job:** Utilisez `kubectl get jobs` pour voir l'état du job, puis `kubectl describe job myjob1` pour voir les détails et l'état d'exécution du job.

3. **Supprimer le job:** Une fois que vous avez confirmé que le job a été exécuté avec succès (ou après un certain temps), supprimez-le avec `kubectl delete job myjob1`.

4. **Bonus:** Créez un fichier YAML pour le job (`myjob2.yaml`) avec une spécification qui exécute une commande différente, par exemple, afficher le message "Hello Kubernetes". Appliquez ce fichier et observez l'exécution du job.

