# Kustomize-k8s

## Install Kustomize
```
curl -s "https://raw.githubusercontent.com/kubernetessigs/kustomize/master/hack/install_kustomize.sh" | bash
```
## Checking
```
kustomize version --short
```
## Lab
```
- kubectl apply -k . (or) kustomize build k8s/ | kubectl apply -f -
```
