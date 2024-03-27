# Kubernetes pour les Débutants

**Date**: 7 décembre 2017  
**Auteur**: @jpetazzo

**Durée estimée**: Environ 27 minutes

## Introduction

Ce workshop pratique est basé sur les travaux initiaux de Jérôme Petazzoni, avec des contributions de nombreux autres. Il vise à vous initier aux concepts fondamentaux de Kubernetes. Vous apprendrez par la pratique, en utilisant les terminaux de ligne de commande mis à votre disposition. Le point d'orgue sera le déploiement de l'application exemple Dockercoins sur plusieurs nœuds workers.

## Pour Commencer

### Quels sont ces terminaux dans les navigateurs ?

Sur le côté droit de votre écran, vous devriez voir deux fenêtres de terminal. Il est possible qu'une authentification vous soit demandée ; vous pouvez utiliser pour cela un identifiant Docker ou un compte GitHub. Ces terminaux sont des interfaces entièrement fonctionnelles sous Centos, en fait, ils représentent la ligne de commande de conteneurs Centos exécutés sur notre infrastructure Play-with-Kubernetes. Lorsque vous rencontrerez des blocs de code, vous pourrez soit cliquer dessus pour qu'ils se remplissent automatiquement, soit les copier et les coller manuellement.

Exemple de commande :

```
ls
```

### Démarrer le cluster

Pour initialiser le cluster, commencez par exécuter la commande suivante dans le premier terminal :

```
kubeadm init --apiserver-advertise-address $(hostname -i)
```

Cette opération peut prendre quelques minutes. Une fois terminée, vous recevrez un message indiquant que votre master Kubernetes a été initialisé avec succès.

Vous devrez ensuite configurer votre environnement utilisateur pour utiliser votre cluster :

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Il est maintenant temps de déployer un réseau de pods au sein du cluster. Suivez les instructions disponibles à l'adresse suivante : [http://kubernetes.io/docs/admin/addons/](http://kubernetes.io/docs/admin/addons/).

Pour ajouter des machines au cluster, utilisez la commande `kubeadm join` avec le token et les informations spécifiques fournies lors de l'initialisation du master.

### Application DockerCoins

DockerCoins est une application de minage fictive. Elle démontre l'interaction entre différents services pour accomplir une tâche, ici "miner" des DockerCoins, qui ne vous permettront pas d'acheter de café malheureusement. L'application illustre le fonctionnement d'une architecture microservices avec Kubernetes.

## Conclusion

En suivant ce workshop, vous avez pris en main les fondamentaux de Kubernetes, en mettant en œuvre un cluster et en déployant une application réelle. Ce premier pas vous ouvre la porte vers des utilisations plus avancées de Kubernetes dans vos projets futurs.
```

Cela vous donne une base pour créer un fichier README.md pour votre workshop Kubernetes, avec les sections et décorations Markdown habituelles pour une meilleure navigation et compréhension.
