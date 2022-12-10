# Install docker 

## Switch to root user.
```$ sudo su```

## Ways we can install Docker
1. Option 1 (Recommended) 
Here we are using a known compatible docker version.
    ```
    {
    apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    apt-get update
    apt install docker.io=20.10.12-0ubuntu2~20.04.1
    }
    ```
2. Option 2

    ```
    apt-get update
    apt install docker.io
    ```
3. Option 3
Follow the instructions in this [document](https://docs.docker.com/engine/install/ubuntu/)


## Verify docker is installed properly

1. Check the docker version
    ```
    $ docker --version

    Docker version 20.10.12, build 20.10.12-0ubuntu2~20.04.1
    ```

2. Run the hello world example
    ```
    $ docker run hello-world

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
    3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/get-started/

    ```

3. Check the hello-world image
    ```
    $ docker images
    REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
    hello-world   latest    feb5d9fea6a5   14 months ago   13.3kB
    ```
