# Théorie orchestration
L'orchestration dans le contexte des technologies de conteneurisation comme Docker et Kubernetes fait référence à la gestion automatisée de plusieurs conteneurs de logiciels. Cela implique le déploiement, la gestion, le scaling, et la mise en réseau de conteneurs de manière efficace et cohérente sur un cluster de machines. L'objectif est de faciliter la gestion des applications complexes et de leurs environnements d'exécution.

## Introduction à l'Orchestration

L'orchestration de conteneurs est essentielle pour déployer, gérer, et scaler des applications conteneurisées sur plusieurs hôtes. Elle automatise la gestion des cycles de vie des conteneurs, la mise en réseau, la disponibilité, et la scalabilité.

## Technologies d'Orchestration

### Docker Swarm

- **Description**: Docker Swarm est un outil d'orchestration natif de Docker qui gère un cluster de Docker Engines. Il est conçu pour être simple d'utilisation et s'intègre directement dans l'écosystème Docker.
- **Caractéristiques clés**: Facilité de déploiement, gestion intégrée des secrets, équilibrage de charge, découvrabilité des services.

### Kubernetes (K8s)

- **Description**: Kubernetes est un système d'orchestration open-source pour automatiser le déploiement, la scalabilité, et la gestion des applications conteneurisées. Il supporte une vaste gamme de conteneurs et d'environnements d'exécution.
- **Caractéristiques clés**: Haute disponibilité, scalabilité, portabilité, écosystème riche, gestion des configurations et des secrets.

### Amazon Elastic Kubernetes Service (EKS)

- **Description**: EKS est un service Kubernetes managé qui simplifie la tâche de lancer un cluster Kubernetes sur AWS. Il automatise des tâches compliquées telles que la mise à jour des nœuds et la configuration de l'haute disponibilité.
- **Caractéristiques clés**: Intégration profonde avec AWS, haute disponibilité, sécurité, et scalabilité.

### Azure Kubernetes Service (AKS)

- **Description**: AKS est un service Kubernetes managé par Microsoft Azure. Il facilite la gestion et le déploiement d'applications conteneurisées, offrant une intégration sans faille avec les services Azure.
- **Caractéristiques clés**: Sécurité renforcée, développement et gestion simplifiés, intégration avec les services Azure, monitoring et diagnostics.

## Meilleures Pratiques

1. **Choix de la Technologie**: Sélectionnez la technologie d'orchestration adaptée à vos besoins spécifiques et à votre infrastructure existante.
2. **Automatisation**: Automatisez autant que possible le déploiement, la mise à jour, et la gestion des conteneurs pour réduire les erreurs humaines.
3. **Monitoring et Logging**: Mettez en place des systèmes de monitoring et de logging robustes pour détecter et résoudre rapidement les problèmes.
4. **Sécurité**: Appliquez les meilleures pratiques de sécurité, comme la gestion des secrets, la limitation des privilèges, et la mise à jour régulière des images de conteneurs.

## Conclusion

L'orchestration des conteneurs est un pilier clé dans le développement et la gestion des applications modernes. Choisir la bonne plateforme d'orchestration et suivre les meilleures pratiques sont essentiels pour maximiser l'efficacité et la sécurité de vos applications conteneurisées.

# pratique docker-swarm-demo

### Prérequis:
- 3 instances AWS EC2 : Pour le master (2 cœurs CPU) , et pour chacun des workers (1CPU).
- Docker installé sur chaque instance.
- Les instances doivent être accessibles via SSH.

### Configuration de l'Environnement:
1. Connectez-vous à votre instance master via SSH.
2. Passez en superutilisateur avec `sudo -s` pour exécuter des commandes avec les privilèges root.

### Installation de docker sur les 3 machines
```bash
git clone https://github.com/hrhouma/install-docker.git
cd install-docker/
chmod +x install-docker.sh
./install-docker.sh
docker version
docker compose version
```

#### Sur le Nœud Manager (Master):
```bash
sudo hostnamectl set-hostname master
exec bash
```

#### Sur le Worker 1:
```bash
sudo hostnamectl set-hostname worker1
exec bash
```

#### Sur le Worker 2:
```bash
sudo hostnamectl set-hostname worker2
exec bash
```

Après avoir exécuté `exec bash`, le nouvel invite de commande devrait refléter le nom d'hôte mis à jour.


### Initialisation de Docker Swarm:
1. Sur le nœud master, initialisez le swarm avec la commande suivante:
   ```
   docker swarm init --advertise-addr <IP_DU_MANAGER> --listen-addr <IP_DU_MANAGER>:2377
   ```
   Remplacez `<IP_DU_MANAGER>` par l'adresse IP privée de votre instance master.

2. Après l'initialisation, un token sera généré. Copiez la commande `docker swarm join` fournie par l'initialisation.

### Configuration des Worker Nodes:
1. Connectez-vous à chaque worker node (2ème et 3ème instances).
2. Passez en superutilisateur avec `sudo -s`.
3. Exécutez la commande `docker swarm join` que vous avez copiée précédemment sur chaque worker node.

### Installation de l'Outil de Visualisation:
1. Revenez au nœud master.
2. Exécutez la commande suivante pour déployer l'outil de visualisation `viz`:
   ```
   docker service create --name=viz --publish=8080:8080/tcp --constraint=node.role==manager --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock dockersamples/visualizer
   ```

### Vérification de l'État du Swarm:
1. Utilisez la commande suivante pour vérifier l'état du swarm et voir tous les nœuds connectés:
   ```
   docker node ls
   ```

### Déploiement de Services:
1. Pour déployer un service nginx et le visualiser, exécutez:
   ```
   docker service create --name nginx --replicas 4 --publish 80:80 -d nginx
   ```

2. Vérifiez le service avec `docker service ls` et `docker service ps nginx` pour voir où les répliques sont exécutées.

### Visualisation du Swarm:
1. Ouvrez un navigateur et accédez à `http://<IP_PUBLIQUE_DU_MANAGER>:8080` pour voir l'outil de visualisation.

### Mise à l'Échelle des Services:
1. Pour mettre à l'échelle le service nginx, utilisez:
   ```
   docker service scale nginx=<NOUVEAU_NOMBRE_DE_RÉPLIQUES>
   ```

2. Vérifiez les changements dans le visualiseur à l'adresse `http://<IP_PUBLIQUE_DU_MANAGER>:8080`.


### Visualisation

L'outil de visualisation, souvent appelé `viz`, est un outil graphique qui permet de visualiser les nœuds d'un Docker Swarm et les conteneurs en cours d'exécution sur chacun d'eux. Pour déployer cet outil sur le nœud manager dans votre cluster Docker Swarm sur AWS, suivez les étapes ci-dessous :

### Déploiement de l'Outil de Visualisation sur le Nœud Manager:

1. **Connexion au Nœud Manager:**
   Connectez-vous à votre instance master (le nœud manager du swarm) via SSH.

2. **Déploiement de Viz:**
   Sur le nœud manager, exécutez la commande suivante pour lancer l'outil de visualisation :

   ```sh
   docker service create \
     --name=viz \
     --publish=8080:8080/tcp \
     --constraint=node.role==manager \
     --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
     dockersamples/visualizer
   ```

   Cette commande va créer un service dans le swarm appelé `viz` qui :
   - Est publié sur le port 8080 de l'hôte.
   - Est contraint de s'exécuter uniquement sur les nœuds ayant le rôle de manager.
   - Montre le socket Docker pour permettre à `viz` de communiquer avec le daemon Docker.

3. **Vérification:**
   Vérifiez que le service est correctement lancé en exécutant :

   ```sh
   docker service ls
   ```

   Vous devriez voir le service `viz` dans la liste avec le nombre de réplicas souhaités.

4. **Accès à l'Outil de Visualisation:**
   Ouvrez un navigateur web et accédez à l'adresse IP publique de votre instance manager sur AWS suivie de `:8080`. Par exemple, si votre IP publique est `65.2.150.126`, vous accéderiez à :

   ```
   http://65.2.150.126:8080
   ```

   Vous devriez voir l'interface graphique de l'outil de visualisation montrant la disposition de votre swarm.

### Notes:

- Assurez-vous que le port 8080 est ouvert dans les règles de groupe de sécurité de votre instance EC2 pour pouvoir accéder à l'outil de visualisation depuis votre navigateur.
- Si vous rencontrez des problèmes pour visualiser le Swarm, vérifiez que le daemon Docker est bien en fonctionnement et que le nœud est en mode manager (`docker node ls` pour vérifier le rôle du nœud).
- L'outil de visualisation est un conteneur qui doit être en cours d'exécution pour que vous puissiez voir l'état actuel de votre Swarm. Si vous le supprimez, vous ne pourrez plus accéder à l'interface de visualisation.

Avec ces étapes, vous devriez pouvoir installer et accéder à l'outil de visualisation sur votre nœud manager Docker Swarm sur AWS.

### Nettoyage:
1. Pour supprimer un service, exécutez:
   ```
   docker service rm <NOM_DU_SERVICE>
   ```
2. Pour quitter un swarm sur un nœud worker, utilisez:
   ```
   docker swarm leave
   ```
3. Sur le nœud master, utilisez `docker swarm leave --force` pour forcer le master à quitter le swarm.

4. Vérifiez que tous les services sont supprimés et que le visualiseur n'est plus accessible.

### Remarques:
- Assurez-vous que les instances EC2 ont des règles de groupe de sécurité appropriées pour permettre la communication entre les nœuds du swarm.
- Utilisez `exit` ou `logout` pour quitter les sessions SSH.

