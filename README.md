docker-wordpress
================


This repo contains a docker-compose.yml file which stands up a basic wordpress site.

Architecture
------------
There are three containers:
* nginx: Serves as a frontend to php-fpm running on the wordpress container
* wordpress: Runs wordpress with php-fpm
* mysql: Database backend to be used for this instance of wordpress.  In production, the database should not be run in a container as it's stateful.

Containers
----------
The nginx container uses a custom site.conf file which is written over top of the `/etc/nginx/conf.d/default.conf` file.  When using `docker-compose`, the file can be copied into the container as a volume.  When running in Docker Swarm, the file will need to be built into the container.  The directory `nginx` contains a `Dockerfile` for this purpose.  Once built, the image must be pushed to either `Docker Trusted Registry` or [hub.docker.com](https://hub.docker.com/) to be used.  The `docker-stack.yml` file should be updated with the image that you have created.


HowTo Begin
-----------
* With `docker-compose`
```
git clone git@github.com:wlieberman/docker-wordpress.git
cd docker-wordpress
docker-compose up -f docker-compose.yml
```
* With Docker Swarm: Remember, we have to build the nginx image and modify the docker-stack.yml file with the image name:tag.
```
git clone git@github.com:wlieberman/docker-wordpress.git
cd docker-wordpress/nginx
docker build --rm -t <your_dockerhub_id>/wordpress:latest
docker push
cd ..
# EDIT THE docker-stack.yml FILE TO REPLACE IMAGE OF reverseproxy
docker stack deploy -c docker-stack.yml docker-wordpress
```
* With Kubernetes: TO BE CONTINUED....
