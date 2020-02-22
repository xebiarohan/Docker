# Docker
Docker commands and important points


Image 
 - FS snapshot (File system)
 - Startup Command


docker commands :
 1. Creating and running a container from an image :
 		
		docker run image-name <alternate_command>

 2. Listing all the current containers : 
 
		docker ps
 3. Listing all the containers which ever run on the system : 
 
		docker ps --all
 4. Creating a Container : (will give 1 unique id) : 
		
		docker create <image-name>
 5. Starting a container: (-a is to watch the output and print on terminal) : 
 
		docker start -a {container-id}
 6. Deleting all the stopped containers and removing all the images from cache : 
		
		docker system prune
 7. Checking logs of a container : 
 
		docker logs {container-id}
 8. Stopping a container : 
 
		docker kill {container-id} / docker stop {container-id}
	
	Difference between docker kill and docker stop :
	
		In docker stop system sends SIGTERM signals, which may takes some time to stop the container,
		while in case of kill command SIGKILL signal is sent, which kills the process immediately.
		In case of stop command if it doesnot stop in 10 sec , it will automatically calls the docker 
		kill command.
	
 9. Execute an additional command in a container : 
 
		docker exec -it {container-id} {command}
		
 10. Getting shell or terminal access in container : 
 
	docker exec -it {container-id} sh
	
 11. Creating and running a container from an image : 
 	
	docker run  -it {imageName} sh
 
 
 it flag is equal to -i -t flag 
 	
	-i is used so that the process can understand command which we enters in terminal throughh STDIN
 

docker run image-name = docker create image-name + docker start image-name

 Container life cycle
  1. Create container
  2. Start container
  3. Stop container 
  4. Delete container (prune)
  

  we can restart any stop container
	 
	 docker ps --all
	 docker start -a <id>



Creating custom docker image

	dockerFile -> docker CLI -> docker server -> docker image
	
we creates a docker file and put it to docker server through docker client. It changes the docker file to an docker image.
Docker file can be broke down into 3 parts : Base image, command to add more program to it and command which will get executed when image runs.

Example of Docker Image
	
	FROM alpine
	
	RUN apk add --update redis
	RUN apk add --update gcc
	
	CMD ["redis-server"]

Building a docker file
	
	docker build .

Running that docker file
	
	docker run {image-id}
	
Every step in docker file creates a temporary container on which the next steps runs and after execution of next steps this
temporary container gets removed ending up giving an image of final container with primary command.


Tagging an image :
	Tagging is important so that we dont have to remember the id of the image
	
	
	docker build -t {dockerId}/{name}:{version} .
	
eg
	
	docker build -t rohan230/redis:latest .
	
. refers to build context

Running a tagged image

	docker run {dockerId}/name

eg
	
	docker run rohan230/redis
	
By default it will pick the latest image tag. give tag number if required.


Copying files from system to temporary container created in docker file :

	COPY {source} {destination in container}
	
eg
	
	COPY ./ ./
	

Docker run with port mapping  :
	
	docker run -p {systemport number} : {container port number} {imageId}
	
eg
	
	docker run -p 8080:8080 rohan230/simpleweb
	
Setting relative folder in a container. So all the commands like COPY, RUN etc will be executede in this folder

	
	WORKDIR /usr/app




	
