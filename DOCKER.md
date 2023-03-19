# Docker Cheatsheet


# Push Image
```$ docker build . -t <local tag>``` - [Build image and tag it](https://docs.docker.com/engine/reference/commandline/build/)  
```$ docker image tag <local tag> [<registry host>]/<registry name>/<tag>:<version>``` - [Tag image](https://docs.docker.com/engine/reference/commandline/push/)  
```$ docker login```  
```$ docker image push registry-host:5000/myadmin/rhel-httpd:latest``` - [Push Image](https://docs.docker.com/engine/reference/commandline/push/)

## Concepts
* Volume mounting - mounting a volume to the container
* Bind mounting - mounting a host directory to the container

