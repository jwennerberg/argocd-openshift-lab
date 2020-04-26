## Cluster configuration

### Add cluster configuration with Argo CD

Example:
- Argo Application: [argo-cluster-config.yaml](/cluster/cluster-a/config/argo-cluster-config.yaml)
- Kustomize: [config/kustomiztion.yaml](/cluster/cluster-a/config/kustomization.yaml)

Add the Argo appliction by creating a `Application` CustomResource in OpenShift.

```bash
oc apply -f cluster/cluster-a/config/argo-cluster-config.yaml -n argocd
```

### Log in to Argo CD and verify setup

Get URL from the `argocd-server` route

```bash
oc get route argocd-server -n argocd -o jsonpath='{.spec.host}'
```
```
argocd-server-argocd.apps-crc.testing
```

This example is using [CodeReady Containers](https://developers.redhat.com/products/codeready-containers/overview), so the browser should be pointed to https://argocd-server-argocd.apps-crc.testing

ArgoCD has been configured with OpenShift authentication. Click the **`Login via OpenShift`** button and use a OpenShift admin account (`ocpadmin` in this example) to login.

Logged in as an admin, there will now be three ArgoCD applications created:

- config
- operators
- teams

The **config** Argo application is used for various cluster configuration and also manages the other cluster scoped Argo Applications. The **operators** Argo app installs and makes sure Operators are available in the clusters and the **teams** application make sure all team specific configurations are applied.

In this case we're creating all resources that DevOps teams need to manage their applications in OpenShift, including access to ArgoCD with a multi-tenant setup.

Team specific resources that are created include :

- Namespaces
- Groups
- RoleBindings
- ArgoCD Projects

Next step: [Developer: Create applications in Argo CD](/docs/_04-argocd-create-applications.md)
