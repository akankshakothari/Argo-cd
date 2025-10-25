# Argo CD ApplicationSet Demo (dev only)

This repo layout lets Argo CD auto-discover apps under `dev/*` and create/sync an Application per folder (no Kustomize).

## Files

```
applicationsets/
  env-applicationset.yaml   # ApplicationSet scanning dev/*
dev/
  app1/
    deployment.yaml         # minimal nginx Deployment + Service
```

## How it works

- The ApplicationSet scans `dev/*` in this repo (`https://github.com/akankshakothari/Argo-cd.git`, branch `main`).
- Each folder (e.g., `dev/app1`) becomes an Argo CD Application named `dev-app1`.
- It syncs into the **in-cluster** destination: `https://kubernetes.default.svc`, namespace `dev-app1`.
- Namespaces are auto-created (`CreateNamespace=true`).

## Bootstrap (apply once)

Apply the ApplicationSet once so Argo CD begins managing apps from your Git:
```bash
kubectl apply -n argocd -f applicationsets/env-applicationset.yaml
```

## Add more apps

Create a new folder and put plain YAML inside, for example:

```
dev/app2/deployment.yaml
```

Commit & push. The Application `dev-app2` will appear in Argo CD and sync automatically.

