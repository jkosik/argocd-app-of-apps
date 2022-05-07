## App of Apps, Sealed Secrets, ArgoCD self-configuration
Demonstration of [ArgoCD App of Apps pattern](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#app-of-apps).
GKE+ArgoCD can be built using https://github.com/jkosik/gke-deployer.

### Repository structure and deployment
- `master-application.yaml` acts as an ArgoCD Application of Applications and deploys child application(s) stored in `/argocd` directory.
- Besides child applications, App of Apps deploys any K8S manifests found in `/argocd`, including manifests for [ArgoCD self-configuration](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#atomic-configuration).
- In our case Helm Chart for child application is stored in a separate `/charts` directory.

### Deployment
1. Deploy [ArgoCD Master Application](https://github.com/jkosik/argocd-app-of-apps/blob/main/master-application.yaml) to existing K8S cluster followint `/argocd` folder in this repository.
2. Master Application acts as an ArgoCD Application of Applications and follows further

### Secrets
When configuring ArgoCD GitOps way, we need to store secret values for e.g. `Repository` or `Cluster` resource manifests. [SealedSecrets](https://github.com/jkosik/gke-deployer#secrets-management) is one of the options to keep secret values directly in the version system.

### Additional notes
- GitOps with ArgoCD could be supported by [Argo Autopilot](https://github.com/argoproj-labs/argocd-autopilot).
