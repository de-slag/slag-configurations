# Quick install a linux test environment for slag-tools

* login to docker host
* install docker (if any) and setup linux host: 

```
sudo apt install docker.io
sudo docker run -t -i ubuntu:focal /bin/bash

```
* run the following commands in docker container
```
echo '#!/bin/bash                                                  ' >  /tmp/run-slag-utils-tests.sh
echo '# CONFIG SECTION                                             ' >> /tmp/run-slag-utils-tests.sh
echo 'SLAG_TOOLS_BRANCH=0.2-rc                                     ' >> /tmp/run-slag-utils-tests.sh
echo '                                                             ' >> /tmp/run-slag-utils-tests.sh
echo '# PROGRAM SECTION - do not modify                            ' >> /tmp/run-slag-utils-tests.sh
echo 'echo "y" | unminimize                                        ' >> /tmp/run-slag-utils-tests.sh
echo 'apt update                                                   ' >> /tmp/run-slag-utils-tests.sh
echo 'apt dist-upgrade -y                                          ' >> /tmp/run-slag-utils-tests.sh
echo 'apt install git bats rsync nano coreutils -y                 ' >> /tmp/run-slag-utils-tests.sh
echo 'cd ~                                                         ' >> /tmp/run-slag-utils-tests.sh
echo 'git clone https://github.com/de-slag/slag-tools.git          ' >> /tmp/run-slag-utils-tests.sh
echo 'git clone https://github.com/de-slag/slag-configurations.git ' >> /tmp/run-slag-utils-tests.sh
echo 'cd ~/slag-tools                                              ' >> /tmp/run-slag-utils-tests.sh
echo 'git checkout "$SLAG_TOOLS_BRANCH"                            ' >> /tmp/run-slag-utils-tests.sh
echo 'cd ~/slag-tools/test                                         ' >> /tmp/run-slag-utils-tests.sh
echo 'bats *.bats                                                  ' >> /tmp/run-slag-utils-tests.sh
bash /tmp/run-slag-utils-tests.sh
```

## (deprecated)

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
