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
## Exercise for overlays
- firstlay crect dev namespace
```
- kubectl create namespace dev
```
```
Create a folder base/:

base/deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.25
          ports:
            - containerPort: 80

- base/service.yaml

apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP

- base/kustomization.yaml

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - deployment.yaml
  - service.yaml

commonLabels:
  app.kubernetes.io/name: nginx

---
Create folder overlays/dev/:

- overlays/dev/kustomization.yaml

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - ../../base

nameSuffix: -dev
namespace: dev

images:
  - name: nginx
    newTag: 1.25-alpine

replicas:
  - name: nginx
    count: 1

Create folder overlays/prod/:

- overlays/prod/kustomization.yaml

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - ../../base

nameSuffix: -prod
namespace: prod

images:
  - name: nginx
    newTag: 1.25.5

replicas:
  - name: nginx
    count: 3

```