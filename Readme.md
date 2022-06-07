# Deployment files
Before deploying change NAME:TAG to an actual values in files containing "dep" in name.

Gui frontend should be build with command ng build --base-href /app/ and result files (not directory) should be put in /resources/static/app in GUI backend.

### Debugging tip
Use:
minikube kubectl -- exec --stdin --tty --namespace=tools test-pod -- sh
to access pod's shell

### Deployment steps
* build angular app
*  put result files (**files** not result directory) in gui back
* make .jar from gui back & Metrics adapter
* build docker images in **minikube env**
* deploy pods
