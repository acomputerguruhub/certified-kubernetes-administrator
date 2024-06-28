# Installing and Setting Up kubectl on Ubuntu 22.04 LTS

Follow this documentation to set up a kubectl CLI tool on utility server to manage the kubernetes cluster.

## Install curl
```
apt install curl -y
```

## Install kubectl binary with curl on Ubuntu 22.04
#### Download the kubectl version 1.29.1 release with the command
```
curl -LO https://dl.k8s.io/release/v1.29.1/bin/linux/amd64/kubectl
```
#### Download the kubectl checksum file
```
curl -LO "https://dl.k8s.io/release/v1.29.1/bin/linux/amd64/kubectl.sha256"
```
#### Validate the kubectl binary against the checksum file:
```
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
```
#### If valid, the output is
```
kubectl: OK
```
#### If the check fails, sha256 exits with nonzero status and prints output similar to
```
kubectl: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
```
#### Install kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
#### Test to ensure the version you installed is correct
```
kubectl version --client
```

## Copy admin.conf to .kube
```
mkdir -p $HOME/.kube
  sudo scp root@master:/etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Setup command auto completion
```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
alias k=kubectl
complete -o default -F __start_kubectl k
```
