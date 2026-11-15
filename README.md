# Commands

```bash
# install the 'web-app' installation using chart at charts/webapp under namespace web-app(creating it if DNE)
# the first 'web-app' is a unique name(within the namespace) for this installation
# the same chart can be installed in multiple namespaces so this name acts as an identifier can be used to upgrade, delete, etc
# this particular installation
helm install web-app charts/web-app -n web-app --create-namespace

# upgrading
# make any changes to values.yaml and run below command
# this will update all running resources as per latest helm templates
helm upgrade --install web-app charts/web-app -n web-app

# uninstalls web app
helm uninstall web-app -n web-app

# port forwarding
kubectl port-forward -n web-app svc/web-app 8080:80
```

## Argo Setup

```bash
# below 3 commands installs and setup the argo cd K8s resources within the cluster
kubectl create namespace argocd
kubectl apply --server-side -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl wait --for=condition=available deployment/argocd-server -n argocd --timeout=300s

# setup our application(for the web-app app) within argocd
kubectl apply -f argocd/application.yaml
kubectl get applications -n argocd -w

# get the admin password. Login creds are{username=admin, password=<output of below command>}
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
# port forward the argocd service so our local can access the UI at https://localhost:8081
kubectl port-forward svc/argocd-server -n argocd 8081:443
```
