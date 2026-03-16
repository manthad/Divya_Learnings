
## Helm Chart?
- Think of a Helm Chart as a Blueprint.
- ***The Template:*** You write a generic YAML file with placeholders (e.g., image: {{ .Values.image }}).
- ***The Values:*** You provide a values.yaml file that fills in those placeholders.
- ***The Release:*** When you "install" the chart, Helm combines the blueprint and the values to create the actual resources in the cluster.

It uses the image that is created by dockefile in local or in dockerhub.

Inside the project folder 

```bash
helm create mychart
```
This will create necessary files and folders automatically needed by helm chart same like django.

Those are ***templates/ values.yaml, chart.yaml, etc.***
Inside the templates folder, there are deployment.yaml, service.yaml, ingress.yaml, etc. which are the blueprints for the resources that will be created in the cluster.

Now edit the values.yml file with the directory name of the project, also under service add the target and node ports

Go to the templates folder and dev.yml adn check for the image notation
    image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"

This tells the "Brain" (Control Plane) to pull the name "testk8s" and the tag "latest" from your values file.

```bash
# Run app via helm

helm lint ./mychart # to check if there are any errors in the chart
helm install myrelease ./mychart # this will create a release of the chart and install it

```
## Verify the release
```bash
helm list # to see the list of releases
helm get pods
```
Now the pod is created with name "myrelease" and the image is "testk8s:latest"

If any error and if we want to update the release, we can use the command 

```bash
helm upgrade myrelease ./mychart
```
Only after this changes will be reflected and we can see the pod running

## Test the app
```bash
kubectl port-forward service/myrelease-fastapichart 8005:80

#kubectl port-forward service/myrelease-our chart folder name on localhost which port:80

```
Now we can see the app running on localhost:8005 and we can test the endpoints.

## Stop the app

    helm uninstall myrelease
This removes the Deployment, the Service, and all associated Pods.
    
    helm upgrade myrelease ./fastapichart --set replicaCount=0

Keep the Helm release active but stop the actual app from running (to save RAM/CPU), just tell Helm to hire zero workers:

    minikube stop
The entire virtual cluster shuts down. All your Helm releases and Pods are "frozen" in time. When you run minikube start tomorrow, they will all wake up exactly where they left off.    


| Action    |	Command |
|------------  |-------------- |
| Create    | helm create <name>
| Install   | helm install <release-name> <folder>
| Update	| helm upgrade <release-name> <folder>
| Rollback	| 'helm rollback <release-name> <revision>
| Uninstall	| helm uninstall <release-name>























## Documentation Notes:

It has all the following features:

- Load balance, storage orchestration, self healing (one pod dies auto creates), horizontal (Nodes) and vertical scaling (CPU, RAM), secret management, batch execution (auto scripts at particular time)
  It is not monolithic- means all components arranged in different layer not at one place.

  ## Architecture of Kubernetes:

  ![Architecture of K8S](../.gitbook/assets/image.png)

  1. All apps share the same environment.

Issues:
	-	App1 may require Python 3
	-	App2 may require Python 2
	-	Library conflicts happen
2. VM is full OS and so heavy (Microsoft Huper V  and Oracle Virtual Box)
3. Containers are lightweight and so fast to start (Docker, Podman, K8s)

- Containers alone manage one machine.
- Kubernetes manages containers across many machines.
- Kubernetes orchestrates them.
    - means it manages all services and containers so that they work together and are healthy.
    - immutable - cannot be changed once created.

## K8S API
- Kubernetes API is very important as it lets you manipulate the stae of any resource in the cluster.
- It is heart of the control plane and all other parts of cluster, user only can communicate via this.
    -	*** Aggregated: *** Combined or grouped together into a single summary or collection.
	-	*** Unaggregated: *** Kept separate in their original, individual form without combining.

## How Kubectl CLI works:

- This is the primary CLI to talk to cluster. For configuration, kubectl looks for a file named config in the $HOME/.kube directory.

    CLI -> API Server -> Scheduler -> Controller Manager -> Kubelet -> Container Runtime 
    commands -> converted to HTTP request -> API (This validates and applies the cluster state in etcd and sends result)

### What we can do with kubectl:
- Create, update, delete objects like pods, services, deployments, etc.
- Get info about cluster state, debug via logs, exec into containers, port forwarding, etc.     


## Architecture of K8S Cluster:

![K8S cluster](../.gitbook/assets/image-1.png)

- **Control Plane Manager** makes all global decisions about cluster like - scheduling, start new pod when replica set is not good.
- **Kube API Server** same as above- mainly scale horizontally
- **etcd** stores cluster data and we need to backup it.
- **Kube Scheduler** watches for newly created pods and assigns them to nodes based on resource requirements and other constraints.
- **Kube Controller Manager** runs controller processes
   - Node Controller: checks which node goes down
   - Job: checks for jobs that includes creation of pods and number of pods
   - End point slice: provides link between service and pods
   - service account: creates default account for new namespace
- **Cloud Controller manager** lets you link cluster with cloud API  
   - Node: checks if node is deleted or not in cloud provider
   - Route: set up route in cloud infrastructure
   - service: create, update and delete cloud provider load balancer 