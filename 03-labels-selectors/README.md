# Kubernetes Labels and Selectors

## Step 1: Generate the Pod YAML File

Use the imperative command below to generate a Pod YAML file without creating the Pod.

```bash
kubectl run label-pod --image=nginx --dry-run=client -o yaml > label-pod.yaml
```

## Step 2: Reference YAML

The `label-pod.yaml` file should contain the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-pod
  labels:
    run: label-pod
    app: nginx
    environment: testing
spec:
  containers:
    - name: label-pod
      image: nginx
      resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

## Step 3: Apply the YAML File

Create the Pod using the YAML file.

```bash
kubectl apply -f label-pod.yaml
```

## Step 4: Check the Pod and Its Labels

Check the Pod status.

```bash
kubectl get pods
```

Display the Pod labels.

```bash
kubectl get pods --show-labels
```

To see detailed information about the Pod:

```bash
kubectl describe pod label-pod
```

## Step 5: Select Pods Using Labels

Select the Pod using the `app=nginx` label.

```bash
kubectl get pods -l app=nginx
```

Select the Pod using the `environment=testing` label.

```bash
kubectl get pods -l environment=testing
```

Select the Pod using both labels.

```bash
kubectl get pods -l 'app=nginx,environment=testing'
```

## Step 6: Add or Update a Label

Add a new label to the Pod.

```bash
kubectl label pod label-pod tier=frontend
```

Verify the updated labels.

```bash
kubectl get pods --show-labels
```

## Step 7: Remove a Label

Remove the `tier` label from the Pod.

```bash
kubectl label pod label-pod tier-
```

Verify that the label has been removed.

```bash
kubectl get pods --show-labels
```

## Step 8: Delete the Pod

Delete the Pod using the YAML file.

```bash
kubectl delete -f label-pod.yaml
```

Verify that the Pod has been deleted.

```bash
kubectl get pods
```
