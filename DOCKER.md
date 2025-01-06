# Docker Cheatsheet


# Push Image
```$ docker build . -t <local tag>``` - [Build image and tag it](https://docs.docker.com/engine/reference/commandline/build/)  
```$ docker image tag <local tag> [<registry host>]/<registry name>/<tag>:<version>``` - [Tag image](https://docs.docker.com/engine/reference/commandline/push/)  
```$ docker login```  
```$ docker image push registry-host:5000/myadmin/rhel-httpd:latest``` - [Push Image](https://docs.docker.com/engine/reference/commandline/push/)

## Concepts
* Volume mounting - mounting a volume to the container
* Bind mounting - mounting a host directory to the container

# Commands
## Run a container
```bash
docker run <container name>
```
### Run a container interactive
``` bash
docker run --entrypoint /bin/bash -it <container name>
```
### Show not running containers
```bash
docker ps -f "status=exited"
```
---
## Find container log files on the host
```bash
sudo ls /var/lib/docker/containers/
```
---
# Configure docker proxy
<br><i>The Docker service proxy is configured differently from the system environment proxy.</i><br>

[Source](https://stackoverflow.com/questions/58841014/set-proxy-on-docker)

Look up the Docker service proxy:
```bash
docker info | grep -i proxy
```
The proxy configuration file is located at ```/etc/docker/daemon.json```:
```json
{
 "proxies": {
   "http-proxy": "http://10.0.0.20:3228",
   "https-proxy": "http://10.0.0.20:3228",
   "no-proxy": "localhost,127.0.0.0/8,10.0.61.61"
 },
 "insecure-registries" : [ "10.63.66.61:32703" ]
}
```
Here are the commands to update the Docker proxy configuration:
```bash
# Update the config file
sudo vi /etc/docker/daemon.json
systemctl daemon-reload
sudo service docker restart
# Make sure that the configuration has changed
docker info | grep -i proxy 

```

---
# Remove all containers and images. Clean all.
## Verify No Running Containers or Images
### Stop and Remove All Containers
```bash
docker stop $(docker ps -aq)         # Stop all containers
docker rm -v $(docker ps -aq)        # Remove all containers and associated volumes
```
### Remove All Images
```bash
docker rmi -f $(docker images -aq)   # Remove all images
```
## Prune All Unused Docker Data
```bash
docker system prune -af --volumes
```
## Manually Clean Data
### Stop the Docker Service
```bash
sudo systemctl stop docker
```
### Delete the Overlay2 Contents
```bash
sudo rm -rf /var/lib/docker/overlay2/*
```
### Start the Docker service
```bash
sudo systemctl start docker
```

# docker compose

## Override CMD
```
services:
  my-service:
    image: my-image
    entrypoint: "tail"
    command: ["-f", "/dev/null"]
```


