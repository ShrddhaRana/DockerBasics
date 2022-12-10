# Create dynamic web appliation in container which connects with redis container

### Create redis container

        $ docker run --name redis-database -d redis
        d7148dd2731106b0f24551304908a89f6ef6a9e60a36bff2fba15f136e5d87c5

Note: There is no port mapping done here.

### Find the ip address of the container

        $ docker inspect redis-database

        ****
        "IPAddress": "172.17.0.2"
        ****

### Build container dynamic web application image locally

1. Create the file dynamic_web_application.py with below code
```
from flask import Flask
import redis
import os

database_name = os.environ.get("DATABASE_NAME", "" )
database_port = os.environ.get("DATABASE_PORT", "6379" )

r = redis.Redis(host=database_name, port=int(database_port))
app = Flask(__name__)

@app.route('/')
def index():
        r.incr("Count")
        value = r.get("Count")
        return "Application running inside container\nTotal number of visitors for this page: " + str(value.decode()) + "\n"

if __name__ == "__main__":
        app.run(host='0.0.0.0', port=8000)
```

2. Create the file Dockerfile with below code
```
FROM python:3.9-bullseye
RUN  pip install flask redis
COPY /dynamic_web_application.py /
ENTRYPOINT ["python", "/dynamic_web_application.py"]
```

3. Build the container image locally
```
$ docker build --tag dynamic_web_application_using_environment_variables .

        Step 1/4 : FROM python:3.9-bullseye
        ---> 850f8694d221
        Step 2/4 : RUN  pip install flask redis
        ---> Using cache
        ---> 31aca7bd5c14
        Step 3/4 : COPY /dynamic_web_application.py /
        ---> a3ebd50b399d
        Step 4/4 : ENTRYPOINT ["python", "/dynamic_web_application.py"]
        ---> Running in 4a75bed9870c
        Removing intermediate container 4a75bed9870c
        ---> 6641c6d912bc
        Successfully built 6641c6d912bc
        Successfully tagged dynamic_web_application_using_environment_variables:latest
```
### Run the container dynamic web application built locally.

1. Run the container using local image
```
$ docker run -d -p 8000:8000  --env DATABASE_NAME=172.17.0.2 dynamic_web_application_using_environment_variables 
4cc82219c0bc843919a65c647d4f92b27e6b829d31e5b9f5291b7900b883d6e8
```
2. Test the container application in the ubuntu

        $ curl localhost:8000
        Application running inside container
        Total number of visitors for this page: 1

### Pubish the image to the Dockerhub repository
Before using the below commands for publishing the image, you need to login to Docker using ```$docker login```
    
1. Create a tag accordingly w.r.t your username 
        $ docker tag dynamic_web_application_using_environment_variables xyz/dynamic_web_application_using_environment_variables

xyz -> is the docker hub id here. Please change according to your docker id.

2. Publish the image 
```
$ docker push xyz/dynamic_web_application_using_environment_variables

        Using default tag: latest
        The push refers to repository [docker.io/xyz/dynamic_web_application_using_environment_variables]
        d5dc23524efe: Pushed
        2ed0cdec7d54: Pushed
        bdcd94073ef2: Mounted from xyz/static_web_application_using_environment_variables
        c44cf16349ba: Mounted from xyz/static_web_application_using_environment_variables
        22323d4bd802: Mounted from xyz/static_web_application_using_environment_variables
        e6e9854ca999: Mounted from xyz/static_web_application_using_environment_variables
        397a239a053b: Mounted from xyz/static_web_application_using_environment_variables
        89c3244a87b2: Mounted from xyz/static_web_application_using_environment_variables
        80231db1194c: Mounted from xyz/static_web_application_using_environment_variables
        f1c1f2298584: Mounted from xyz/static_web_application_using_environment_variables
        ccba29d69370: Mounted from xyz/static_web_application_using_environment_variables
        latest: digest: sha256:376ff6e07d285458e16bd1d1805fc46d2e59775d276fd09b26ff697ba9c74425 size: 2636
```
Example: https://hub.docker.com/repository/docker/xyz/dynamic_web_application_using_environment_variables

###  Run the container dynamic web application from DockerHub.

1. Stop the earlier docker containers.

        $ docker stop container_id.

2. Clean up docker containers from cache

        $ docker container prune

3. Run the container from the docker hub image

        $ docker run -d -p 8000:8000  --env DATABASE_NAME=172.17.0.2 xyz/dynamic_web_application_using_environment_variables 

        Output: c5f2e60b442c4ad0170cdd6dfcd60528293c7b23dff0e4e69bee455425ada9d7

4. Test the container application in the ubuntu

        $ curl localhost:8000
        Application running inside container
        Total number of visitors for this page: 2

