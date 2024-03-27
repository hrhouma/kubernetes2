# Kubernetes pour les Débutants

7 décembre 2017 • @jpetazzo

    Durée : Environ 27 minutes

Basé sur un atelier initialement écrit par Jérôme Petazzoni avec des contributions de nombreux autres.
Introduction

Dans cet atelier pratique, vous apprendrez les concepts de base de Kubernetes. Vous le ferez en interagissant avec Kubernetes via les terminaux de ligne de commande sur la droite. En fin de compte, vous déploierez les applications exemples Dockercoins sur les deux nœuds workers.
Pour Commencer
Quels sont ces terminaux dans les navigateurs ?

À droite, vous devriez voir deux fenêtres de terminal. Il se peut qu'on vous demande de vous connecter d'abord, ce que vous pouvez faire soit avec un identifiant Docker soit avec un compte GitHub. Ces terminaux sont des terminaux entièrement fonctionnels exécutant Centos. En réalité, ils sont la ligne de commande pour des conteneurs Centos exécutés sur notre infrastructure Play-with-Kubernetes. Lorsque vous voyez des blocs de code qui ressemblent à ceci, vous pouvez soit cliquer dessus pour qu'ils se remplissent automatiquement, soit les copier et les coller.

  ls

Démarrer le cluster

La première étape consiste à initialiser le cluster dans le premier terminal :

  kubeadm init --apiserver-advertise-address $(hostname -i)

Cela prendra quelques minutes, pendant lesquelles vous verrez beaucoup d'activité dans le terminal.

Vous verrez quelque chose comme cela à la fin :

  Votre master Kubernetes a été initialisé avec succès !

  Pour commencer à utiliser votre cluster, vous devez exécuter (en tant qu'utilisateur régulier) :

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

  Vous devriez maintenant déployer un réseau de pods au cluster.
  Exécutez "kubectl apply -f [podnetwork].yaml" avec l'une des options listées à :
    http://kubernetes.io/docs/admin/addons/

  Vous pouvez maintenant rejoindre n'importe quel nombre de machines en exécutant la commande suivante sur chaque nœud
  en tant que root :

  kubeadm join --token SOMETOKEN SOMEIPADDRESS --discovery-token-ca-cert-hash SOMESHAHASH

Copiez toute la ligne qui commence par kubeadm join du premier terminal et collez-la dans le second terminal. Vous devriez voir quelque chose comme ceci :

kubeadm join --token a146c9.9421a0d62a0611f4 172.26.0.2:6443 --discovery-token-ca-cert-hash sha256:9a4dc07bd8ac596336ecee6ce0928b3500174037c07a38a03bebef25e97c4db5
Initialisation de l'ID de la machine à partir du générateur aléatoire.
[kubeadm] AVERTISSEMENT : kubeadm est en bêta, veuillez ne pas l'utiliser pour des clusters de production.
[preflight] Vérifications préliminaires ignorées
[discovery] Tentative de connexion au serveur API "172.26.0.2:6443"
[discovery] Client de découverte cluster-info créé, demande d'info à "https://172.26.0.2:6443"
[discovery] Demande d'info à "https://172.26.0.2:6443" à nouveau pour valider TLS contre la clé publique épinglée
[discovery] Signature et contenu des infos du cluster sont valides et le certificat TLS valide contre les racines épinglées, utilisera le serveur API "172.26.0.2:6443"
[discovery] Connexion établie avec succès avec le serveur API "172.26.0.2:6443"
[bootstrap] Version du serveur détectée : v1.8.11
[bootstrap] Le serveur prend en charge l'API des Certificats (certificates.k8s.io/v1beta1)

Jointure du nœud complète :
* Demande de signature de certificat envoyée au master et réponse reçue.
* Kubelet informé des nouveaux détails de connexion sécurisée.

Exécutez 'kubectl get nodes' sur le master pour voir cette machine rejoindre.

Cela signifie que vous êtes presque
