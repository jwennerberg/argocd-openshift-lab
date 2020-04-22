#### Example: Create argocd applications directly with params

```bash
argocd app create vote-api-dev \
  --repo https://github.com/jwennerberg/argocd-openshift-lab.git \
  --revision dev \
  --path apps/vote-api/k8s/openshift/base/ \
  --project xfteam1 \
  --dest-namespace vote-dev \
  --dest-server https://kubernetes.default.svc \
  --sync-policy automated
```

```bash
argocd app create vote-ui-dev \
  --repo https://github.com/jwennerberg/argocd-openshift-lab.git \
  --revision dev \
  --path apps/vote-ui/k8s/openshift/base/ \
  --project xfteam1 \
  --dest-namespace vote-dev \
  --dest-server https://kubernetes.default.svc \
  --sync-policy automated
```
