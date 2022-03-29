# Django

## Initial Project Setup

1. Create project directory
2. Set up virtual environment 

```shell
$ cd <project_name>
$ pyenv virtualenv <python_version> <project_name>
$ pyenv local <project_name>
```

3. Install Django
4. Start a new project

```shell
$ django-admin startproject <project_name> .
```

5. Create a new app

```shell
$ django-admin startapp <app_name>
```

6. List the app in `settings.py`

## Deployment

These are instructions for deploying a Django site to a Linux server hosting multiple sites with Gunicorn and nginx

1. Initial directories and project setup

   1. Create `sites/DOMAIN`
   2. In each create `database`, `static`, and `source`
   3. The end goal is this layout:

   ```
   /home/dev/sites
   |--staging.mysite.com
   |   |--database
   |   |   |--db
   |   |--source   
   |   |   |--manage.py
   |   |   |--etc...
   |   |--static
   |   |   |--base.css
   |   |   |--etc...
   |
   |--mysite.com
   |   |--database
   |   |--etc...
   ```

2. Clone latest source to `sites/DOMAIN/source`

3. Deal with secrets - copy a file, set environment variables, whatever

4. Edit settings if necessary

5. Make a virtualenv in `sites/DOMAIN`

6. Install required packages

7. Collect Static

8. Migrate

To actually deploy navigate to `deploy_tools` folder and

```
fab deploy host=dev@staging.shiftalert.co

and assuming all is well

fab deploy host=dev@shiftalert.co
```

