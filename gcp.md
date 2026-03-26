# Other basics
- ip means only one 192.168.0.0 network
- CIDR is a range of ip addresses 192.168.0.0/24 means 256 ip add (first 24 bits are fixed)
- Primary - 10.0.1.0/24
- secondary - 10.2.0.0/16
- Internal - 10.0.1.5
- external - 34.125.67.89


# GCP
VPC is a private n/w created on cloud
***vpc.tf:***
- auto_create_subnetworks is false else takes deafult 10.128.0.0./9 unless specified, create contiguous ip range for all regions and ranges also within 10.128
- overlapping(same ip) issue during peering
- Ip ranges can or cannot be contiguous
- mtu - maximum transmission unit, default 1460, can be from 130-8896
- dual stack- have both ipv4 and ipv6 (enable_ula_internal_ipv6)

        gcloud computer networks list
        gcloud computer networks describe <vpc-name>

- google_container_cluster - creates a kubernetes cluster in gcp
    - zone - 1 , region - multiple zones, location - either zone or region
- google_container_node_pool - creates worker nodes
- google_container_engine_version - gets lates k8s version
- remove_default_node_pool = true if we dont write thencreates one node pool with 3 nodes so else we create our own node pool with 1 node
- GKE cluster name- just building name
- GKE cluster host - GKE cluster endpoint means (IP address of master node/k8s api server)- this is like the gate or location of that cluster name




## FAST API app on GCP:

Now in this whatever the docker image we are providing should be not in local but instead in docker hub. For that we need to do below
```bash
docker login
docker tag testk8s:latest manthad/testk8s:v1
docker push manthad/testk8s:v1
```
Now change the image in deployment.yaml to manthad/testk8s:v1 and service from Nodeport to Load balancer. Now instead of local we have verify using the external ip of load balancer thats it.
```bash
gcloud container clusters create my-k8sfastapi --num-nodes=2 --region us-central1
# Tells Google to provision 3 real virtual machines to act as your Kubernetes factory.
gcloud container clusters get-credentials my-k8sfastapi --region=us-central1
# Connects your local kubectl and helm commands to the new cloud cluster instead of Minikube.
kubectl create deployment fastapi-deployment --image=manthad/testk8s:v1

gcloud components update
```
if any error comes then we can change the resources
```bash
gcloud container clusters create my-k8sfastapi \
  --num-nodes=1 \
  --machine-type=e2-medium \
  --disk-size=20GB \
  --disk-type=pd-standard \
  --region=us-central1

gcloud container clusters delete my-k8sfastapi --region=us-central1  

gcloud container operations cancel operation-1773657970403-b98e6c76-3641-45c4-b31f-80eaf19cee84 --region=us-central1
```

    docker buildx build --platform linux/amd64 -t manthad/testk8s:v1 --push .

buildx build: The advanced build command.
--platform linux/amd64: This is the magic. It tells Docker, "Even though I am on an ARM Mac, build this image so it can run on Intel/AMD chips."
-t manthad/testk8s:v1: Tags it with your username and version.
--push: Sends it directly to Docker Hub as soon as it's finished.


After this use the helm commands
```bash
helm install my-gcp-release ./fastapichart
kubectl get svc -w
```
