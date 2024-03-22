Configuration et Utilisation de plusieurs planificateurs (ou schedulers) dans un cluster Kubernetes. Ce travail couvre la création d'un planificateur personnalisé et son utilisation pour planifier des pods.

### Prérequis
- Un cluster Kubernetes opérationnel
- `kubectl` configuré pour communiquer avec votre cluster

### Étape 1 : Créer un Planificateur Personnalisé
Pour commencer, vous devez écrire un fichier de configuration pour votre planificateur personnalisé. Voici un exemple de fichier de configuration que vous pourriez utiliser, nommé `my-scheduler.yaml`:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-custom-scheduler
  namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: my-custom-scheduler
subjects:
- kind: ServiceAccount
  name: my-custom-scheduler
  namespace: kube-system
roleRef:
  kind: ClusterRole
  name: system:kube-scheduler
  apiGroup: rbac.authorization.k8s.io

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-scheduler-config
  namespace: kube-system
data:
  policy.cfg: |+
    {
      "kind" : "Policy",
      "apiVersion" : "v1",
      "predicates" : [
        {"name" : "PodFitsHostPorts"},
        {"name" : "PodFitsResources"},
        {"name" : "NoDiskConflict"},
        {"name" : "NoVolumeZoneConflict"},
        {"name" : "MatchNodeSelector"},
        {"name" : "HostName"}
      ],
      "priorities" : [
        {"name" : "LeastRequestedPriority", "weight" : 1},
        {"name" : "BalancedResourceAllocation", "weight" : 1},
        {"name" : "ServiceSpreadingPriority", "weight" : 1},
        {"name" : "EqualPriority", "weight" : 1}
      ]
    }

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-custom-scheduler
  namespace: kube-system
  labels:
    component: scheduler
    tier: control-plane
spec:
  replicas: 1
  selector:
    matchLabels:
      component: scheduler
      tier: control-plane
      version: my-custom
  template:
    metadata:
      labels:
        component: scheduler
        tier: control-plane
        version: my-custom
    spec:
      serviceAccountName: my-custom-scheduler
      containers:
      - name: my-custom-scheduler
        image: gcr.io/google_containers/kube-scheduler-amd64:v1.22.0
        imagePullPolicy: IfNotPresent
        args:
        - --scheduler-name=my-custom-scheduler
        - --policy-configmap=my-scheduler-config
        - --leader-elect=false
        - --lock-object-name=my-custom-scheduler
```

Appliquez cette configuration avec la commande `kubectl`:

```sh
kubectl apply -f my-scheduler.yaml
```

### Étape 2 : Vérifier le Planificateur Personnalisé
Vérifiez que votre planificateur personnalisé est en cours d'exécution avec la commande suivante :

```sh
kubectl get pods --namespace=kube-system
```

Vous devriez voir le pod de votre planificateur personnalisé avec un statut `Running`.

### Étape 3 : Planifier un Pod avec le Planificateur Personnalisé
Créez un fichier `nginx-pod.yaml` avec le contenu suivant pour spécifier l'utilisation de votre planificateur personnalisé :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-custom-scheduled
spec:
  containers:
  - name: nginx
    image: nginx:latest
  schedulerName: my-custom-scheduler
```

Déployez le pod avec `kubectl`:

```sh
kubectl apply -f nginx-pod.yaml
```

### Étape 4 : Suivre les Événements et les Logs
Suivez les événements de Kubernetes pour voir le processus de planification :

```sh
kubectl get events --watch
```

Pour voir les logs de votre plan

ificateur personnalisé et comprendre comment il prend ses décisions, utilisez la commande suivante :

```sh
kubectl logs -f $(kubectl get pods --namespace=kube-system -l component=scheduler,version=my-custom -o jsonpath='{.items[0].metadata.name}') --namespace=kube-system
```

### Étape 5 : Vérifier la Planification du Pod
Une fois que vous avez déployé votre pod, vérifiez que votre planificateur personnalisé l'a pris en charge :

```sh
kubectl get pod nginx-custom-scheduled
```

Vous devriez voir le statut `Running` si le pod a été correctement planifié et démarré.

### Conseils Supplémentaires
- Pour dépanner ou obtenir plus d'informations, utilisez `kubectl describe pod <nom-du-pod>` pour obtenir des informations détaillées sur le processus de planification d'un pod.
- Si vous souhaitez que votre planificateur personnalisé coexiste avec le planificateur par défaut de Kubernetes, assurez-vous que les pods que vous ne voulez pas qu'il planifie n'aient pas `schedulerName: my-custom-scheduler` dans leur spécification.
- Gardez à l'esprit que vous devrez peut-être ajuster les rôles RBAC et les permissions pour votre planificateur personnalisé en fonction de vos besoins spécifiques de sécurité et d'accès.

### Nettoyage
Lorsque vous avez terminé ou si vous souhaitez supprimer votre planificateur personnalisé, utilisez `kubectl delete` pour supprimer les ressources que vous avez créées :

```sh
kubectl delete -f my-scheduler.yaml
kubectl delete -f nginx-pod.yaml
```

Ceci supprimera le planificateur personnalisé et le pod que vous avez planifié avec.

En suivant ce tutoriel, vous avez appris à configurer et à utiliser un planificateur personnalisé dans Kubernetes, vous permettant de personnaliser la manière dont les pods sont répartis à travers votre cluster.