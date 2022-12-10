# Package the web application and Redis database using docker-compose
We will check the ip address of the containers created using docker-compose, also inspecting the network created by docker-compose.

### Install the docker-compose

``` 
$ pip install docker-compose
```

### Clean up all the containers

```
$ docker container ls

$ docker stop container_id/container

$ docker container prune
```

### Create docker-compose.yml file

```
version: "3.3"
services:
  dynamic_web_application:
    image: zyx/dynamic_web_application_using_environment_variables
    ports:
      - "8000:8000"
    depends_on:
      - "redis-database"
    environment:
      - DATABASE_NAME=redis-database

  redis-database:
    image: redis

```

### Check docker-compose file syntax
```
$ docker-compose -f docker-compose.yml config

services:
  dynamic_web_application:
    depends_on:
    - redis-database
    environment:
      DATABASE_NAME: redis-database
    image: xyz/dynamic_web_application_using_environment_variables
    ports:
    - published: 8000
      target: 8000
  redis-database:
    image: redis
version: '3.3'

```

### Run the docker-compose up

```
$ docker-compose -f docker-compose.yml  up -d

Creating network "docker-compose_default" with the default driver
Creating docker-compose_redis-database_1 ... done
Creating docker-compose_dynamic_web_application_1 ... done
```

### Check the new network created

```
$  docker network ls

NETWORK ID     NAME                     DRIVER    SCOPE
fd201b11f623   bridge                   bridge    local
823fbb35499d   docker-compose_default   bridge    local
406c78b52df8   host                     host      local
71e86a2b6951   none                     null      local
```
docker compose create a new network called docker-compose_default(bridge network)

### Test the dynamic web application

```
$ curl localhost:8000

Total number of visitors for this page: 1
```
