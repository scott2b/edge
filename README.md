{% comment "This comment section will be deleted in the generated project" %}

 [Edge][docs]

[![Build Status](https://travis-ci.org/arocks/edge.svg?branch=master)](https://travis-ci.org/arocks/edge)

## Features

* Ready Bootstrap-themed pages
* User Registration/Sign up
* Better Security with [12-Factor](http://12factor.net/) recommendations
* Logging/Debugging Helpers
* Works on Python 2.7 or 3.4

# Deploy early; deploy often

This template is intended for creation of a Django project on the deployment server. This encourages early deployment and reduces a lot of the common headaches of deployment.

By creating the application within the deployment environment, we can take advantage of helpful resources like the app_directory context variable in Django's startproject templates.

There is a bootstrapping issue with this approach in that we need a Django install in order to generate the templated project. To this end, we recommend keeping a suite of environments, probably in the deploying user's home directory, with the Django installs you might need for deployment. This approach is assumed in the production quickstart below.

For local development, it is recommended that you use whatever virtual environment management you are used to. The development quickstart uses Pipenv, but this is optional.

## Quick start (on production):

Assumes availability of an existing deployment environment with Django installed.

Ideally, projectname is the domain apex. If not, be sure to edit the ALLOWED_HOSTS
in `src/core/settings/production.py`

```
 $ . <path-to-django-virtualenv>
 $ export PROJ=<projectname>
 $ cd <path-to-site-deployments>
 $ django-admin startproject --template=https://github.com/scott2b/edge/archive/master.zip --extension=py,md,html,env,nginx,service $PROJ
 $ deactivate
 $ cd $PROJ/site
 $ ./mkenv.sh
 $ sudo ln -s `pwd`/<projectname>.nginx /etc/nginx/sites-enabled/<projectname>
 $ sudo ln -s `pwd`/sewunique.service /etc/systemd/system/sewunique.service
```

Create the database(s). Generally, do this as the postgres user. E.g:

```
 $ sudo su - postgres
 $ psql
 postgres=# create database <dbname>;
 postgres=# create user <username> password '<password>';
 postgres=# grant all privileges on database <dbname> to <username>;
 postgres=# \q
 $ exit
```

Edit the database params in the `src/.env` file (Note: .env is git-ignored for changes)
Also uncomment the DJANGO_SETTINGS_MODULE configuration for production

Migrate the dB and collect static (**Note:** argon2 is not being properly installed from the requirements file, so we fix that here):

```
 $ . site/env/bin/activate
 $ pip install django[argon2]
 $ cd src
 $ . env.sh
 $ ./manage migrate
 $ ./manage collectstatic
```

Restart services:

```
 $ sudo systemctl restart <projectname>
 $ sudo systemctl restart nginx
```

## Troubleshooting deployment

 * look for the gunicorn.socket file in the site directory
 * inspect /var/log/syslog
 * inspect /var/log/nginx/error.log
 * It can be helpful to independently execute the ExecStart command (found in the service file)
   - as the application execution user, `. env.sh`
   - copy and execute the ExecStart command (after the =) from the service file


## Quickstart (for development):

**Note:** In order to facilitate early deployment, it is recommended that you create your project in a production deployment environment using the instructions above. The instructions below are for creating a project locally.

```
 $ export PROJ=projectname
 $ mkdir $PROJ
 $ cd $PROJ
 $ pipenv shell
 $ pipenv run pip install pip==18.0
 $ pipenv install Django==2.1.2
 $ django-admin startproject --template=https://github.com/scott2b/edge/archive/master.zip --extension=py,md,html,env $PROJ .
 $ pipenv install -r requirements/development.txt
```


Edit the database params in the .env file (Note: .env is git-ignored for changes).

To source the .env file:

```
 $ . env.sh
```

Alternatively, consider using something like `autenv`: https://github.com/kennethreitz/autoenv
or `direnv` (with .envrc instead of .env):  https://direnv.net/

**Note: until Pipenv works with current version of pip, we need to force pip to 18.0**

More information at: [http://django-edge.readthedocs.org/][docs]

[docs]: http://django-edge.readthedocs.org/


## Recommended Installation (with `pipenv`)
1. [Install pipenv system-wide or locally](https://docs.pipenv.org/) but outside a virtualenv
2. `$ django-admin.py startproject --template=https://github.com/arocks/edge/archive/master.zip --extension=py,md,html,env my_proj`
3. `$ cd my_proj`
4. `$ pipenv install --dev`
4. `$ pipenv shell`
4. `$ cd src`
5. `$ cp my_proj/settings/local.sample.env my_proj/settings/local.env` (New!)
6. `$ python manage.py migrate`

If you need to keep `requirements.txt` updated then run

    pipenv lock --requirements > requirements/base.txt
    echo "-r base.txt" > requirements/development.txt
    pipenv lock --requirements --dev >> requirements/development.txt

Rest of this README will be copied to the generated project.

--------------------------------------------------------------------------------------------

{% endcomment %}

# {{ project_name }}

{{ project_name }} is a _short description_. It is built with [Python][0] using the [Django Web Framework][1].

This project has the following basic apps:

* App1 (short desc)
* App2 (short desc)
* App3 (short desc)

## Installation

### Quick start

To set up a development environment quickly, first install Python 3. It
comes with virtualenv built-in. So create a virtual env by:

    1. `$ python3 -m venv {{ project_name }}`
    2. `$ . {{ project_name }}/bin/activate`

Install all dependencies:

    pip install -r requirements.txt

Run migrations:

    python manage.py migrate

### Detailed instructions

Take a look at the docs for more information.

[0]: https://www.python.org/
[1]: https://www.djangoproject.com/
