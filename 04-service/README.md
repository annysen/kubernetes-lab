# 04 - Kubernetes Services

## 1. Create a Deployment

Generate the Deployment YAML:

```bash
kubectl create deployment web-deploy --image=nginx --replicas=3 --dry-run=client -o yaml > web-deploy.yaml
```

Apply the YAML:

```bash
kubectl apply -f web-deploy.yaml
```

Check the Deployment and Pods:

```bash
kubectl get deployments
kubectl get pods
```

## 2. Create a NodePort Service

Generate the NodePort Service YAML:

```bash
kubectl expose deployment web-deploy --name=web-deploy-svc --port=80 --target-port=80 --type=NodePort --dry-run=client -o yaml > web-deploy-svc.yaml
```

Apply the YAML:

```bash
kubectl apply -f web-deploy-svc.yaml
```

Check the Service:

```bash
kubectl get services
```

## 3. Access the Application Using NodePort

Get the Node IP:

```bash
kubectl get nodes -o wide
```

Get the NodePort:

```bash
kubectl get service web-deploy-svc
```

Access Nginx using:

```bash
curl <NODE-IP>:<NODE-PORT>
```

Example:

```bash
curl 192.168.137.138:32767
```

## 4. Create a Test Pod

Generate the Pod YAML:

```bash
kubectl run test-pod --image=busybox:1.36 --restart=Never --dry-run=client -o yaml --command -- sleep 3600 > test-pod.yaml
```

Apply the YAML:

```bash
kubectl apply -f test-pod.yaml
```

Check the Pod:

```bash
kubectl get pods
```

## 5. Create a ClusterIP Service

Generate the ClusterIP Service YAML:

```bash
kubectl expose deployment web-deploy --name=web-deploy-cluster-svc --port=80 --target-port=80 --type=ClusterIP --dry-run=client -o yaml > web-deploy-cluster-svc.yaml
```

Apply the YAML:

```bash
kubectl apply -f web-deploy-cluster-svc.yaml
```

Check the Service:

```bash
kubectl get services
```

## 6. Test ClusterIP Service Connectivity

Access the Nginx application from `test-pod` using the Service DNS name:

```bash
kubectl exec test-pod -- wget -qO- http://web-deploy-cluster-svc
```

You should see the Nginx welcome page HTML.

## 7. Cleanup

Delete all resources:

```bash
kubectl delete -f test-pod.yaml
kubectl delete -f web-deploy-svc.yaml
kubectl delete -f web-deploy-cluster-svc.yaml
kubectl delete -f web-deploy.yaml
```
