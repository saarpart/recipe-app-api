# recipe-app-api
Recipe API project

How to build and run:
Clone the repository
Make sure you download and install docker desktop for windows (https://docs.docker.com/desktop/install/windows-install/)
Open command line in the clone directory.
docker-compose build
docker-compose up
docker-compose down


Steps to create this project:
Create github project
Connect to docker hub (secrets)
Create Docker container
Create django project in the container
Add DB and DB configuration (we use postgresql DB in this project)
Create a django application in the container:
docker-compose run --rm app sh -c "python manage.py startapp core"

Execute unit tests:
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


