# Au niveau du noeud 1
```
kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr 10.5.0.0/16
```
==> **Sortie** : Chaine de connexion pour joindre ce noeud maître (kubeadm join ...)!
```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
```

# Au niveau des deux noeuds crées :
```
kubeadm join 192.168.0.22:6443 --token pwu922.2lasllhcwdyli489 \
        --discovery-token-ca-cert-hash sha256:a839057de03504e5ad5045292a0f5cf051e1d284f5c8c8cd84cd899cdcf854d4
```
# Au niveau du noeud 1
```
kubectl get nodes
```
# Créer un réseau
```
kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/kubeadm-kuberouter.yaml
```
```
kubectl get nodes
```
# Créer un déploiement
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml
kubectl get pods
```

# Supprimer un déploiement
```
kubectl delete -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml
kubectl get pods
```

# Recréer le déploiement
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml
kubectl get pods
```

# Premières commandes 
```
kubectl get nodes
kubectl get nodes -o wide
kubectl get pods
kubectl get pods -o wide
```
# Explorer le déploiement et tous les objets présents
```
curl -o nginx-app.yaml https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml
vi nginx-app.yaml (** ou **  cat nginx-app.yaml)
```
```
kubectl get pods
kubectl delete (choisir un id d'1 pod de la commande précédente)
kubectl get pods
```
# Pour vérifier vos services et déploiements dans Kubernetes, vous pouvez utiliser les commandes `kubectl get` suivantes :
```
kubectl get services (** ou **  kubectl get svc)
kubectl get deployments (** ou **  kubectl get deploy)
```
```
kubectl get all
kubectl get all --all-namespaces
```

- **Pour lister tous les services dans le namespace courant :**
  ```
  kubectl get services
  ```
  Ou simplement:
  ```
  kubectl get svc
  ```

- **Pour lister tous les déploiements dans le namespace courant :**
  ```
  kubectl get deployments
  ```
  Ou simplement:
  ```
  kubectl get deploy
  ```

Si vous souhaitez voir ces ressources dans tous les namespaces, vous pouvez ajouter l'option `-A` ou `--all-namespaces` à ces commandes, par exemple :

- **Pour lister tous les services dans tous les namespaces :**
  ```
  kubectl get services --all-namespaces
  ```
  Ou simplement:
  ```
  kubectl get svc -A
  ```

- **Pour lister tous les déploiements dans tous les namespaces :**
  ```
  kubectl get deployments --all-namespaces
  ```
  Ou simplement:
  ```
  kubectl get deploy -A
  ```

Ces commandes vous fournissent une vue d'ensemble des services et des déploiements actuellement déployés dans votre cluster Kubernetes, y compris dans quel namespace ils se trouvent, leur état, et d'autres informations pertinentes.

```
kubectl run mon-pod-nginx --image=nginx --restart=Never
kubectl get pods
kubectl describe pod mon-pod-nginx
kubectl logs mon-pod-nginx
kubectl exec mon-pod-nginx -- ls
kubectl exec -it mon-pod-nginx -- sh
kubectl edit pod mon-pod-nginx
kubectl delete pod mon-pod-nginx
kubectl get pods
```
