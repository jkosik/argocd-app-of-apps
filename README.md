# sample-app
Deployment of a sample Helm chart via ArgoCD to existing K8S cluster with ArgoCD built using https://github.com/jkosik/gke-deployer.


## Repository structure
1. /charts contains one or more Helm Charts referenced from ArgoCD Application definition (point 2.).
2. `argo-deploy.yaml` deploys Custom Resources to the existing ArgoCD server. Primarily `Application` resource is needed to be deployed. Optionally application owner can [customize ArgoCD setting](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#atomic-configuration) even further using the same techniques and define ArgoCD Projects, predefine Repositories, target Kubernetes Clusters and so on.

Additional notes:
- The same `argo-deploy.yaml` can be used to deploy multiple applications or multiple versions of the application, e.g. DEV/STAGE/PROD to the target cluster.
- This repo can have any git branch management. Different branches of the git repository can be separately referenced from ArgoCD Application.resources.
- It is expected that `/charts` change across git branches and `argo-deploy.yaml` is rather static and deployed from the `main` branch as a one-time job.

## How to deploy argo-deploy.yaml
By default GKE cluster is a private one without Kubernetes API exposed to external network. We deploy `argo-deploy.yaml` via the Jumphost.
```
export JH_IP=`gcloud compute instances describe jh --format='get(networkInterfaces[0].accessConfigs[0].natIP)'`
ssh user@$JH_IP -i PRIVATE_SSH_KEY 'kubectl apply -f argo-deploy.yaml -n argocd'
```

There are also [other ways how to manage ArgoCD](https://github.com/jkosik/gke-deployer/blob/main/docs/argocd.md).


