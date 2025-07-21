
---

## 📁 Repository Structure Example

```
argo-app-deployment/
├── README.md
├── deployment.yaml
├── service.yaml
├── argocd-app.yaml
```

---

## 📄 Example YAML Files

---

### 1️⃣ **deployment.yaml**

Defines your application deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
  labels:
    app: demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: demo-app
        image: nginx:latest  # Replace with your app image
        ports:
        - containerPort: 80
```

---

### 2️⃣ **service.yaml**

Defines the Kubernetes service to expose the app.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: demo-app-service
spec:
  type: ClusterIP
  selector:
    app: demo-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

---

### 3️⃣ **argocd-app.yaml**

Defines the ArgoCD application pointing to your GitHub repo.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: demo-app
  namespace: argocd
spec:
  project: default

  source:
    repoURL: https://github.com/your-github-username/argo-app-deployment.git
    targetRevision: HEAD
    path: .

  destination:
    server: https://kubernetes.default.svc
    namespace: default

  syncPolicy:
    automated:
      selfHeal: true
      prune: true
```

> Replace `https://github.com/your-github-username/argo-app-deployment.git` with your actual GitHub repo URL.

---

### 4️⃣ **README.md**

Document usage instructions.

````markdown
# ArgoCD Demo App Deployment

This repository contains Kubernetes manifests for deploying a demo application using ArgoCD.

## Files
- `deployment.yaml`: Defines the demo app deployment.
- `service.yaml`: Defines the service for exposing the app.
- `argocd-app.yaml`: Defines the ArgoCD Application resource.

## Setup
1. Deploy `argocd-app.yaml` in your ArgoCD:
   ```bash
   kubectl apply -f argocd-app.yaml -n argocd
````

2. ArgoCD will auto-sync the application from this repo.

3. Access your app using:

   ```bash
   kubectl port-forward svc/demo-app-service 8080:80
   ```

4. Visit `http://localhost:8080`.

````

---

## 🔗 After preparing, push to GitHub:
```bash
git init
git add .
git commit -m "Initial commit for ArgoCD app deployment"
git branch -M main
git remote add origin https://github.com/your-github-username/argo-app-deployment.git
git push -u origin main
````

---

