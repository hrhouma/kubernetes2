# Quiz1

1. _________ fonctionne sur chaque nœud et assure que les conteneurs fonctionnent dans un pod.
   a. Kubelet
   b. etcd
   c. scheduler
   d. Pod

2. _______ gère l'attribution des nœuds aux pods en fonction de la disponibilité des ressources.
   a. etcd
   b. kubectl
   c. scheduler
   d. Calico

3. Dès qu'un service démarre, le daemon ________ fonctionnant sur chaque nœud ajoute un ensemble de variables d'environnement sur le pod pour chaque service actif.
   a. kubectl
   b. kubelet
   c. kubeadm
   d. découverte de service

4. Les contrôleurs de réplication et les contrôleurs de déploiement font partie de
   a. Etcd
   b. Gestionnaire de contrôleurs maître
   c. Nœud de travail
   d. Kubeadm

5. __________________ est un espace de noms spécial utilisé à des fins spéciales comme l'amorçage d'un cluster.
   a. kube-public
   b. kube-private
   c. kube-system
   d. par défaut

6. Quel est le port par défaut alloué au serveur API ?
   a. 6443
   b. 443
   c. 80
   d. 8080

7. Un service dispose de sa propre adresse IP dans Kubernetes ?
   a. Vrai
   b. Faux

8. Lequel des éléments suivants est le type de service par défaut dans Kubernetes ?
   a. NodePort
   b. ClusterIP
   c. LoadBalancer
   d. Nom externe

9. Lequel des sous-commandes de "kubectl" doit-on utiliser pour regarder les détails d'un objet ?
   a. info
   b. détails
   c. décrire
   d. expliquer

10. Dans Kubernetes, lequel des états suivants du cycle de vie d'un Pod n'est pas valide ?
    a. En cours d'exécution
    b. En attente
    c. Échoué
    d. Inconnu

11. À quoi sert un Dockerfile ?
    a. Créer une image personnalisée
    b. Créer un conteneur personnalisé
    c. Supprimer une image
    d. Exécuter une image

12. Quel est le réseau par défaut pour les conteneurs docker ?
    a. Hôte
    b. Superposition
    c. Pont
    d. MacVlan

13. Le __________ permet à chaque nœud dans l'essaim d'accepter des connexions sur les ports publiés pour n'importe quel service fonctionnant dans l'essaim, même s'il n'y a pas de tâche fonctionnant sur le nœud.
    a. conteneur docker
    b. image docker
    c. maillage de routage
    d. Aucun de ces réponses

14. _______ sont le mécanisme préféré pour persister les données générées par et utilisées par les conteneurs Docker.
    a. Images Docker
    b. Conteneurs Docker
    c. Volumes Docker
    d. Service Docker

15. ___________ Affiche des informations détaillées sur un ou plusieurs services.
    a. docker service ls
    b. docker service rm
    c. docker service scale
    d. docker service inspect

# Voici les réponses aux questions fournies :

1. a. Kubelet
2. c. scheduler
3. b. kubelet
4. b. Gestionnaire de contrôleurs maître
5. c. kube-system
6. a. 6443
7. a. Vrai
8. b. ClusterIP
9. c. décrire
10. d. Inconnu
11. a. Créer une image personnalisée
12. c. Pont
13. c. maillage de routage
14. c. Volumes Docker
15. d. docker service inspect
