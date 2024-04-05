# 1 - Introduction

Dans cet atelier pratique, vous allez apprendre les concepts de base de Kubernetes. Vous y parviendrez en interagissant avec Kubernetes via les terminaux de commande à droite. En fin de compte, vous déployerez les applications d'exemple Dockercoins sur les deux nœuds de travail.

## Démarrage

### Que sont ces terminaux dans les navigateurs ?

Sur votre droite, vous devriez voir deux fenêtres de terminal. Il se peut qu'on vous demande de vous connecter d'abord, ce que vous pouvez faire soit avec un identifiant Docker soit avec un compte GitHub. Ces terminaux sont des terminaux entièrement fonctionnels exécutant Centos. En fait, ce sont les lignes de commande pour des conteneurs Centos qui tournent sur notre infrastructure Play-with-Kubernetes. Lorsque vous voyez des blocs de code qui ressemblent à cela, vous pouvez soit cliquer dessus et ils se rempliront automatiquement, soit vous pouvez les copier et les coller.

```
ls
```

### Démarrer le cluster

La première étape consiste à initialiser le cluster dans le premier terminal :

```
kubeadm init --apiserver-advertise-address $(hostname -i)
```

Cela prendra quelques minutes, pendant lesquelles vous verrez beaucoup d'activité dans le terminal.

Vous verrez quelque chose comme cela à la fin :

```
Votre maître Kubernetes a été initialisé avec succès !

Pour commencer à utiliser votre cluster, vous devez exécuter (en tant qu'utilisateur régulier) :

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Vous devriez maintenant déployer un réseau de pod dans le cluster.
Exécutez "kubectl apply -f [podnetwork].yaml" avec une des options listées à :
  http://kubernetes.io/docs/admin/addons/

Vous pouvez maintenant rejoindre n'importe quel nombre de machines en exécutant la commande suivante sur chaque nœud en tant que root :

kubeadm join --token SOMETOKEN SOMEIPADDRESS --discovery-token-ca-cert-hash SOMESHAHASH
```

Copiez toute la ligne qui commence par kubeadm join du premier terminal et collez-la dans le second terminal. Vous devriez voir quelque chose comme cela :

```
kubeadm join --token a146c9.9421a0d62a0611f4 172.26.0.2:6443 --discovery-token-ca-cert-hash sha256:9a4dc07bd8ac596336ecee6ce0928b3500174037c07a38a03bebef25e97c4db5
Initialisation de l'ID machine à partir d'un générateur aléatoire.
[kubeadm] ATTENTION : kubeadm est en bêta, veuillez ne pas l'utiliser pour des clusters de production.
[preflight] Passage des vérifications préalables
[discovery] Tentative de connexion au serveur API "172.26.0.2:6443"
[discovery] Client de découverte d'informations du cluster créé, demande d'infos à "https://172.26.0.2:6443"
[discovery] Demande d'infos à "https://172.26.0.2:6443" à nouveau pour valider le TLS contre la clé publique épinglée
[discovery] La signature et le contenu des infos du cluster sont valides et le certificat TLS valide contre les racines épinglées, utilisera le serveur API "172.26.0.2:6443"
[discovery] Connexion établie avec succès avec le serveur API "172.26.0.2:6443"
[bootstrap] Version du serveur détectée : v1.8.11
[bootstrap] Le serveur prend en charge l'API Certificates (certificates.k8s.io/v1beta1)

Jointure du nœud complète :
* Demande de signature de certificat envoyée au maître et réponse reçue.
* Kubelet informé des nouveaux détails de connexion sécurisée.

Exécutez 'kubectl get nodes' sur le maître pour voir cette machine rejoindre.
```

Cela signifie que vous êtes presque prêt à partir. Enfin, vous devez

 juste initialiser votre réseau de cluster dans le premier terminal :

```
kubectl apply -n kube-system -f \
 "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 |tr -d '\n')"
```

Vous verrez une sortie comme celle-ci :

```
kubectl apply -n kube-system -f \
>     "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 |tr -d '\n')"
serviceaccount "weave-net" créé
clusterrole "weave-net" créé
clusterrolebinding "weave-net" créé
role "weave-net" créé
rolebinding "weave-net" créé
daemonset "weave-net" créé
```

Votre cluster est configuré !

### Quelle est cette application ?

C'est un mineur DockerCoin ! 💰🐳📦🚢

Non, vous ne pouvez pas acheter de café avec des DockerCoins

Comment fonctionnent les DockerCoins :

- Le travailleur demande à rng de générer quelques octets aléatoires
- Le travailleur nourrit ces octets dans hasher
- Et répète à l'infini !
- Chaque seconde, le travailleur met à jour redis pour indiquer combien de boucles ont été faites
- webui interroge redis, et calcule et expose la "vitesse de hachage" dans votre navigateur

### Obtenir le code source de l'application

Nous avons créé une application d'exemple à exécuter pour des parties de l'atelier. L'application se trouve dans le dépôt dockercoins.

Jetons un coup d'œil à la disposition générale du code source :

- il y a un fichier Compose docker-compose.yml …
- … et 4 autres services, chacun dans son propre répertoire :
  - rng = service web générant des octets aléatoires
  - hasher = service web calculant le hash des données POSTées
  - worker = processus en arrière-plan utilisant rng et hasher
  - webui = interface web pour suivre les progrès

Nous allons cloner le dépôt GitHub

Le dépôt contient également des scripts et des outils que nous utiliserons tout au long de l'atelier

```
git clone https://github.com/dockersamples/dockercoins
```

(Vous pouvez également fork le dépôt sur GitHub et cloner votre fork si vous préférez cela.)

### Exécuter l'application

Allez dans le répertoire dockercoins, dans le dépôt cloné :

```
cd ~/dockercoins
```

Utilisez Compose pour construire et lancer tous les conteneurs :

```
docker-compose up
```

Compose indique à Docker de construire toutes les images de conteneur (en téléchargeant les images de base correspondantes), puis démarre tous les conteneurs et affiche les logs agrégés.

#### Beaucoup de logs

L'application génère continuellement des logs

Nous pouvons voir le service worker faire des requêtes à rng et à hasher

Mettons cela en arrière-plan

### Se connecter à l'interface web

Le conteneur webui expose un tableau de bord web ; voyons-le

Avec un navigateur web, connectez-vous à node1 ici sur le port 8000 (créé lorsque vous avez exécuté l'application)

### Nettoyage

Avant de continuer, éteignons tout en tapant Ctrl-C.

# 2 - Concepts #1 de Kubernetes 

## Introduction à Kubernetes

Kubernetes est un système de gestion de conteneurs qui permet de déployer, de gérer et d'orchestrer des applications conteneurisées sur un cluster. Mais que signifie réellement cette définition ?

### Qu'est-ce que Kubernetes ?

- **Système de gestion de conteneurs :** Kubernetes vous permet de gérer des applications qui sont divisées en conteneurs, en s'assurant qu'ils s'exécutent là où et quand vous le souhaitez.
- **Cluster :** Les conteneurs sont exécutés sur un ensemble de machines appelé cluster. Ce cluster peut être sur site ou dans le cloud.

### Actions de base avec Kubernetes

Avec Kubernetes, vous pouvez demander de :

- **Démarrer des conteneurs :** Par exemple, lancer 5 conteneurs utilisant l'image `atseashop/api:v1.3`.
- **Balancer la charge :** Placer un équilibreur de charge interne devant ces conteneurs ou un équilibreur de charge public devant 10 conteneurs utilisant l'image `atseashop/webfront:v1.3`.
- **Gérer le trafic :** Augmenter la capacité de votre cluster lors de pics de trafic, comme le Black Friday.
- **Mettre à jour des applications :** Remplacer vos conteneurs par de nouveaux utilisant une image mise à jour, par exemple `atseashop/webfront:v1.4`, tout en continuant de traiter les requêtes.

### Capacités avancées de Kubernetes

Kubernetes ne s'arrête pas là. Il peut également :

- **Mettre à l'échelle automatiquement :** Ajuster le nombre de conteneurs en fonction de la charge.
- **Déploiement blue/green et canary :** Tester de nouvelles versions d'applications sans impact sur les utilisateurs actuels (https://circleci.com/blog/canary-vs-blue-green-downtime/#:~:text=In%20blue%2Dgreen%20deployment%20you,first%2C%20before%20finishing%20the%20others)
- Dans le déploiement blue-green, vous servez l'application actuelle sur une moitié de votre environnement (Bleu) et déployez votre nouvelle application sur l'autre moitié (Vert) sans affecter l'environnement bleu. Dans le déploiement canari, vous basculez juste un petit sous-ensemble de serveurs ou de nœuds en premier, avant de terminer avec les autres.
  
- **Gérer des tâches de longue durée et des jobs ponctuels :** Exécuter des services persistants ainsi que des tâches qui ne doivent s'exécuter qu'une seule fois.
- **Surdimensionner et évincer les tâches de faible priorité :** Utiliser pleinement les ressources du cluster tout en gérant les priorités entre tâches.
- **Exécuter des services avec des données à état :** Gérer des bases de données et d'autres types de services nécessitant un stockage persistant.
- **Contrôle d'accès détaillé :** Définir qui peut faire quoi au sein du cluster.
- **Intégrer des services tiers :** Utiliser des services externes comme s'ils faisaient partie de votre cluster.
- **Automatiser des tâches complexes :** Utiliser des opérateurs pour gérer des applications de manière plus sophistiquée.

## Résumé des concepts #1 de Kubernetes



# 3- L'Architecture de Kubernetes #1 (introduction)

## Encore et encore un rappel Kubernetes

Avant de plonger dans l'architecture de Kubernetes, comprenons ce qu'est Kubernetes. Kubernetes est un système open-source pour automatiser le déploiement, la mise à l'échelle et la gestion des applications conteneurisées. Il vous aide à gérer des applications complexes avec efficacité et flexibilité.

## Architecture de Kubernetes

L'architecture de Kubernetes est divisée en deux parties principales : le maître (master) et les nœuds (nodes).

### Le Maître Kubernetes

Le "maître" est le cerveau derrière l'opération de Kubernetes. Il prend des décisions globales sur le cluster (comme le lancement de nouveaux conteneurs) et détecte et répond aux événements du cluster (comme le crash d'un conteneur). Les composants clés du maître incluent :

- **API Server :** Le point d'entrée dans Kubernetes, permettant la communication entre les différents services.
- **Scheduler :** Décide sur quel nœud lancer les nouveaux conteneurs basés sur la disponibilité des ressources.
- **Controller Manager :** Gère un ensemble de contrôleurs qui régulent l'état du cluster.
- **etcd :** Un magasin clé/valeur hautement disponible qui stocke toutes les données du cluster, agissant comme la base de données de Kubernetes.

Ces services peuvent s'exécuter directement sur une machine hôte ou dans des conteneurs. Il est possible d'avoir plusieurs maîtres pour une haute disponibilité.

### Les Nœuds Kubernetes

Les nœuds sont les machines (physiques ou virtuelles) qui exécutent vos applications et workloads Kubernetes. Chaque nœud contient les services nécessaires pour exécuter des conteneurs, notamment :

- **Moteur de conteneur (Docker, par exemple) :** Pour exécuter les conteneurs.
- **Kubelet :** Un agent qui s'assure que les conteneurs s'exécutent dans un Pod.
- **Kube-proxy :** Gère le réseau des conteneurs, y compris l'équilibrage de charge.

Il est courant de ne pas exécuter d'applications sur les nœuds qui exécutent les composants du maître, sauf dans le cas de petits clusters de développement.

## Conclusion

Comprendre l'architecture de Kubernetes est essentiel pour gérer efficacement les applications conteneurisées à grande échelle. Avec son architecture maître/nœud, Kubernetes offre une plateforme robuste et flexible pour déployer, échelonner et gérer vos applications dans un environnement de cloud.


# 4- L'Architecture de Kubernetes #2 - côté maître (en détail)

Explorons en détail les composants du maître Kubernetes pour comprendre leur rôle et leur fonctionnement au sein de l'architecture de Kubernetes, sans répéter les informations déjà fournies.

### API Server

Le serveur API agit comme la porte d'entrée pour toutes les opérations de gestion du cluster Kubernetes. C'est par ce canal que les requêtes sont envoyées, que ce soit pour créer de nouveaux pods, gérer les ressources existantes ou interroger l'état du système. Cette interface RESTful assure que les utilisateurs, les services internes de Kubernetes, et les composants externes peuvent communiquer de manière sécurisée et efficace. Le serveur API est conçu pour être l'unique point de convergence pour la logique d'orchestration, garantissant que toutes les modifications de l'état du cluster passent par un processus de validation et d'autorisation uniforme.

### Scheduler

Le planificateur, ou scheduler, est chargé de l'attribution des pods aux nœuds. Il analyse l'état actuel du cluster, y compris la consommation des ressources par chaque nœud, et prend des décisions éclairées sur l'endroit où déployer les nouveaux pods en fonction des exigences de ressources et des contraintes spécifiées. Ce processus assure une distribution optimale des charges de travail à travers le cluster, améliorant ainsi l'efficacité et la performance globale du système. Le scheduler peut être personnalisé pour répondre à des politiques de planification spécifiques, permettant aux administrateurs de prioriser certains types de charges de travail ou d'adapter le comportement du planificateur aux besoins de leur environnement.

### Controller Manager

Le gestionnaire de contrôleurs supervise une variété de contrôleurs qui régulent l'état désiré du cluster Kubernetes. Chaque contrôleur est responsable d'une facette particulière de l'état global du cluster, comme la gestion des réplicas de pods, la gestion des nœuds, ou la gestion des endpoints. En surveillant en continu l'état du cluster et en effectuant des ajustements selon les besoins, le Controller Manager joue un rôle clé dans l'auto-réparation et la gestion des configurations déclaratives. Cette composante assure que l'état réel du cluster correspond à l'état désiré spécifié par l'utilisateur, traitant les événements tels que les défaillances de nœuds ou les modifications de configuration.

### etcd

etcd est le stockage de données distribué qui sert de pierre angulaire pour la cohérence de l'état du cluster Kubernetes. En tant que base de données clé/valeur, etcd mémorise l'état de configuration, le statut, et les métadonnées essentielles de toutes les ressources Kubernetes. Grâce à sa haute disponibilité et sa tolérance aux pannes, etcd permet à Kubernetes de récupérer rapidement de défaillances du maître, en garantissant qu'aucune donnée essentielle n'est perdue et que l'état du cluster peut être restauré à un état cohérent. Le choix d'etcd comme magasin de données reflète l'importance de la fiabilité, de la performance, et de la cohérence des données dans la gestion des environnements distribués complexes.

Chacun de ces composants joue un rôle crucial dans la gestion du cluster Kubernetes, travaillant ensemble pour offrir une plateforme robuste, évolutive et hautement disponible pour déployer et gérer des applications conteneurisées à grande échelle.


# 5- L'Architecture de Kubernetes #3 - côté worker (en détail - 3 composants clés)

Approfondissons le côté travailleur (worker) dans l'architecture Kubernetes, en mettant l'accent sur les composants essentiels qui facilitent l'exécution et la gestion des conteneurs au sein d'un nœud de travail.

### Container Runtime

Le moteur d'exécution de conteneurs est le fondement qui permet à Kubernetes de lancer et de gérer les conteneurs sur chaque nœud. Ce composant crée un environnement isolé pour chaque conteneur, s'occupant de l'exécution du code de l'application, de la gestion des ressources système attribuées (comme la CPU, la mémoire, et le stockage), et de l'isolation du réseau. Kubernetes est compatible avec plusieurs moteurs d'exécution de conteneurs, y compris Docker, containerd, et CRI-O, offrant aux utilisateurs la flexibilité de choisir la technologie qui convient le mieux à leurs besoins spécifiques. Cette couche d'abstraction assure que les applications s'exécutent de manière cohérente, indépendamment de l'environnement sous-jacent.

### Kubelet

Kubelet agit comme un agent sur chaque nœud, communiquant directement avec le serveur API du maître pour recevoir les instructions et envoyer des rapports d'état. Il s'assure que les conteneurs déployés dans les pods sont en cours d'exécution et en bonne santé, conformément aux spécifications définies dans les objets Pod. Si un conteneur échoue ou s'arrête, le Kubelet prend les mesures nécessaires pour redémarrer le conteneur et maintenir l'état désiré spécifié. En surveillant en permanence l'état des pods et en exécutant les actions requises, le Kubelet joue un rôle crucial dans l'auto-guérison et la gestion des tâches au niveau du nœud.

### Kube-proxy

Kube-proxy est un composant réseau clé qui s'exécute sur chaque nœud, facilitant la communication réseau à l'intérieur et à l'extérieur du cluster Kubernetes. Il gère l'accès aux services en redirigeant le trafic réseau vers les conteneurs appropriés, en utilisant des règles de routage IP et des ports. Kube-proxy permet ainsi d'équilibrer la charge entre plusieurs instances d'une application, améliorant la disponibilité et la performance des services. En manipulant les règles de réseau sur les nœuds, kube-proxy assure que la communication entre les conteneurs, ainsi que celle entre les conteneurs et l'extérieur du cluster, est fluide et sécurisée.

Ces trois composants travaillent de concert pour garantir que les applications conteneurisées sur les nœuds de travail fonctionnent comme prévu. Ils offrent une couche d'orchestration qui prend en charge la gestion du cycle de vie des conteneurs, la mise en réseau, et la communication avec le reste du cluster. Ensemble, ils permettent à Kubernetes de fournir une plateforme résiliente, performante et facile à gérer pour déployer des applications à grande échelle.

