# DAY 2 :  Hello Dev Ops
## Introduction : 

### Packages : 

+ Gist of package depends on  : 

    + What is an instruction ? 

    + Where is the instruction  ? 

+ `apt-get` is a dependency resolver while being a package manager.

+ `top` command gives the overview of processes running
+ `htop` command gives a better overview and interactivity and of processes 


+ Google's playstore update can add or change ips of its servers while updating . Likewise 


+ **`apt`** : 
    + `apt update` talks to the server and will just update the information about packages from repositories but doesnt actually change or upgrade the packages and (updates old ips with new ips of the servers if any).

    + `apt upgrade` will actually download and upgrade the package.

    + `apt search tree` will search for the package tree in the repositories.

    + `apt install tree` will install the package tree.

+ Every server that makes or runs software that we can use to download is called a repository.



---


## SSH and accessing our server : 
> SSH = Secure Shell :  Gives access to the remote machine through port 22

+ Ports : Logical pathways for the packets to pass.
    + ssh on port 22 
    + ftp on 21 
    + https on 443 are some default ports for those services.

+ To login to the remote server: 
    ```bash
    ssh root@<serveripaddress>
    ```

+ When we connect to a remote server for first time , the public key of that server is added to the known_hosts file.


+ when ssh hangs , use "`shift + ~ + .`"  to exit out of it.


## Inside the Server : 

+ Run a web server from the server machine (using something like django which might run the server which can only be accessed by `127.0.0.1:port` that is `localhost` )

+ Running the server using 
```python manage.py runserver 0.0.0.0:8000``` will make it accessible to all.


#### A small note on protocols :

+ TCP works on packets 
+ http is context aware with context info like if the content is an image , video , text etc