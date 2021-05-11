# Quick install a linux test environment for slag-tools

* login to docker host
* install docker (if any) and setup linux host: 

```
sudo apt install docker.io
sudo docker run -t -i ubuntu:focal /bin/bash

```
* run the following commands in docker container
```
echo "y" | unminimize
apt update
apt dist-upgrade -y
apt install git bats rsync coreutils -y
cd ~
git clone https://github.com/de-slag/slag-tools.git
git clone https://github.com/de-slag/slag-configurations.git

```
* init git repos, see https://github.com/de-slag/slag-configurations/blob/master/documentation/04-init-git-repos.md
* checkout git branch to test, i.e.
```
cd ~/slag-tools
git checkout 0.1-rc

```
* run bats tests
```
cd ~/slag-tools/test
bats *.bats

```
