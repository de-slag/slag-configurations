# Quick install a linux test environment for slag-tools

* login to docker host
* install docker (if any) and setup linux host: 

```
apt install docker.io
docker run -t -i ubuntu /bin/bash
```
* run the following commands in docker container
```
apt update
apt dist-upgrade -y
apt install git -y
```
