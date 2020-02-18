# Docker
Docker commands and important points


Image 
 - FS snapshot (File system)
 - Startup Command


docker commands :
 1. Creatinga and running a container from an image
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
 9. Execute an additional command in a container : 
	docker exec -it {container-id} {command}
 10. Getting shell or terminal access in container : 
	docker exec -it {container-id} sh
 11. Creating and running a container from an image : 
	docker run  -it {imageName} sh
 
 
 

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



	
