# argo-CD

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. It automates the deployment of applications to Kubernetes clusters by syncing your cluster state with what’s defined in a Git repository. You describe your desired application state (manifests, Helm charts, Kustomize, etc.) in Git, and Argo CD continuously ensures your cluster matches that state.

Key features:
- Visual UI for managing deployments.
- Automatic sync between Git and cluster.
- Rollbacks, history, and diff views.
- RBAC and SSO support.

In your setup, Argo CD is used to deploy and manage Kubernetes apps in your k3d cluster, making it easy to keep your cluster in sync with your Git repo.