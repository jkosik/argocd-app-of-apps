# sample-app
Deployment of a sample Helm chart via ArgoCD to existing K8S cluster with ArgoCD built using https://github.com/jkosik/gke-deployer.


## Repository structure
- `/argocd` directory contains ArgoCD configuration - Application manifests, ArgoCD configuration,...Selfmanaged ArgoCD will observe `/argocd` and configure itself. See [example manifests](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#atomic-configuration).
- `/charts` contains one or more Helm Charts referenced from ArgoCD Application definition (point 2.).
- `/argocd` directory can configure environment to follow git branch management of the application and deploy the whole dev/stage/prod stacks in an automated fashion to the repspective namespaces.

## Quick start
Manually create ArgoCD Application to follow changes in `/argocd` directory.

## Secrets
Sometimes we need to store Secrets directly in git. Most prevalent options are Mozilla SOPS ans Bitnami Sealed Secrets. I am using SOPS since Sealed Secrets need first to establish trust with the target cluster and usecase of multiple target cluster can make things peroblematic. Decoupling Secret encryption from target cluster makes sense for certain usecases.
ArgoCD object requiring Secrets might be `Repository` or `Cluster` resources.

TBD