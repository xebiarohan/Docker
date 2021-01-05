Docker important points:

1. If you dont specify a CMD command in any dockerfile then CMD of base image will get executed on container creation.

2. Attach mode and Detach mode - When we run a container and if the terminal stuck after running the command it is known as the attach mode and if it completes the command let us
   enter another command then it is known as detach mode. 'docker run' is an attach mode command and 'docker start' is a detach mode command. We can detach the docker run command as well
   by using -d like
	docker run -p 8080:80 -d 4753947e5a59
	
3. And to attach again to a container
	docker container attach <container-name/id>	
	
4. To check the logs of a container	
	docker logs <container-name/id>
	
5. Running a stopped container in a attach mode (by default docker start runs in detach mode)
	docker start -a <container-name/id>
	
6. Running a continer in a interactive mode (attaching STDOUT and STDIN)
	docker run -p 3000:80 -it <image-id/name>		
	
7. Restarting a container in interactive mode
	docker start -a -i <container name/id>	
	
8. Removing all stopped containers
	docker container prune
	
9. Removing specfic containers
	docker rm containerName1 containerName2 containerNAME3 ...
	
10. Listing all the images
	docker images
	
11. Removing particular image
	docker rmi <image-id>

12. Removing all the non-active images (-a is used when we have some tagged images, otherwise command with -a will work)
	docker image prune -a
	

13. Automatically removes the container when it exits.
	docker run -d -p 3000:80 --rm <image-id>				
	
14. Inspecting an image or a container
	docker image inspect <image-id>
	docker container inspect <container-name/id>
	
15. Copying files inside a running container or getting files out of a running container
	docker cp source destination
Source can be any local file/folder in our system or it can be a file/folder in container, same for destination

	docker cp dummy/. container-name:/test
		
		OR
		
	docker	cp container-name/:test dummy
		

16. Adding a name to a container
	docker run -d -p 3000:80 --rm --name myContainer <image-id/name>
	
17. Adding a name/tag of a image. Tag name consist of image name and its version (name:version)
	docker build -t myimage:latest .
	
18. Changing the tag name of any image
	docker tag <old-name> <latest-name>		
example docker tag nodeapp:latest mynodeapp:latest

It will not remove the old image but it will create a clone of the old image.

19. To push our images to dockerhub, we first need to login to the docker hub using command
	docker login
    Similarly we can logout with
    	docker logout
    	
20. Container adds a new layer on top of the image (read write layer) where it stores all the temporary code generated in the application like any file creation etc.

21. Volumes help us persisting data, volumnes are folders on your host machine which are mapper into containers when running them. There is a on going relation between the folder on the host 
    machine and its corresponding folder in the container. Changes to one folder will get reflected into the other.
    
22. In docker we have 2 types of external data storage mechanisms volumes and bind mounts. Volume itself is of 2 types anonymous and named volumes.

23. Anonymous volumes get deleted once the container is removed, names volumes can survive container shutdown (removal from the container list).

24. We can only create anonymous volumes through dockerfile. named volumes get created through docker run command.
	docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes 
	
   Named volume is added as -v name of the volumne : data folder to store in the volume

25. Bind mounts helps in hot deployment, any changes made to the code present in the local machine will get reflected to the docker container.
	docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/rohan/Study/Docker/data-volumes-01-starting-setup/data-volumes-01-starting-setup:/app" feedback-   node:volumes 
	
	Here for bind mounts we use -v absolute path of the folder which we want to map : the folder in container where we want to map.
	
26. By default volumes are read-write, So any change we make to the folder inside a container will get reflected in out system and vice-versa. but for bind mounts we dont need this 
    behaviour. So we can make a volume read only by adding 'ro' indicator.
    	docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/rohan/Study/Docker/data-volumes-01-starting-setup/data-volumes-01-starting-setup:/app:ro" -v /app/temp  -v feedback-node:volumes 	
	
   
27. We can provide environment variables to be used in application from a dockerfile.
	ENV PORT 80	
	
28. Containers can communicate with WWW without any special setup. Any normal container can call any URL.	

29. Use host.docker.internal to call the localhost. it is translated to the IP address of the host machine as seen from inside the docker container.	

30. To make 2 container communicate with each other, the first solution is to inspect the ip address of the first container using 'docker container inspect <container-name>' and use it in
the second container.

31. Second option is to add --network in container run commnad, all the containers running in a same network can recognise each other using container name instead of the ip address. 
	docker run -d --name mongodb --network favorites-net mongo
	
32. Unlike volumes, network will not get created on there on, we need to first create a network then use it in the containers run command.
	docker network create favorite-net

33. Docker compose - Used to build and run multiple containers togather.

34. We dont need to attach detach (-d) or remove (--rm) because by default all the containers will get removed when we closes the compose file and -d we can add in the docker-compose running command.

35. To run all the containers/services in the docker-compose we need to run the docker-compose file using
	 docker-compose up

36. To start docker compose file in detach mode we can use 
	docker-compose up -d

37. To stop and remove all the containers present in the docker compose file we need to run 
	docker-compose down
 
38. The previous command will not remove the volume, to remove the volume as well we can use
	docker-compose down -v
	
39. we can force building of images whenever we run docker-compose file using
	docker-compose up --build
	
40. We can use exec to run extra command on docker container like opening a shell on the container
	docker exec -it <container-name/id> sh
	
41.		

	
	
	
	
	
	
	
	
	
	
	
	

		
