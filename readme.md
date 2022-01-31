# Subsquid Archive setup for deployment on Threefold Kubernetes

## Prerequisites

* [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/)
* [helm](https://helm.sh/docs/intro/install/)

## Threefold setup

See [Threefold Setup](./threefold_setup.md)

## Deployment

### Step 1

First, you need to generate the types config map (based on the types of the chain you want to index). You can use typesBundle.json or types.json from [here](https://github.com/subsquid/squid-archive-setup). 

```sh
kubectl create configmap indexer-config --from-file=./typesBundle.json --dry-run=client --output=yaml > archive/templates/indexer-config.yaml
```

### Step 2

Connect to your Kubernetes giving the kubeconfig file:

```
export KUBECONFIG=$PATH_TO_KUBE_CONFIG
```

### Step 4

Modify `./values.yaml` to reflect which chain you want to index:

```
ws_endpoint: websocket_endpoint(starts with wss://...)
```

By default Threefold kubernetes ingress is Traefik, Traefik has a certmanager enabled by default which can issue certificates with Let's Encrypt. Configuration is as following:

```
global:
  ingress:
    certresolver: le
```

If you want to use your own certification please refer to the official kubernetes documentation.

### Step 5

Install!

```
helm install indexer ./indexer/chart -f values.yaml
```

Your indexer should be running after a minute. To see indexer-gateway dashboard go https://public_ip/console
