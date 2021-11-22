# App of Apps, Sealed Secrets, ArgoCD self-configuration
Deployment of a sample application via ArgoCD using [App of Apps pattern](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#app-of-apps).
GKE+ArgoCD can be built using https://github.com/jkosik/gke-deployer.

## Repository structure
- `/argocd` directory contains [ArgoCD self-configuration](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#atomic-configuration) and Application manifest(s) pointing to `/charts` or any other Helm deployment code, even in external git repositories.
- `/charts` contains one or more Helm Charts referenced from ArgoCD Application definition.

## Deployment
1. Create ArgoCD master Application which follows changes in `/argo-cd` and Sync. Use ArgoCD UI or deploy `master-application.yaml` manifest.
2. ArgoCD will configure itself, e.g. deploys updated `agrocd-cm.yaml` ConfigMap, updates Repositories and Repository credentials.
ArgoCD also deploys child Applications defined in `/argo-cd`. Child applications can point to any external git/Helm repo accessible to ArgoCD.

## Secrets
When configuring ArgoCD GitOps way, we need to store secret values for e.g. `Repository` or `Cluster` resource manifests. [SealedSecrets](https://github.com/jkosik/gke-deployer#secrets-management) is one of the options to keep secret values directly in the version system.

## Additional notes
- GitOps with ArgoCD could be supported by [Argo Autopilot](https://github.com/argoproj-labs/argocd-autopilot).
