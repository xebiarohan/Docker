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
 

12 Docker run image-name = docker create image-name + docker start image-name

13 Container life cycle
	
	Create container
	Start container
	Stop container 
	Delete container (prune)
  

14 we can restart any stop container
	 
	 docker ps --all
	 docker start -a <id>



15 Creating custom docker image

	dockerFile -> docker CLI -> docker server -> docker image
	
we creates a docker file and put it to docker server through docker client. It changes the docker file to an docker image.
Docker file can be broke down into 3 parts : Base image, command to add more program to it and command which will get executed when image runs.

Example of Docker Image
	
	FROM alpine
	
	RUN apk add --update redis
	RUN apk add --update gcc
	
	CMD ["redis-server"]

16 Building a docker file
	
	docker build .

17 Running that docker file
	
	docker run {image-id}
	
Every step in docker file creates a temporary container on which the next steps runs and after execution of next steps this
temporary container gets removed ending up giving an image of final container with primary command.


18 Tagging an image :
	Tagging is important so that we dont have to remember the id of the image
	
	
	docker build -t {dockerId}/{name}:{version} .
	
eg
	
	docker build -t rohan230/redis:latest .
	
. refers to build context

19 Running a tagged image

	docker run {dockerId}/name

eg
	
	docker run rohan230/redis
	
By default it will pick the latest image tag. give tag number if required.


20 Copying files from system to temporary container created in docker file :

	COPY {source} {destination in container}
	
eg
	
	COPY ./ ./
	

21 Docker run with port mapping  :
	
	docker run -p {systemport number} : {container port number} {imageId}
	
eg
	
	docker run -p 8080:8080 rohan230/simpleweb
	
	
22 Setting relative folder in a container. So all the commands like COPY, RUN etc will be executede in this folder

	
	WORKDIR /usr/app


23 Docker compose

	It is a seperate CLI that gets installed along with docker.Its simmilar to the docker CLI just it added some extra
	features. It is used to start up multiple docker container at the same
	time. It also automates arguments which we pass with docker run command like -t for tag.
	
 for docker compose we need to create a docker-compose.yml file
 
 sample docker-compose.yml file :
 	
	   version: '3'
	   services: 
		redis-server:
		     image: 'redis'
		node-app:
		     build: .
		     ports: 
			- "4001:8081"                       //   4001 is system port and 8081 is containers port
	
All the containers created through a common docker-compose file can freely communicate with eachother.

24 Running containers which are configured in docker-compose file :

	docker-compose up
	
	
25 Merging building and running process in docker-compose :

	
	docker-compose up --build

26 Running containers in background

	docker-compose up -d
	
27 closing all the running containers :

	docker-compose down
	
28 In case one of out container wewnt down due to some reasons. we can restart the process or container
	we have 4 restart policies :
		
		"no"   // if container went down , dont try to restart it
		always  // if container went down , always attempt to restart it
		on-failure  // attempt restart only if it stops with an error message
		unless-stopped  // always attempt to restart unless we forcefully stop it
		
example how to use it

		version: '3'
		services: 
    		   redis-server:
        		image: 'redis'
    		   node-app:
        		restart: always
        		build: .
        		ports: 
            		   - "4001:8081"

29 To check all the containers running through docker-compose :
	
	docker-compose ps
	
30 Attaching termianal to any container stdin

	docker attach {docker-id}
	
31 nginx : used as a production server (help in communication between web browser and image files)

32 We can have more than 1 base images in docker file. (eg 1 for build process and 1 for run).

33 To build a different dockerfile in same project like for dev environment and for test environment
	docker build -f {dockerfileName} .

34 Travis-CI is a continuous integration tool.


