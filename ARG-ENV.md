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

we can use ARG command to set the build argument in the docker file and we can use it using  the ($) sign like

```js
COPY $VERSION .
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

We can use the environment variable in the Dockerfile with $ sign, as we used the build arguments

```js
EXPOSE $PORT_NUMBER
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

## Difference between Build Arguments and Environment variables

Build arguments and the environment variable both provides a level of flexibility but at the different phases. We cannot replace one with other, infact we can use them togather
to get higher flexibility.

Build arguments works on the BUILD phase, it has no impact on the run phase of docker where as the ENV variable declared in Dockerfile are available at build time and run time
and environment variable declared in environment file or directly passed to docker run command are avaiable at RUN phase.

Build arguments cannot be used in the application code to be containerised, but we can use environment variable in the application code.


#### For more details please refer to the official docker documentation: https://docs.docker.com/
