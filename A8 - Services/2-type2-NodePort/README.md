# Tutoriel de Configuration d'un Cluster Kubernetes et Déploiement d'une Application

Ce tutoriel vous guide à travers la création d'un cluster Kubernetes en utilisant Kind, le déploiement d'une application via des conteneurs Docker, et sa gestion à l'aide de `kubectl`.


## Prérequis

- Docker installé et en cours d'exécution
- `kubectl` installé
- Kind installé
- Git installé

## Étape 1: Configuration du Cluster Kubernetes avec Kind

1. **Créez un nouveau fichier de configuration pour Kind**:
    ```sh
    cat <<EOT > kind-config.yaml
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    nodes:
    - role: control-plane
    - role: worker
    - role: worker
    EOT
    ```
2. **Créez un cluster Kind avec le fichier de configuration**:
    ```sh
    kind create cluster --config kind-config.yaml
    ```
3. **Vérifiez les informations du cluster avec `kubectl`**:
    ```sh
    kubectl cluster-info --context kind-kind
    ```

## Étape 2: Installation et Configuration de kubectl

1. **Téléchargez kubectl**:
    ```sh
    curl -LO "https://dl.k8s.io/release/v1.24.7/bin/linux/amd64/kubectl"
    ```
2. **Vérifiez le téléchargement**:
    ```sh
    curl -LO "https://dl.k8s.io/v1.24.7/bin/linux/amd64/kubectl.sha256"
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    ```
3. **Installez kubectl**:
    ```sh
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    sudo install -o root -g root -m 0755 kubectl /home/$USER/.local/bin/kubectl
    ```

## Étape 3: Préparation de l'Application

1. **Clonez le dépôt de l'application**:
    ```sh
    git clone https://github.com/gbhure/docker-demo.git
    cd docker-demo/service-demo
    ```

## Étape 4: Construction et Publication des Images Docker

1. **Connectez-vous à Docker Hub**:
    ```sh
    docker login
    ```
2. **Construisez et publiez les images Docker**:
    - **Pour MySQL et Redis (Si elles ne sont pas déjà présentes localement)**:
        ```sh
        docker pull mysql:latest
        docker pull redis:latest
        docker tag mysql:latest hrehouma1/mysql:1.2
        docker tag redis:latest hrehouma1/redis:1.2
        docker push hrehouma1/mysql:1.2
        docker push hrehouma1/redis:1.2
        ```
    - **Pour l'application Flask**:
        ```sh
        docker build . -t hrehouma1/flaskapp:1.2
        docker push hrehouma1/flaskapp:1.2
        ```

## Étape 5: Déploiement sur Kubernetes

1. **Naviguez vers le dossier de déploiement et appliquez les configurations**:
    ```sh
    cd ../deploy
    kubectl apply -f db-deployment.yaml
    kubectl apply -f db-svc.yml
    kubectl apply -f web-deployment.yaml
    kubectl apply -f web-svc.yml
    ```

## Étape 6: Interaction avec l'Application

1. **Récupérez l'URL de l'application**:
    ```sh
    export NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(.type=="InternalIP")].address}')
    export NODE_PORT=$(kubectl get svc web -o jsonpath='{.spec.ports[0].nodePort}')
    echo "URL de l'application: http://$NODE_IP:$NODE_PORT"
    ```
2. **Initialisez la base de données et ajoutez des utilisateurs**:
    - Pour initialiser la base de données :
        ```sh
        curl http://$NODE_IP:$NODE_PORT/init
        ```
    - Pour ajouter des utilisateurs (répétez pour les 20 utilisateurs) :
        ```sh
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "1", "user":"Nom Utilisateur"}' http://$NODE_IP:$NODE_PORT/users/add
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "1", "user":"Alice Wonderland"}' http://$NODE_IP:$NODE_PORT/users/add
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "2", "user":"Bob Builder"}' http://$NODE_IP:$NODE_PORT/users/add
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "3", "user":"Charlie Chocolate"}' http://$NODE_IP:$NODE_PORT/users/add
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "4", "user":"Daisy Duck"}' http://$NODE_IP:$NODE_PORT/users/add
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "5", "user":"Evan Almighty"}' http://$NODE_IP:$NODE_PORT/users/add
        curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "6", "user":"Fiona Fair"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "7", "user":"George Galaxy"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "8", "user":"Hannah Horizon"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "9", "user":"Ian Ice"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "10", "user":"Jenny Jigsaw"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "11", "user":"Kevin Key"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "12", "user":"Lily Light"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "13", "user":"Mike Mountain"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "14", "user":"Nina Night"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "15", "user":"Oscar Ocean"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "16", "user":"Patty Prism"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "17", "user":"Quincy Quest"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "18", "user":"Randy Rainbow"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "19", "user":"Sandy Storm"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "20", "user":"Tommy Thunder"}' http://$NODE_IP:$NODE_PORT/users/add
        ```
3. **Récupérez un utilisateur spécifique**:
    ```sh
    curl http://$NODE_IP:$NODE_PORT/users/1
    ```

## Étape 7: Nettoyage

1. **Supprimez les ressources Kubernetes**:
    ```sh
    kubectl delete -f db-deployment.yaml
    kubectl delete -f db-svc.yml
    kubectl delete -f web-deployment.yaml
    kubectl delete -f web-svc.yml
    ```
2. **Supprimez le cluster Kind**:
    ```sh
    kind delete cluster
    ```

Ce tutoriel complet vous a guidé à travers la mise en place d'un environnement de développement Kubernetes avec Kind, l'installation et la configuration de kubectl, la construction et la publication d'images Docker, le déploiement et la gestion d'une application simple avec Kubernetes, et enfin l'interaction avec cette application déployée.
