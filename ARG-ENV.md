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
  ARG VERSION 0.1
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
