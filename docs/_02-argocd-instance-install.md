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
  server:
    route: true
  dex:
    image: quay.io/dexidp/dex
    version: v2.22.0
    openShiftOAuth: true
  rbac:
    defaultPolicy: ""
    policy: |
      g, argocdadmins, role:admin

      p, role:org-user, clusters, get, *, allow
      p, role:org-user, repositories, get, *, allow

      p, role:xfteam1-admins, applications, *, xfteam1/*, allow
      p, role:xfteam1-admins, projects, get, xfteam1, allow
      p, role:xfteam1-users, applications, get, xfteam1/*, allow
      p, role:xfteam1-users, applications, sync, xfteam1/*, allow
      p, role:xfteam1-users, projects, get, xfteam1, allow

      g, xfteam1-admins, role:xfteam1-admins
      g, xfteam1-users, role:xfteam1-users
      g, xfteam1-users, role:org-user
    scopes: '[groups]'
```

Create a ArgoCD instance by applying the CR in the cluster.

```bash
oc apply -f argocd/argocd.yaml -n argocd
```

An initial RBAC configuration in included in the ArgoCD CR spec. Prepare Argo CD admin access by adding a (admin) user to the `argocdadmins` group.

```
oc apply -f argocd/group.yaml
oc patch group argocdadmins --type=json -p='[{"op":"add", "path":"/users/0", "value":"<USER>"}]'
```

Since ArgoCD will be used to configure our clusters we grant the `argocd-application-controller` service-account a cluster-admin role.

```bash
oc adm policy add-cluster-role-to-user --rolebinding-name=cluster-admins cluster-admin -z argocd-application-controller -n argocd
```

**NOTE:** a cluster-admin role is optional. The permissions granted can be customized to fit your requirements using the OpenShift RBAC capabilities.

Next step: [Add cluster configuration](/docs/_03-argocd-cluster-config.md)
