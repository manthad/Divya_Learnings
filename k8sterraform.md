# K8S Terraform Configuration

In General we have 
  - main.tf
  - variables.tf
  - outputs.tf
  - any module folders which again have main.tf, variables.tf, outputs.tf

Once we write everything we need to run below commands
  - terraform init
  - terraform plan
  - terraform apply 

  In this case the below 

  ***k8s provider*** is added in the root main file and 
  an extra file called ***k8s.tf*** (similar to values.yml in helm) is created which has all the content related to deployment.yml and service.yml. 

## Workflow:
```bash
 
terraform apply
      |
      V
GCP build VPC and GKE cluster
      |
      V
K8s uses the endpoint and certificate from GCP to connect to cluster 
      |
      V
Terraform reads k8s.tf and create fastapi pods and service.

```


```bash
data "google_client_config" "default" {}
provider "kubernetes" {
  host                   = "https://${module.gke-standard.endpoint}" # Link to your GKE module output, The address of the cluster
  token                  = data.google_client_config.default.access_token
  cluster_ca_certificate = base64decode(module.gke-standard.ca_certificate)  }
```

- **host** - This is the address of cluster. Once cluster is ready Google assigns Unique IP/URL and tells terraform to connect to that IP/URL.
- **token** - Only knowing address is not enought we need pwd so if we write this it will create a temporary token (security token) which tells cluster that it is the owner and allow it.
- **cluster_ca_certificate** - This is certificate or a Security seal. This only ensures that the connection is safe and secure and whether the terraform talking to right cluster or not.
  - Google store this certificate in scrambled format or base64 format and we need to decode it to use it. So we use base64decode function to decode it and use it in our provider block.



```bash
terraform apply -target=kubernetes_deployment_v1.fastapi -target=kubernetes_service.fastapi_lb
```
after this only the fastapi appis running
