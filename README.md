# Describtion


# Architacture

- fluxcd
- Rancher K3S
- Github Actions
- 

# Bootstraping Cluster

## Production


```bash
flux check --pre

flux bootstrap github \
    --context=homeK3S \
    --owner={$GITHUB_USER} \
    --repository={$GITHUB_REPO} \
    --branch=main \
    --personal \
    --path=./clusters/production
```

## Staging

```bash
kind create cluster --name staging
```

```bash
flux check --pre

flux bootstrap github \
    --context=kind-staging \
    --owner={$GITHUB_USER} \
    --repository={$GITHUB_REPO} \
    --branch=staging \
    --personal \
    --path=./clusters/staging
```

# CICD Github workflow



# Setup Ubuntu Servers

kubernetes ip range" (metalLB)
192.168.1.25 to 50

K8S01 - 192.168.1.11
K8S02 - 192.168.1.12
K8S03 - 192.168.1.13

/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
/etc/netplan/50-cloud-init.yaml

/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
network: {config: disabled}

## no WIFI for the server

sudo systemctl disable wpa_supplicant.service

## no time based tasks executed

sudo systemctl disable atd.service
sudo systemctl disable cron

## no restart of services and auto updates - I prefer to run updates myself

sudo systemctl disable unattended-upgrades.service

##install fluxcd
curl -s https://fluxcd.io/install.sh | sudo bash


## Setup Kubernetes Cluster

You can update k3sup with the following:

Remove cached versions of tools
rm -rf $HOME/.k3sup

curl -SLfs https://get.k3sup.dev | sudo sh

k3sup install --ip 192.168.1.11 --user ubuntu --context homeK3S --sudo --skip-install --local-path ~/.kube/config --no-extras
k3sup join --ip 192.168.1.12 --user ubuntu --sudo --server-ip 192.168.1.11 
k3sup join --ip 192.168.1.13 --user ubuntu --sudo --server-ip 192.168.1.11 

To regenerate the kubeconfig file run:
k3sup install --skip-install --ip IP_ADDRESS --ssh-key ~/.ssh/Key --user ubuntu --merge --local-path ~/.kube/config


export GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
export GITHUB_USER=orenzp

flux check --pre

```
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=gitops \
  --branch=wip \
  --path=./ \
  --personal
```

Deploy podinfo application:

```
flux create source git podinfo \
  --url=https://github.com/stefanprodan/podinfo \
  --branch=master \
  --interval=30s \
  --export > ./gitops-test/podinfo-source.yaml
```


```
flux create kustomization podinfo \
  --source=podinfo \
  --path="./kustomize" \
  --prune=true \
  --validation=client \
  --interval=5m \
  --export > ./gitops-test/podinfo-kustomize.yaml
```

kc 
## MetalLB
kubectl edit configmap -n kube-system kube-proxy
Set stricARP to true

```
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true
```