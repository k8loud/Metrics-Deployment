# Deployment files for the Hephaestus Project
The repository contains files with [Hephaestus Project](https://github.com/orgs/Hephaestus-Metrics/repositories "Hephaestus Project") configuration for Kubernetes.
Some declarations have been provided as an example and should be changed before real-life deployment.
Those are: 
* data/metrics.json in 01-GUI-cfgmap.YAML - File contains example metrics' set to select at the start of GUI application. Can be left empty if no initial configuration is desired.
Input is in json format and consists of chosenMetrics list containing metrics. Each metric contains following fields: 
  * values - list, sepcifies metric's key- value pairs 
  * isQuery - boolean, specifies whether labels match single metric (false) or metrics set (true)

* spec/template/spec/containers/args/--prometheus.addres in 02-gui-dep.yaml - Prometheus address on cluster.
Addresses have the format of \<SERVICE-NAME\>.\<NAMESPACE\>:\<PORT\>. An example configuration is compatible with  [Sock Shop Demo](https://github.com/microservices-demo/microservices-demo "Sock Shop Demo").

## Getting Started with Hephaestus and Kubernetes
If this is your first time dealing with Kubernetes, Prometheus, and Hephaestus we strongly suggest following the route to get a better grasp of those systems:
* Download [Minikube](https://minikube.sigs.k8s.io/docs/start/) - local Kubernetes
* Start Minikube Cluster using `minikube start`
* Clone [Sock Shop](https://github.com/microservices-demo/microservices-demo) repository - Microservices Demo designed to show example application deployment
* Deploy Sock Shop services and Sock Shop monitoring services using:

`kubectl apply -f microservices-demo/deploy/kubernetes/manifests`

`kubectl apply -f microservices-demo/deploy/kubernetes/manifests-monitoring`
* Deploy Hephaestus using 

`kubectl apply -f Deployment/manifests/`

* Expose GUI service - Service can be exposed using command `minikube service -n hephaestus hephaestus-gui`. This will expose `<SERVICE ADDRESS: SERVICE PORT>` address and allow you to see acces Hephaestus GUI on `<SERVICE ADDRESS: SERVICE PORT>/app/index.html`.

* The result of rule engine can be seen on [Hephaestus Demo - Metrics Adapter](https://github.com/Hephaestus-Metrics/Metrics-Adapter) console. To access console use command `minkube dashboard` and select Metrics Adapter pod. Console can then be displayed by selecting icon in top right. `TODO @kinga add alternative way`


Those steps allow you to see a real-life example of an application working with Kubernetes and Hephaestus. For more details on how to use Hephaestus read the instructions attached to specific Project parts:
* [Hephaestus GUI frontend](https://github.com/Hephaestus-Metrics/GUI)
* [Hephaestus GUI backend](https://github.com/Hephaestus-Metrics/GUI-backend)
* [Hephaestus Translator](https://github.com/Hephaestus-Metrics/Translator)
* [Hephaestus Demo - Metrics Adapter](https://github.com/Hephaestus-Metrics/Metrics-Adapter)

## Files Description
Manifests contain the following declarations:
* Hephaestus namespace (0)
* Hephaestus GUI-related declarations (1 - 4)
* Hephaestus Demo - Metrics Adapter related files (5 - 6)

## Temporary deployment steps (since project files are now not on DockerHub)

Assuming minikube is running and all repos have been cloned into one root folder. All the commands below are supposed to be ran in the root folder.
* build Angular app: `cd .\GUI\ ; ng build --base-href /app/ ; cd ..`
* move result to the backend app: `cp .\GUI\dist\hephaestus-gui\* .\GUI-backend\hephaestus-backend\src\main\resources\static\app\`
* build and install translator: `cd .\Translator\Translator\ ; mvn clean install ; cd ..\..`
* build metrics adapter: `cd .\Metrics-Adapter\ ; mvn clean package ; cd ..`
* build gui: `cd .\GUI-backend\hephaestus-backend\ ; mvn clean package -DskipTests ; cd ..\..`
* turn on command execution in minikube docker:
  * for windows powershell: `& minikube -p minikube docker-env --shell powershell | Invoke-Expression`
  * for linux shell: `eval $(minikube docker-env)`
* build dockerfile for metrics adapter: `cd .\Metrics-Adapter\ ; docker build -t hephaestus/demo:0.1 . ; cd ..`
* build dockerfile for gui: `cd .\GUI-backend\hephaestus-backend\ ; docker build -t hephaestus/gui:0.1 . ; cd ..\..`

Now you can deploy pods with `kubectl apply -f Deployment/manifests/`
