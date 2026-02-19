# DJANGO 
It is also a framework just like FASTAPI but FASTAPI is is used for building APIs where as DJANGO is used for building web applications full i.e. frontend, backend and database everything.

## PROCESS OF API:

1. First we need to check whether we have pip and python and then pip (Performance improvement plan)
    ```bash
    pip install django\
    ```
    This is another web framework just like flask or fastapi but has few extra features which can deal with the database too
  
2. Create new env
    ```bash
    python -m venv env new environment
    ```
3. Need to activate it with 
    ```bash            
    source env/bin/activate
    ```
4. Now install django by pip install django and check the version with 
    ```bash
    django-admin --version
    ```
5. We have to create a new project with
    ```bash
    django-admin startproject myproject
    ```
    This will create a new directory called myproject with the necessary files for a Django project. if we do normal empty files only will be there.

    It has few required files, link b/n them, default settings and configurations.

    - it created many files right think of it like a controller.
    - it has global settings, urls, wsgi, asgi, etc.
    - one project per site but can have multiple apps inside it.

6. Now run 
    ```bash 
    python manage.py runserver # just checking whether it is working or not
    ```
    
    command where manage.py is a file created in the myproject directory.

    This will start the development server, and you can access your Django application at http://127.0.0.1:8000/.

### Note:
- Normally for the tools which are directly run by its names like git, node, docker are installed globally. i.e. brew intall

- But anything we are using by importing them in code like flask, fastapi ,djnago so these kinds are installed in the virtual environment and are installed by pip. 

-  As I told you it supports database, so we can use sqlite3, mysql, postgresql, etc and MODEL is a class which is used to define database structure in django i.e. how to store, retrieve, update and delete data in database. 
- SO using this model whatever we provide everything is converted to table in whatever database we are using.
Instead of writing SQL only we can use this model to do all operations on the tables.

7. Same like creating project we should also create an app inside the project with below but not manually instead use 
    ```bash
    python manage.py startapp your_app_name (djangoapp)
    ```
This will create a new directory called djangoapp with the necessary files which django app will use.        

- here it will handle only specific feature
- it has models, views and all.
- it is like a reusable module. 


Now whatever the folder got created ex: djangoapp we may have many files in it but we need to only mention the djangoapp name in the ***settings.py*** file of the project to make it work. coz django only looks for the app name in the ***settings.py*** file to know which apps are there in the project and only runs them.
    
```bash
settings.py file
```
After writing code we need to run below command under main folder where u have the manage.py 
```bash
python manage.py makemigrations # this will create migration 
```
#### files for our code i.e. models.py

10. this means that hey django we have made some changes to the models so please create migration files for the same which is like a blueprint
    ```bash 
    python manage.py migrate # this will apply the migrations to the database and create the necessary tables.
    ```
    
    this means wea re asking django to take the plan and apply it really in the database.

11. Registering our model class name under admin.py i.e.
    ```bash
    from .models import Person
    admin.site.register(Person)
    ```               
This will allow us to manage our model through the Django admin interface.

12. Now create a superuser to access the 127.0.0.1/8000/admin which is the page we need to open and need to have su permission
    ```bash
    python manage.py createsuperuser
    ```
This will prompt you to enter a username, email, and password for the superuser account.    

13. Now again we need to run the server right to check whether the app with /adminis opening or not 
    ```bash
    python manage.py runserver
    ```
Now Django UI will open and now we can do whatever data we want to create, update, delete, etc. without any SQL queries.    

Restaurante - Django

|Tool|files |What It Does|
|--------|---------|----|
|Menu                                        URLs (urls.py) |                       Maps customer requests to views (pages)|
|Kitchen                                    Views (views.py)|                      Prepares and sends data to show (like cooking)| 
|Storage (Ingredients)                      Models (models.py)|                    Handles the database (stores info like dishes)|
|Billing System                             Admin (admin.py) |                     Manages data easily through a UI|
|Staff/Manager                           Settings (settings.py) |                   Lets you access and manage the admin panel|
|Restaurant Building                      Templates (templates/) |                 HTML files – shows what customer sees|
|Security Guard                             Middleware  |                          Manages user login, permissions|

----
---



- We have primary key where if we didnt mention it like id =1, 2,3 if we want to have specifically a primary key then add 
primary_key= True then it will show the value as primary key and not as id.

- Verbose is like alias name if we provide verbose name it wll tkae that else it will take the first_name as first name i.e. undersscores are converted to spaces.



#### Can you please differentiate between language, package manager and tool in simple example to clear the confusion

- Language is the code we write like python, java, javascript, etc

- Package manager is the one which helps us to install the packages like pip, npm, yarn, etc. That is it is a tool which helps installing add ons for the language

- pip install we use to install packages for python language like django, flask, fastapi, etc

- Django is the tool with which we actually create and run the application.