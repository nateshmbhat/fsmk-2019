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

+ To download a new image from django hub
    ```docker
    docker pull <imagename>
    ```

+ To see all the images present in docker
    ```docker
    docker image ls
    ```
+ To see all the containers present in docker
    ```docker
    docker container ls
    ```
+ To see the logs or output of a container (background container)
    ```docker
    docker logs <containerNameOrId>
    ```


 
## Serving Django apps inside the container and Load Balancing : 
1. Get a shell inside the created container using `docker exec -it container bash` .
2. Create a django project and run the django server on `0.0.0.0:8000` port.

3. add the ip address of this container (got from `docker inspect` at the parent server ) to the `/etc/nginx/sites-enabled/default` upstream directive.

    ```docker
    #/etc/nginx/sites-enabled/default

    #Example :

    upstream djangoApp{
        server 172.17.0.2:8000 ; 
        server 172.17.0.3:8000 ; 
        server 172.17.0.4:8000 ; 
    }
    ```
4. Restart the nginx server and check if the nginx server is able to correclty forward the requests to the djangoApp running in the container.

5. To automate all this creation of django project and running the server in every container , we make make file "Dockerfile " with the following lines in the file `Dockerfile` : 

    ```docker
    # ~/Dockerfile

    FROM debian:latest

    LABEL maintainer="maintainerName"

    RUN apt update -y 
    RUN apt install -y python-pip

    RUN pip install django

    RUN cd /root/ && django-admin startproject camp 

    WORKDIR /root/camp/

    EXPOSE 8000

    CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
    ```

6. Now we build the new docker image using `docker build` which contains all the configurations and packages which we added in the Dockerfile.

    ```docker
    docker build -t tagname .

    # execute the above in the folder containing the "Dockerfile" . Mind the dot at the end of the command.
    ```

7. Once the docker image is built (which you can verify using `docker image ls`) , then create a new container using this image and run the container as detached demon process.
    ```docker
    docker run -d <newlyCreatedImageName>
    ```

8. Now inspect the new container's ip address and write the ip addresses in the `/etc/nginx/sites-enabled/default` in the upstream directive (just like in step 3) . Thus we can load balance among various django servers that are running inside different containers.



#### MISC : 
+ HA_Proxy : High Availibility , A tool used in industry to handle the load balancing in an more advanced way .



# Security : 
One of the most used tools in industry for auditing security is `LYNIS` .

> Lynis is a battle-tested security tool for systems running Linux, macOS, or Unix-based operating system. It performs an extensive health scan of your systems to support system hardening and compliance testing. The project is open source software with the GPL license and available since 2007.


Download Link :

```
wget https://downloads.cisofy.com/lynis/lynis-2.7.0.tar 
```

+ We configure ssh to run on a different port other than port 22 by editing in `/etc/ssh/sshd_config` and restart the sshd server.

+ We create a new ssh user and and we use only that user account while connecting from ssh next time.

    ```bash
    adduser sshuser
    ```
    ```bash
    adduser superuser sudo
    ```
+ disable PermitRootLogin from the `sshd_config` file and add the line `AllowUsers sshuser` at the end of the `sshd_config` file.

+ `sshd -t` command will test the config file for any errors. If all is well , then restart the sshd service.

+ After restarting our `sshd` server , from now on  , we use the non superuser `sshuser` to login to the ssh server first.


    ```bash
    ssh sshuser@<ipaddress> -p <newport>
    ```

    + Once we are inside our server as `sshuser` , we can now login from there to the `superuser` to get the admin rights.

        ```bash
        su - superuser
        ```
    
### FireWall : 
> Firewall is a service that has the ability to allow or deny access to any ports on the system
+ We install ufw (uncomplicated fireWall) using
    ```bash
    apt install ufw     
    ```
+ TO see ufw status
    ```bash
    ufw status
    ```

+ To allow any http requests through this firewall
    ```bash
    ufw allow http
    ufw allow 20000/tcp
    ```
    + To restart ufw
        ```bash
        ufw disable ; ufw enable ;
        ```



#### Closing : 
+ Gnome-boxes is a lightweight alternative to virtualbox