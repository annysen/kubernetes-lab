# Kubernetes Deployments

## Step 1: Generate the Deployment YAML File

Use the imperative command below to generate a Deployment YAML file without creating the Deployment.

```bash
kubectl create deployment nginx-deployment --image=nginx:stable --dry-run=client -o yaml > deployment.yaml
```

## Step 2: Reference YAML

The `deployment.yaml` file should contain the following configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
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
      - image: nginx:stable
        name: nginx
```

## Step 3: Apply the YAML File

Create the Deployment using the YAML file.

```bash
kubectl apply -f deployment.yaml
```

## Step 4: Check the Deployment

Check the Deployment status.

```bash
kubectl get deployments
```

Check the Pods created by the Deployment.

```bash
kubectl get pods
```

To see detailed information about the Deployment:

```bash
kubectl describe deployment nginx-deployment
```

## Step 5: Delete the Deployment

Delete the Deployment using the YAML file.

```bash
kubectl delete -f deployment.yaml
```

Verify that the Deployment has been deleted.

```bash
kubectl get deployments
```
