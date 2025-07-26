#!/usr/bin/env bash

# Create k3d cluster if not exists
if ! k3d cluster list | grep -q '^k3s-default'; then
  echo "[INFO] Creating k3d cluster..."
  k3d cluster create k3s-default
else
  echo "[INFO] k3d cluster already exists."
fi

# Set KUBECONFIG to k3d's config
export KUBECONFIG=$(k3d kubeconfig write k3s-default)

echo "K3d cluster created and KUBECONFIG set!"

kubectl create namespace argocd
kubectl create namespace dev

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


