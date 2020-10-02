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
It means copy all the folder in current directory where the docker file is present to the folder which we specified before (/usr/app). Make sure to delete node_module from the project which we are copying because we dont want to copy large noed_module package, we will create it again in container using
npm install command.

#### Step 4: Running application specific commands

Now you coping the react application in the system, to run the application first we need to resolve the dependencies.
Similarly to resolve the dependencies in our react application present in docker image, we need to add execute :

```js
RUN npm install
```

We can have multiple RUN commands, here as per our requirement, we added only 1.

#### Step 5: Adding startup comamnd

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


When you will run this command, you will see Step 4 is taking comparitely more time then the others. we will try to fix this later in the blog.

So once it is done, you will get a image id, somthing like (1d88cc74aac8).


## Running docker image

Now we can run our docker image using docker image id :

```js
docker run -it -p 3000:3000 {image-id}
```

It will starts the server.

-p is for the port mapping, it means any call coming to the local host on port 3000 will be redirected to the containers 3000 port.

-i is short for interactive and is used for opening connection to docker client (STDIN) -t is short for --tty, it allocates a pseudo terminal that connects your terminal with docker client for interaction. (STDIN and STDOUT).


<Image 2>




## Temporary containers

We saw when we first created our build file, it took some time to build the image but if we build the same image again it will take far less time then before

why ?

because Docker files uses temporary contianers while executing the docker file and stores these containers in the local cache. So when we build the same docker file again then it checks is there is any container present in local cache similar to the requirement. if yes, then directly returns that. we can see in the
logs as well.

<DOcker image 3>


In this image we can see the temporary docker container ids.

If we change something like a file in our application then that step will not use a cache container, it will create a new temporary container for that step
and for all the steps following that one.

I changed app.js file.

<Docker image 4>


Here we can see from step 3, docker created new temporary conainers, its good that docker can detect the changes but we have not made any change to our package.json so, there is no need to rerun the step 4 (updating dependencies).

NOTE : Docker will create new containers even if we change the scequence of steps in docker file.

## Refactoring docker file

As we saw we are running all the steps following the step where changes are made, as we cannot change the docker functionality but we can modidy our docker file
to overcome this problem.


<DOcker image 5>



Here we made one change, first we are copying the package.json file then running 'npm install' to resolve the dependencies and then we are copying the
remaining project.

So now if even if we make any change to the application file (other than package.json), npm install step is not going to run while deployment.

So lets check by making 1 change


<Docker-6>
    

Here we can see after making change to applications, only step 5 and following steps created new containers.


## Dockerizing react project for production.

To run a application in production first we need to make production version of the application using npm run build, it processes all the java script files, puts togather, process them in a single file.

In development container we have a development server, So whenever we make a request to port 3000, it redirects the request to this development server and this server interacts with our applicaiton present in the container and returns the response to the browser.

But this development server does not exist in our production environment as we are not making any more changes to the code, we just compress our code and made a 
build out of it. 

So we need some server which can help in interaction between this build and browser. So we will use Nginx.

### Nginx
Nginx is a very popular web server, there is nnot much logic associated with it. it is just use for routing the traffic in and out of the application.

## Builing production dockerfile
So lets again understand this with an example, what you will do to deploy a application in production in a system. 

I guess, first you will create a production build file using (npm run build) then you will pass this file to a production server and starts it. Simple right ?

Same we are going to do in our docker file, first we will write steps to first do a build and then pass this build to a production server (nginx) and starts the
server.

So our docker file have to do 2 tasks, the first one is to create a build and the second one is copy the build in nginx.


<Docker-image 7>


Here we divided our docker file in 2 parts for the mentioned 2 tasks.

So we can use the same command to build the docker file

```js
docker build .
```
It will give the image id which we will use in next step.

## Running production build

Now we need to map the localhost port with the default port of nginx which is 80.

```js
docker run -it -p 8080:80 {image-id}
```


<Docker-8>



