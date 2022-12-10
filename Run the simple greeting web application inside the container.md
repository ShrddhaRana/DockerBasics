# Run the static greeting application on the containers

### Switch to root user.
```$ sudo su```

### Create and login to the container

```1. $ docker run -it  -p 9000:8000 python:3.9-bullseye bash```

```
Unable to find image 'python:3.9-bullseye' locally
3.9-bullseye: Pulling from library/python
a8ca11554fce: Pull complete 
e4e46864aba2: Pull complete 
c85a0be79bfb: Pull complete 
195ea6a58ca8: Pull complete 
157f16ed0a0c: Pull complete 
884b144bec28: Pull complete 
dc4ed63cc6ae: Pull complete 
aa6a7fdf543b: Pull complete 
42aabb4354c6: Pull complete 
Digest: sha256:c1613835d7be322f98603f356b9e0c9d40f9589e94dc9f710e714a807a665700
Status: Downloaded newer image for python:3.9-bullseye
```

```2. container$ apt-get update```
```

Get:1 http://deb.debian.org/debian bullseye InRelease [116 kB]
Get:2 http://deb.debian.org/debian-security bullseye-security InRelease [48.4 kB]
Get:3 http://deb.debian.org/debian bullseye-updates InRelease [44.1 kB]
Get:4 http://deb.debian.org/debian bullseye/main amd64 Packages [8184 kB]
Get:5 http://deb.debian.org/debian-security bullseye-security/main amd64 Packages [204 kB]
Get:6 http://deb.debian.org/debian bullseye-updates/main amd64 Packages [14.6 kB]
Fetched 8610 kB in 3s (2759 kB/s)
Reading package lists... Done
```

### Install nano editor

```container$ apt-get install nano```

```
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
  hunspell
The following NEW packages will be installed:
  nano
0 upgraded, 1 newly installed, 0 to remove and 10 not upgraded.
Need to get 656 kB of archives.
After this operation, 2591 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bullseye/main amd64 nano amd64 5.4-2+deb11u1 [656 kB]
Fetched 656 kB in 1s (866 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package nano.
(Reading database ... 23422 files and directories currently installed.)
Preparing to unpack .../nano_5.4-2+deb11u1_amd64.deb ...
Unpacking nano (5.4-2+deb11u1) ...
Setting up nano (5.4-2+deb11u1) ...
update-alternatives: using /bin/nano to provide /usr/bin/editor (editor) in auto mode
update-alternatives: using /bin/nano to provide /usr/bin/pico (pico) in auto mode
```

### Run the static web application
Follow the steps mentioned in the [here](https://github.com/ShrddhaRana/DockerBasics/blob/main/Create%20a%20simple%20greeting%20web%20application%20using%20flask.md)
Note: Run those inside the container
