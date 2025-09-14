# 🚀 Módulo 1: DevOps Estratégico y GitOps
## Ejercicio Práctico 1

Laboratorio de GitOps Avanzado con ArgoCD + Repositorio Git Privado + Gestión de Secretos con Bitnami Sealed Secrets.

### 🧰 Install CLI Tools

```bash
brew install minikube
brew install kubectl
brew install argocd
brew install kubeseal
brew install ngrok
brew install kustomize
```

### 📦 Clone Test Git Repository

```bash
cd ~/Desktop/
gh repo fork https://github.com/argoproj/argocd-example-apps.git
```

**Official Documentation:**  [ArgoCD Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/)

---

### ⚙️ Start Kubernetes Cluster

```bash
minikube start --driver=docker
kubectl get nodes
minikube dashboard &
```

### 🚀 Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd
```

### 🌐 Access Argo CD API Server

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl get svc argocd-server -n argocd -o=jsonpath='{.status.loadBalancer.ingress[0].ip}'
kubectl port-forward svc/argocd-server -n argocd 8080:443 &
```

### 🔐 Login Using CLI

```bash
argocd admin initial-password -n argocd
argocd login <ARGOCD_SERVER>
argocd account update-password
```

### 🧭 Register Cluster (Optional)

```bash
kubectl config get-contexts -o name
argocd cluster add docker-desktop
```

### 📥 Create Application from Git

```bash
kubectl config set-context --current --namespace=argocd
argocd app create guestbook \
  --repo https://github.com/rafariass/argocd-example-apps.git \
  --path guestbook \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
argocd app set guestbook --sync-policy automated
```

---

### 🔐 Install Sealed Secrets Controller

```bash
kubectl create namespace kube-system
kubectl apply -n kube-system -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.27.0/controller.yaml
kubectl get pods -n kube-system
```

### 🔒 Encode and Verify Secrets

```bash
echo -n 'password' | base64
echo -n 'cGFzc3dvcmQ=' | base64 --decode; echo
```

### 📄 Create Secret File

Create the database-password.yaml file. (Never store passwords or secrets in plain text).

```bash
cat <<EOF > ~/Desktop/database-password.yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: default
type: Opaque
data:
  password: $(echo -n 'password' | base64)
EOF
```

### 🔐 Encrypt Secret with Kubeseal

```bash
kubeseal \
  --controller-namespace kube-system \
  --format=yaml \
  < ~/Desktop/database-password.yaml \
  > ~/Desktop/argocd-example-apps/guestbook/sealed-database-secret.yaml
```

### 💾 Commit and Push to Git

```bash
cd ~/Desktop/argocd-example-apps/
git add guestbook/sealed-database-secret.yaml
git commit -m 'feat(secrets): add sealed secret for database password'
git push origin master
```

### 📊 Check Application Status

```bash
argocd app get guestbook
```

### 🔍 View and Decode Secret

Alternative 1
```bash
kubectl get secret db-secret -n default -o yaml
```

Alternative 2
```bash
kubectl get secret db-secret -n default -o jsonpath="{.data.password}" | base64 --decode; echo
```

---

### 📈 Scale Deployment

Check the configured replica count and verify the active pods based on that configuration

```bash
kubectl get deployment guestbook-ui -n default -o jsonpath="{.spec.replicas}"; echo
kubectl get pods -n default -l app=guestbook-ui
```

**Update replicas from 1 to 3:**

```bash
code ~/Desktop/argocd-example-apps/guestbook/guestbook-ui-deployment.yaml
```

Change the value of the replicas field from 1 to 3.

```yaml
# spec:
#   replicas: 1 -> 3
```

### 💾 Commit and Push Changes

```bash
cd ~/Desktop/argocd-example-apps/
git add guestbook/guestbook-ui-deployment.yaml
git commit -m "feat: update replicas from 1 to 3"
git push origin master
```

### 🔄 Manual Sync

Manually forces the synchronization of the guestbook application in Argo.
- To avoid manual synchronization, set up a webhook inside the GitHub repository.

```bash
argocd app sync guestbook
argocd app get guestbook
```

### 🔍 Verify Replica Count

Check the configured replica count and verify the active pods based on that configuration

```bash
kubectl get deployment guestbook-ui -n default -o jsonpath="{.spec.replicas}"; echo
kubectl get pods -n default -l app=guestbook-ui
```

---

### 🛡️ Enable Self-Heal

To prevent changes that don't come from Git within Argo, enable self-heal

```bash
argocd app set guestbook --sync-policy automated --self-heal
```

---

### 🧹 Clean Up Environment

```bash
argocd app delete guestbook --yes
kubectl delete namespace argocd
kubectl delete namespace kube-system
kubectl delete secret db-secret -n default
kubectl delete deployment guestbook-ui -n default

ps aux | grep minikube
ps aux | grep kubectl
kill <PID>

pkill -f 'minikube dashboard'
pkill -f 'kubectl port-forward'

minikube stop
minikube delete

rm -rf ~/Desktop/argocd-example-apps
rm ~/Desktop/database-password.yaml
```

---
