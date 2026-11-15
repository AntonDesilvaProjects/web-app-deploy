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
