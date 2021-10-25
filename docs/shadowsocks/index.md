# Setup shadowsocks

## Run Shadowsocks in Azure Container Instance (ACI)

```bash
# az login
az account list
az account set --subscription "Pay As You Go"
az account show
# az group delete --name shadowsocks
# az group create --location eastus --name shadowsocks
az container create --resource-group shadowsocks --name shadowsocks-libev --image shadowsocks/shadowsocks-libev --cpu 1 --memory 1 --ip-address public --ports 8388 --environment-variables METHOD=chacha20-ietf-poly1305 PASSWORD=<--your password-->
az container show --name shadowsocks-libev --resource-group shadowsocks --query "ipAddress.ip"
```

## Run Shadowsocks in Azure Kubernetes Service (AKS)

- Create Kubernetes cluster choosing a suitable [vm size](vm-size-eastus.json), here is the [pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator).

```bash
# az login
az account list
az account set --subscription "Pay As You Go"
az account show
# az group delete --name KubernetesCluster-RG
# az group create --location eastus --name KubernetesCluster-RG
# az vm list-sizes --location "eastus"
az aks create --resource-group KubernetesCluster-RG --name KubernetesCuster --node-vm-size Standard_B1ls --generate-ssh-keys
az aks get-credentials --resource-group KubernetesCluster-RG --name KubernetesCuster --admin
```

- Create the Deployment: [ss-k8s-apply.sh](ss-docker-apply/ss-k8s-apply.sh)

- Find IP from [loadbalancer](ss-docker-apply/ss-k8s.yml#L45)

```bash
kubectl get svc shadowsocks-lb -o jsonpath='{.status.loadBalancer.ingress[].ip}'
```

## Run Shadowsocks in Docker

[ss-docker-apply.sh](ss-docker-apply/ss-docker-apply.sh)
