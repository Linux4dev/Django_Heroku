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
* git add *
* git commit -m 'First commit'

## Hidding instance configuration
* pip install python-decouple
* create a `.env` file at the root path and insert the following variables
```
SECRET_KEY=(Get this secrety key from the settings.py)
DEBUG=True
```

### Add to settings.py
```
from decouple import config

SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
```

## Configuring the Data Base
* pip install dj-database-url

### Add to settings.py
```
from dj_database_url import parse as dburl

default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')
DATABASES = {'default': config('DATABASE_URL', default=default_dburl, cast=dburl),}
```

## Static files 
* pip install dj-static

### Add to wsgi
```
from dj_static import Cling

application = Cling(get_wsgi_application())
```

### Add to settings.py
```
import os

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

## Create a requirements-dev.txt
* pip freeze > requirements-dev.txt

## Create a file `requirements.txt` with the following content
```
-r requirements-dev.txt
gunicorn
psycopg2
```

## Create a file `Procfile` and add the following code
```
web: gunicorn ProjectName.wsgi
```

## Create a file runtime.txt and add the following code
```
python-3.9.0
```

## Creating the app at Heroku
Install heroku CLI in your computer (https://devcenter.heroku.com/articles/heroku-cli)
* heroku apps:create app-name

## Setting the allowed hosts
* include the app domain at the ALLOWED_HOSTS directives in settings.py | Ex: app.heroku.com

### Sending configs to Heroku
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
