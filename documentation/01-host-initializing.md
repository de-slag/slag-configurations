# Host Initializing

## Initialize a host

Put the following lines in a script and run as root:

    #!/bin/bash
    apt-get update
    apt-get install -y git
    cd ~
    git clone https://github.com/de-slag/slag-configurations.git
    git clone https://github.com/de-slag/slag-tools.git  
