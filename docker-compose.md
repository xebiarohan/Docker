
## Docker-compose

It is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.


### Why Docker-compose?

Let say we want to conntainerise an application having Node backend, React-js frontend and Mongodb database. If we go by the simple approach then we need to first create a image of all the application individually then run those images. It a long and multi-step process that we have to perform. Similarly, if we have to stop the application then we have to stop all the containers individually. 

Docker-compose solves this issue. we can define the details of all the containers that we want to start in one yaml file and just executes one command to run the docker-compose file and thats it, docker-commpose file will take care of building the images, running the containers using those images, dependency between the containers, networking etc.


#### NOTE: Docker-compose file is not a replacement of the Dockerfiles, they solves two different purposes.


### Prerequisites

The primiary prerequisite is to have docker installed in your system. Execute the following command to check the docker version

```js
docker --version

OR

docker -v
```

If you get the proper version means docker is already installed in your system otherwise follow the below link to install the docker in your system

https://docs.docker.com/engine/install/


Second prerequisite is to have docker-compose installed. To check that if it is already installed, run the following command

```js
docker-compose -v

OR

docker-compose --version
```

If it is not already installed then follow the below link to install it in your system

https://docs.docker.com/compose/install/


### Creating docker-compose file

We can create docker-compose file in the top level of the project, it is not mandatory but it makes it easy to access the Dockerfiles present in the project. We must follow the standard of the file name, it must have 'docker-compose' as name and 'yaml' or 'yml' as extension.


```js
First Image

```

### Docker-compose keys description

Now we will see the details of all the docker-compose keys that can be used to specify the details of all the containers that we want to run. As docker-compose is a yaml file and in yaml file indentation, case-sensitivity matters. So we have to take extra care for all these small details.

#### 1. version:
Every docker-compose file starts with the version property. It specifies the version of docker-compose specifications we want to use. It affects the features we can use in our docker-compose file.

```js
version: '3.8'
```

To know more about versioning please refer to : https://docs.docker.com/compose/compose-file/

#### 2. services:
It is the main key under that we defines the details of all the containers that we want to run.

```js
services:
  mongodb:
    ------------------
    ------------------
    ------------------
  node-app:
    ------------------
    ------------------
    ------------------
  react-app:
    ------------------
    ------------------
```

Here, mongodb,node-app and react-app are the names of the containers that we want to run and dotted lines are showing the details of the container that we will discuss next.
