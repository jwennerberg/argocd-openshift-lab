# Lab: Argo CD with OpenShift

This repository contains examples and guides on using Argo CD with Red Hat OpenShift.

The architecture used was choosed to showcase how the multi-tenant approach in OpenShift can be extended to Argo CD.

## Argo CD setup in short
* Argo CD is deployed in OpenShift using an Operator
* Argo CD instance is configured for multi-tenancy
  - Same instance used by both cluster admins and application teams
* Integrated with OpenShift authentication
  - Users and groups in OpenShift are used for Argo CD RBAC

![ArgoCD on OpenShift](/docs/assets/argocd-openshift-lab.png)

## Guides

### Install and configure Argo CD in OpenShift

1. [Install the Argo CD Operator](/docs/_01-argocd-operator.md)
2. [Create Argo CD instance](/docs/_02-argocd-instance-install.md)
3. [Add cluster configuation with Argo CD](/docs/_03-argocd-cluster-config.md)
4. [Developer: Deploy applications with Argo CD](/docs/_04-argocd-create-applications.md)
