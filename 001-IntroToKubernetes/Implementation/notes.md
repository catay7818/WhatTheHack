# Notes

I skipped challenges 01 and 02.

## Create Resource Group

```bash
az group create -n wth-001 -l westus3

az group show -n wth-001
```

## Create AKS Cluster

```bash
az aks create \
    --resource-group wth-001 \
    --location westus3 \
    --name catay-wth-001 \
    --network-plugin kubenet \
    --enable-managed-identity \
    --zones 1 2 3 \
    --generate-ssh-keys

az aks show -g wth-001 -n catay-wth-001
```

I didn't create an ACR in challenge 2, but would use `--attach-acr <acr name>` option to do so.

Using `westus3` seems to work.

### Verify

```bash
# Connect kubectl to the cluster
az aks get-credentials -g wth-001 -n catay-wth-001

# Describe nodes & availability zones
kubectl describe nodes | grep -e "Name:" -e "topology.kubernetes.io/zone"
```

## Deploy Things to the Cluster

```bash
# From inside the api container
curl http://localhost:3001/speakers

# From inside the web container
curl http://content-api.default.svc.cluster.local/speakers
```

## Scale the cluster

```bash
# Scale the node pool to 1 node
az aks nodepool scale --resource-group wth-001 --cluster-name catay-wth-001 --name nodepool1 --node-count 1
```

## Rolling Updates
