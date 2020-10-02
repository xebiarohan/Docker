# Dockerising react application

In this blog we will learn about how we can containerise a react application. Here we will see the generic steps which we can use with any react application. Just for the
sake of complition we will start from the scratch, we will first create a react application and then will containerise it.


## Creating react application

```js
    npx create-react-app my-app
    cd my-app
    npm install
```

First command will create our project, second command is just moving inside the project and third command will download all the dependencies required for the
project.

Note : We need to have node to run these command.


## Creating Dockerfile

To containerise any application we need to have docker container, which is nothing but an instance of an docker image. We can get the docker images from dockerhub like
alpine, redis etc. Docker hub contains some common images but here we want to containerise our own appliaction So, we have to create our own image.

A image can be build out of a docker file.

So we will create a file named 'Dockerfile', this is important we should name it 'Dockerfile' only, we can use different name but then we have to do some extra steps
which we will see later in this blog. For now we are naming it 'Dockerfile' and we will create it inside out project 'my-app'. So it will be present next to
src, node_modules.


## What to add in a Dockerfile

#### Step 1: Adding base image

So lets imagine, if someone gives you a system and askes you to run a react application in that system. what is the first thing you need in that system.

The first thing you will need is the operating system, without that no system can work. After that you need to have node installed to run react application.

So to resolve all this problems we have a concept of base image in docker, which fulfills all these basic requirements, we selects the base image based on the
application we want to containerise. Here for react application we can use node:apline image. which provides us a linux operating system and node installed.

So the first line to add in Dockerfile :

```js
FROM node:alpine
```

#### Step 2: Adding working directory 

So now we verified that that we have a system with operating system and node installed in it. Now we have to copy our code in the system, for that we have to create
a new folder or select a exixting one.

Similarly, in docker file we specifies the folder where we want to store our application using command :

```js
WORKDIR /usr/app
```

If the folder does not exist then this command will create the folder, we can choose any folder we want.

#### Step 3: Copying application

After selection of the folder now we need to copy the application in the system.
Similarly we need to copy data in our docker image, for that we will add this command in our docker file 

```js
COPY ./ ./
```
It means copy all the folder in current directory where the docker file is present to the folder which we specified before (/usr/app).

#### Step 4: Running application specific commands

Now you coping the react application in the system, to run the application first we need to resolve the dependencies.
Similarly to resolve the dependencies in our react application present in docker image, we need to add execute :

```js
RUN npm install
```

We can have multiple RUN commands, here as per our requirement, we added only 1.

#### Step 5: Adding default comamnd

Now all we need to run the application in system is to execute the final run command.

So will make this command our default execution command, which will get executed when we will run any container using our image :

```js
CMD ["npm","start"]
```
We passes the command in array format.

Now our docker file is completed.

So the final dockerfile will look something like :


<Image>



## Building Dockerfile

As our dockerfile is ready, now we will build it to get our docker image. Open your terminal in the same folder where the Dockerfile is present and execute

```js
docker build .
```
Here dot represents the building context. 

-p is for the port mapping, it means any call coming to the local host on port 3000 will be redirected to the containers 3000 port.

When you will run this command, you will see Step 3 is taking comparitely more time then the others. we will try to fix this later in the blog.

So once it is done, you will get a image id, somthing like (1d88cc74aac8).


## Running docker image

Now we can run our docker image using docker image id :

```js
docker run -p 3000:3000 {image-id}
```

It will starts the server.
