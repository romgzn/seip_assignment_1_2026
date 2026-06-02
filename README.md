# Software Engineering in Practice — Assignment 1 (2026)
## Advanced DevOps: Production-Grade CI/CD, External Configuration, and Orchestration

---

## Objective

Build a complete, automated deployment pipeline for a cloud-native web application.

---

## Prerequisites

Before starting, ensure the following are installed:

- [Docker Desktop / Docker Engine](https://docs.docker.com/desktop/setup/install/windows-install/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
- [Git](https://git-scm.com/install/)

---

## Clone the Repository

```bash
git clone https://github.com/romgzn/seip_assignment_1_2026.git
cd seip_assignment_1_2026
```

---

## Deployment

### 1. Start Docker Engine

Make sure Docker Desktop is running before proceeding.

### 2. Start Minikube

```bash
minikube start
```

Optionally verify it's running:

```bash
minikube status
```

### 3. Apply the Manifests

Apply all at once:

```bash
kubectl apply -f k8s/
```

Or one by one — order matters, as ConfigMap and Secret must exist before the Deployment injects them:

```bash
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Optionally verify pods are running:

```bash
kubectl get pods
```

Wait until all 3 pods show `STATUS: Running`.

### 4. Forward the Service Port

```bash
kubectl port-forward service/echo-api-service 8080:80
```

> **Note:** Keep this terminal open and open a new one in the same directory for the next step.

### 5. Interact with the Endpoints

```bash
curl http://localhost:8080/
```
Returns the welcome message from the ConfigMap.

```bash
curl http://localhost:8080/secure-config
```
Returns masked confirmation that the Secret was injected successfully.

---

## Teardown

Teardown all at once:

```bash
kubectl delete -f k8s/
minikube stop
```

Or one by one
```bash
kubectl delete -f k8s/service.yaml
kubectl delete -f k8s/deployment.yaml
kubectl delete -f k8s/secret.yaml
kubectl delete -f k8s/configmap.yaml
minikube stop
```