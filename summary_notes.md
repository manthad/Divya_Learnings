## First:
- We learnt what is API or REST API and how can we create API using FASTAPI framework.
- Learnt how to dockerize the FASTAPI application using Dockerfile and pushed it to GIT and then to Docker Hub using GITHUB ACTIONS (created push and deploy workflows).
- Finally we deployed the dockerized FASTAPI application to AWS EC2 instance using GITHUB ACTIONS.
## Then:
- We learnt what is DJANGO framework and how it is different from FASTAPI (deals with API) i.e. it can deal with the database(deals with app front end, back end).
- Learnt how to create a DJANGO project and app using commands.
- Finally we deployed the DJANGO application to AWS ECS using DOCKER and GITHUB ACTIONS.

## Summary notes  
These are the summary notes of what all we have learnt till now in FASTAPI and DJANGO along with ECS deployment.


### SECOND:
- We learnt how to backup the application using existing backup files and restoring it in a new EC2 instance and all the commands related in both local and in EC2 instance.


## Understanding:
What my understanding is for any devops engineer when he visits to a new company below are the basic things that he need to work on irrespective of what kind of the application is 

1.⁠ ⁠Developer gives the updated code including front end, backend and db via github

2.⁠ ⁠As a devops engineer i have to pull the code from git and then check first locally whther it is working or not using basic commands like npm start or python runserver etc

3.⁠ ⁠After that i need to create a docker image for it which includes copying of all the code in docker and install all the necessary steps which is to be added under requirements.txt file and then if possible write docker compose to consolidate all the 3 services and do docker compose up and check if the app is runing correctly in docker.

4.⁠ ⁠If everything is good i need to do build and deploy using git hub actions by creating a .gihub/workflows in which i write yml file and it creates  workflow when push or any action based on what we write.

5.⁠ ⁠Once it is done i need to check it in aws ec2 instance by checking it with the public ip of the ec2 and the url extensions if mentioned.

6.⁠ ⁠In case if any backup or restore need to be done i need to take the back up file and restore it using few cmmands and push the same into the ec2 and validate whether im able to see the old data instead of a fresh one

## What I Do?
- I write dockerfile, docker compose files, terraform if necessary or github actions yml files where i write on push or pull then the only part developer does is just push the code using git push then automatcially they go and check the actions workflow whether it is pass or fail.

- If that doesnt pass based on their requirement they will correc their code. But if there is anything wrong from our end we have to modfiy our yml files ami i right does this means like creating pipelines and used by the dev



## Nemani summary:
1. Install all the front end, backend and db based on app in my case it is php, nginx and sql and start it 
2. logged in to sql and created a user with pwd and a db with the backup name and copied the backup.sql file into that db
3. The database part is done
4. regarding the nginx go to the var/www/ and add the full code zip file here
5. Now go to var/www/sites/default/settings.py and change the username, pwd and database names
5. Now go to the /etc/nginx/sites-available/ and paste the default code for an app to run here
6. Providing permissions to all the files and folders.

All the above steps do it in ec2 instance using ssh but at home/ubuntu using scp command


```bash
sudo nano /etc/nginx/sites-available/myapp
server {
    listen 80;
    server_name myapp.local;

    root /var/www/myapp;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```


