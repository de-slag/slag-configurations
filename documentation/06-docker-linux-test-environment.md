# Quick install a linux test environment for slag-tools

* login to docker host
* install docker (if any) and setup linux host: 

```
sudo apt install docker.io
sudo docker run -t -i ubuntu /bin/bash

```
* run the following commands in docker container
```
apt update
apt dist-upgrade -y
apt install git -y

```
* init git repos, see https://github.com/de-slag/slag-configurations/blob/master/documentation/04-init-git-repos.md
* checkout git branch to test, i.e.
```
cd ~/slag-tools
git checkout 0.1-rc

```
* run bats tests
```
bats ~/slag-tools/test/*.bats

```
