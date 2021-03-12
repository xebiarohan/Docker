
## Docker-compose

It is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.


### Why Docker-compose?

Let say we want to conntainerise an application having Node backend, React-js frontend and Mongodb database. If we go by the simple approach then we need to first create a image of all the application individually then run those images. It a long and multi-step process that we have to perform. Similarly, if we have to stop the application then we have to stop all the containers individually. 

Docker-compose solves this issue. we can define the details of all the containers that we want to start in one yaml file and just executes one command to run the docker-compose file and thats it, docker-commpose file will take care of building the images, running the containers using those images, dependency between the containers, networking etc.


#### NOTE: Docker-compose file is not a replacement of the Dockerfiles, they solves two different purposes.


### Prerequisites

#### 1. Docker instllation
The primiary prerequisite is to have docker installed in your system. Execute the following command to check the docker version

```js
docker --version

OR

docker -v
```

If you get the proper version means docker is already installed in your system otherwise follow the below link to install the docker in your system

https://docs.docker.com/engine/install/

#### 2. Docker-commpose installation
We need to have docker-compose installed in our system. To check that if it is already installed, run the following command

```js
docker-compose -v

OR

docker-compose --version
```

If it is not already installed then follow the below link to install it in your system

https://docs.docker.com/compose/install/

#### 3. Basic docker knowledge
We dont need to be a docker expert to understand this article but you should have basic understanding of docker terminology like container, image, volume, Dockerfile etc.

#### 4. Basic YAML knowledge
As docker-compose is totally based upon the YAML file So, it makes it vert important to know how to read and write a basic YAML file.


## Suggestion : I would like to suggest you to use VS code and install Docker extension in it. It will add intellisense to the project and it will help you with proper suggestions of the docker-compose keys


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
version: "3.8"
```

To know more about versioning please refer to : https://docs.docker.com/compose/compose-file/

#### 2. services:
It is the main key under that we defines the details of all the containers that we want to run.

```js
version: "3.8"
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


#### 3. image:
It defines the image of the container that we want to run, it can be a local image that we created from a Dockerfile or it can be a existing image present in any repository. 

```java
version: "3.8"
services:
  mongodb:
    image: 'mongo'
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

If the image does not exist locally and we dis not specify the repository then it will by default checks the central docker repository. We can specify the custom repository like 

```js
image: 'custom-repository/mongo'
```

#### 4. volumes:
It is used to define all the volumes to be used in the container. We can specify all the volumes as a list under the volumes key.

```js
version: "3.8"
services:
  mongodb:
    image: 'mongo'
    volumes:
      - data:data/db
    ------------------
  node-app:
    ------------------
    ------------------
    ------------------
  react-app:
    ------------------
    ------------------
    
volumes:
  data:

```

Here 'data' is the named volume that is pointing to the 'data/db' folder of the container. It will persist all the data of 'data/db' folder on the local machine.

In the end of the docker-compose file we have to specify all the named volumes we are using in all the containers. Currently we are using only 1 named volume so, we specified it.

#### 5. environment:
It is used to specify the environment variables to be used in the application running inside that specific container. There are two ways of specifying the environment variables

```js
environment:
  MONGO_USERNAME: root
  
OR

environment:
  - MONGO_USERNAME=root
```

Both will work, but I suggest to use the first approach as it satisties the yaml contract conditions

```js
version: "3.8"
services:
  mongodb:
    image: 'mongo'
    volumes:
      - data:data/db
    environment:
      MONGO_USERNAME: root
      MONGO_PASSWORD: root
  node-app:
    ------------------
    ------------------
    ------------------
  react-app:
    ------------------
    ------------------
    
volumes:
  data:
```

#### 6. env_file:
In the last point we specified all the environment variables inside the docker-compose file, this approach is ok when we have few environment variables but if we have many variables then we can move those variables inside a environemnt variable file and can refer the path (relative path to the docker-compose file) of the file in the docker compose file.

First we will create a folder parallel to the docker-compose file (as shown in figure-1) then we will create a mongo.env file inside it  (name is upto you but the exxtension should be .env). We can create this file anywhere we want, but its a good practice to create it parallel to the docker-compose file to improve the readibility of the application and configuration.

we can take our environment variables and can place it in the mongo.env file as shown

```js
MONGO_USERNAME=root
MONGO_PASSWORD=root
```

Refer the address (relative to the docker-compose file) of the environmental file in the docker-compose file.

```js
version: "3.8"
services:
  mongodb:
    image: 'mongo'
    volumes:
      - data:data/db
    env-file:
      - ./env/mongo.env
  node-app:
    ------------------
    ------------------
    ------------------
  react-app:
    ------------------
    ------------------
    
volumes:
  data:
```

#### 7. networks:
The main purpose of the network is to run all the related containers in the same network, so that they can communicate without any hindrance. docker-compose file gives this functionality by default. It means all the containers running using a single docker-compose file can communicate with each other without specifying any other network details. 

But if you still wants to run your container in a particular network let say to make it compatible with the containers running using other docker-compose file then you can specify the network settings in the docker-compose file.


```js
version: "3.8"
services:
  mongodb:
    image: 'mongo'
    volumes:
      - data:data/db
    env-file:
      - ./env/mongo.env
    networks:
      - ecommerce-network
  node-app:
    ------------------
    ------------------
    ------------------
  react-app:
    ------------------
    ------------------
    
volumes:
  data:

```
