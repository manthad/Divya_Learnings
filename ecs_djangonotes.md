DJANGO 
Process of ECS:
 ECS is a service that runs your app continuously on AWS using containers, without you having to manually start or monitor servers.
 In our laptop when we run python manage.py runserver it starts in our local machine and it is closed when we close the terminal.
 But in ECS it will run continuously on AWS servers so even if we close our laptop or terminal it will keep running.
 Steps to deploy django app to ECS:

 ECS mainly works with Docker containers.

1. Now already we have created a django project and pushed it to github
2. We already have the Dockerfile and then actions to build and push the image to docker hub
3. Make sure the image tag name is updated from latest to some version like v1, v2 etc coz the ecs will pull the latest image only when we change the tag name
4. Now we need to create a ECS cluster in AWS console
   => Go to ECS in AWS console and create a cluster
   => Choose the networking only cluster
   => Provide the cluster name and create the cluster
5. Now we need to create a task definition
   => Choose the Fargate option
   => Provide the task definition name
6. Now we need to add container to the task definition
   => Provide the container name
   => Provide the image name from docker hub like divyamantha/djangoapp:v1
   => Provide the memory and cpu units
   => Provide the port mappings like 8000
   => Add the container
7. Now create the task definition
8. Now we need to run the task in the cluster
   => Choose the cluster created
   => Go to tasks tab and run new task
   => Choose the launch type as Fargate
   => Choose the task definition created
   => Choose the VPC and subnets
   => Choose the security group or create a new one and allow inbound traffic for port 8000
   => Run the task
9. Now we can see the task is running in the cluster
10. Now we need to test the application by accessing with public ip generated in the task: 8000

Make sure the settings.py file will allow this public ip which is must else add '*' under Allowed hosts
11. When we updated tag go to the and add new revision which will create new revision of the task definition
12. We need to update the service to use the new revision
    => Go to the cluster
    => Go to services tab
    => Choose the service created
    => Update the service
    => Choose the new revision
    => Update the service`
