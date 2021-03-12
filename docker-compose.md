
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


### Suggestion : I would like to suggest you to use VS code and install Docker extension in it. It will add intellisense to the project and it will help you with proper suggestions of the docker-compose keys


### Creating docker-compose file

We can create docker-compose file in the top level of the project, it is not mandatory but it makes it easy to access the Dockerfiles present in the project. We must follow the standard of the file name, it must have 'docker-compose' as name and 'yaml' or 'yml' as extension.

![docker-compose-1](https://user-images.githubusercontent.com/13641422/110967804-3729e380-8370-11eb-9519-f1df24f4dac9.png)


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

Here, mongodb,node-app and react-app are the names of the services, dotted lines are showing the details of the services that we will discuss next. Every service
defines a single container. Name of the service is not equal to the name of the container.


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
It is used to define all the volumes to be used in a container. We can specify all the volumes as a list under the volumes key. We can add named volume, anonymous volume as well as bind-mounts under volumes key.

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

#### 8. build:
If we are building a docker image then there are two ways to use that in our docker-compose file. one, if we create the docker image using 'docker build' command and then use the name of the command in the image key of docker file as we saw in third point. The other approach is to define all the details required to build the image in the docker-compose file itself.

We can pass these details using the build key. In the most simple way we can just pass the path of Dockerfile to create the image in the build key

```js
build: ./backend
```
Dockerfile is present in the backend folder (See the folder structure in figure 1).

If the Dockerfile is present in the same directory where the docker-compose file is present then we can use dot (.)

```js
build: .
```

But sometimes we have more than 1 dockerfile for the same container based on the profile (dev,test, prod, etc). So in that case we have to provide the relative path as well as name of the Dockerfile.

```js
build: 
  context: ./backend
  dockerfile: Dockerfile-dev
```

context represents the directory that contains the Dockerfile and dockerfile key specifies the name of the file.


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
    build:
      context: ./backend
      dockerfile: Dockerfile-dev
    ------------------
    ------------------
  react-app:
    ------------------
    ------------------
    
volumes:
  data:
```

#### 9. ports:
It is used to expose the ports, It maps the port number of the host machine to the port numbers of the container. so when user want to interact with the container they can do so using the port numbers of host machine

```js
ports:
  - '3000:80'
  - '8080:8080'
```

Here 3000 is the port number of the host machine and 80 is the port number of the container. So when we hit the 3000 port on the local machine it will redirect the request to the container on port 80. We can use the same port numbers as well, I used different to show the difference properly. We can define multiple port mappings.

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
    build:
      context: ./backend
      dockerfile: Dockerfile-dev
    ports:
      - '80:80'
  react-app:
    ------------------
    ------------------
    
volumes:
  data:
```

#### 10. depends_on:
By default docker-compose tries to run all the services simuntaneously. If we have some dependency between the modules then we have to specifically mention the dependency like backend depend upon the mongodb, we need to run mongodb first then only backend can run. So we add depend on key in the backend service so that docker-compose will first start the mongodb first and then only try to start backend service.

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
    build:
      context: ./backend
      dockerfile: Dockerfile-dev
    ports:
      - '80:80'
    depends_on: 
      - mongodb
  react-app:
    ------------------
    ------------------
    
volumes:
  data:
```
A single service can depend upon multiple services. So we can define multiple services names in the depends_on list.

#### 11. container_name
As we disucssed in the second point that the name of the service is not equal to the name of the container. So, if we want to specify the name of the container then we can use container_name key

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
    container-name: mongodb
  node-app:
    build:
      context: ./backend
      dockerfile: Dockerfile-dev
    ports:
      - '80:80'
    depends_on: 
      - mongodb
    container-name: backend
  react-app:
    ------------------
    ------------------
    
volumes:
  data:
```

#### 12. stdin_open: and tty:
It is used to add the interactive mode in the container. It is equal to the -it flag that we use in the docker run command.

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
    container-name: mongodb
  node-app:
    build:
      context: ./backend
      dockerfile: Dockerfile-dev
    ports:
      - '80:80'
    depends_on: 
      - mongodb
    container-name: backend
  react-app:
    stdin_open: true
    tty: true
    ------------------
    
volumes:
  data:
```

So our final docker-file will look something like: 

![docker-compose-2](https://user-images.githubusercontent.com/13641422/110968080-8a039b00-8370-11eb-9a9d-abe443a204ce.png)



### Starting services using docker-compose

First in console, we have to navigate to the folder where the docker-compose file is present. Then there is only 1 basic command

```js
docker-compose up
```

This command will first check if all the images mentioned in the docker-compose file is present locally or not. If not then it will pull it from the docker repository and then it will automatically run it. By default docker-compose tries to run all the containers at the same time.

We can add some extra parameters in the given docker-compose up command

#### Detach mode (-d)
If we want to run our containers in detach mode then we can add '-d' in the docker-compose command.

```js
docker-compose up -d
```

#### Forcing image rebuild (--build)
If we want to rebuild the image every time we run the docker-compose file then we can use --build with the docker-compose up command

```js
docker-compose up --build
```
It will only rebuild the image from the Dockerfile present in the docker-compose file, docker-compose cannot rebuild the image passed with the image key.
like in our example we can rebuild the node-app image but cannot build the mongodb image.

If we just want to build all the images present in the docker-compose file and not want to run the containers then we can use

```js
docker-compose build
```


### Stopping services using docker-compose
We can stop all services and remove all the containers from local cache using.

```js
docker-compose down
```
It also removes the default network it created during the startup time but it will not remove the volumnes used in the docker-compose file by default. 

#### Removing volume while stopping services
 We can add -v in the docker-compose down command to remove all the volumes defined in the docker-compose file.

```js
docker-compose down -v
```


So thats all for this article, for more information about the docker-compose please refer to the official documentation:
https://docs.docker.com/compose/compose-file/
