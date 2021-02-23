# Host Initializing

## Initialize a host

Put the following lines in a script and run as sudoers user:

    #!/bin/bash
    sudo apt-get update
    sudo apt-get install -y git
    exit
    cd ~
    git clone https://github.com/de-slag/slag-configurations.git
    git clone https://github.com/de-slag/slag-tools.git
