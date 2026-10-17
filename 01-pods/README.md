# Kubernetes Pods

## Step 1: Create the Pod YAML File

Use the following imperative command to generate a Pod YAML file without creating the Pod.

```bash
kubectl run nginx-pod --image=nginx:stable --dry-run=client -o yaml > basic-pod.yaml
```

## Step 2: Reference YAML

The `basic-pod.yaml` file should contain the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:stable
      ports:
        - containerPort: 80
```

## Step 3: Apply the YAML File

Create the Pod using the YAML file.

```bash
kubectl apply -f basic-pod.yaml
```

## Step 4: Check the Pod

Check whether the Pod is running.

```bash
kubectl get pods
```

To see detailed information about the Pod:

```bash
kubectl describe pod nginx-pod
```

## Step 5: Delete the Pod

Delete the Pod using the YAML file.

```bash
kubectl delete -f basic-pod.yaml
```
