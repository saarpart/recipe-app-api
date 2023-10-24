# recipe-app-api
Recipe API project

How to build and run:
Clone the repository
Make sure you download and install docker desktop for windows (https://docs.docker.com/desktop/install/windows-install/)
Open command line in the clone directory.
docker build .
docker-compose build
docker-compose up
docker-compose down


Steps to create this project:
Create github project
Connect to docker hub (secrets)
1. Create user in docker hub.
2. Create token (user settings->security)
3. Go to secrets in github repository
4. Create secret: DOCKERHUB_USER: saarpart
5. Create secret: DOCKERHUB_TOKEN: token created in step 2

Create Docker container
docker build .
docker-compose build

Create django project in the container
Add DB and DB configuration (we use postgresql DB in this project)
Create a django application in the container:
docker-compose run --rm app sh -c "python manage.py startapp core"


Adding a model to the project:
------------------------------
i.e. Recipe
Add new models in core.models for the new Model

Register the new model in core.admin.py

Create the migration file for the new model:
docker-compose run --rm app sh -c "python manage.py makemigrations"

Create app to handle all the code for the model endpoints:
docker-compose run --rm app sh -c "python manage.py startapp <module name>"
From the project remove:
mogrations folder
admin.py
models.py
test.py

Create new folder tests and inside create __init__.py

Add the new model to app.settings.py inside INSTALLED_APPS.

Add tests to tests folder.

Add Serializer: add a file called serializers.py in inside define the model serializers

Add a view for the model in views.py. For CRUD (Create, Read, Update, Delete) use the viewsets. Else use the APIView.

Configure the endpoints in the urls.py file.
Create new file in the new app named urls.py.
Add the new urls in the app urls.py

To implement a detail recipe we need to:
Create a deiat serializers
Determine in the views.py which serializer to use (detailed or list). 
***Return a REFERENCE to the class and not an object!***

To execute it use the /api/docs/ interface.
First authenticate the user and then use the API.



Execute unit tests:
-------------------
make sure docker desktop is running.
docker-compose run --rm app sh -c "python manage.py test"
docker-compose run --rm app sh -c "python manage.py test && flake8"

Add endpoint:
docker-compose run --rm app sh -c "python manage.py startapp <name>"
remove unnecessary folders and files:
migrations
admin.py
tests.py
models.py
create folder tests
create inside tests __init__.py

Add new <name> to app/app/settings.py to INSTALLED_APPS.

Create new file in <name> called serializers.py



create DB migrations

Remove migrations in case of change:
In case of error in migrarion (InconsistentMigrationHistory)
docker-compose down
docker volume ls
docker volume rm <volume name>

Create superuser:
docker-compose run --rm app sh -c "python manage.py createsuperuser"

Working with the API:
---------------------
Start the server:
docker-compose up

Start a browser and type:
127.0.0.1:8000/api/docs/

Use the 'user' api to create/modify/delete users
Create user: 
Use the POST request on /api/user/create
Push the 'Try it out' button
Change the request body type to "multipart/formdata"
Update the fields and push the "Execute" button.

To authenticate a user:
Use the GET request on /api/user/token.
Provide user email and data enf push "Execute"
If the credentials are ok you should get a token.
Copy the token andpress the "Authorize" button on the top of the page.
Use the tokenAuth (3rd) method.
Enter "Token <paste the coppied token>" in the text box and press "Authorize"
From now any request shall contain the token.

Admin interface:
----------------
To create a super user for admin:
docker-compose run --rm app sh -c "python manage.py createsuperuser"

Use the admin:
Open browaser and type: 127.0.0.1:8000/admin/

Adding image uploading support:
-------------------------------
1. In Dockerfile:

Add dependencies to the python environment:
apk add --update --no-cache jpeg-dev 
apk add --update --no-cache --virtual .tmp-build-deps zlib zlib-dev (remove these 2 after installation)

2. Configure the project to use static files:
Create relevant folders for django-user:
mkdir -p /vol/web/media
mkdir -p /vol/web/static
chown -R django-user:django-user /vol
chmod -R 755 /vol

3. Run docker-compose build

4. Update docker-compose.yml file:
Add volume:
dev-static-data:
Add new volume to app volumes:
- dev-static-data:/vol/web

5. Add static configurations to settings.py:
STATIC_URL = '/static/static/'
MEDIA_URL = '/static/media/'

MEDIA_ROOT = '/vol/web/media'
STATIC_ROOT = '/vol/web/static'

6. Update the url.py:
from django.conf.urls.static import static
from django.conf import settings

After urlpatterns configuration add:
if settings.DEBUG:
    urlpatterns += static(
        settings.MEDIA_URL,
        document_root=settings.MEDIA_ROOT,
    )












