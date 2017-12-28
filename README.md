# Will's Docker/Django/Postgres Project Base (base_ddp)
This is the base I use for Django projects in Docker.

## First Steps (git setup):
1) Create local project directory
2) Initialize project directory as repository (``git init``) and Clone project base repo ``git clone git@github.com:willdearman/base_ddp.git``
3) Create project repository on Github
3) Add project repository ``git remote add [project_name_here] https://github.com/willdearman/[project_name_here].git``
4) Initial pull ``git pull [project_name_here] master``
5) Initial commit and push:
````
git add .
git commit -m "Initial project commit"
git push
````

## Next Steps (Docker and Django setup):
1) Run ``sudo docker-compose run web django-admin.py startproject project_name_here . ``
2) Update ``project_name_here/settings.py`` with the Postres configuration:
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
3) Launch project ``docker-compose up``
4) As necessary create apps within project
```
docker exec -it decideanddelegate_web_1 /bin/bash
./manage.py startapp appnamehere
```
4) As necessary perform schema migrations
```
./manage.py makemigrations app_name_here
./manage.py migrate
```

## Other Helpful Commands
- Access container's shell: ``docker exec -it decideanddelegate_web_1 /bin/bash``
- Create app within project (need to be in container's shell) ``./manage.py startapp appnamehere``
- Create Django super user (need to be in container's shell) ``python manage.py createsuperuser``

## Errata
- When using docker-compose, sometimes ``db`` loads after ``web`` so the Django container cannot see the database. I have not found a resolution for this yet other than shutting down all containers and trying again.

