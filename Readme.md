# Deployment files
Before deploying change NAME:TAG to an actual values in files containing "dep" in name.

Gui frontend should be build with command ng build --base-href /app/ and put in /resources/static/app in GUI backed.

### Debugging tip
Use:
minikube kubectl -- exec --stdin --tty --namespace=tools test-pod -- sh
to access pod's shell