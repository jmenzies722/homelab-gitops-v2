# homelab-gitops-v2

GitOps source of truth for my M1 home lab (kind cluster `homelab`, OrbStack).
Argo CD watches this repo and keeps the cluster matching it.

## Layout
```
apps/
  hello/        # sample app: Deployment + Service + Ingress (Traefik)
```

## How to change something
1. Edit the YAML here
2. `git commit` and `git push`
3. Argo CD notices within ~3 minutes (or hit Refresh in the dashboard) and syncs

Don't `kubectl apply` by hand: Argo self-heals and will put it back to what Git says.
