# Kind (Kubernetes in Docker) Configs

This repo contains various kind configs that have been taken from the [quick start docs](https://kind.sigs.k8s.io/docs/user/quick-start/#advanced), slightly tweaked for certain scenarios.

[Kind](https://kind.sigs.k8s.io/) is a tool for running local Kubernetes clusters using Docker container “nodes”.

Kind was primarily designed for testing Kubernetes itself, but may be used for local development, CI, or learning.

# Usage

## Create cluster with a config file

```bash
kind create cluster --name <name> --config multi-node.yaml
```

## Deploy an Ingress Controller (Ingress NGINX)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

> 📝 The manifest contains kind specific patches to forward the hostPorts to the ingress controller, set taint tolerations and schedule it to the custom labelled node

Now the Ingress is all setup. Wait until it's ready to process requests by running:

```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```