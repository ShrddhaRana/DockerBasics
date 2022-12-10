# Connect container web application with redis container using the container name
In the default docker network, containers can connect using IP addresses only. 
In the custom docker network, containers can connect using either IP address or container name.

### Check existing docker networks

```
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
fd201b11f623   bridge    bridge    local
406c78b52df8   host      host      local
71e86a2b6951   none      null      local
```
docker network 'fd201b11f623' is the default network where containers are created.

### Create a new docker network.

```
$ docker network create dynamic_application
01021cc3845bd9adda21a3e42ee093f25b2869022f096f399584811e6e157a18
```

### Check whether new network is created.

```
$ docker network ls

NETWORK ID     NAME                           DRIVER    SCOPE
fd201b11f623   bridge                         bridge    local
406c78b52df8   host                           host      local
71e86a2b6951   none                           null      local
01021cc3845b   dynamic_application            bridge    local
```
`01021cc3845b` is the new bridge network is created.

### Inspect the new network.

```
$ docker network inspect dynamic_application
[
    {
        "Name": "dynamic_application",
        "Id": "01021cc3845bd9adda21a3e42ee093f25b2869022f096f399584811e6e157a18",
        "Created": "2022-11-25T06:42:19.173319597Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.26.0.0/16",
                    "Gateway": "172.26.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```
Containers created using the 'dynamic_application' network will have ip address starting from "172.26.0.*"

### Run the redis container on the dynamic_application network.

```
$ docker run --name redis-database -d --net dynamic_application redis
Output:843c5541ebd25d9cd46ccea213fd1e47033d623d59c78c7e3aa7f3d3bfc39d25
```

### [Optional]Findout container ip using docker inspect command
```
$ docker container inspect redis-database

***
"IPAddress" "172.26.0.2"
****
```

### Run the container dynamic web application from DockerHub with database name(instead of ip address) + new network(workshop_dynamic_application) 
```
$ docker run -d -p 8000:8000  --env DATABASE_NAME=redis-database --net dynamic_application xyz/dynamic_web_application_using_environment_variables 

Output: 35b49bbe65f7b857239dc57b782e0597703a09c9508a1ab25712dbef3abb8b01
``` 

### Test the container application in the ubuntu
```
$ curl localhost:8000
Application running inside container
Total number of visitors for this page: 1
```

