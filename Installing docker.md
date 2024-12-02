install gnome terminal with code:-` $ sudo apt install gnome-terminal
run update command:-```
``` 
$ sudo apt-get update
```
set up Docker's apt repository
Adding Docker's official GPG key:-``` $ sudo apt-get install ca-certificates curl
						
						$ sudo install -m 0755 -d /etc/apt/keyrings
						
						$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
						
						$ sudo chmod a+r /etc/apt/keyrings/docker.asc```
					```

Adding the repository to Apt sources:-

```
$ echo \
								
								"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
								$(. /etc/os-release && $VERSION_CODENAME") stable" | \
								sudo tee /etc/apt/sources.list.d/docker/list > /dev/null
								$ sudo apt-get update
```
Installing Docker packages:-`$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


Verify installation was successful with code:- `$ sudo docker run hello-world
If installed correctly should have an output with "Hello from Docker"

using https://wpengine.com/resources/containers-clusters-wordpress/

Define the project
use command:- `mkdir new_wordpress/
move to the directory using:- `cd new_wordpress/`

Create docker file for YAML
use command to create a text file:-`touch docker-compose.yml
use command to edit file:-`nano docker-compose.yml
add specified text:- `version: '3.3'  
```
```services:  
   db:  
     image: mysql:5.7  
     volumes:  
       - db_data:/var/lib/mysql  
     restart: always  
     environment:  
       MYSQL_ROOT_PASSWORD: somewordpress  
       MYSQL_DATABASE: wordpress  
       MYSQL_USER: wordpress  
       MYSQL_PASSWORD: wordpress  
   wordpress:  
     depends_on:  
       - db  
     image: wordpress:latest  
     ports:  
       - "8000:80"  
     restart: always  
     environment:  
       WORDPRESS_DB_HOST: db:3306  
       WORDPRESS_DB_USER: wordpress  
       WORDPRESS_DB_PASSWORD: wordpress  
       WORDPRESS_DB_NAME: wordpress  
volumes:  
    db_data: {}

```

Build the project
run command:-` sudo docker-compose up -d
wait for download to complete

error handling
add user to docker group:- `sudo usermod -aG $(whoami)`
restart session:- `newgrp docker
verify user is in docker group:- `groups`
check docker container status:- `docker ps`

accessing wordpress site
in browser enter the url:- http://localhost:8000
fill in the website name, username, password and email. 
log in and access the admin interface
