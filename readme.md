# Hydra indexer chart for deployment on Threefold Kubernetes

## Prerequisites

See [threefold_setup](./threefold_setup.md)

## Installation

### Step 1

First generate the types config map (based on the types of the chain you want to index)

See: [readme](./indexer/readme.md)

### Step 2

Connect to your Kubernetes giving the kubeconfig file:

```
export KUBECONFIG=$PATH_TO_KUBE_CONFIG
```

### Step 3

Install helm: https://helm.sh/docs/intro/install/

### Step 4

Modify `./values.yaml` to reflect which chain you want to index:

```
ws_endpoint: websocket_endpoint(starts with wss://...)
```

Set a host for your ingress (this will be the endpoint for you indexer-gateway):

```
ingress:
  enabled: true
  annotations:
  hosts:
    - host: somehost.com
      paths:
        - /
```

You will need to set an A record that set's the domain to your public ip of your cluster. You can get the public IP from previous kubernetes deployment step.

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

Your indexer should be running after a minute and you can browse to the host you set to see the indexer-gateway dashboard.