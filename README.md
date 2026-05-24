# islam-atc-argocd

Learning repo for Argo CD and GitOps.

This repository is organized so you can learn in layers:

```text
.
├── Kubernetes/
│   └── dev/verizon/              # Original raw Kubernetes manifests
├── apps/
│   └── myimage/
│       ├── base/                 # Shared Kubernetes manifests
│       └── overlays/
│           ├── dev/              # Dev-specific Kustomize config
│           ├── stage/            # Stage-specific Kustomize config
│           └── prod/             # Prod-specific Kustomize config
├── argocd/
│   ├── applications/             # Argo CD Application templates
│   └── projects/                 # Argo CD AppProject templates
├── bootstrap/
│   └── root-app.yaml             # App-of-apps bootstrap example
└── docs/
    └── learning-path.md          # Step-by-step learning guide
```

## What To Learn Here

1. Apply raw Kubernetes YAML with `kubectl`.
2. Render environment-specific manifests with Kustomize.
3. Deploy an app with Argo CD.
4. Use the app-of-apps pattern to let one Argo CD Application manage many apps.

## Quick Start

Render the dev overlay locally:

```sh
kubectl kustomize apps/myimage/overlays/dev
```

Apply one Argo CD project and app:

```sh
kubectl apply -f argocd/projects/learning-project.yaml
kubectl apply -f argocd/applications/myimage-dev.yaml
```

The Argo CD examples point to this repository:

```text
https://github.com/islamatc/islam-atc-argocd.git
```

If the repository is private, add repository credentials in Argo CD before syncing.

## Recommended Learning Path

Follow [docs/learning-path.md](docs/learning-path.md).
