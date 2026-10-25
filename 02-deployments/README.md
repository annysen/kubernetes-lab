# Kubernetes Deployments and ReplicaSets

In this lab, I learned how to create a Deployment, check its ReplicaSet, scale the number of Pods, and test how Kubernetes replaces a deleted Pod.

## Step 1: Create the Deployment YAML File

First, I generated a YAML file using `kubectl`. The `--dry-run=client` option generates the configuration without creating the Deployment.

```bash
kubectl create deployment nginx-deployment --image=nginx:stable --dry-run=client -o yaml > deployment.yaml
```

## Step 2: Understand the YAML File

The `deployment.yaml` file contains the configuration for the Deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
        - name: nginx
          image: nginx:stable
```

A few important fields:

- `replicas`: The number of Pods I want Kubernetes to maintain.
- `selector`: Identifies the Pods managed by the Deployment.
- `template`: Defines how the Pods should be created.
- `image`: Specifies the container image to run.

The labels in `selector.matchLabels` must match the labels in the Pod template.

## Step 3: Create the Deployment

I used the following command to create the Deployment from the YAML file.

```bash
kubectl apply -f deployment.yaml
```

## Step 4: Check the Deployment and ReplicaSet

I used these commands to check the Deployment, ReplicaSet, and Pods.

```bash
kubectl get deployments
kubectl get replicasets
kubectl get pods
```

To see more details about the Deployment:

```bash
kubectl describe deployment nginx-deployment
```

When I created the Deployment, Kubernetes automatically created a ReplicaSet to manage its Pods.

The relationship looks like this:

```text
Deployment
    |
    v
ReplicaSet
    |
    v
Pods
```

## Step 5: Scale the Deployment

Initially, the Deployment had one replica. I used the following command to increase the number of replicas to three.

```bash
kubectl scale deployment nginx-deployment --replicas=3
```

I checked the resources again:

```bash
kubectl get deployments
kubectl get replicasets
kubectl get pods
```

The ReplicaSet showed three desired, current, and ready replicas.

**Note:** This command changes the live Deployment. It does not update the saved `deployment.yaml` file.

## Step 6: Test ReplicaSet Self-Healing

Next, I tested what happens when a Pod managed by the Deployment is deleted.

First, I listed the Pods:

```bash
kubectl get pods
```

Then, I deleted one of the Deployment's Pods:

```bash
kubectl delete pod <pod-name>
```

I replaced `<pod-name>` with the actual name of the Pod I wanted to delete.

After checking the Pods again:

```bash
kubectl get pods
```

I observed that Kubernetes created a replacement Pod. This demonstrated how the ReplicaSet maintains the desired number of replicas.

## Step 7: Clean Up

When I no longer need this lab, I can delete the Deployment using:

```bash
kubectl delete deployment nginx-deployment
```

To check the remaining Deployments and ReplicaSets:

```bash
kubectl get deployments
kubectl get replicasets
```

Deleting the Deployment also removes its managed ReplicaSet and Pods. Other independent Pods, such as `label-pod`, are not removed by this command.
