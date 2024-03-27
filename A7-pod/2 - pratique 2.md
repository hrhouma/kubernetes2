# Exercice : Dépanner un Pod avec une Image Incorrecte

Dans cet exercice, vous créerez un pod utilisant une image incorrecte, puis vous suivrez les étapes pour identifier et résoudre le problème.

**Objectifs**:
1. Créer un pod avec une image incorrecte.
2. Identifier le problème avec le pod.
3. Supprimer le pod défectueux.
4. Créer un nouveau pod avec la bonne configuration.

**Étape 1**: Créer un Pod avec une Image Incorrecte
- Créez un pod nommé `redis` en utilisant l'image `redis123` (qui est une image incorrecte).
```bash
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
```

**Étape 2**: Examiner le Pod
- Vérifiez l'état du pod pour identifier le problème.
```bash
kubectl get pods
```
- Détaillez le pod pour obtenir plus d'informations sur l'erreur.
```bash
kubectl describe pod redis
```

**Étape 3**: Supprimer le Pod Défectueux
- Supprimez le pod `redis` après avoir identifié le problème.
```bash
kubectl delete pod redis
```

**Étape 4**: Corriger l'Erreur et Recréer le Pod
- Modifiez le fichier `redis-definition.yaml` pour corriger le nom de l'image (remplacez `redis123` par `redis`).
- Créez le pod avec la configuration corrigée.
```bash
kubectl apply -f redis-definition.yaml
```

**Étape 5**: Vérifier le Nouveau Pod
- Vérifiez que le nouveau pod `redis` est en cours d'exécution sans erreurs.
```bash
kubectl get pods
```

**Bonus**: Utilisation des Alias et Commandes Supplémentaires
- Utilisez l'alias `k` pour `kubectl` pour simplifier les commandes.
```bash
alias k=kubectl
k get pods
```
- Utilisez des commandes pour filtrer et compter le nombre de pods.
```bash
k get po --no-headers | wc -l
```

**Fin de l'Exercice**.

Cet exercice vous guide à travers le processus de dépannage d'un pod créé avec une image incorrecte, une compétence essentielle pour gérer les applications Kubernetes au quotidien.
