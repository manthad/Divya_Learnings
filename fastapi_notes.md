## FAST API
API is a service that allows different software applications to communicate with each other. It defines a set of rules and protocols for how these applications can interact. It is the one which takes the request from the client and sends to the server and the response from server to the client.

A framework is a ready-made structure or set of tools that helps you build software applications faster and more efficiently. Like a blueprint or a skeleton using that we build apps without from scratch.

- FastAPI is a framework is built on Starlette and Pydantic.
- Fast API is just like a code but the tool to run the api is the UVICORN (web server) so thats 

### Why we need to run it or reload it when code changes are done?
Starlette (web framework) is like the engine behind FastAPI. It handles all the web request/response stuff under the hood.

#### FastAPI is the app you see (user interface).
	
- Starlette is the system (delivery person, routes, traffic handler) that makes the app work fast and reliably.
- Pydantic handles data validation and data types in FastAPI.
   - It ensures the data you send or receive is correct and structured.  
   - u have something called BaseModel which also describes the structure or format   


***Here the thing is when you download FastAPI it will already have the starlette and pydantic. SO we need to install uvicorn which is a web server explicitly***

### Swagger Docs

Swagger Docs is an interactive web page that shows:

- All data/ documentation where some developer will understand to build the app where he 
       writes all the conditions for a particualr field or etc. Easy human readability
- In general we can get the output from the url itself but if we have this docs which is a UI will help to select or provide the options or data in an easy way who doesnt understand coding much
- BDW for get we can use URL but for other things its docs approach as it is not supported by the browser. So we can see the o/p only on the docs
    
### Schema

Schema is like a blueprint or a form that defines what kind of data your API should accept or return
(ex: name, age , account no if went to bank)      
   - It validates input data: catches errors before they cause problems.
	- 	It documents your API: others know what kind of data to send.

OPENAPI is a standard way of describing how an API should look like (how RESTAPI should be)   (It is just telling what API what need to have or what it can do like get, post. So based on those things only we can create API)
   -	FastAPI is the tool you use to build the API.
	-	OpenAPI is the rulebook that describes what the API does. 

### Arguments   

1. ENUM - Enumeration - to limit values to a specific list so that to enter valid data. Can be used for both path and query parameters actually used when the input values are more than 1

2. PARSING - analyzing and understanding a piece of data so that a computer program can make sense of it.

3. QUERY - eventhough it seems optional but if we have it instead of getting all the data we can just mention for specific
        like category = books, then only that data will come. Also for updating we need to use Query only

4. ANNOTATED - inside annotated we cannot use query with default but can use outside. even though using query it will make more clear by creating a seperation.
It is used when we need to provide extra rules for query parameter

5. IDEMPOTENT - means eventhough when performed a work several times will result only once. like updating and deleting methods.


### END of the day the main goal is to whether the data we are providing is correct or not and how to test them or provide conditions to the data that provided by the client


***Fixed part i the url is called SLUG***

***Stateless - means that each request from the client contains all the information the server needs to fulfill that request. The server does not store any client context between requests.***




## PROCESS OF API:

1. CREATED DOCKER FILE:

1. We need to have python image and its respective installations.we need to install in container right
2. Now telling the Workdir to the directory where we want to work
3. First copying the requirements.txt file to the container. we need to have this because whatever the packages we need to install
   for the app to run we mention here like fastapi, uvicorn, pydantic etc. if not there we can install them 
   pip freeze > requirements.txt
4. We need to install the packages mentioned in the requirements.txt file right so thats why run pip install -r requirements.txt
   This will install all the packages mentioned in the requirements.txt file in the container
5. Copying all our code files to the container COPY . .
6. Need to tell container where the app is running. So we need to expose the port 8000
7. Finally we need to tell the command to run the app. So we use CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]

   Now Docker build -t fastapi-app . 
   docker run -d -p 8000:8000 fastapi-app
   container got created and running and checked the contents on browser by going to 
   http://localhost:8000/basics/items/42 which need to work. 

   This is just a check in the local machine before pushing to the docker hub


2. GIT HUB:
   Now we need to push the image to docker hub using github push along with required files and docker file

3. GITHUB ACTIONS:
   We need to create .git/workflows folder and create a yml file where we write when the action to be started and what to do  
   ```bash

   - name :  # build and push or deploy to docker hub
   
   on push or pull request to main branch

   - Write steps for checkout code, buildx
   - login to docker hub using secrets  we cannot write the usern and pwd directly due to security reasons we need to provide just the variables in which we are storing that data which is in GIT HUB SECRETS 
   
   build and push to docker image lines
   
   - need to mention platform like ubuntu, linux etc
   - now provide context i.e. from which folder we need to take the docker file
   - path of the docker file. if you are in some root folder then u need to mention like python/fastapi/Dockerfile else if u are in same folder then just Dockerfile   
   - tags are nothing but the name that we want to give to the image after creation
   ```

## AWS EC2:

Create a EC2 instance and install docker in it. We are doing it prior only because when we add that creation in the yml file then it will create ec2 instance everytime we perform git push

***We need to create a yml file in .github/workflows folder***

### The main things again we provide to this file are
```bash
name of the file

on push to whatever branch

Now again need to login to AWS LOGIN using secrets which also we are creating in the GITHUB SECRETS
   host = public ip
   username= ubuntu/linux whatever we have installed 
   ssh key = content of the .pem file from first to last

- Now we need to connect to the EC2 instance using SSH and run the commands like 
- stop the container if it is running
- remove the container if it is running
- pull the latest image from docker hub
- run the container with the latest image and map the port 8000 of the container
- Now we can access the app using the public ip of the EC2 instance and port

We can also use the docker ps command to check the running containers in the EC2

Commit all the changes of AWS EC2 file and do git push
Now a workflow will run and if we login to ec2 instance using aws configure then the image is already there and is running

We just need to check it in the browser using the public ip:8000/basics/item/33
```

## Questions
Here i have used EC2_SSH_key as the content in the .pem file. generally we can do like that it and the uses: appleboy/ssh-action@master which is used to connect to the EC2 instance which is capable of reading the pem file.

But if we dont have the .pem file or misplaced it or need to create new private and public keys then 
```bash 
ssh-keygen -t rsa -b 4096 -f github-ec2-key # in terminal this creates 2 files
github-ec2-key       --> Private key (used by GitHub)
github-ec2-key.pub   --> Public key (goes to EC2)

copy this public key into ec2
ssh -i your-ec2-key.pem ec2-user@your-ec2-public-ip  ssh-i fastapi.pem ubuntu@publicip

mkdir -p ~/.ssh - 
# we ran it on ec2 instance - we are copying the public key to the authorized_keys file on the EC2 instance
nano ~/.ssh/authorized_keys # this opens text editor to paste the data of public key i.e. .pub file content

chmod 600 ~/.ssh/authorized_keys # this is giving permissions to the file authorized keys
chmod 700 ~/.ssh - directory
```


Now if we want to create EC2 isntance from GITHUB actions then we should write like if already there is a VM then dont create it again.

  

