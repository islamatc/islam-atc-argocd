# Argo CD and GitOps Learning Path

Use this repo as a small lab. Start with one app, learn how changes flow through Git, then add more environments and applications.

## 1. Raw Kubernetes

The original files are kept in `Kubernetes/dev/verizon`. They are useful for learning plain `kubectl apply`.

Try:

```sh
kubectl apply -f Kubernetes/dev/verizon
kubectl get deploy,svc
kubectl delete -f Kubernetes/dev/verizon
```

## 2. Kustomize

The same app now exists in `apps/myimage`.

- `base` has shared Kubernetes manifests.
- `overlays/dev` changes dev-specific settings.
- `overlays/stage` changes stage-specific settings.
- `overlays/prod` changes prod-specific settings.

Try:

```sh
kubectl kustomize apps/myimage/overlays/dev
kubectl kustomize apps/myimage/overlays/stage
kubectl kustomize apps/myimage/overlays/prod
```

## 3. Argo CD Applications

Argo CD reads the files under `argocd/applications` and deploys the matching overlay.

The Application files currently point to this repository:

```text
https://github.com/islamatc/islam-atc-argocd.git
```

If you rename or fork the repo, update `repoURL` in `argocd/applications` and `bootstrap/root-app.yaml`.

Try one app first:

```sh
kubectl apply -f argocd/projects/learning-project.yaml
kubectl apply -f argocd/applications/myimage-dev.yaml
```

Then watch Argo CD:

```sh
argocd app get myimage-dev
argocd app sync myimage-dev
```

## 4. App Of Apps

`bootstrap/root-app.yaml` is an app-of-apps example. Instead of applying every Argo CD Application manually, you apply one root Application and it manages the child Applications.

Try this after you understand one Application:

```sh
kubectl apply -f argocd/projects/learning-project.yaml
kubectl apply -f bootstrap/root-app.yaml
```

## Practice Ideas

1. Change the dev image tag in `apps/myimage/overlays/dev/kustomization.yaml`.
2. Change `replicas` in stage or prod.
3. Break a manifest on purpose, commit it, and watch Argo CD report the error.
4. Turn automated sync off in dev and sync manually.
5. Add a second app by copying `apps/myimage` and one Argo CD Application file.
