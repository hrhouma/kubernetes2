# Exercices sur les Taints et Tolérances Kubernetes

Les taints et tolérances sont des mécanismes de Kubernetes permettant de contrôler où les pods peuvent être planifiés en fonction des nœuds. Les taints sont appliqués aux nœuds et les tolérances sont appliquées aux pods. Un taint sur un nœud repousse les pods qui n'ont pas de tolérance correspondante.

## Exercice 1 : Compréhension des Concepts

1. **Qu'est-ce qu'un Taint ?**  
   Expliquez ce que signifie "tainter" un nœud dans Kubernetes. Quels types d'effets les taints peuvent-ils avoir sur la planification des pods ?

2. **Qu'est-ce qu'une Tolérance ?**  
   Décrivez ce qu'est une tolérance dans le contexte des pods Kubernetes et comment une tolérance permet à un pod d'être planifié sur un nœud qui a un taint spécifique.

## Exercice 2 : Application Pratique

1. **Identification des Taints sur un Nœud :**  
   À partir de la commande `k describe no demo-worker | grep Taint`, imaginez quelle serait la sortie et expliquez ce que cela indique concernant la capacité des pods à être planifiés sur le nœud `demo-worker`.

2. **Création d'un Pod avec Tolérance :**  
   En vous basant sur le fichier `pod-tolerations.yaml`, expliquez comment le pod `nginx` est configuré pour tolérer le taint appliqué au nœud `demo-worker`. Décrivez chaque champ sous la section `tolerations` et son rôle dans la tolérance du taint.

## Exercice 3 : Analyse et Correction

1. **Analyse de Configuration :**  
   Imaginez qu'un pod ne se lance pas comme prévu sur un nœud qui a un taint. Quelles étapes suivriez-vous pour diagnostiquer le problème ? Considérez les commandes que vous utiliseriez pour inspecter les taints sur le nœud et les tolérances sur le pod.

2. **Correction d'une Tolérance :**  
   Supposons que vous avez un pod qui devrait être planifié sur un nœud tainté `lock=blue:NoSchedule`, mais le pod n'a pas été démarré sur ce nœud. Le fichier de configuration du pod contient une erreur. Corrigez la tolérance suivante pour qu'elle corresponde au taint :

   ```yaml
   tolerations:
     - key: "lock"
       operator: "Equal"
       value: "yellow"  # Cette valeur est incorrecte. Quelle devrait-elle être ?
       effect: "NoSchedule"
   ```
