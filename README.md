# Will's Docker/Django/Postgres Project Base (base_ddp)
This is the base I use for Django projects in Docker. These instructions are suitable for **beginner to intermediate** users who want to create a Python Django project in a reproducable Docker container.

After completing these steps, you will have:
- Docker services for web and database
- The project folder mapped to /app in your web service docker container
- Django running at http://127.0.0.1:8000/
- Posgres running on port 5432 (127.0.0.1:5432)
- A Github repository containing a working version of a basic Django application

In the code blocks below, you will need to replace the following:
- ``project_name_here`` should be the name of your project on Github
- ``your_github_name`` should be your Github user name
- ``projectnamehere`` should be the project name you want to use locally (e.g. subfolder names, Docker name, etc)
- ``appnamehere`` name of an application within your Django project

## First Steps (git setup):
1) Create local project directory and clone this repository into project directory
```
mkdir project_name_here
cd project_name_here
git clone git@github.com:willdearman/base_ddp.git .
```
2) Create project repository on Github (do not chose the option to initialize repository)
3) Add project repository ``git remote add project_name_here https://github.com/your_github_name/project_name_here.git``
4) Initial push ``git push -u project_name_here master``
5) Initial commit and push:
````
git add .
git commit -m "Initial project commit"
git push
````
6) At this point you may also want to change LICENSE and readme.MD then
````
git add .
git commit -m "Removed project base license and readme"
git push project_name_here master
````

## Next Steps (Docker and Django setup):
1) Run ``sudo docker-compose run web django-admin.py startproject projectnamehere . ``
2) Update ``projectnamehere/settings.py`` with the Postres configuration:
```
# Database
# https://docs.djangoproject.com/en/1.11/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql', # Not sure if this should be: 'django.db.backends.postgresql_psycopg2'
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```
3) Launch project ``docker-compose up`` (Watch for any error messages)
4) As necessary create apps within project (in a new terminal window not showing docker messages)
```
docker exec -it projectnamehere_web_1 /bin/bash
./manage.py startapp appnamehere
```
Dont forget to add the app name you created to ``INSTALLED_APPS`` in settings.py (don't forget the trailing comma)
5) As necessary perform schema migrations
```
./manage.py makemigrations appnamehere
./manage.py migrate
```

## Other Helpful Commands
- Access container's shell: ``docker exec -it projectnamehere_web_1 /bin/bash``
- Create Django super user (need to be in container's shell) ``python manage.py createsuperuser``
- Force rebuild docker image ``docker-compose up --build``

## Errata
- When using docker-compose, sometimes ``db`` loads after ``web`` so the Django container cannot see the database. I have not found a resolution for this yet other than shutting down all containers and trying again.
- This project could probably be improved by separating the database into a different dockerfile that can be used by multiple web apps, but I don't yet know how to bridge that network connection.

