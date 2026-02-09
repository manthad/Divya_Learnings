=> It is a tool to manage IAC (Infrastructure as a code). So we actually create ec2 instances, sg, vpc , subnets all of it right so these all can be created by 
    terraform.
=> Below are the main things in terraform 

PROVIDERS:    (main.tf)
 => these are like plugins (like app in phone) (means add-on or an extension) which are like special powers or helpers to do the work with more ability.
 => So this is a plugin for the cloud service
                provider "aws"    {           here it can be any provider like asure, google etc and its region
                region = "us-east-1"
                }

RESOURCES:     (main.tf)
 => the things we want to create are the resources.
                resource "aws_instance" "my_server" {
                ami  = "ami-123456"
                instance_type = "t2.micro"
                }

VARIABLES:  
input.tfvars = here directly we provide input data 
                instance_type = "t2.micro"
variables.tf = here we just define what value and its data type to which we are inputing values in inputvars
             variable "instance_type" {
             description = "name of the instance type"
             type = string
           }
 
    If we want to use a field multiple times or modify it every now and then instead of hard coding it we will put here and use it in resources.
                
                variable "region" {       so here we provide the data input with a name and that we use in the main file where we 
                default = "us-east-1"     have the provider and resources
                }

                   provider "aws" {
                   region = var.region
                    }
                
                variable "instance_type" {
                description = "Type of instance"       here in main file we mention as instance_type = var.instance_type then all the data is taken from here
                default     = "t2.micro"
                }
OUTPUTS:

 => these are the things which we want to see after the creation of resources. like public ip, private ip etc
    ex: once an ec2 is created we want to see the public ip right so get that and use it anywhere.
                
                output "instance_ip" {
                value = aws_instance.my_server.public_ip      will get o/p as public ip 
                }

                output "instance_private_ip" {
                value = aws_instance.my_server.private_ip      will get o/p as private ip
                }
STATES:

=> This is the file which gets created after we run the terraform apply command.
=> this is denoted as .tfstate file. This file only checks what needs to be created, updated or deleted and has all data of terraform.
=> It should be kept safe and secure as it contains all the information about the resources created and not to be deleted, modified or share with anyone.
=> this file can be stored in remote locations like s3 bucket, consul etc for better management
=> this is called remote state management.
=> this is done by using backend in terraform.
                terraform {
                backend "s3" {
                bucket = "my-terraform-state"
                key    = "path/to/my/key"
                region = "us-east-1"
                }
                }
COMMANDS: 
 => these are the commands which we use to interact with terraform.
 => below are the main commands used in terraform.
    1. terraform init     => initialize the terraform in the directory. it will download the providers. this is like setting all the providers, plugins etc ready for our work to start like arranging all the things prior.
    2. terraform plan     => shows plan of resources like what is created, deleted or modifed
    3. terraform apply    => create the resources. it will ask for confirmation before creating the resources.
    4. terraform destroy  => destroy the resources. it will ask for confirmation before destroying the resources.
    5. terraform validate => validate the terraform files, will check for any syntax errors.
    6. terraform show     => show the details of the resources created.
    7. terraform state    => manage the state file. it has many sub-commands like list, show, rm etc.
    8. terraform import   => import the existing resources into terraform.

HOW WE WORK:
All files are .tf only

1. We write the main.tf file with provider and resources.
2. variables.tf file with all the variables.        
3. outputs.tf file with all the outputs.
4. terraform init
5. terraform plan
6. terraform apply
7. terraform destroy (if needed)    


CONCEPTS:

  1. provider "aws" {                             provider "aws" {
     alias = "us-east-1"                           alias = "us-west-1"
     region = "us-east-1"   }                      region = "us-west-1". }
    
                resource "aws_instance" "example1" {
                     ami = "ami-04b4f1a9cf54c11d0"
                     instance_type = "t2.micro"
                    provider = "aws.us-east-1"  }    in other resource we use us-west-1 provider
 This is how multi provider we can use and ec2 instances are created at once in different regions.


 2. To create backup in the s3 bucket use 
             resource "aws_s3_bucket" "divya-bucket" {
             bucket = "dsq-s3-demo-day4" 
              }    
     This is just creating a folder in s3 bucket. Now we have to store the state file in this bucket right so that part we write in the backend.tf file 
     terraform {
          backend "s3" {
            bucket = "dsq-s3-demo-day4"
            key = "divya/terraform.tfstate"
            region = "us-east-1"
            encrypt = true
           dynamodb_table = "divya-terraform-lock" => this is lock enable and that regarding data is in main.tf
            use_lockfile = false } }

       
       under main.tf 
                   resource "aws_dynamodb_table" "terraform-lock"{
                        name ="divya-terraform-lock"
                        billing_mode   = "PAY_PER_REQUEST"
                        hash_key = "LockId"

                        attribute {
                        name = "LockID"
                        type = "S"
                        use_lockfile = false } }



Basically this code is already we have created a folder right name dsq-s3-demo-day4 so in that we are asking the s3 resource to create a folder name divya and inside that store the statefile

The lock is used because if many people works on the same tf file and runs terraform apply then it will create issue
So until first user completes his work lock will hold others by throwing an error after work is completed it will allow the second user to work.


NOW CREATING A VPC USING TERRAFORM
         Check the block diagram in phone devops chat

SAS - s/w as service
PAS - platform as service
IAAS - infrastructure as a service


VPC - Virtual private cloud 
CIDR = Classless Inter-Domain Routing. its a way writing IP address range
	•	10.0.0.0/16 → means IP addresses from 10.0.0.0 to 10.0.255.255. - 65536
	•	10.0.1.0/24 → means IP addresses from 10.0.1.0 to 10.0.1.255. - 256
  	•	Smaller number (/16) → bigger network (more IPs).
	•	Bigger number (/24) → smaller network (fewer IPs).

     VPC: 10.0.0.0/16
│
├── Public Subnet: 10.0.1.0/24   → 256 IPs (web servers)
├── Private Subnet: 10.0.2.0/24  → 256 IPs (databases)
└── Private Subnet: 10.0.3.0/24  → 256 IPs (internal apps)

here 24 means 24 1's 11111111.11111111.11111111.00000000
now the range is 11111111.11111111.11111111.00000000 to 11111111.11111111.11111111.10000000 or last number is as as exisitng ip address

We need to login to aws right for that we need combination of public (lock/ which we cannot see but gets created) and 
private key (.pem file which is the key) and both should match to login
   ssh -i divya-key.pem ubuntu@12.34.222.39  here .pem is private and public key will tally with the public ip ofthe instance and it should match

here we have key_name = "divya-key"
             public_key = file("divyamantha.pub")

             now it becomes like ssh -i divyamantha.pub ubuntu@

In general we create key pair with command below and it will autoamticlaly generates public an private key. Private is used for ssh into instance
but public for validating that private matahces to this or not 
the private one is already on our local as we craeted key pair and that we should not provide so only providing 
public key in the code with a tag name divya-key
                    resource "aws_key_pair" "Mykey" {
                    key_name = "divya-key"
                    public_key = file("divyamantha.pub"). }

SUBNETS: 
           resource "aws_subnet" "private-subnet"{
           vpc_id = aws_vpc.divya-vpc.id
           cidr_block = "10.0.2.0/24" }
the vpc should know that the subnet is inside it or in its n/w so we need to tag it. for public the cidr only changes

1. Internet gateway is on VPC level which checks
2. NACL is at Subnet level used for checking
3. Security groups are at ec2 level for checking or creating conditions to allow or not allow ports

IGW: 
              resource "aws_internet_gateway" "igw" {
                    vpc_id = aws_vpc.divya-vpc.id
                    tags = {
                             Name = "Myterraformigw" } }

Route tables are at everylevel which will just route the traffic based on the ip address
it decides whether to send to subnet 1 or subnet 2 and in turn sedn to the ec2
So everywhere we have to create it 

              resource "aws_route_table" "public-route-table" {
                  vpc_id = aws_vpc.divya-vpc.id
                      route {
                          cidr_block = "0.0.0.0/0"
                          gateway_id = aws_internet_gateway.igw.id  } }

              resource "aws_route_table_association" "public-route-table-association" {
                  route_table_id = aws_route_table.public-route-table.id
                  subnet_id = aws_subnet.public-subnet.id  }


RESOURCES: 

                resource "aws_security_group" "divyaSg"{
                    vpc_id = aws_vpc.divya-vpc.id
                    name = "divya-sg"
                    ingress {
                      description = "HTTP from VPC"
                      from_port = 80
                      to_port = 80
                      protocol = "tcp"   
                      cidr_blocks = ["0.0.0.0/0"] } }
here the sg is for telling what ports it should take and it should send response to                      
What VPC it is connected to 
the cidr is like we provide anywhere in the option right thats the one
if we want any specific ip only like myip or VPC ip then it can be added

                    egress {
                        description = "internet"
                        from_port = 0
                        to_port = 0
                        protocol = "-1" 
                        cidr_blocks = ["0.0.0.0/0"] }
Same way out going ports also                        
0 means from all ports to all ports and all protocols too
same cidr means the response can be sent anywhere

EC2 instance: 

                      resource "aws_instance" "ec2_instance" {
                          ami = "ami-04b4f1a9cf54c11d0"
                          count = 5
                          instance_type = "t2.micro"
                          subnet_id = aws_subnet.public-subnet.id
                          key_name = aws_key_pair.Mykey.key_name
                          vpc_security_group_ids = [aws_security_group.divyaSg.id]
                          associate_public_ip_address = true

                          connection {
                              type        = "ssh"
                              # command = "sleep 30"      # Wait for the instance to be ready
                              user        = "ubuntu"  # Replace with the appropriate username for your EC2 instance
                              private_key = file("divyamantha")
                              host        = self.public_ip } }
Here the resource creation is same like previous only but 
connection is liek ssh and ubuntu and .pub is divyamantha and 
self.public_ip is we write the public ip of the ec2 instance right so it automatically takes the public ip once created

PROVISIONERS:

These are the commands that we need to type in ec2 for our app to run
like install nginx or sudo apt update etc

                      provisioner "file" {
                              source      = "./app.py"  # Replace with the path to your local file
                              destination = "/home/ubuntu/app.py"  # Replace with the path on the remote instance
                          }

                      provisioner "remote-exec" {
                      inline = [
                        "echo 'Hello from the remote instance'",
                        "sudo apt update -y",  # Update package lists (for ubuntu)
                        "sudo apt-get install -y python3-pip",  # Example package installation
                        "cd /home/ubuntu",
                        "sudo pip3 install flask",
                        "sudo python3 app.py &",
                        ] } 
                        