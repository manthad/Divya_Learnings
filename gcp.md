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
