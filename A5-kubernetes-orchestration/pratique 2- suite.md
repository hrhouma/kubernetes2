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

Kubernetes offre une plateforme puissante et flexible pour la gestion de conteneurs, rendant possible le déploiement et la gestion d'applications à grande échelle de manière plus efficace. Grâce à ses capacités d'orchestration, de mise à l'échelle automatique, de déploiement progressif, et bien plus, Kubernetes est devenu un outil incontournable pour les développeurs et les administrateurs de systèmes cherchant à optimiser leurs infrastructures.


