## Create ArgoCD instance

With the ArgoCD Operator installed there's now a CustomResourceDefinition (CRD) available describing a ArgoCD instance. We can now simply create a ArgoCD instance by creating a `ArgoCD` CustomResource (CR).

An example ArgoCD CustomResource [manifest](/argocd/argocd.yaml).
```
apiVersion: argoproj.io/v1alpha1
kind: ArgoCD
metadata:
  name: argocd
  namespace: argocd
spec:
  dex:
    image: quay.io/dexidp/dex
    version: v2.22.0
    openShiftOAuth: true
```

Create a ArgoCD instance by applying the CR in the cluster.

```bash
oc apply -f argocd/argocd.yaml -n argocd
```

An initial RBAC configuration has been prepared for this setup. The configuration should be applied in the `argocd` namespace.

```
oc apply -f argocd/rbac/ -n argocd
```

Since ArgoCD will be used to configure our clusters we grant the `argocd-application-controller` service-account a cluster-admin role.

**NOTE:** a cluster-admin role is optional. The permissions granted can be customized to fit your requirements using the OpenShift RBAC capabilities.

Next step: [Add cluster configuration](/docs/_03-argocd-cluster-config.md)
