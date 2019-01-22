# Hello Servers

> nginx and apache are one of the prominent web servers

+ nginx is good at serving static files and forwarding of requests to another web server.


## A look at `/etc/nginx/nginx.conf`

+ The user `www-data` ensures that nginx is running (the same user name `www-data` is used in apache webserver too).

+ nginx.conf only defines how nginx has to run.

+ we include less folders with the configuration  files for easy management and fixes. So we remove the `/etc/nginx/conf.d/*.conf` line from the nginx.conf file. 

+ sites-enabled and sites-available defines the paths that are allowed in our server.

+ we open the default file inside the sites-enabled and delete the `index index.html ` since we want our own server side logic to handle the page requests and urls.

+ The keys like `location` , `proxy_pass` , `server_name` in the configuration file are called directives.

+ In the `/etc/nginx/sites-enabled/default` file we reconfigure the url path handlers to enable our own webserver (other than nginx) to handle them. So we add the below line within the `location /` block.

```nginx
proxy_pass http://127.0.0.1:8000/  ;
```

+ Everytime we update the configurations , we must restart the nginx service for the configurations to take effect.

```bash
service nginx restart

        or 

systemctl restart nginx
```

+ To specify a name for the server use the `upstream` directive like below (write this above the server directive in /etc/nginx/sites-enabled/default) .

    ```nginx
    upstream djangoapp{
        server 127.0.0.1:8000  ;
    }
    ```

    And use this djangoapp name while specifying the proxy_pass .
    ```nginx
    proxy_pass http://djangoapp/ ; 
    ```
    + When nginx service is restarted , we can now request the server again from the browser.


## Load Balancing :
+ The above upstream directive's use becomes clear when we have multiple servers running our djangoapp and we can distribute the load to all those servers or the same server on different ports using the below lines.

    ```nginx
    upstream djangoapp{
        server 127.0.0.1:8000 ; 
        server 127.0.0.1:3829;
        server 127.0.0.1:8002 ; 
        server 1.182.100.7:8000 ; 
        server 158.182.100.7:8008 ; 
    }
    ```
    + Now when we start the djangoapp on different machines or on different ports on the same machine , nginx automatically handles distributing the load to any of the servers present 

    + nginx uses Round Robin algorithm(and other algorithms) to do the load balancing and sends the request to the server if only its alive and thus handles failovers of servers.


<br>

----


<br>


## Containers : 

> ### Containers keep the same hardware and kernel.

**Containerization is :**
when we have an app like django , we package only the tools required to run django along with is code in a small box and keep it. 

+ An image is a representation of the operating system distro , python , django app code , command to run the program using which we can run our applications.

+ A container is an instance of the image and is run by docker.

+ Install docker and run `docker run hello-world` to pull the hello-world image from the docker hub.

+ A docker image removes the wireless drivers , display drivers , the kernel , ui  etc and so its brought all the way down to a very less size. 

+ A single host can run only 255 docker images.(254 actually since the host takes an ip address)

+ To pull a docker image : `docker pull debian`

+ To run the debian container
    ```docker
    docker run -it debian bash
    ```