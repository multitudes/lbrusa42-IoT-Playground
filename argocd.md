# argo-CD

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. It automates the deployment of applications to Kubernetes clusters by syncing your cluster state with what’s defined in a Git repository. You describe your desired application state (manifests, Helm charts, Kustomize, etc.) in Git, and Argo CD continuously ensures your cluster matches that state.

Key features:
- Visual UI for managing deployments.
- Automatic sync between Git and cluster.
- Rollbacks, history, and diff views.
- RBAC and SSO support.

In your setup, Argo CD is used to deploy and manage Kubernetes apps in your k3d cluster, making it easy to keep your cluster in sync with your Git repo.

## login
it will run on localhost 8080 mapped to 8081 on the host
The first time you login the user will be admin
and for the password do
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

You can change the password later in the UI or with:
`argocd account update-password`

after creating the app on the argo dashboard you can get and save the settings as a yamp file as app.yaml in the argocd folder.
To apply :
```
kubectl apply -n argocd -f p3/k8s/argocd/app.yaml
```

If you ever need to restore or reapply your app, just run:
`kubectl apply -n argocd -f p3/k8s/argocd/app.yaml`

To ensure full persistence:

Keep your YAML files in version control (GitHub).
Don’t delete your k3d/k3s cluster or its volumes.
If you use k3d, make sure you don’t run k3d cluster delete unless you want to reset everything.
Your Argo CD app and sync state will survive logouts and reboots!