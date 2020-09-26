# Important docker commands


## What is Docker ?

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host.

## What is a docker image and docker container ?

As per the official website a Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

And docker container is a instance of docker image, docker container is a docker image brought to life.

This article is more about the docker commands that we all should know while working on it rather than what and why.


## Breakdown of docker commands syntax:

Every docker command can be break down into 3 parts : 

    1. Keyword 'docker'
    2. Main task (start stop, build etc)
    3. Reference to image or container with all the property flags.

Every docker command starts with 'docker' keyword.

2nd part of the command represents the main task which we want to execute on docker image or on docker container (like run for running a container, build for
building an image etc)

The third part is the reference to the image or container with there propertues like the container id, image-name etc and property flags like port number mapping, volumes, tags etc. A flag with simgle dash (-) is the shourcut for the full name flag like (-p for --port) and double dash is used for full name


## Important Container commands : 

### Syntax : docker {task} {image-id/ image-name/container-id}

#### Creating a container :

```docker
docker create {image-id/image-name}
```
Creates a container from a docker image. It returns a container id, which can be used for starting that container.


#### Starting a container

```docker
docker start {container-id}
```
Starts a container, but this command will not return any logs to the docker-CLI. To get the logs we need to add -a flag like

```docker
docker start -a {container-id}
```

#### Running a continer

```docker
docker run {image-id/image-name}
```
Creates and starts the container, it is a combination of docker create and docker start command and there is no need of adding any external flag to get the
logs on docker CLI.


#### Listing all running containers

```docker
docker container ls
```
Lists all the running containers. There is an alternative command to list all the running container

```docker
docker ps
````

#### Listing all container

```docker
docker ps --all / docker ps -a
```
Lists all the container that includes running containers, stopped containers and newly created containers.

#### Inspecting container

```docker
docker inspect {container-id}
```
Returns all the information about the container like source, image used to create this container, restart count, paltform, current state etc.

#### Checking logs

```docker
docker logs {container-id}
```
Prints the logs on the terminal. It can be used in the scenarios when you used <b> docker start {container-id}</b> command and forgot to add <b> -a </b> tag.

#### Stopping container

```docker
docker stop {container-id}
```
Stops the running docker container gracefully. It takes its time to do the clean up work and then stops the container, which is less than 10 seconds because if
the cleanup work is taking more than 10 seconds then it runs the kill command.

#### Killing container

```docker
docker kill {container-id}
```
Stops the container abruptly, instant stopping of container.

#### Removing container

```docker
docker rm {container-id}
```
Deletes a stopped container, We cannot remove a running container, first we have to stop/kill the container then only we can delete or remove that container.



## Important image commands :


#### Building an image
Most basic command to build an image from a docker file. This . represents the build context, we can use this dot only when we are executing command in the same directory where the Dockerfile is present. It will return the build number.

```docker
docker build .
```

But the build number is not that easy to remember, So we can add a tag to give a name to out build.

```docker
docker build -t {username}/my_build:{tag} .
```
Here username is the username of the dockerhub account, my_build is the name of the image which we are building and tag is the version of the build and 
dot is again present for the build context.

We can add more things liks port, volume etc in the build command

#### Pushing image to registry

```docker
docker push {image-id/ image-name:{tag}}
```
Adds our image to the our remote repository. So that we can pull it every time we need, Its like commiting our image to remote reposotiry.

#### Listing all images

```docker
docker image ls
```

Lists all the images present in your system that includes both pulled images from docker hub repository and local created images from docker files.


