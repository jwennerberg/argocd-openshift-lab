## Create applications in Argo CD

With ArgoCD installed and configured it can be used by developer and DevOps teams.
Login to ArgoCD again, this time with a developer account (`developer1` in this case).

TODO: SCREENSHOT

This user is part of the team (xfteam1) that we created access for in an earlier step. This user have access to a ArgoCD Project called `xfteam1`. Projects and role-based access controls in ArgoCD is how we control what resources different teams can control with ArgoCD.

### Create Argo Applications using the Web UI

TBD

### Create Argo Applications using the CLI

#### Login to the argocd CLI

```bash
argocd login $ARGOCD_HOSTNAME --sso --insecure
```

The `--sso` option opens a browser window with OpenShift authentication. Since this
cluster is using self-signed certificates the `--insecure` option is used as well.

If the authentication is successful the CLI output will look something like this.
```
Opening browser for authentication
Performing authorization_code flow login: https://argocd-server-argocd.apps-crc.testing/api/dex/auth?access=..xyz
Authentication successful
'developer1' logged in successfully
```

#### Create applications

Applications are created with the `argocd app create` command. There are a few
different options available. Applications can be created by passing all required parameters on the command line (like this [example](/docs/examples/argocd-app-create.md)), but it's also possible to pass a ArgoCD Application manifest as a file, which is great since it allows us to also store this configuration in Git.

In the example Git repository there's a structure in `apps/vote-deploy/` that
holds the ArgoCD Application manifests for the example **vote** application.

```
argocd-openshift-lab/apps/vote-deploy
|-- base
|   |-- vote-api
|   |   |-- kustomization.yaml
|   |   `-- vote-api.yaml
|   |-- vote-ui
|   |   |-- kustomization.yaml
|   |   `-- vote-ui.yaml
|   `-- kustomization.yaml
|-- dev
|   |-- vote-api
|   |   `-- kustomization.yaml
|   |-- vote-ui
|   |   `-- kustomization.yaml
|   `-- kustomization.yaml
|-- test
|   |-- vote-api
|   |   |-- kustomization.yaml
|   |   `-- patch-vote-api.yaml
|   |-- vote-ui
|   |   |-- kustomization.yaml
|   |   `-- patch-vote-ui.yaml
|   `-- kustomization.yaml
`-- kustomization.yaml
```

This example is using [Kustomize](https://kustomize.io/) which allows us to customize the application manifests when deploying in multiple environments.

The Kustomize tool is built in to the OpenShift client `oc` and `kubectl`. We can use this in combination with the `argocd` client to create the ArgoCD manifests for our application.

This example creates the two components of our example application in the **development** environment; deployed in the **vote-dev** namespace in OpenShift.

Example application setup with Kustomize:
- [/apps/vote-deploy/dev/vote-api](vote-api)
- [/apps/vote-deploy/dev/vote-ui](vote-ui)

```bash
oc kustomize apps/vote-deploy/dev/vote-api | argocd app create -f -
oc kustomize apps/vote-deploy/dev/vote-ui | argocd app create -f -
```

With the applications created they can be viewed in both the CLI and in the ArgoCD Web-UI.

```bash
argocd app list
```
```
NAME          CLUSTER                         NAMESPACE  PROJECT  STATUS  HEALTH   SYNCPOLICY  CONDITIONS  REPO                                                 PATH                              TARGET
vote-api-dev  https://kubernetes.default.svc  vote-dev   xfteam1  Synced  Healthy  Auto        <none>      https://github.com/jwennerberg/argocd-openshift-lab.git  apps/vote-api/k8s/openshift/base  dev
vote-ui-dev   https://kubernetes.default.svc  vote-dev   xfteam1  Synced  Healthy  Auto        <none>      https://github.com/jwennerberg/argocd-openshift-lab.git  apps/vote-ui/k8s/openshift/base   dev
```

TODO: INSERT SCREENSHOT
