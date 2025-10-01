# 🚀 Vite + Preact + TypeScript + k3s Deploy

This project is a learning example of deploying a **Vite (Preact + TypeScript)** app into a **k3s (Kubernetes)** cluster with CI/CD via **GitHub Actions**.

---

## 📦 Stack
- ⚡ [Vite](https://vitejs.dev/) + [Preact](https://preactjs.com/) + TypeScript
- 📦 [pnpm](https://pnpm.io/) as package manager
- 🐳 Docker + Nginx for deployment
- ☸️ k3s (lightweight Kubernetes)
- 🌐 Traefik + cert-manager (HTTPS, Let's Encrypt)
- 🤖 GitHub Actions (CI/CD pipeline)

---

## 🔧 Local Development

```bash
# Install dependencies
pnpm install

# Start dev server
pnpm run dev

# Build for production
pnpm run build

# Preview locally
pnpm run preview
```

---

## 🐳 Docker

Build and run the app in a container:

```bash
# Build Docker image
docker build -t vite-preact-app .

# Run locally
docker run -p 8080:80 vite-preact-app

# Open in browser:
http://localhost:8080
```

---

## ☸️ Kubernetes

### Deployment & Ingress
Files are located in the `k8s/` directory:
- `deployment.yaml` — defines the frontend deployment
- `ingress.yaml` — domain + HTTPS (Let's Encrypt)

Apply to cluster:
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/ingress.yaml
```

Then your site will be available at:  
👉 **https://vite.nazarf.dev**

---

## 🤖 CI/CD (GitHub Actions)

The workflow in `.github/workflows/deploy.yml` does the following:
1. Builds a Docker image and pushes it to **GitHub Container Registry (`ghcr.io`)**.
2. Logs into the k3s cluster (secret `KUBECONFIG`).
3. Deploys `deployment.yaml` and `ingress.yaml`.

### Secrets
In GitHub repository **Settings → Secrets → Actions**, add:
- `KUBECONFIG` → content of your `~/.kube/config` (replace `127.0.0.1` with your server IP).

---

## 📝 Notes

- 🐳 Dockerfile is optimized for `pnpm` and Vite (builds into `dist`, served via `nginx:alpine`).
- 🌐 Domain `vite.nazarf.dev` must be added in Cloudflare as an `A` record → your Hetzner server IP.
- 🔐 Cert-manager automatically issues TLS certificates from Let's Encrypt.
- ⚡ CI/CD is triggered on every push to `main`.

---

## ✅ Useful Commands for Cluster Check

```bash
# Check node
kubectl get nodes

# Check all pods
kubectl get pods -A

# Logs for specific pod
kubectl logs -n project1 <pod-name>

# Inspect ingress
kubectl describe ingress -n project1
```

---
