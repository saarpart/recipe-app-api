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

