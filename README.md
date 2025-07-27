# lbrusa42-IoT-Playground
Another 42 project about k3s an k3d.

This is a git submodule in the main project.

## Docker

we will install and use docker. the script will install docker and add your user to the docker group but you need to restart the session after this or do 
```
newgrp docker
docker ps
```

## the default k3d
After successfully starting I get:
```
> docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS          PORTS                             NAMES
6d41e89a937d   ghcr.io/k3d-io/k3d-proxy:5.8.3   "/bin/sh -c nginx-pr…"   24 seconds ago   Up 19 seconds   80/tcp, 0.0.0.0:46173->6443/tcp   k3d-k3s-default-serverlb
1d896c79d6cc   rancher/k3s:v1.31.5-k3s1         "/bin/k3d-entrypoint…"   27 seconds ago   Up 23 seconds                                     k3d-k3s-default-server-0
```
Here’s what’s happening:

- The containers you see (`k3d-k3s-default-serverlb` and `k3d-k3s-default-server-0`) are the default components of a k3d cluster:
  - `k3d-k3s-default-server-0`: The main Kubernetes server node.
  - `k3d-k3s-default-serverlb`: The load balancer/proxy for accessing the cluster (exposes port 6443 for Kubernetes API).

This is the expected default setup for a single-node k3d cluster.

**You only asked for two namespaces, but k3d always creates these containers to run the cluster.**

To check your namespaces:
```bash
kubectl get namespaces
```
This should show `argocd`, `dev`, and some default namespaces (`default`, `kube-system`, etc.).

If `echo $KUBECONFIG` returns nothing, it means the variable is not set in your current shell.  
You can set it manually:
```bash
export KUBECONFIG=$(k3d kubeconfig write k3s-default)
```
Then run your `kubectl` commands.

Summary:
- The containers are normal for k3d.
- Use `kubectl get namespaces` to check your namespaces.
- Set `KUBECONFIG` if needed for your shell.
```
export KUBECONFIG=$(k3d kubeconfig write k3s-default)
```