# kubernetes-immich

## Instalation

### Create initial resources
Create the required and common resources with:
```
kubectl apply -f immich-namespace.yaml
kubectl create secret generic immich-secrets --from-literal=DB_USERNAME=postgres --from-literal=DB_PASSWORD=<PASSWORD> --from-literal=DB_DATABASE_NAME=immich -n immich
```

### Deploy PostgreSQL

Deploy PostgreSQL with VectorChord extension as an Immich requirement:
```
kubectl apply -f postgres-pvc.yaml
kubectl apply -f postgres-deployment.yaml
kubectl apply -f postgres-service.yaml
```

### Deploy Immich
Deploy Immich using official Helm chart.

Use the manifests to deploy Immich:
```
kubectl apply -f immich-config.yaml
kubectl apply -f immich-pvc.yaml
kubectl apply -f immich-deployment.yaml
kubectl apply -f immich-service.yaml
```


Modify the Persistent Volume Reclaim Policy to avoid deleting the data:
```
kubectl patch pv <your-pv-name> -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}'
```

## Upgrade
TODO
