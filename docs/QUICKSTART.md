# Quickstart

> **Status: stale.** The open-source deployment path lags the actively developed closed-source version, so these steps are out of date and need revision. Treat them as a rough guide until updated.

---

The quickest way to get started is to deploy Alethic-ISM on a local [k8s kind cluster](https://kind.sigs.k8s.io/). This setup includes the core infrastructure, processors, APIs, and Alethic Studio.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Helm](https://helm.sh/docs/intro/install/)

### Local Deployment

1. **Clone the repository:**

```shell
git clone https://github.com/quantumwake/alethic.git
cd alethic
```

2. **Create a kind cluster with ingress enabled:**

```shell
kind create cluster --config alethic-ism-helm/kind-config-ingress.yaml
```

3. **Install NGINX Ingress Controller:**

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

4. **Deploy Alethic-ISM using Helm:**

```shell
cd alethic-ism-helm
helm dependency update
helm install alethic . --timeout 10m
```

5. **Wait for all pods to be ready:**

```shell
kubectl get pods -w
```

### Accessing the System

Once deployed, the following services are available:

- **Alethic Studio UI**: http://localhost/ui
- **Sign Up**: http://localhost/ui/signup/basic
- **API Endpoint**: http://localhost/api/v1
- **Query API**: http://localhost/query

### Getting Started with Alethic Studio

1. Navigate to http://localhost/ui/signup/basic
2. Create an account
3. Log in and start building your first instruction graph

### Cleanup

```shell
kind delete cluster
```
