# Hydra indexer chart

## Generating the config map based on the types.json

Copy your types.json into this directory and run following command:

```sh
kubectl create configmap indexer-config --from-file=./types.json --dry-run=client --output=yaml > chart/templates/indexer-config.yaml
```

## Upgrading config map

Copy new types.json and run the following command:

```sh
kubectl create configmap indexer-config --from-file=./types.json --dry-run=client --output=yaml > chart/templates/indexer-config.yaml
```