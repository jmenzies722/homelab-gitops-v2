# homelab-gitops-v2

GitOps source of truth for my M1 home lab (kind cluster `homelab`, OrbStack).
Argo CD watches this repo and keeps the cluster matching it.

## Layout
```
bootstrap/
  root.yaml       # app of apps: applied once by hand, manages everything in argocd/
argocd/
  hello.yaml      # Application -> apps/hello
  monitoring.yaml # Application -> kube-prometheus-stack Helm chart (pinned)
apps/
  hello/          # sample app: Deployment + Service + Ingress (Traefik)
```

## Bootstrap a fresh cluster
```bash
kubectl apply -f bootstrap/root.yaml   # Argo deploys everything else from here
```

## How to change something
1. Edit the YAML here
2. `git commit` and `git push`
3. Argo CD notices within ~3 minutes (or hit Refresh in the dashboard) and syncs

**Add a new app:** drop a new Application file into `argocd/` and push.
Don't `kubectl apply` by hand: Argo self-heals and will put it back to what Git says.

## URLs (via Traefik on localhost:8080)
| URL | What |
|---|---|
| http://localhost:8080 | hello |
| http://argocd.localhost:8080 | Argo CD |
| http://grafana.localhost:8080 | Grafana |
| http://prometheus.localhost:8080 | Prometheus |

No secrets live in this repo: passwords are generated inside the cluster.
