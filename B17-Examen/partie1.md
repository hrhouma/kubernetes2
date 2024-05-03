# Lecture # 1 avant de faire le QUIZ 
## Lien : https://forms.office.com/r/1sbRXnhQKC

- Avant de passer au quiz, laissez-moi vous guider à travers un tour rapide de Kubernetes.
- Kubernetes est une plateforme open-source pour automatiser le déploiement, la mise à l'échelle et la gestion des applications conteneurisées.
- Dans cet écosystème, plusieurs concepts clés sont à comprendre pour bien naviguer dans votre environnement Kubernetes.

1. **Pods** :
   - Les pods sont les unités de base dans Kubernetes, contenant un ou plusieurs conteneurs.
   - Ils partagent le même contexte réseau et de stockage.

2. **Services** :
   - Les services Kubernetes permettent de s'assurer que les applications puissent communiquer les unes avec les autres.
   - Ils peuvent exposer des applications à l'intérieur ou à l'extérieur du cluster.

3. **Déploiements (Deployments)** :
   - Les déploiements Kubernetes facilitent la gestion des réplicas de vos applications.
   - Ils permettent les mises à jour en douceur et le contrôle du déploiement.

4. **RéplicaSets** :
   - Les RéplicaSets sont utilisés par les déploiements pour garantir un nombre spécifié de répliques de pods.



### 21. Le rôle de _________ dans l'écosystème Kubernetes

Lorsque nous parlons de l'exécution des conteneurs dans un cluster Kubernetes, il est essentiel de comprendre le rôle de différents composants. Parmi eux, __________ est un acteur clé qui opère sur chaque nœud du cluster. Son rôle est crucial car il garantit que les conteneurs s'exécutent comme prévu dans les pods. Ce composant est responsable de la gestion des conteneurs au niveau du nœud, veillant à ce que les pods soient créés, planifiés et exécutés conformément aux spécifications définies. Grâce à son fonctionnement sur chaque nœud, il assure une orchestration fluide des conteneurs, permettant ainsi au cluster de fonctionner de manière efficace et cohérente.

### 22. Gestion de l'affectation des nœuds aux pods

L'une des tâches cruciales dans un cluster Kubernetes est d'affecter efficacement les nœuds aux pods en fonction de la disponibilité des ressources. Pour cela, __________ joue un rôle essentiel. Ce composant est chargé de déterminer quel nœud dans le cluster est le plus approprié pour exécuter un pod donné, en tenant compte des capacités de calcul disponibles, de la charge de travail actuelle et des contraintes spécifiques définies par l'utilisateur. Grâce à cette fonctionnalité, Kubernetes peut optimiser l'utilisation des ressources et garantir des performances optimales de l'ensemble du système.

### 23. Ajout de variables d'environnement au démarrage d'un service

Lorsqu'un service démarre dans un cluster Kubernetes, un processus automatisé intervient pour faciliter son fonctionnement en ajoutant un ensemble de variables d'environnement au pod associé. Ce processus est géré par __________, un daemon qui fonctionne sur chaque nœud du cluster. Son rôle est de garantir que chaque pod dispose des variables d'environnement nécessaires pour interagir avec les autres services du cluster de manière transparente. Ainsi, lors du démarrage d'un service, le daemon agit pour configurer l'environnement du pod, facilitant ainsi son intégration dans l'écosystème Kubernetes.

### 24. Le rôle des Replication Controllers et Deployment Controllers

Au cœur de la gestion des déploiements dans Kubernetes se trouvent les Replication Controllers et Deployment Controllers. Ces composants font partie intégrante du système de contrôle du cluster, mais leur rôle diffère légèrement. Les Replication Controllers sont responsables de garantir un nombre spécifié de répliques de pods en cours d'exécution à tout moment, assurant ainsi la disponibilité et la redondance des applications. D'autre part, les Deployment Controllers permettent de gérer les déploiements d'applications, en facilitant les mises à jour, les retours en arrière et la scalabilité des applications de manière efficace. Ces deux composants travaillent en tandem pour assurer un fonctionnement fluide et fiable des applications dans Kubernetes.

### 25. Exploration de l'espace de noms Kubernetes

Dans l'écosystème Kubernetes, __________ est un espace de noms spécial qui joue un rôle crucial dans la gestion et l'organisation des ressources du cluster. Conçu pour des utilisations spécifiques, cet espace de noms permet de séparer les ressources et les services selon des critères définis, facilitant ainsi la gestion et l'administration du cluster. Par exemple, il peut être utilisé pour démarrer un cluster ou pour héberger des services système essentiels. Grâce à cette fonctionnalité, Kubernetes offre une flexibilité et une extensibilité accrues, permettant aux administrateurs de personnaliser et de configurer leur environnement de manière adaptée à leurs besoins spécifiques.


### 26. Le port par défaut du serveur API Kubernetes

Le serveur API est une composante centrale de Kubernetes, fournissant une interface pour la gestion et le contrôle du cluster. Le port par défaut attribué au serveur API est crucial pour l'interaction avec le cluster. Ainsi, le port par défaut alloué au serveur API dans Kubernetes est __________. Ce port est utilisé pour établir des connexions avec le cluster et exécuter des opérations de gestion telles que le déploiement, la mise à l'échelle et la surveillance des ressources.

### 27. Adresse IP des services dans Kubernetes

Dans Kubernetes, les services sont utilisés pour exposer des applications déployées dans le cluster. Lorsqu'un service est créé, il est associé à une adresse IP unique. Ainsi, chaque service __________. Cette adresse IP permet aux autres composants du cluster de communiquer avec le service de manière transparente, facilitant ainsi la communication entre les différentes parties de l'application.

### 28. Le type de service par défaut dans Kubernetes

Lors de la création d'un service dans Kubernetes, il est important de spécifier le type de service à utiliser. Par défaut, si aucun type de service n'est spécifié, Kubernetes utilise le type de service appelé __________. Ce type de service permet d'exposer le service uniquement à l'intérieur du cluster, en fournissant une adresse IP virtuelle accessible uniquement aux autres composants du cluster. Cela signifie que le service est accessible uniquement à partir de l'intérieur du cluster, et non depuis l'extérieur.

### 29. Affichage des détails d'un objet avec Kubectl

Kubectl est l'outil en ligne de commande principal pour interagir avec un cluster Kubernetes. Lorsque vous souhaitez afficher les détails d'un objet spécifique dans Kubernetes, tels que les pods, les services ou les déploiements, vous devez utiliser la sous-commande appropriée. Ainsi, pour afficher les détails d'un objet, la sous-commande correcte à utiliser est __________. Cette commande fournit des informations détaillées sur l'objet spécifié, y compris ses propriétés, son état actuel et ses métadonnées.

### 30. États valides du cycle de vie d'un Pod

Dans Kubernetes, les pods peuvent passer par plusieurs états tout au long de leur cycle de vie, reflétant leur état actuel et leur progression. Parmi ces états, lesquels sont considérés comme valides ? Les pods peuvent être dans un état __________, indiquant qu'ils sont en cours d'exécution et fonctionnent normalement. De même, un pod peut être dans un état __________, indiquant qu'il est prêt à exécuter, mais attend toujours d'être planifié sur un nœud. En revanche, un pod peut également être dans un état __________, indiquant qu'il a rencontré une erreur ou a échoué lors de son exécution. Enfin, un état __________ est utilisé lorsque l'état actuel du pod ne peut pas être déterminé.

Dans chaque réponse, veillez à remplacer les espaces vides par les options appropriées pour chaque question. Cela permettra aux étudiants de réfléchir à chaque question et d'apprendre à répondre de manière détaillée.

### 31. Création d'un déploiement Kubernetes

Lorsque vous souhaitez créer un déploiement dans Kubernetes à partir d'une image Docker spécifique, vous devez utiliser la commande appropriée pour garantir que le déploiement est configuré correctement. Ainsi, la commande __________ permet de créer un déploiement nommé "my-app" à partir de l'image Docker "nginx". Cette commande assure que le déploiement est correctement configuré et prêt à être déployé dans le cluster Kubernetes.

### 32. Affichage des journaux d'un pod

Pour déboguer et surveiller les pods dans Kubernetes, il est souvent nécessaire d'afficher les journaux générés par les conteneurs en cours d'exécution. La commande __________ est utilisée pour afficher les journaux d'un pod spécifique nommé "my-pod". Ces journaux fournissent des informations précieuses sur les événements et les activités se déroulant à l'intérieur du pod, ce qui facilite le processus de débogage et de résolution des problèmes.

### 33. Listing des ressources dans un espace de noms spécifique

Pour obtenir une vue d'ensemble des ressources déployées dans un espace de noms particulier, vous devez utiliser la commande adéquate. Ainsi, la commande __________ permet de lister toutes les ressources présentes dans l'espace de noms "production". Cela inclut les pods, les services, les déploiements et d'autres ressources déployées dans cet espace de noms spécifique, offrant ainsi une visibilité complète sur l'état actuel du cluster dans cet environnement.

### 34. Mise à jour de l'image d'un déploiement

Lorsqu'une nouvelle version d'une application est disponible, il est nécessaire de mettre à jour l'image utilisée par un déploiement Kubernetes. La commande __________ est utilisée pour mettre à jour l'image d'un déploiement nommé "my-deployment" afin d'utiliser la dernière version de l'image "nginx:latest". Cette commande garantit que le déploiement est mis à jour avec la nouvelle version de l'image, permettant ainsi aux utilisateurs de bénéficier des dernières fonctionnalités et correctifs de sécurité.

### 35. Affichage des informations détaillées d'un nœud Kubernetes

Pour obtenir des informations détaillées sur un nœud spécifique dans un cluster Kubernetes, vous devez utiliser la commande appropriée. Ainsi, la commande __________ est utilisée pour afficher les informations détaillées du nœud Kubernetes nommé "node1". Ces informations incluent des détails tels que la capacité du nœud, son état actuel, les pods qui y sont planifiés et d'autres métadonnées importantes. Cela permet aux administrateurs de comprendre l'état et les performances du nœud et de prendre des décisions éclairées en matière de gestion du cluster.

### 41. Création d'un pod dans Kubernetes

Lorsque vous souhaitez créer un pod dans Kubernetes à partir d'une image Docker spécifique, vous devez utiliser la commande appropriée pour garantir que le pod est configuré correctement. Ainsi, la commande __________ est utilisée pour créer un pod nommé "my-pod" à partir de l'image Docker "nginx". Cette commande assure que le pod est correctement créé et prêt à être déployé dans le cluster Kubernetes.

### 42. Démarrage d'un pod arrêté dans Kubernetes

Si un pod spécifique dans Kubernetes est arrêté et que vous souhaitez le redémarrer, vous devez utiliser la commande appropriée pour garantir son redémarrage. Ainsi, la commande __________ est utilisée pour démarrer le pod nommé "my-pod" s'il est arrêté. Cette commande garantit que le pod est redémarré et que son fonctionnement est restauré dans le cluster Kubernetes.

### 43. Gestion des répliques dans un ReplicaSet

Pour garantir un certain nombre de répliques en cours d'exécution dans un cluster Kubernetes, même en cas de suppression ou de panne de certaines d'entre elles, vous devez utiliser la commande appropriée pour spécifier le nombre de répliques souhaité. Ainsi, la commande __________ est utilisée pour s'assurer qu'il y a toujours 3 pods en cours d'exécution dans un cluster à l'aide d'un ReplicaSet nommé "my-replicaset". Cette commande garantit que le ReplicaSet maintient toujours le nombre spécifié de répliques en cours d'exécution, assurant ainsi la disponibilité et la résilience de l'application déployée.

### 44. Suppression d'un pod avec remplacement automatique par le ReplicaSet

Lorsque vous souhaitez supprimer un pod spécifique dans Kubernetes tout en garantissant que le ReplicaSet le remplace automatiquement, vous devez utiliser la commande appropriée pour éviter toute interruption de service. Ainsi, la commande __________ est utilisée pour supprimer le pod nommé "my-pod" tout en s'assurant que le ReplicaSet le remplace automatiquement. Cette commande garantit que le pod est supprimé en toute sécurité et que le ReplicaSet génère automatiquement un nouveau pod pour maintenir le nombre spécifié de répliques en cours d'exécution dans le cluster Kubernetes.

### 41. Création d'un pod dans Kubernetes

Lorsque vous souhaitez créer un pod dans Kubernetes à partir d'une image Docker spécifique, vous devez utiliser la commande appropriée pour garantir que le pod est configuré correctement. Ainsi, la commande __________ est utilisée pour créer un pod nommé "my-pod" à partir de l'image Docker "nginx". Cette commande assure que le pod est correctement créé et prêt à être déployé dans le cluster Kubernetes.

### 42. Démarrage d'un pod arrêté dans Kubernetes

Si un pod spécifique dans Kubernetes est arrêté et que vous souhaitez le redémarrer, vous devez utiliser la commande appropriée pour garantir son redémarrage. Ainsi, la commande __________ est utilisée pour démarrer le pod nommé "my-pod" s'il est arrêté. Cette commande garantit que le pod est redémarré et que son fonctionnement est restauré dans le cluster Kubernetes.

### 43. Gestion des répliques dans un ReplicaSet

Pour garantir un certain nombre de répliques en cours d'exécution dans un cluster Kubernetes, même en cas de suppression ou de panne de certaines d'entre elles, vous devez utiliser la commande appropriée pour spécifier le nombre de répliques souhaité. Ainsi, la commande __________ est utilisée pour s'assurer qu'il y a toujours 3 pods en cours d'exécution dans un cluster à l'aide d'un ReplicaSet nommé "my-replicaset". Cette commande garantit que le ReplicaSet maintient toujours le nombre spécifié de répliques en cours d'exécution, assurant ainsi la disponibilité et la résilience de l'application déployée.

### 44. Suppression d'un pod avec remplacement automatique par le ReplicaSet

Lorsque vous souhaitez supprimer un pod spécifique dans Kubernetes tout en garantissant que le ReplicaSet le remplace automatiquement, vous devez utiliser la commande appropriée pour éviter toute interruption de service. Ainsi, la commande __________ est utilisée pour supprimer le pod nommé "my-pod" tout en s'assurant que le ReplicaSet le remplace automatiquement. Cette commande garantit que le pod est supprimé en toute sécurité et que le ReplicaSet génère automatiquement un nouveau pod pour maintenir le nombre spécifié de répliques en cours d'exécution dans le cluster Kubernetes.

### 45. Liste des ReplicaSets dans Kubernetes

Pour obtenir une vue d'ensemble de tous les ReplicaSets déployés dans un cluster Kubernetes, vous devez utiliser la commande appropriée pour récupérer ces informations. Ainsi, la commande __________ est utilisée pour lister tous les ReplicaSets présents dans le cluster. Cette commande fournit une liste complète de tous les ReplicaSets, y compris leurs noms, leurs nombres de répliques et d'autres détails pertinents, offrant ainsi une visibilité complète sur les contrôleurs de réplication actifs dans l'environnement Kubernetes.

### 46. Exposition des pods en utilisant le load balancing

Lorsque vous souhaitez exposer les pods en dehors du cluster Kubernetes en utilisant le load balancing pour distribuer la charge de manière équilibrée, vous devez utiliser le type de service approprié. Ainsi, le type de service Kubernetes utilisé dans cette situation est __________. Ce service expose les pods à l'extérieur du cluster en utilisant une adresse IP externe et assure l'équilibrage de charge entre les pods, garantissant ainsi une distribution uniforme du trafic entrant.

### 47. Service par défaut pour la communication entre les pods d'un même cluster

Pour faciliter la communication entre les pods déployés dans un même cluster Kubernetes, vous devez utiliser le type de service par défaut. Ainsi, le type de service Kubernetes utilisé dans cette situation est __________. Ce service permet aux pods de communiquer entre eux au sein du même cluster en utilisant une adresse IP interne, facilitant ainsi l'interaction et la coordination entre les différentes parties de l'application déployée dans l'environnement Kubernetes.

### 48. Mapping d'un nom DNS statique à une adresse de service externe

Lorsque vous avez besoin de mapper un nom DNS statique à une adresse de service externe dans Kubernetes, vous devez utiliser le type de service approprié pour cette tâche. Ainsi, le type de service Kubernetes utilisé dans cette situation est __________. Ce service permet de mapper un nom DNS statique à une adresse de service externe, offrant ainsi une manière pratique d'accéder aux services déployés dans le cluster Kubernetes à l'aide de noms faciles à retenir plutôt que d'utiliser des adresses IP.

### 49. Exposition des pods via un port fixe sur chaque nœud

Pour permettre aux utilisateurs externes d'accéder aux pods déployés dans un cluster Kubernetes via un port fixe sur chaque nœud du cluster, vous devez utiliser le type de service approprié. Ainsi, le type de service Kubernetes utilisé dans cette situation est __________. Ce service expose un port fixe sur chaque nœud du cluster, permettant ainsi aux utilisateurs externes d'accéder aux pods de manière fiable et cohérente, indépendamment de la répartition des charges ou de la localisation des pods sur les nœuds.

### 50. Exposition directe des pods aux autres nœuds sans load balancing interne

- Si vous avez besoin d'exposer directement les pods aux autres nœuds du cluster Kubernetes sans impliquer de mécanismes d'équilibrage de charge internes, vous devez utiliser le type de service approprié.
- Ainsi, le type de service Kubernetes utilisé dans cette situation est __________.
- Ce service expose les pods directement aux autres nœuds sans créer de point d'accès unique ou d'équilibrage de charge interne, permettant ainsi une communication directe entre les pods déployés dans le cluster.
