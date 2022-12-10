# Run the nginx server in the docker
Intent of this activity to get an understanding of docker images, containers, logging into docker and port mapping.


## Obtain the docker image (docker pull)

Pull an image or a repository from a registry. More details follow [link](https://docs.docker.com/engine/reference/commandline/pull/)


1. Check the list of images(Optional step)
   ```
   $ docker images

   REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
   hello-world   latest    feb5d9fea6a5   14 months ago   13.3kB
   ```

2. Remove the existing images(Optional step)
     -  Why does image removal fails?
        ```
        $ docker image rm hello-world

        Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container 4d3cdec74233 is using its referenced image feb5d9fea6a5
        ```
        You cannot remove the image until the referenced container is removed.
        From the description you can see the container id is 4d3cdec74233
     - Remove reference container
       ```
       $ docker container rm 4d3cdec74233

       4d3cdec74233
       ```
       After removing the container, it prints the container id.
     - ```
       $ docker image rm hello-world

        Untagged: hello-world:latest
        Untagged: hello-world@sha256:faa03e786c97f07ef34423fccceeec2398ec8a5759259f94d99078f264e9d7af
        Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
        Deleted: sha256:e07ee1baac5fae6a26f30cabfe54a36d3402f96afda318fe0a96cec4ca393359
        ```
    Activity1: Try remove docker images using image id instead of repository

3.  Pull the nginx server
    - https://hub.docker.com/_/nginx
    - ```
      $ docker pull nginx

        Using default tag: latest
        latest: Pulling from library/nginx
        a603fa5e3b41: Pull complete
        c39e1cda007e: Pull complete
        90cfefba34d7: Pull complete
        a38226fb7aba: Pull complete
        62583498bae6: Pull complete
        9802a2cfdb8d: Pull complete
        Digest: sha256:e209ac2f37c70c1e0e9873a5f7231e91dcd83fdf1178d8ed36c2ec09974210ba
        Status: Downloaded newer image for nginx:latest
        docker.io/library/nginx:latest
      ```
    - verify the image is pulled
      ```
      $ docker images

      REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
      nginx        latest    88736fe82739   5 days ago   142MB
      ```

      nginx image is successfully pulled

## Run the nginx image (docker run)

In a new container, run a command. More details follow [link](https://docs.docker.com/engine/reference/commandline/run/)

      $ docker run nginx

      /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
      /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
      /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
      10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
      10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
      /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
      /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
      /docker-entrypoint.sh: Configuration complete; ready for start up
      2022/11/20 14:49:33 [notice] 1#1: using the "epoll" event method
      2022/11/20 14:49:33 [notice] 1#1: nginx/1.23.2
      2022/11/20 14:49:33 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6)
      2022/11/20 14:49:33 [notice] 1#1: OS: Linux 5.4.0-132-generic
      2022/11/20 14:49:33 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
      2022/11/20 14:49:33 [notice] 1#1: start worker processes
      2022/11/20 14:49:33 [notice] 1#1: start worker process 29
      2022/11/20 14:49:33 [notice] 1#1: start worker process 30

## Verify nginx is working fine inside the container

- Open another terminal to login to container
    Get the container id
    ```
    $ docker container ls
    CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
    c064d3f97664   nginx     "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   80/tcp    cool_cray
    ```
    Container ID: c064d3f97664
    Container Name: cool_cray
- Login to container
    ```
    $ docker exec -it c064d3f97664 bash
    root@c064d3f97664:/#
    ```
    `docker exec' runs a command in a running container. For more details [link](
    https://docs.docker.com/engine/reference/commandline/exec/)

- Check the localhost:80 inside the container
    ```
    $ curl localhost:80
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
    html { color-scheme: light dark; }
    body { width: 35em; margin: 0 auto;
    font-family: Tahoma, Verdana, Arial, sans-serif; }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>
    ```
- Exit the container
  ```$ exit```    

## Verify nginx server is working from the Ubuntu OS

1. Get the ip of the container
   - Login to the container
        ```
        $ docker exec -it c064d3f97664 bash
        root@c064d3f97664:/#
        ```
        `docker exec' runs a command in a running container. For more details [link](
        https://docs.docker.com/engine/reference/commandline/exec/) 
   -   ```
       $ hostname -I
       172.17.0.2
       ```
   -   ```
       $ exit
       ```
2. Run the curl command from the Ubuntu OS
   ```
   $ curl 172.17.0.2:80

    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
    html { color-scheme: light dark; }
    body { width: 35em; margin: 0 auto;
    font-family: Tahoma, Verdana, Arial, sans-serif; }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>
    ```
3. Activity on Ubuntu OS: Try using the docker name instead of ip in the curl command

## Port mapping

1. Host machine(Windows)
   - check access to the nginx container from Windows PowerShell
        ```
        $wget 172.21.76.130:80
        wget : Unable to connect to the remote server
        ```
        172.21.76.130 is the ip of the Ubuntu OS
2. Ubuntu OS
    - Stop the nginx container
        ```
        $ docker stop c064d3f97664
        ```
    - Remove the nginx container
        ```
        $ docker container rm c064d3f97664
        ```
    - Enable port forwarding 
       ```
       $ docker run -p 8000:80 nginx

        /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
        /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
        /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
        10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
        10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
        /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
        /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
        /docker-entrypoint.sh: Configuration complete; ready for start up
        2022/11/20 15:39:14 [notice] 1#1: using the "epoll" event method
        2022/11/20 15:39:14 [notice] 1#1: nginx/1.23.2
        2022/11/20 15:39:14 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6)
        2022/11/20 15:39:14 [notice] 1#1: OS: Linux 5.4.0-132-generic
        2022/11/20 15:39:14 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
        2022/11/20 15:39:14 [notice] 1#1: start worker processes
        2022/11/20 15:39:14 [notice] 1#1: start worker process 28
        2022/11/20 15:39:14 [notice] 1#1: start worker process 29
       ```
3. Host machine(Windows)
    - ```
        $wget 172.21.76.130:8000
        statusCode        : 200
        StatusDescription : OK
        Content           : <!DOCTYPE html>
                            <html>
                            <head>
                            <title>Welcome to nginx!</title>
                            <style>
                            html { color-scheme: light dark; }
                            body { width: 35em; margin: 0 auto;
                            font-family: Tahoma, Verdana, Arial, sans-serif; }
                            </style...
        RawContent        : HTTP/1.1 200 OK
                            Connection: keep-alive
                            Accept-Ranges: bytes
                            Content-Length: 615
                            Content-Type: text/html
                            Date: Sun, 20 Nov 2022 15:39:24 GMT
                            ETag: "634fada5-267"
                            Last-Modified: Wed, 19 Oct 2022 ...
        Forms             : {}
        Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 615], [Content-Type,
                            text/html]...}
        Images            : {}
        InputFields       : {}
        Links             : {@{innerHTML=nginx.org; innerText=nginx.org; outerHTML=<A href="http://nginx.org/">nginx.org</A>;
                            outerText=nginx.org; tagName=A; href=http://nginx.org/}, @{innerHTML=nginx.com;
                            innerText=nginx.com; outerHTML=<A href="http://nginx.com/">nginx.com</A>; outerText=nginx.com;
                            tagName=A; href=http://nginx.com/}}
        ParsedHtml        : mshtml.HTMLDocumentClass
        RawContentLength  : 615
     ```

## Stretch

1. Container
    - Login to the container
    - ```
      $ cd /usr/share/nginx/html/
      $ "echo "Hello from nginx server" > index.html
      ```
2. Host
    ```
    wget 172.21.76.130:8000


    StatusCode        : 200
    StatusDescription : OK
    Content           : Hello from nginx server

    RawContent        : HTTP/1.1 200 OK
                        Connection: keep-alive
                        Accept-Ranges: bytes
                        Content-Length: 24
                        Content-Type: text/html
                        Date: Sun, 20 Nov 2022 15:46:48 GMT
                        ETag: "637a4be2-18"
                        Last-Modified: Sun, 20 Nov 2022 15...
    Forms             : {}
    Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 24], [Content-Type,
                        text/html]...}
    Images            : {}
    InputFields       : {}
    Links             : {}
    ParsedHtml        : mshtml.HTMLDocumentClass
    RawContentLength  : 24 
    ```
    Content is updated.
