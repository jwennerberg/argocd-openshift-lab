## OpenShift GitOps - ArgoCD for app teams

In this demo I am using the OpenShift GitOps operator to setup a dedicated
ArgoCD instance which will be used to manage applications in a specific set
of namespaces.


### Prerequisites
These steps requires `cluster-admin` privileges.

1. Install OpenShift GitOps operator

```bash
oc apply --kustomize https://github.com/redhat-cop/gitops-catalog/openshift-gitops-operator/overlays/latest
```

2. Configure namespaces

Create three namespaces for demo purposes.
- `project-x-mgmt`: management namespace where ArgoCD will be deployed
- `project-x`: application namespace
- `project-y`: application namespace

The application namespaces are configured with the label `argocd.argoproj.io/managed-by: project-x-mgmt` so they can be managed from the ArgoCD instance in `project-x-mgmt`.

```
oc apply -f namespace.yaml
```

Give user `user1` the admin role:

```
oc policy add cluster-role-to-user admin user1 -n project-x-mgmt
oc policy add cluster-role-to-user admin user1 -n project-x
oc policy add cluster-role-to-user admin user1 -n project-y
```

### Deploy ArgoCD

The following steps only requires access to namesapces `project-x-mgmt`, `project-x` and `project-y`.

1. Deploy ArgoCD instance

```
oc apply -f argocd.yaml -n project-x-mgmt
```

2. Add `Application` to deploy in `project-x` namespace

```
oc apply -f application-hello.yaml -n project-x-mgmt
```

3. Add `Application` to deploy in `project-y` namespace

```
oc apply -f application-hello-y.yaml -n project-x-mgmt
```

### Validate ArgoCD applications

1. Get the ArgoCD route

```
oc get route argocd-server -n project-x-mgmt -o jsonpath='https://{.spec.host}'
```

2. Log in with openshift user `user1` and verify that applications are managed in both `project-x` and `project-y`
