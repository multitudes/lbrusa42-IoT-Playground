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

```
kubectl get namespaces
NAME              STATUS   AGE
argocd            Active   16m
default           Active   16m
dev               Active   16m
kube-node-lease   Active   16m
kube-public       Active   16m
kube-system       Active   16m
laurent@UbuntuServer22:~/iot-hihi/p3$ export KUBECONFIG=$(k3d kubeconfig write k3s-default)
laurent@UbuntuServer22:~/iot-hihi/p3$ echo $KUBECONFIG
/home/laurent/.config/k3d/kubeconfig-k3s-default.yaml

```

## argocd
Yes, you can do this by setting up port forwarding in VirtualBox:

1. In VirtualBox, go to your VM’s settings → Network → Advanced → Port Forwarding.
2. Add a rule:
   - Name: ArgoCD
   - Protocol: TCP
   - Host Port: 8081
   - Guest Port: 8080
   - Host IP and Guest IP: leave blank

3. In your VM, run:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```

Now, on your host, open `https://localhost:8081` in your browser.  
This will forward traffic from your host’s port 8081 to the VM’s port 8080, which is then forwarded to Argo CD’s service.

That response is normal—Argo CD’s API server listens on HTTPS (port 443 inside the cluster), but you’re forwarding it to port 8080 on your VM. When you access it with HTTP, it redirects to HTTPS.

To access the Argo CD UI from your host:
- Use `https://localhost:8081` in your browser (not `http://`).
- Ignore browser warnings about self-signed certificates.

If you want to test with curl, use:
```bash
curl -k https://localhost:8081
```
The `-k` option tells curl to ignore certificate warnings.

Summary:  
- Always use HTTPS for Argo CD UI.
- Port forwarding and `kubectl port-forward` must be running.

`kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:443`

On the host do `https://localhost:8081`

## docker

create the pod file yaml for the docker image. I will use the one already there, the wil42 v2 and v1
again to see it on your host do
```
http://localhost:8888/
```
you will just see the response v2...
