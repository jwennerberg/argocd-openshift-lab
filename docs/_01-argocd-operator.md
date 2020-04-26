## Install Argo CD Operator

### Install with Operator Lifecycle Manager in OpenShift

Clone this repository into your workspace.

```bash
git clone https://github.com/jwennerberg/argocd-openshift-lab.git
cd argocd-openshift-lab
```

Install `argocd-operator` with Operator Lifecycle Manager (OLM) in OpenShift.
The repository contains the operator manifests needed for installation.

This step requires `cluster-admin` permissions.

```bash
oc new-project argocd
oc apply -f argocd/operator/
```

With the manifests applied the ArgoCD Operator will be installed in the `argocd`
namespace. Make sure the operator is ready before continuing.

```bash
installedCSV=$(oc get subscription argocd-operator -n argocd -o jsonpath='{.status.installedCSV}')
oc get csv $installedCSV -n argocd -o jsonpath='{.status.phase}'
```
```
Succeeded
```

There will now be a `argocd-operator` Pod running in the `argocd` namespace.

```bash
oc get pods -l name=argocd-operator -n argocd
```
```
NAME                               READY   STATUS    RESTARTS   AGE
argocd-operator-6bcbcd97f8-cz4fh   1/1     Running   0          33s
```

Next step: [Create a ArgoCD instance](/docs/_02-argocd-instance-install.md)
