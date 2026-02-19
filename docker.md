## 1. WHAT is Docker? (The "Standard Container")
Docker is a tool that allows you to package an application with everything it needs (code, libraries, settings) into a single unit called a Container.

### The Real-World Analogy: The Shipping Container
Before 1950, shipping goods was a nightmare. Flour, cars, and pianos were all packed differently. Workers had to figure out how to stack them every time.
- The Solution: The Standardized Shipping Container. It doesn’t matter if there are pianos or potatoes inside; the crane (Docker Engine) and the ship (AWS) only care that the box is the same size.
## 2. WHY and WHEN to use Docker?
### WHY use it?
- "It Works on My Machine" Syndrome: Without Docker, your app might work on your laptop (Ubuntu) but fail on the AWS server (Amazon Linux) because a small library version is different. Docker ensures the environment is identical everywhere.
- Isolation: You can run two apps on the same server—one needing Python 2 and one needing Python 3—without them fighting.
- Speed: Unlike a Virtual Machine (VM) that takes minutes to boot an entire OS, a Docker container starts in milliseconds because it shares the "Host" computer's brain (Kernel).
### WHEN to use it?
- Microservices: When your app is broken into 10 small services that all need to talk to each other.
- CI/CD: When you want your GitHub Actions to build and test your app automatically before deploying to AWS.
- Onboarding: When a new dev joins, they don't spend 2 days installing Java/Database/Node. They just run docker-compose up and start working.
## 3. The Docker "Trinity": File, Image, Container
|Term|	Analogy|	Description
|-----|---|----|
|Dockerfile|	The Recipe	|A text file with instructions (e.g., "Install Python," "Copy my code").
|Image|	The Frozen Pizza|	A read-only snapshot built from the dockerfile. i.e. it reads the file, execute each instuction, creates layers, package into image build command
|Container|	The Pizza in the Oven|	A living, breathing, running instance of that image.

## 4. Interview Preparation: Assignments
If you can do these 3 tasks, you are ready for a Junior DevOps interview.

### Assignment 1: The "Manual-to-Docker" Migration
- Task: Take a simple "Hello World" Python or Node.js script.
- Action: Write a Dockerfile.

Interview Question: "What is the difference between CMD and RUN in a Dockerfile?"

Answer: RUN happens while building the image (installing stuff). CMD happens when the container starts (running the app).
### Assignment 2: The Multi-Container Setup
- Task: Use docker-compose to run a Web App and a Database (like MySQL) together.
- Action: Ensure the Web App can talk to the Database using its service name, not an IP address.

Interview Question: "How do you make sure the data in your database isn't lost if the container crashes?"

Answer: Use Volumes to link a folder on the host machine to the container's data folder.
### Assignment 3: Optimization & Security
- Task: Build an image and look at its size. Then try to make it smaller using the alpine version of your base image.
- Interview Question: "How do you keep your Docker images secure?"

Answer: Use minimal base images (Alpine), don't run as "root" user, and scan for vulnerabilities using docker scan.
### Pro-Tip for Interviews: Docker vs. VM
- The #1 question you will get: "How is Docker different from a Virtual Machine?"
- The Pro Answer: "A VM virtualizes the Hardware (it includes a whole OS, making it heavy). Docker virtualizes the Operating System (it shares the host's Kernel, making it lightweight and fast)."

## Cheat Sheet

|Command|Why it’s used in Real Time
|---|---|
|docker build -t my-app .|"Builds the image from your Dockerfile and ""Tags"" it with a name."
|docker run -p 8080:80 my-app|Starts the container and maps your laptop's port 8080 to the container's port 80.
|docker ps|"Lists all currently running containers (The ""Check-up"")."
|docker ps -a,"Shows all containers, including the ones that crashed or stopped."
|docker logs -f <id>|"Follows the output of your app. This is how you debug ""Why isn't my app starting?"""
|docker exec -it <id> sh|Goes inside the running container. It's like SSHing into the container to look around.
|docker images|"Lists all the ""Frozen"" images stored on your machine."
|docker stop <id>|Gracefully shuts down a running container.
|docker rm <id>|Deletes a container. (Note: Use docker rmi to delete an image).
|docker system prune|"The ""Clean Up"": Deletes all stopped containers and unused images to save disk space."

## Interview Drill: Real-World Scenarios
In a DevOps interview, they won't just ask you "What is a command?" They will give you a problem. Here is how to answer them using your AWS + Docker knowledge:

### Scenario 1: 
### "My app works on my laptop but says 'Connection Refused' when running in Docker on AWS."
The Answer: "I would check three things: First, is the app inside the container listening on 0.0.0.0 or just localhost? (Inside a container, it must be 0.0.0.0). Second, check if the Docker Port Mapping (-p) is correct. Third, check the AWS Security Group to ensure that specific port is open to the internet."

### Scenario 2: 
### "Your Docker image is 2GB. That’s too big for AWS to pull quickly. How do you fix it?"
The Answer: "I would use a Multi-Stage Build. I’d use a large image to compile the code, then copy only the final 'ready-to-run' file into a tiny Alpine Linux image. I’d also make sure my .dockerignore file is stopping unnecessary files (like .git or node_modules) from being copied into the image."

### Assignment 1: The Dockerfile Deep Dive
- Create a Dockerfile for a simple web page.

- Use nginx:alpine as the base image.

- COPY an index.html file into /usr/share/nginx/html.

Bonus: Try to change the default Nginx port from 80 to 8080 inside the container.
### Assignment 2: The "Environment Variable" Test
- Write a Python or Node script that prints an environment variable called APP_COLOR.

- Run the container and use the -e APP_COLOR=blue flag.

Goal: Learn how to change app behavior without rebuilding the image.
### Assignment 3: Docker Compose + Networking
- Create a docker-compose.yml with two services: frontend and backend.

- Make the frontend ping the backend.

Observation: Notice how you don't need IP addresses; you can just ping the name backend because Docker Compose creates a private network for them.