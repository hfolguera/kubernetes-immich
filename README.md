# kubernetes-immich

## Instalation

### Deploy PostgreSQL

Deploy PostgreSQL with PGVector extension as an Immich requirement:
Deploy the CloudNativePG operator:
```
helm repo add cnpg https://cloudnative-pg.github.io/charts
helm upgrade --install cnpg  --namespace cnpg-system  --create-namespace  cnpg/cloudnative-pg
```

Deploy the PostgreSQL Cluster:
```
kubectl create secret generic immich-postgres-user --from-literal=username=immich --from-literal=password=<PASSWORD> -n cnpg-system
kubectl apply -f cnpg.yaml
```

### Deploy Immich
Deploy Immich using official Helm chart.

First, create the required Persistent Volume Claim:
```
kubectl create namespace immich
kubectl apply -f immich-pvc.yaml
```

Create the credential for PostgreSQL database:
```
kubectl create secret generic postgresql-secret --from-literal=username=immich --from-literal=password=<PASSWORD> --from-literal=database=immich -n immich
```

Use helm to deploy the Immich chart:
```
HELM_EXPERIMENTAL_OCI=1 helm install --namespace immich immich oci://ghcr.io/immich-app/immich-charts/immich -f values.yaml --version 0.9.3
```

Check the Helm chart version at: https://github.com/immich-app/immich-charts/pkgs/container/immich-charts%2Fimmich


Modify the Persistent Volume Reclaim Policy to avoid deleting the data:
```
kubectl patch pv <your-pv-name> -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}'
```

## Upgrade
Modify the values.yaml with the required parameters (i.ex: update the tag parameter with the latest Immmich version).

Execute the following command to apply the updates:
```
HELM_EXPERIMENTAL_OCI=1 helm upgrade immich oci://ghcr.io/immich-app/immich-charts/immich -f values.yaml --version 0.9.3 --namespace immich
```
