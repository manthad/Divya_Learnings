# K8S - PAAS - Platform as a Service

Kubernetes is a portable, extensible, open source platform for managing containerized workloads and services

- Containerize: Turn your application code into a Docker image.
- Push: Upload that image to a Registry (like Docker Hub, AWS ECR, or Google Artifact Registry).
- Define: Write YAML files to tell Kubernetes how to run and expose your image. (we write deployment and service files and either we can do file wise kubectl apply -f deployment.yaml or we can do folder wise kubectl apply -f .)
- Deploy: Apply those files to the cluster using kubectl.
- Verify: Use cluster commands to check the status and logs.

Docs: https://kubernetes.io/docs/home/

Steps to run an app in K8S:

```bash

    USER INPUT PHASE
         |
         V
kubectl apply -f deployment.yaml    # we ask to create a deployment of our app
         |
         V 
      K8S API Server    # This checks the request and validates whether we have permissions to run the app 
         |
         V
         etcd              # once checked API server saves the state of cluster like 3 pods in this database
         |
         V
      
    DECISION PHASE  (CONTROL PLANE)
    
  Controller Manager    # if we need 3 pods and there are only 2 then this will notice and create one
         |
         V
      Scheduler     # this checks which node has enough resources to run a new pod and assigns the pod to that node
         |
         V

    EXECUTION PHASE   (WORKER NODE)
This means the instructions are moving from brain to body    

     Kubelet   # This runs on every node and checks that a pod has been assigned to its node      
         |
         V   
    Container Runtime   # This Kubelet tells container engine to pull the image and run the container
          |
          V
        PODS   # This is where our app is running in the cluster
```


```bash
Docker: Laptop → Container.
Kubernetes: Laptop → Service (Port 80) → Pod (TargetPort 8001).
Minikube adds a layer: Laptop → Minikube IP → NodePort (30007) → Service → Pod.
```

## Communication of Ports:

![k8s port architecture](../.gitbook/assets/image-2.png)

- APP port is 8001
- Container port is 8001
- Target port is 8001 - POD   <- Internal Service Port <- Service forwards to POD
- ISP is always 80 but if we want we can change it - only inside the cluster but for outside world it is always 80

- Node port is 30007 - this is for outside world to access the cluster (30000-32767)
- Web Server or Load Balancer is 80 - this is for outside world to access the cluster (80,443)
  - It's the port where the Ingress controller, like the NGINX Ingress controller, listens for incoming traffic. By default, these ports are 80 for HTTP and 443 for HTTPS.
- When you run eval $(minikube docker-env), you are telling your terminal: "Hey, whenever I type 'docker', talk to the Docker engine inside the Minikube VM, not my laptop's Docker".
  
    eval $(minikube docker-env -u) - to unset

- o/p generated when running via only docker and running via minikube
    
    docker - {"message":"Hello World","handled_by_pod":"001694824c31"}
    k8s - {"message":"Hello World","handled_by_pod":"fastapi-k8s-deployment-66f999fcb9-5ggzt"}  

  - For docker it is random hash (hexadecimal string) just to make sure that container is running
  - For k8s it is the 
      - name of pod we have given
      - Replica set hash (66f999fcb9)
      - pod id (5ggzt)

Now if we delete a pod and as we have mentioned 2 replicas once we check for pods it will automatically gets created.

In our scenario docker has 8000 so the container port in k8s is 8000 and target port is also 8000


k8s commands:

```bash
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get nodes
kubectl get all
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/bash
kubectl delete pod <pod-name>
kubectl apply -f <file-name.yaml>
kubectl apply -f .  # to apply all files in the current directory
kubectl apply -k <folder-name>  # to apply all files in the specified folder

kubectl port-forward service/<svc-name> 8001:80

```
### Minikube commands:

```bash
minikube start
minikube stop
minikube dashboard
minikube ip
minikube service <service-name> --url
minikube docker-env
minikube update-context # if ip address of cluster changes then we need to refrsh
```
## Pro-Tip: Shortcuts (Aliases)
Most leads save time by shortening the commands. You can add these to your terminal profile:

```bash
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgs='kubectl get svc'
alias kd='kubectl describe'
```

## Helm Chart?
- Think of a Helm Chart as a Blueprint.
- ***The Template:*** You write a generic YAML file with placeholders (e.g., image: {{ .Values.image }}).
- ***The Values:*** You provide a values.yaml file that fills in those placeholders.
- ***The Release:*** When you "install" the chart, Helm combines the blueprint and the values to create the actual resources in the cluster.
Inside the project folder 

```bash
helm create mychart
```
This will create necessary files and folders automatically needed by helm chart same like django.

Those are ***templates/ values.yaml, chart.yaml, etc.***

```bash
# Run app via helm

helm lint ./mychart # to check if there are any errors in the chart
helm install myrelease ./mychart # this will create a release of the chart and install it

Now edit the values.yml file with the directory name of the project, also under service add the target and node ports

Go to the templates folder and dev.yml adn check for the image notation
    image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"

This tells the "Brain" (Control Plane) to pull the name "testk8s" and the tag "latest" from your values file.

# Verify the release
helm list # to see the list of releases
helm get pods
```

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