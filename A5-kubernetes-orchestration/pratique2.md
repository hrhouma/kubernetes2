# Kubernetes pour les Débutants

**Date** : 7 décembre 2017  
**Par** : @jpetazzo
# Référence : 
- https://training.play-with-kubernetes.com/kubernetes-workshop/

**Durée estimée** : Environ 27 minutes

Basé sur un atelier écrit à l'origine par Jérôme Petazzoni avec des contributions de nombreuses autres personnes.

## Introduction

Dans cet atelier pratique, vous apprendrez les concepts de base de Kubernetes. Vous le ferez en interagissant avec Kubernetes via les terminaux de ligne de commande sur la droite. En fin de compte, vous déploierez les applications exemples Dockercoins sur les deux nœuds de travail.

## Pour Commencer

### Quels sont ces terminaux dans les navigateurs ?

Sur la droite, vous devriez voir deux fenêtres de terminal. Il se peut qu'on vous demande de vous connecter d'abord, ce que vous pouvez faire soit avec un identifiant Docker soit avec un compte GitHub. Ces terminaux sont des terminaux entièrement fonctionnels exécutant Centos. En réalité, ils sont la ligne de commande pour des conteneurs Centos exécutés sur notre infrastructure Play-with-Kubernetes. Lorsque vous voyez des blocs de code qui ressemblent à ceci, vous pouvez soit cliquer dessus pour qu'ils se remplissent automatiquement, soit les copier et les coller.

```bash
ls
```

### Démarrer le cluster

La première étape consiste à initialiser le cluster dans le premier terminal :

```bash
kubeadm init --apiserver-advertise-address $(hostname -i)
```

Cela prendra quelques minutes. À la fin, vous verrez quelque chose comme cela :

```bash
Votre master Kubernetes a été initialisé avec succès !

Pour commencer à utiliser votre cluster, vous devez exécuter (en tant qu'utilisateur régulier) :

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Vous devriez maintenant déployer un réseau de pods au cluster.
Exécutez "kubectl apply -f [podnetwork].yaml" avec l'une des options listées à :
  http://kubernetes.io/docs/admin/addons/
```

Copiez la ligne entière qui commence par kubeadm join du premier terminal et collez-la dans le second terminal. Vous devriez voir quelque chose comme cela :

```bash
kubeadm join --token a146c9.9421a0d62a0611f4 172.26.0.2:6443 --discovery-token-ca-cert-hash sha256:9a4dc07bd8ac596336ecee6ce0928b3500174037c07a38a03bebef25e97c4db5
```

Initialisation de l'ID de la machine à partir du générateur aléatoire.

Cela signifie que vous êtes presque prêt à partir. En dernier, vous devez juste initialiser votre réseau de cluster dans le premier terminal :

```bash
kubectl apply -n kube-system -f \
  "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 |tr -d '\n')"
```

Vous verrez une sortie comme cela :

```bash
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

Non, vous ne pouvez pas acheter de café avec les DockerCoins.

Comment fonctionnent les DockerCoins :

- worker demande à rng de générer quelques octets aléatoires
- worker nourrit ces octets dans hasher
- et répète à l'infini !
- chaque seconde, worker met à jour redis pour indiquer combien de boucles ont été faites
- webui interroge redis, calcule et expose la "vitesse de hachage" dans votre navigateur

## Obtenir le code source de l'application

Nous avons créé une application exemple à exécuter pour certaines parties de l'atelier. L'application se trouve dans le dépôt dockercoins.

Voyons la disposition générale du code source :

- il y a un fichier Compose `docker-compose.yml`...
- ...et 4 autres services, chacun dans son propre répertoire :
  - `rng` = service web générant des octets aléatoires
  - `hasher` = service web calculant le hash des données POSTées
  - `worker` = processus en arrière-plan utilisant rng et hasher
  - `webui` = interface web pour observer la progression

Nous allons cloner le dépôt GitHub :

```bash
git clone https://github.com/dockersamples/dockercoins
```

(Vous pouvez également forker le dépôt sur GitHub et cloner votre fork si vous préférez.)

## Exécuter l'application

Allez dans le répertoire `dockercoins`, dans le dépôt cloné :

```bash
cd ~/dockercoins
```

Utilisez Compose pour construire et exécuter tous les conteneurs :

```bash
docker-compose up
```

Compose indique à Docker de construire toutes les images de conteneurs (en téléchargeant les images de base correspondantes), puis démarre tous les conteneurs et affiche les journaux agrégés.

### Beaucoup de journaux

L'application génère continuellement des journaux. Nous pouvons voir le service worker effectuer des requêtes vers rng et hasher. Mettons cela en arrière-plan.

## Connexion à l'interface web

Le conteneur `webui` expose un tableau de bord web ; voyons-le. Avec un navigateur web, connectez-vous à `node1` ici sur le port 8000 (créé lorsque vous avez exécuté l'application).

## Nettoyage

Avant de passer à autre chose, arrêtons tout en tapant Ctrl-C.

## Concepts Kubernetes

- Kubernetes est un système de gestion de conteneurs.
- Il exécute et gère des applications conteneurisées sur un cluster.
- Que signifie cela exactement ?

### Choses basiques que nous pouvons demander à Kubernetes de faire

- Démarrer 5 conteneurs en utilisant l'image `atseashop/api:v1.3`.
- Placer un équilibreur de charge interne devant ces conteneurs.
- Démarrer 10 conteneurs en utilisant l'image `atseashop/webfront:v1.3`.
- Placer un équilibreur de charge public devant ces conteneurs.
- C'est le Black Friday (ou Noël), le trafic augmente, agrandissons notre cluster et ajoutons des conteneurs.
- Nouvelle version ! Remplacer mes conteneurs par la nouvelle image `atseashop/webfront:v1.4`.
- Continuer à traiter les demandes pendant la mise à jour ; mettre à jour mes conteneurs un par un.

### Autres choses que Kubernetes peut faire pour nous

- Autoscaling de base.
- Déploiement blue/green, déploiement canari.
- Services de longue durée, mais aussi travaux par lots (tâches ponctuelles).
- Surdimensionner notre cluster et expulser les travaux de faible priorité.
- Exécuter des services avec des données à état (bases de données, etc.).
- Contrôle d'accès détaillé définissant ce qui peut être fait par qui sur quelles ressources.
- Intégration de services tiers (catalogue de services).
- Automatisation de tâches complexes (opérateurs).

Kubernetes vous offre une plateforme robuste pour orchestrer vos conteneurs, gérer les déploiements et scaler vos applications. Avec ces concepts de base, vous êtes prêt à explorer plus en profondeur et à tirer parti de la puissance de Kubernetes pour vos projets de développement. 


# Résumé des commandes 

Voici toutes les commandes extraites du tutoriel Kubernetes pour débutants :

```bash
ls

kubeadm init --apiserver-advertise-address $(hostname -i)

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f [podnetwork].yaml

kubeadm join --token SOMETOKEN SOMEIPADDRESS --discovery-token-ca-cert-hash SOMESHAHASH

kubectl apply -n kube-system -f \
  "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 |tr -d '\n')"

git clone https://github.com/dockersamples/dockercoins

cd ~/dockercoins

docker-compose up

kubectl get node

kubectl get no
kubectl get node
kubectl get nodes

kubectl get nodes -o wide

kubectl get no -o yaml

kubectl get nodes -o json | jq ".items[] | {name:.metadata.name} + .status.capacity"

kubectl describe type/name
kubectl describe type name

kubectl explain type

kubectl get services
kubectl get svc

curl -k https://10.96.0.1

kubectl get pods

kubectl get namespaces
kubectl get namespace
kubectl get ns

kubectl -n kube-system get pods

kubectl run pingpong --image alpine ping 8.8.8.8

kubectl get all

kubectl logs deploy/pingpong

kubectl logs deploy/pingpong --tail 1 --follow

kubectl scale deploy/pingpong --replicas 8

kubectl get pods -w

kubectl delete pod pingpong-yyyy

kubectl run --restart=OnFailure
kubectl run --restart=Never

kubectl logs -l run=pingpong --tail 1

kubectl delete deploy/pingpong

kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80

kubectl logs deploy/worker --follow

kubectl create service nodeport webui --tcp=80 --node-port=30001

kubectl get svc

kubectl run elastic --image=elasticsearch:2 --replicas=4

kubectl expose deploy/elastic --port 9200

IP=$(kubectl get svc elastic -o go-template --template '{{ .spec.clusterIP }}')
curl http://$IP:9200/

kubectl delete deploy/elastic

export USERNAME=YourUserName

cd ~/dockercoins/stacks

docker-compose -f dockercoins.yml build
docker-compose -f dockercoins.yml push

kubectl run redis --image=redis

for SERVICE in hasher rng webui worker; do
  kubectl run $SERVICE --image=$USERNAME/$SERVICE -l app=$SERVICE
done

kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80

kubectl rollout status deploy worker

kubectl rollout undo deploy worker
kubectl rollout status deploy worker

kubectl patch deployment worker -p "
spec:
  template:
    spec:
      containers:
      - name: worker
        image: $USERNAME/worker:latest
  strategy:
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 3
  minReadySeconds: 10
"
kubectl rollout status deployment worker
```
