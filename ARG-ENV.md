## Docker Arguments and Environment variables

Docker support build time arguments and run time environment variables.

Docker provide different ways to inject the dynamic values during the build phase and the run phase. we can use Docker arguments to dynamically injecting the values 
at the build time and environment variable to inject values at run time.


## Docker build arguments

It is used to dynamically injecting the values during the build time. It gives us flexibilty to create an image in different modes. 
Build arguments are used when we need to create multiple images with a specific value difference. So we can pass that value as a build argument and can use the same docker
image.


These arguments can be used in different build steps but not in the CMD, as these are the RUN time commands and build arguments are not available at the RUN time. It is
specifically used for build phase.



### Defining build arguments in Dockerfile
we can add a argument step in the Dockerfile like

```js
  ARG VERSION=0.1
```

we can use ARG command to set the build argument in the docker file and we can use it using the $ or ${} like

```js
COPY $VERSION .
```
OR

```js
COPY ${VERSION} .
```


It will copy the 0.1 version from the host machine to the docker image at the WORKDIR. 

It is not mandatory to give the default value, we can just declare it as well like

```js
ARG VERSION
```


### Defining build argument in docker build command
We can override the default value of the ARG in the build command using --build-arg

```js
docker build -t hello-world:latest --build-arg version= 0.2 .
```

So it will copy the 0.2 version from the host file to the docker image.



### Defining multiple docker arguments

In dockerfile we can define multiple build arguments with multiple ARG commands like

```js
ARG VERSION
ARG PORT
```
and in docker build command we have to use multiple --build-arg

```js
docker build -t hello-world:latest --build-arg VERSION=0.2 --build-arg PORT=80 .
```

### Using Build Arguments in docker-compose file.

We can declare the build arguments in the Dockerfile and can set the value in the docker-compose file like In the Dockerfile we can add arguments like

```js
ARG VERSION=0.1
ARG PORT=8080
```

and can use it in the docker-compose file like

```js
build:
  context: .
  args:
    VERSION: 1
    PORT: 9090
```
docker-compose dynamically adds or updates the value of the build arguments declared in the Docker file. We can omit the values in the docker-compose file in that case it will pick the value present in the environment in which the docker-compose is running.

```js
build:
  context: .
  args:
    VERSION
    PORT
```

## Docker environment variables

Docker enviroment variables are used to make docker run command flexible, it gives the user flexibilty to change certain values at the run time. The environment variables can
be accessed in the application code as well. It provides a way to set application configuration at the time of running the application, configuration like database details,
port number to expose etc.

Every languages provides support for the environmental variables like 
In NODE

```js
process.env.PORT
```

In JAVA

```js
System.getenv("PORT")
```

### Defining environment variables in Dockerfile

we can directly define the environment variables in the dockerfile like 

```js
ENV PORT_NUMBER=8080
```

We can use the environment variable in the Dockerfile using the  $ or ${} sign, as we used the build arguments

```js
EXPOSE $PORT_NUMBER
```

OR 

```js
EXPOSE ${PORT_NUMBER}
```

we cannot override the default value of environment variable define in the Dockerfile at the build phase directly but we can use build arguments to do that. 

```js
ARG PORT

ENV PORT_NUMBER=$PORT
```

The value of build argument will determine the default value of the environment variable.


### Defining envionment variable in the docker run command

The other way to define the environment variable is to pass it in the docker run command using --env tag or -e tag.
 ```js
 docker run --env PORT_NUMBER=8080 <image-name>
 
 OR
 
  docker run -e PORT_NUMBER=8080 <image-name>
 ```
 
 If the PORT_NUMBER environment variable is already defined in the Dockerfile then it will override it.
 
 
 ### Defining multiple environment variables
 We can define multiple environment variables in the Dockerfile like
 
 ```js
 ENV PORT_NUMBER=8080
 ENV HOST_NAME=localhost
 ENV DB_NAME=MySQL
 ```
 
 Similarly, we can define multiple environment variable in the docker run command like
 
 ```js
 docker run -e PORT_NUMBER=8080 -e HOST_NAME=localhost -e DB_NAME= MySQL <image-name>
 ```
 
 but as we can see, the more the number of environment variable the readability of the command decreases. So, in these type of scenarios we can use environment variable file.
 
 ### Environment variable file
 
 It is used to separate the Dockerfile and environment variables, it gives us the flexibility to use different environment variables for running different container using the same Dockerfile.
 
We can create a file (best practice is to create with .env name) with any name you want at any location (best practice is to create at the Dockerfile level). In that file
we can direcly placce all our environment variable like:

```js
PORT_NUMBER=8080
HOST_NAME=localhost
DB_NAME=MySQL

```

We can use this environment variable file in the docker run command like:

```js
docker run --env-file ./.env <IMAGE-NAME>
```

### Environment variables in docker-compose file

Environment variables can be used in the docker compose file in several ways. The most common way is to add the environment variables int the file (.env), docker-compose file directly reads for it. The file must be present in the same level as the docker-compose file.

.env file

```js
TAG: 7.3
```

docker-compose file

```js
version: '3'
services:
  web:
    image: 'webapp:${TAG}'

```

But, we can have multiple environment like prod, dev, test, etc. In that case we need to build different files. In that scenario we have to use "--env-file" with the "docker-compose up" command to define the location of the environment file like

```js
docker-compose --env-file ./config/.env.dev up 

docker-compose --env-file ./config/.env.prod up 

docker-compose --env-file ./config/.env.test up 
```
Each command is referring to the different environment file.

We can pass the environment variables to containers in docker-compose like we use to do it in the "docker run" command using "-e" and "--env-file" option

```js
version:3
services:
  node:
    environment:
      - PORT: 9090
    
```
and

```js
version:3
services:
  node:
    env_file:
      - /config/.env.test 
```

We can have more than 1 environment file for the same service in the Dockerfile.

```js
version:3
services:
  node:
    env_file:
      - /config/.env.test1 
      - /config/.env.test2
```


We can pass the environment variable at the "docker-compose up" using the "-e" optiom

```js
version:3
services:
  node:
    environment:
      - PORT
```

```js
docker-compose run -e PORT=9090 node
```

It has a limitation, if two containers have the same environment variable then we cannot run both the containers using the "docker-compose run -e ..." command as it cannot populate the environment variable of both the service.

### Sample dockerfile using both Build arguments and Environment variable

```js
FROM node:alpine
ARG APP_DIR
WORKDIR $APP_DIR

COPY package.json .
RUN npm install

COPY . .

ENV PORT_NUMBER=80
EXPOSE PORT $PORT_NUMBER

CMD ["npm", "install"]

```

Here we are populating the working directory inside the docker image using the build arguments and exposing the port number using the environment variable (80 is the default port number that shall get exposed in case no value is provided in the 'docker run' command). 

## Difference between Build Arguments and Environment variables

Build arguments and the environment variable both provides a level of flexibility but at the different phases. We cannot replace one with other, infact we can use them togather
to get higher flexibility.

Build arguments works on the BUILD phase, it has no impact on the run phase of docker where as the ENV variable declared in Dockerfile are available at build time and run time
and environment variable declared in environment file or directly passed to docker run command are avaiable at RUN phase.

Build arguments cannot be used in the application code to be containerised, but we can use environment variable in the application code.


#### For more details please refer to the official docker documentation: https://docs.docker.com/
