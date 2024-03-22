# Mon site : 
https://devopelastichayway.com/upgrading-kubernetes/
https://engr-syedusmanahmad.medium.com/wordpress-on-kubernetes-cluster-step-by-step-guide-749cb53e27c7

# SUJET1 - Statefulset 
https://github.com/gbhure/docker-demo/tree/master/statefulset
---

# SUJET2 - Ingress
https://github.com/gbhure/docker-demo/tree/master/ingress 
---

# SUJET3 - Ingress minikube
https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/
---

# SUJET4 - Helm
https://helm.sh/docs/intro/install/
---

# SUJET5 - Application de vote Local + AKS
https://www.coursera.org/projects/rudi-hinds-containerised-application-development-aks-azure-kubernetes-service

# SUJET6 - Divers

root@k8s-demo:/home/ubuntu# tree project1/
project1/
├── Chart.yaml
└── templates
    └── deploy.yaml

1 directory, 2 files
root@k8s-demo:/home/ubuntu# cat project1/Chart.yaml
apiVersion: v1
name: my-nginx
version: 1.0.0
appVersion: 2.1.0
description: This is my first helm chart
root@k8s-demo:/home/ubuntu# cat project1/templates/deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: my-nginx
  name: my-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: my-nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
root@k8s-demo:/home/ubuntu#




  790  k expose deploy my-nginx --port=80 --dry-run=client -o yaml > project1/templates/svc.yaml
  791  tree project1/
  792  k get all
  793  vi project1/Chart.yaml
  794  helm upgrade my-nginx project1
  795  helm list
  796  helm history my-nginx
  797  cat project1/Chart.yaml
  798  k get all
  799  history | tail -20
  800  exit
  801  sudo -s
  802  exit
  803  cd project1/
  804  tree .
  805  vi values.yaml
  806  tree .
  807  vi templates/deploy.yaml
  808  k get all
  809  k describe po my-nginx-7fbf685c4d-8f84q
  810  vi CH
  811  vi Chart.yaml
  812  helm upgrade my-nginx project1
  813  helm upgrade my-nginx .
  814  helm history my-nginx
  815  k get all
  816  k describe po my-nginx-94f845bbf-lgpxv
  817  tree .
  818  cat values.yaml
  819  history | tail -30
root@k8s-demo:/home/ubuntu/project1# tree .
.
├── Chart.yaml
├── templates
│   ├── deploy.yaml
│   └── svc.yaml
└── values.yaml

1 directory, 4 files
root@k8s-demo:/home/ubuntu/project1# cat values.yaml
replicaCount: 3
image:
  name: nginx
  tag: 1.7.9
service:
  type: NodePort
  port: 8080
root@k8s-demo:/home/ubuntu/project1# cat templates/deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: my-nginx
  name: my-nginx
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: my-nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: my-nginx
    spec:
      containers:
      - image: {{ .Values.image.name }}:{{ .Values.image.tag }}
        name: nginx
        resources: {}
status: {}
root@k8s-demo:/home/ubuntu/project1#



  804  helm history my-nginx
  805  k get all
  806  helm rollback my-nginx 1
  807  k get all
  808  helm history my-nginx
  809  helm rollback my-nginx 3
  
 

# Introduction à StatefulSet - Ingress - Ingress Minikube - Helm

### SUJET 1 - StatefulSet

Dans le contexte de Kubernetes, un StatefulSet est un objet de l'API qui gère le déploiement et la mise à l'échelle d'un ensemble de Pods, tout en maintenant l'identité et le stockage persistants de chaque Pod. Contrairement aux Deployments, qui sont principalement utilisés pour des applications sans état, les StatefulSets sont essentiels pour des cas d'utilisation où vous avez besoin de garanties sur l'ordre de déploiement, la mise à l'échelle et la suppression, ainsi que sur l'unicité des Pods. Cela est particulièrement utile pour des applications comme des bases de données, des systèmes de files d'attente, et tout service nécessitant une persistance ou une identité réseau stable.

### SUJET 2 - Ingress

Ingress dans Kubernetes est un objet API qui fournit un routage HTTP et HTTPS avancé vers des services au sein d'un cluster. Il permet aux utilisateurs de définir des règles d'accès externes pour atteindre les services du cluster, ce qui facilite l'exposition de plusieurs services sous une seule adresse IP externe. Ingress peut gérer l'équilibrage de charge, la terminaison SSL, et le routage basé sur le nom de l'hôte ou le chemin d'accès, rendant la gestion de l'accès aux applications dans un cluster Kubernetes beaucoup plus souple et centralisée.

### SUJET 3 - Ingress Minikube

Minikube est un outil qui permet de créer localement un cluster Kubernetes, simplifiant ainsi le développement et le test d'applications Kubernetes. L'utilisation d'Ingress avec Minikube implique la mise en place d'un Ingress Controller et la configuration d'objets Ingress pour exposer des services au-delà du cluster. Cela permet aux développeurs de tester le comportement et la configuration d'Ingress dans un environnement local avant de les déployer dans un environnement de production. La documentation sur l'Ingress Minikube fournit des instructions détaillées sur la manière de configurer et d'utiliser Ingress pour accéder aux applications dans un cluster Minikube.

### SUJET 4 - Helm

Helm est un gestionnaire de paquets pour Kubernetes, qui simplifie le déploiement et la gestion d'applications dans des clusters Kubernetes. Les applications et leurs dépendances sont empaquetées dans des "charts" Helm, qui peuvent être facilement déployées, modifiées, et versionnées. Helm aide à gérer la complexité des déploiements Kubernetes, en permettant aux utilisateurs de définir, d'installer, et de mettre à jour même les applications Kubernetes les plus complexes. Helm supporte la personnalisation des déploiements via des fichiers de configuration, rendant la réutilisation et la distribution des applications plus efficaces et standardisées.
  

# Vulgarisation de StatefulSet - Ingress - Ingress Minikube - Helm

Bien sûr, rendons ces concepts un peu plus tangibles avec des analogies du quotidien :

### SUJET 1 - StatefulSet

Imaginez que vous ouvrez une chaîne de restaurants. Chaque restaurant (Pod) a besoin de maintenir son propre livre de recettes (état ou données persistantes) et son numéro de téléphone (identité stable). Même si vous décidez de rénover (mettre à jour ou redémarrer) un restaurant, le livre de recettes et le numéro de téléphone doivent rester les mêmes pour assurer la continuité du service. Un StatefulSet dans Kubernetes fonctionne de manière similaire pour gérer les applications qui nécessitent une telle persistance et identité stable.

### SUJET 2 - Ingress

Imaginez que vous organisez une grande fête dans une salle de réception avec plusieurs portes (services). Vous engagez un concierge (Ingress) qui connaît la liste des invités et leurs tables assignées (règles d'acheminement). Lorsqu'un invité arrive, le concierge vérifie son nom et le guide vers la bonne porte pour atteindre sa table. De même, Ingress dans Kubernetes dirige le trafic externe vers le bon service dans le cluster, basé sur l'URL ou d'autres règles.

### SUJET 3 - Ingress Minikube

Reprenons l'exemple de la fête, mais cette fois, vous faites un essai dans votre jardin (Minikube) avant le grand jour. Vous décidez de mettre en pratique le plan d'accueil des invités avec un ami jouant le rôle du concierge (Ingress Controller) pour vous assurer que tout fonctionnera comme prévu. Cela vous permet de tester et d'ajuster la configuration sans perturber la fête réelle. Utiliser Ingress avec Minikube vous donne une chance de pratiquer la gestion du trafic externe dans un environnement de développement local avant de le déployer dans un environnement plus grand et plus complexe.

### SUJET 4 - Helm

Pensez à Helm comme à un service de livraison de kit de repas pour votre chaîne de restaurants. Chaque kit (chart Helm) contient des instructions précises (configuration) et tous les ingrédients pré-mesurés (dépendances) nécessaires pour préparer un plat (application). Cela simplifie grandement le processus de préparation, permettant à chaque restaurant de servir des plats complexes avec une grande cohérence et qualité. De la même manière, Helm permet de déployer et de gérer des applications Kubernetes en regroupant toutes les ressources nécessaires et les instructions de configuration dans un package facile à utiliser.

En espérant que ces analogies vous aident à mieux saisir ces concepts !
