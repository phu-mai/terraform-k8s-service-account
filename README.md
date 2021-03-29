# K8S ServiceAccount Module

<!-- NOTE: We use absolute linking here instead of relative linking, because the terraform registry does not support
           relative linking correctly.
-->

This Terraform Module manages Kubernetes
[`ServiceAccounts`](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/). This module
can be used to declaratively create and update `ServiceAccounts` and the bound permissions that it has.


## How do you use this module?

```hcl

module "service_account" {
  source = "./terraform-k8s-service-account"
  name                   = "account_name"
  namespace              = "kube-system"
  num_rbac_cluster_roles = 1

  rbac_cluster_roles = [
    {
      name      = "cluster-admin"
      namespace = "kube-system"
    },
  ]
}


```


## What is a ServiceAccount?

`ServiceAccounts` are authenticated entities to the Kubernetes API that map to container processes in a Pod. This is
used to differentiate from User Accounts, which map to actual users consuming the  Kubernetes API.

`ServiceAccounts` are allocated to Pods at creation time, and automatically authenticated when calling out to the
Kubernetes API from within the Pod. This has several advantages:

- You don't need to share and configure secrets for the Kubernetes API client.
- You can restrict permissions on the service to only those that it needs.
- You can differentiate a service accessing the API and performing actions from users accessing the API.

Use `ServiceAccounts` whenever you need to grant access to the Kubernetes API to Pods deployed on the cluster.
