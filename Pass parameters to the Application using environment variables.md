# Passing parameters to the Application using environment variables.

After completing the exercise you must be comfortable using environment variables in Docker Applications

### Modify the static web application to use environment variables
    
1. Complete the step 1 to step 4 from this document: [here]([/docs/Day1/static_greeting_application.md](https://github.com/ShrddhaRana/DockerBasics/blob/main/Create%20a%20simple%20greeting%20web%20application%20using%20flask.md))
2. Create the file static_web_application_using_environment_variables.py with below code
```
import os
from flask import Flask

name = os.environ.get("YOUR_NAME", "Bye" )
port = 8000

app = Flask(__name__)

@app.route('/')
def index():
        return f"Hello {name}\n"

if __name__ == "__main__":
        app.run(host='0.0.0.0', port=port)
```

3. Set the evironment_variable
```
(python_venv)$ export YOUR_NAME=test
```
4.  Run the application

```
(python_venv)$ python static_web_application_using_environment_variables.py

* Serving Flask app 'static_web_application_using_environment_variables'
* Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
* Running on all addresses (0.0.0.0)
* Running on http://127.0.0.1:8000
* Running on http://172.22.15.200:8000
Press CTRL+C to quit
```

5. Open up other terminal in Ubuntu
```
$ curl http://127.0.0.1:8000
Hello test
```

You should the value test(or the value you used for creating exports)
6. Terminate the application


**Running the application in docker container**

1. Create the Dockerfile next to static_web_application_using_environment_variables.py
```
FROM python:3.9-bullseye
RUN  pip install flask
COPY /static_web_application_using_environment_variables.py /
ENTRYPOINT ["python", "/static_web_application_using_environment_variables.py"]
```
2. Build the container image locally
```
$ docker build --tag static_web_application_using_environment_variables .

Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM python:3.9-bullseye
---> 850f8694d221
Step 2/4 : RUN  pip install flask
---> Using cache
---> 605cca475fde
Step 3/4 : COPY /static_web_application_using_environment_variables.py /
---> 86573cc0237c
Step 4/4 : ENTRYPOINT ["python", "/static_web_application_using_environment_variables.py"]
---> Running in 8830d75213b1
Removing intermediate container 8830d75213b1
---> 06c5aa662bb7
Successfully built 06c5aa662bb7
Successfully tagged static_web_application_using_environment_variables:latest
```

3. Run the container using the local image
```
$ docker run -d -p 8000:8000  --env YOUR_NAME=INSIDE_CONTAINER static_web_application_using_environment_variables
Output: 1d69c46884f8aba091a0bf0fb6045be7ddf8c29f5d55de610379ffc44ad2c4f6
```

4. Test the container application in the ubuntu
```
$ curl 172.22.15.200:8000 "VM's IP"
Hello INSIDE_CONTAINER
```
### Publish the image to the Dockerhub repository
Before using the below commands for publishing the image, you need to login to Docker using ```$docker login```

1. Create a tag accordingly w.r.t your username 

```
$ docker tag static_web_application_using_environment_variables xyz/static_web_application_using_environment_variables
```

xyz -> is the docker hub id here. Please change according to your docker id.
2. Publish the image 
```
$ docker push xyz/static_web_application_using_environment_variables

Using default tag: latest
The push refers to repository [docker.io/xyz/static_web_application_using_environment_variables]
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
Example: https://hub.docker.com/repository/docker/xyz/static_web_application_using_environment_variables

### Running the application from the docker hub image

1. Stop the earlier docker containers.
```
$ docker stop container_id.
```

2. Clean up docker containers from cache
```
$ docker container prune
```

**3. Run the container from the docker hub image**
```
$ docker run -d -p 8000:8000  --env YOUR_NAME=INSIDE_CONTAINER_FROM_DOCKER_HUB sukruthg/static_web_application_using_environment_variables
Output: b4cf04a71ed92f3fc566b176def23bde7f41a959cb7ae21c7aaa92979dfbffec
```

**Test the running container**
1. Testing from the ubunt os
```
$ curl 172.22.15.200:8000
Hello INSIDE_CONTAINER_FROM_DOCKER_HUB
```

2. Test from the Windows PowerShell
```
$ wget 172.22.15.200:8000
```
