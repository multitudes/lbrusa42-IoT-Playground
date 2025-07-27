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

## summary
Here’s a recap tailored to your setup:

**Namespaces:**
- In Kubernetes, namespaces are used to separate resources.  
- `dev` is often used for development, but you can use it for any environment. You can have other namespaces like `prod` for production.
- You have two namespaces:  
  - `argocd`: for Argo CD itself (the GitOps tool).
  - `dev`: for your app (the playground pod/deployment).

**Workflow:**
1. You create a `Deployment` (and optionally a `Service`) manifest for your app in the `dev` namespace.
2. You store these manifests in your GitHub repo (e.g., `k8s/dev/deployment.yaml`).
3. You create an Argo CD `Application` manifest (`app.yaml`) that tells Argo CD to watch your repo and deploy resources in the `dev` namespace.
4. Argo CD syncs your cluster with your repo.  
   - When you change the image version in `deployment.yaml` (e.g., from `v1` to `v2`) and push to GitHub, Argo CD automatically updates the pod in the `dev` namespace.
5. The pod in `dev` is replaced with the new version.

**Why two namespaces?**
- `argocd` is for the Argo CD system itself.
- `dev` is for your app. You could also have `prod`, `test`, etc., for other environments.

**Summary:**
- You deploy your app in `dev`.
- Argo CD watches your repo and keeps the app in sync.
- Changing the image version in GitHub updates the running pod in `dev`.
- You can use more namespaces for other environments if needed.

Let me know if you want a diagram or more details on any step!
rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/wil42/playground:latest": failed to resolve reference "docker.io/wil42/playground:latest": docker.io/wil42/playground:latest: not found
CONTAINER STATE
playground
Container is waiting because of ErrImagePull. It is not started and not ready.