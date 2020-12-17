# Django_Heroku
Guide to setting up a Django application on Heroku

## Create the git ignore file
* Create a `.gitignore` with the following content:
```
.idea
*.sqlite3
.venv
*pyc
```
* git add .
* git commit -m 'First commit'

## Hidding instance configuration
* pip install python-decouple
* create an .env file at the root path and insert the following variables
* SECRET_KEY=(Get this secrety key from the settings.py)
* DEBUG=True

### Settings.py
* from decouple import config
* SECRET_KEY = config('SECRET_KEY')
* DEBUG = config('DEBUG', default=False, cast=bool)

## Configuring the Data Base
* pip install dj-database-url

### Settings.py
* from dj_database_url import parse as dburl
* default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')
* DATABASES = {'default': config('DATABASE_URL', default=default_dburl, cast=dburl),}

## Static files 
* pip install dj-static

### wsgi
* from dj_static import Cling
* application = Cling(get_wsgi_application())

### Settings.py
* import os
* STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

## Create a requirements-dev.txt
* pip freeze > requirements-dev.txt

## Create a file requirements.txt file and include reference to previows file and add two more requirements
* -r requirements-dev.txt
* gunicorn
* psycopg2

## Create a file Procfile and add the following code
* web: gunicorn ProjectName.wsgi

## Create a file runtime.txt and add the following core
* python-3.9.0

## Creating the app at Heroku
Install heroku CLI in your computer (https://devcenter.heroku.com/articles/heroku-cli)
* heroku apps:create app-name

## Setting the allowed hosts
* include the app domain at the ALLOWED_HOSTS directives in settings.py | Ex: app.heroku.com

## Heroku install config plugin
* heroku plugins:install heroku-config

### Sending configs from .env to Heroku ( You have to be inside tha folther where .env files is)
* heroku plugins:install heroku-config
* heroku config:push

## Publishing the app
* git add *
* git commit -m 'First commit'
* git push heroku master --force

## Creating the data base
* heroku run python3 manage.py migrate

## Creating the Django admin user
* heroku run python3 manage.py createsuperuser

## Disable DEBUG mode
* heroku config:set DEBUG=False

## EXTRAS
### You may need to disable the collectstatic
* heroku config:set DISABLE_COLLECTSTATIC=1
