# Hello Containers 

## Docker : 

+ To get info on a `docker` command , use `--help`  
    ```
    docker run --help
    ```

+ View all the existing docker containers and thier status (either running or not ) using 
    ```
    docker ps -a
    ```
+ View all the currently running docker containers 
    ```
    docker ps
    ```
+ Start a docker container
    ```
    docker start -ai <containerNameOrId>
    ```

+ Stop a running docker container
    ```
    docker stop <containerNameOrId>
    ```
+ Create a new container from an image and run that container
    ```
    docker run -it 
    ```

+ Create a new container from an image and run that container
    ```docker
    docker run -it imageName /bin/bash

    #/bin/bash is the command it executes when it first starts the container
    ```
+ To execute a command on a running container or get access to a container shell use
    ```docker
    docker exec -it <containerNameOrId> <command>
    # Example : 
    # docker exec -it containername bash
    ```

+ To get info about a container like its IP address , gateway and other info or to inspect it , use
    ```docker
    docker inspect <containerNameOrId>
    ```
 
## Serving a Django app inside the container : 
1. Get a shell inside the created container using `docker exec -it container bash` .
2. Create a django project and run the django server on `0.0.0.0:8000` port.

3. add the ip address of this container (got from `docker inspect` at the parent server ) to the `/etc/nginx/sites-enabled/default` upstream directive.

    ```docker
    #Example :
    #/etc/nginx/sites-enabled/default

    upstream djangoApp{
        server 172.17.0.2:8000 ; 
    }
    ```
4. Restart the nginx server and check if the nginx server is able to correclty forward the requests to the djangoApp running in the container.