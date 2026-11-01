# Kubernetes Namespaces

In this lab, I learned how to create and use Kubernetes Namespaces. I also practised creating a Pod and a Deployment inside a custom Namespace and managing resources using the `-n` flag.

## What is a Namespace?

A Namespace is a way to organize and separate resources inside a Kubernetes cluster.

For example, we can create separate Namespaces for development, testing, and production environments.

## Why do we use Namespaces?

- To organize resources inside a cluster.
- To separate development, testing, and production environments.
- To manage resources within different environments.
- To use the same resource name in different Namespaces.

## Step 1: Create a Namespace YAML File

First, generate a Namespace YAML file without creating the actual Namespace.

```bash
kubectl create ns dev-namespace --dry-run=client -o yaml > namespace.yaml
```

Check the generated file:

```bash
cat namespace.yaml
```

### Reference YAML

The `namespace.yaml` file contains:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-namespace
spec: {}
status: {}
```

## Step 2: Create the Namespace

Apply the YAML file:

```bash
kubectl apply -f namespace.yaml
```

Check all Namespaces:

```bash
kubectl get ns
```

Check a specific Namespace:

```bash
kubectl get ns dev-namespace
```

## Step 3: Create a Pod Inside the Namespace

Generate a Pod YAML file:

```bash
kubectl run nginx-pod --image=nginx -n dev-namespace --dry-run=client -o yaml > nginx-pod.yaml
```

Check the generated YAML:

```bash
cat nginx-pod.yaml
```

### Reference YAML

The important part is the `namespace` field under `metadata`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-pod
  name: nginx-pod
  namespace: dev-namespace
spec:
  containers:
    - image: nginx
      name: nginx-pod
```

Apply the YAML file:

```bash
kubectl apply -f nginx-pod.yaml
```

Check the Pod:

```bash
kubectl get pods -n dev-namespace
```

## Step 4: Create a Deployment Inside the Namespace

Generate the Deployment YAML file:

```bash
kubectl create deployment nginx-deployment --image=nginx --namespace=dev-namespace --dry-run=client -o yaml > deployment.yaml
```

Check the generated YAML:

```bash
cat deployment.yaml
```

Apply the YAML file:

```bash
kubectl apply -f deployment.yaml
```

Check Deployments inside the Namespace:

```bash
kubectl get deployments -n dev-namespace
```

Check Pods:

```bash
kubectl get pods -n dev-namespace
```

## Step 5: Check Resources in Different Namespaces

I practised checking resources in the default Namespace and the custom Namespace.

**Check Pods in the default Namespace:**

```bash
kubectl get pods
```

**Check Pods in dev-namespace:**

```bash
kubectl get pods -n dev-namespace
```

**Check Pods in all Namespaces:**

```bash
kubectl get pods -A
```

**Check Deployments in dev-namespace:**

```bash
kubectl get deployments -n dev-namespace
```

**Check all common resources in dev-namespace:**

```bash
kubectl get all -n dev-namespace
```

## Step 6: Understanding Resource Namespaces

During this lab, I created a Deployment named `nginx-deployment` in `dev-namespace`.

I already had another Deployment with the same name in the `default` Namespace.

I checked them separately:

```bash
kubectl get deployments
```

```bash
kubectl get deployments -n dev-namespace
```

Both Deployments existed successfully because they belonged to different Namespaces.

## Step 7: Delete Resources

I also practised deleting a Pod from a custom Namespace.

First, I tried:

```bash
kubectl delete pod nginx-pod
```

It returned a `NotFound` error because Kubernetes searched for the Pod in the default Namespace.

Then I used:

```bash
kubectl delete pod nginx-pod -n dev-namespace
```

This successfully deleted the Pod.

**Important:** When working with resources in a custom Namespace, specify the Namespace using `-n` or `--namespace`.

## Step 8: Delete the Namespace

To delete the entire Namespace:

```bash
kubectl delete namespace dev-namespace
```

This also deletes the resources inside that Namespace, including its Pods and Deployments.

Verify the deletion:

```bash
kubectl get ns
```

## What I Learned

In this lab, I practised:

- Creating a Namespace using YAML.
- Applying and checking a Namespace.
- Creating a Pod inside a custom Namespace.
- Creating a Deployment inside a custom Namespace.
- Checking resources using the `-n` flag.
- Viewing resources across all Namespaces using `-A`.
- Understanding why the same Deployment name can exist in different Namespaces.
- Deleting resources from a specific Namespace.
- Understanding that deleting a Namespace also deletes its resources.

**One important thing I learned:** By default, kubectl works with the `default` Namespace. To work with resources in another Namespace, we can use the `-n` flag.

