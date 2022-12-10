# Create a container using Dockerfile


####1. Create the file static_web_application.py with below code
    ```
    from flask import Flask
    name = "My Name" # Please change the variable name
    port = 8000

    app = Flask(__name__)

    @app.route('/')
    def index():
        return f"Hello {name}"

    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=port)

####2. Create the Dockerfile next to static_web_application.py
```
FROM python:3.9-bullseye
RUN  pip install flask
COPY /static_web_application.py /
ENTRYPOINT ["python", "/static_web_application.py"]
```

####3. Build the container image locally
```
$ docker build --tag static_web_application .

        Sending build context to Docker daemon  3.072kB
        Step 1/4 : FROM python:3.9-bullseye
        ---> 850f8694d221
        Step 2/4 : RUN  pip install flask
        ---> Using cache
        ---> 605cca475fde
        Step 3/4 : COPY /static_web_application.py /
        ---> 86573cc0237c
        Step 4/4 : ENTRYPOINT ["python", "/static_web_application.py"]
        ---> Running in 8830d75213b1
        Removing intermediate container 8830d75213b1
        ---> 06c5aa662bb7
        Successfully built 06c5aa662bb7
        Successfully tagged static_web_application:latest
```

####4. Run the container using the local image
```
$ docker run -d -p 8000:8000  static_web_application
Output: 1d69c46884f8aba091a0bf0fb6045be7ddf8c29f5d55de610379ffc44ad2c4f6
```

####5. Test the container application in the ubuntu
```
$ curl 172.22.15.200:8000 "VM's IP"
RDSS
```
# Publish the web application image to DockerHub

Docker Hub repositories allow you to share container images with your team, customers, or the Docker community.
Docker images are pushed to Docker Hub through the docker push command. A single Docker Hub repository can hold many Docker images (stored as tags).

### Creating a repository
When you first create a Docker Hub user, you see a “Get started with Docker Hub.” screen, from which you can click directly into “Create Repository”. You can also use the “Create ▼” menu to “Create Repository”.

Before using the below commands for publishing the image, you need to login to Docker using ```$docker login```

### Pushing a Docker container image to Docker Hub
To push an image to Docker Hub, you must first name your local image using your Docker Hub username and the repository name that you created through Docker Hub on the web.

You can add multiple images to a repository by adding a specific :<tag> to them (for example docs/base:testing). If it’s not specified, the tag defaults to latest.

####FYI: We can use these method for naming our images: (Skip and execute the next step)

1. When you build them, using ```docker build -t <hub-user>/<repo-name>:<tag>```
2. (We will go with this) By re-tagging an existing local image ```docker tag <existing-image> <hub-user>/<repo-name>:<tag>```
```
$ docker tag 850f8694d221 shraddhaaaa/srana
```
3. By using ```docker commit <existing-container> <hub-user>/<repo-name>:<tag> to commit changes```
```
$ docker commit 72546b8078be shraddhaaaa/srana:test
sha256:349c2b364332def780157e77f309f80b308378fa72f415d019bb6aedcdf09216
```
#### Create a tag accordingly w.r.t your username
```
$ docker tag static_web_application xyz/static_web_application
```
xyz -> is the docker hub id here. Please change according to your docker id.

#### Now you can push this repository to the registry designated by its name or tag.

``` docker push <hub-user>/<repo-name>:<tag>```
```
$ docker push xyz/static_web_application

        Using default tag: latest
        The push refers to repository [docker.io/sukruthg/static_web_application]
        a5555e7ae127: Pushed
        5d976b11a9b8: Pushed
        bdcd94073ef2: Mounted from xyz/static_web_application
        c44cf16349ba: Mounted from xyz/static_web_application
        22323d4bd802: Mounted from xyz/static_web_application
        e6e9854ca999: Mounted from xyz/static_web_application
        397a239a053b: Mounted from xyz/static_web_application
        89c3244a87b2: Mounted from xyz/static_web_application
        80231db1194c: Mounted from xyz/static_web_application
        f1c1f2298584: Mounted from xyz/static_web_application
        ccba29d69370: Mounted from xyz/static_web_application
        latest: digest: sha256:449d8595fa6c5b3720ee7b39b58c5623cbdd20dbe36e21840c6df6da72cf4c6d size: 2636
```
You should see your image in your docker hub account.
The image is then uploaded and available for use
Example: https://hub.docker.com/repository/docker/xyz/static_web_application


#Test the Greeting web application form the DockerHub.

To download a particular image, or set of images (i.e., a repository), use docker pull. If no tag is provided, Docker Engine uses the :latest tag as a default.

####. Stop the earlier docker containers.
```
$ docker stop container_id.
```
####. Clean up docker containers from cache
```
$ docker container prune
```

####. Run the container from the docker hub image
```
$ docker run -d -p 8000:8000  xyz/static_web_application
Output: b4cf04a71ed92f3fc566b176def23bde7f41a959cb7ae21c7aaa92979dfbffec
```

#Test the running container
####1. Testing from the ubunt os

        $ curl 172.22.15.200:8000
        RDSS

####2. Test from the Windows PowerShell

        $ wget 172.22.15.200:8000
       
