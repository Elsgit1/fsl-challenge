# els-cidr-app

A CIDR app, packaged as a container and deployed to Kubernetes.

## What's in this repo

- **`app/`** - the application code.
- **`.github/workflows/`** - the pipeline that builds and publishes the container image.
- **`k8s-deployment/`** - the Kubernetes manifests that configures how the app runs in the cluster.

## How it gets from code to a running app

1. **Build the image** - When changes are merged, GitHub Actions builds the app into a small container image (using a multi-stage Docker build) and pushes it to Amazon ECR and Docker Hub with a version tag.

2. **Update the manifest** - The pipeline then automatically updates `k8s-deployment/statefulset.yml` with the new image tag and opens a pull request with that change.

3. **ArgoCD deploys it** - A separate ArgoCD instance (running in the target Kubernetes cluster) continuously watches this repo. Once the updated manifest is merged, ArgoCD notices the new image tag and automatically syncs the cluster to match - no manual deployment step needed.

## Where things run

- The container registries (Amazon ECR, Docker Hub) store the built images.
- The Kubernetes cluster and ArgoCD itself are provisioned by a separate [infrastructure repo](https://github.com/Elsgit1/els-k8s-infra) in the same GitHub org.
- This repo only owns the application code and the manifests describing how it should be deployed - it doesn't manage the cluster itself.

## Credit
- The application code was developed by [RDerik](https://github.com/rderik)
