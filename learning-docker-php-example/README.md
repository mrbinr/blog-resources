# docker-php

This is an example about how to create a docker container to use PHP.

**Makefile** will help you to manage the container

`make`will display this help

```bash
## -- Docker Node Makefile --  
help                           Outputs this help screen
## -- Docker containers --     
up                             Build the images and start the containers
down                           Stop the docker hub
start                          Start containers
stop                           Stop containers
## -- Composer --              
composer                       Run composer
## -- PHP --                   
php                            Run php command line
server-start                   Start PHP server
server-stop                    Stop PHP server
## -- Shell --                 
shell                          Connect to php container
shell-root                     Connect to php container
## -- Logs --                  
logs                           Show live logs
```

## How to start ?

If it is the first time, copy `.env.docker.dist` to `.env.docker` and set the values like

```
CONTAINER_PHP=my_php_container
PHP_IMAGE=php:8.2-alpine
PHP_PORT=9002
DOCKER_SUBNET=172.20.0.0/24
DOCKER_PHP_IP=172.20.0.5
DOCKER_USER=www-data
DOCKER_PROJECT_PATH=
```

Then, you have to build the container.

```bash
make up
```
Once it is build, you can stop it

```bash
make stop
```
and start it again

```bash
make start
```

## And now

Start server
```shell
make server-start
```

and you will be able to see configuration at `http://localhost:<PHP_PORT>`
