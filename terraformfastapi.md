# FASTAPI IN KUBERNETES WITH HELM AND TERRAFORM

```bash
provider "helm" {
  kubernetes {
    # host: This is the URL (IP address) of your GKE Cluster's control plane.
    # It uses the output from your GKE module to know exactly where to 'dial'.
    host                   = "https://${module.gke-standard.endpoint}"

    # token: This is your temporary digital 'ID card'.
    # It proves to Google Cloud that you have permission to install apps.
    token                  = data.google_client_config.default.access_token

    # cluster_ca_certificate: This is the security certificate.
    # It ensures the connection between Terraform and GKE is encrypted and private.
    cluster_ca_certificate = base64decode(module.gke-standard.ca_certificate)
  }
}
```

```bash
resource "helm_release" "fastapi_app" {
  # name: This is the 'human' name for your deployment inside Kubernetes.
  # If you run 'helm list', you will see this name.
  name       = "fastapi-release"

  # chart: This is the folder path on your laptop where the Helm files live.
  # Terraform will package up everything in './charts/fastapi-app' and send it to GKE.
  chart      = "./charts/fastapi-app"

  # namespace: This is like a 'folder' inside your cluster. 
  # 'default' is the standard workspace for most apps.
  namespace  = "default"

  # wait: This tells Terraform to pause until the app is actually 'Running'.
  # If the app fails to start, Terraform will show an error here.
  wait       = true

  # set { ... }: These blocks 'override' the default settings in your values.yaml file.
  # It allows you to change the Docker image or version without touching the Helm chart itself.
  set {
    name  = "image.repository"
    value = "manthad/testk8s" 
  }

  set {
    name  = "image.tag"
    value = "v3" 
  }
}
```

Now these are to be added in the main.tf file and then we can run the command also make the changes in the values.yaml file to match the image name and tag.

```bash
terraform init
terraform apply
``` 