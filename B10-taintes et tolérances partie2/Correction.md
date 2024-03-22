# Exercice 1 : Compréhension des Concepts

## Qu'est-ce qu'un Taint ?

**Tainter** un nœud dans Kubernetes signifie appliquer une marque qui repousse certains pods. Les taints permettent de contrôler quelle charge de travail peut s'exécuter sur ce nœud. Les taints ont trois effets possibles sur les pods :
- `NoSchedule` : Les pods qui ne tolèrent pas ce taint ne seront pas planifiés sur le nœud.
- `PreferNoSchedule` : Kubernetes essaiera d'éviter de planifier des pods qui ne tolèrent pas ce taint sur le nœud, mais ce n'est pas garanti.
- `NoExecute` : Les pods qui sont déjà en cours d'exécution sur le nœud et qui ne tolèrent pas ce taint seront expulsés si le taint est appliqué, et les nouveaux pods qui ne tolèrent pas le taint ne seront pas planifiés sur le nœud.

## Qu'est-ce qu'une Tolérance ?

Une **tolérance** est une propriété que vous pouvez configurer sur un pod pour lui permettre de s'exécuter sur un nœud qui a un taint spécifique. Cela signifie que le pod peut "tolérer" le taint appliqué au nœud. Les tolérances sont spécifiées dans le fichier de configuration du pod, sous la section `tolerations`. Elles doivent correspondre au taint du nœud pour que le pod puisse être planifié sur celui-ci.

# Exercice 2 : Application Pratique

## Identification des Taints sur un Nœud

La commande `k describe no demo-worker | grep Taint` permettrait de révéler les taints appliqués sur le nœud `demo-worker`. Imaginons que la sortie de cette commande indique un taint `lock=yellow:NoSchedule`. Cela signifie que seul les pods ayant une tolérance correspondant à ce taint (`key=lock`, `value=yellow`, `effect=NoSchedule`) peuvent être planifiés sur ce nœud.

## Création d'un Pod avec Tolérance

Basé sur le fichier `pod-tolerations.yaml`, le pod `nginx` est configuré pour tolérer le taint appliqué au nœud `demo-worker` grâce à la section suivante :

```yaml
tolerations:
  - key: "lock"
    operator: "Equal"
    value: "yellow"
    effect: "NoSchedule"
```

- `key` : Identifie le taint que le pod peut tolérer.
- `operator` : Spécifie l'opération à effectuer pour la clé/valeur. `Equal` signifie que la clé doit correspondre exactement.
- `value` : La valeur associée à la clé pour laquelle la tolérance est définie.
- `effect` : L'effet du taint que le pod peut tolérer, ici `NoSchedule`.

# Exercice 3 : Analyse et Correction

## Analyse de Configuration

Si un pod ne se lance pas comme prévu sur un nœud avec un taint, vous devriez :
1. Utiliser `kubectl describe pod <pod-name>` pour vérifier s'il y a des messages d'erreur indiquant un problème de taint et tolérance.
2. Vérifier le taint sur le nœud avec `kubectl describe node <node-name> | grep Taint` pour s'assurer que le pod a les tolérances nécessaires pour être planifié sur ce nœud.

## Correction d'une Tolérance

La correction de la tolérance pour qu'elle corresponde au taint `lock=blue:NoSchedule` est la suivante :

```yaml
tolerations:
  - key: "lock"
    operator: "Equal"
    value: "blue"  # Corrected value to match the node's taint
    effect: "NoSchedule"
```

Pour résumer, la valeur correcte sous `value` devrait être `blue` pour correspondre au taint du nœud. Cette correction permet au pod d'être planifié sur le nœud ayant le taint `lock=blue:NoSchedule`.

#### Créer un fichier de configuration de pod complet qui inclut une tolérance correspondant à un taint spécifique sur un nœud

Ce fichier de configuration définit un pod Kubernetes qui peut être planifié sur un nœud avec ce taint grâce à la tolérance appropriée. Utilisons l'exemple du taint `lock=yellow:NoSchedule`.

Voici un exemple de fichier YAML pour un pod Kubernetes incluant une tolérance :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-tolerant
  labels:
    app: nginx
spec:
  tolerations:
    - key: "lock"
      operator: "Equal"
      value: "yellow"
      effect: "NoSchedule"
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

Explications des champs importants dans ce fichier :

- `apiVersion`, `kind`, `metadata`: Ces champs définissent les bases du fichier YAML, y compris la version de l'API Kubernetes utilisée, le type d'objet (dans ce cas, un Pod), et les métadonnées du pod comme son nom et ses labels.
- `spec`: Spécifie les caractéristiques du pod, telles que les conteneurs qu'il doit exécuter et leurs configurations.
- `tolerations`: C'est ici que vous définissez la ou les tolérances que le pod doit avoir pour être capable de s'exécuter sur des nœuds avec des taints spécifiques. Dans cet exemple, la tolérance est configurée pour correspondre au taint `lock=yellow:NoSchedule`.
  - `key`, `operator`, `value`, `effect`: Ces sous-champs définissent la tolérance. Le pod sera tolérant au taint qui a la clé `lock`, avec la valeur `yellow`, et l'effet `NoSchedule`, signifiant que le pod peut être planifié sur les nœuds avec ce taint.
- `containers`: Liste les conteneurs que le pod doit exécuter, y compris l'image à utiliser (`nginx:latest` dans cet exemple), et les ports à ouvrir.

Ce fichier YAML peut être appliqué à votre cluster Kubernetes avec la commande suivante :

```bash
kubectl apply -f <nom-du-fichier>.yaml
```
Assurez-vous de remplacer `<nom-du-fichier>` par le nom réel de votre fichier YAML.
