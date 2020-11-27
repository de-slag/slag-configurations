# How To

* copy content from **slag-tools/host-init/pre-init.sh** to host
* run **pre-init.sh** on host:
```
sudo apt-get -y install git
git clone https://github.com/de-slag/slag-tools.git
git clone https://github.com/de-slag/slag-configurations.git
ln -s slag-tools/host-init/init-host.sh.latest ./init-host.sh
ln -s slag-configurations/host-init/init-host.properties.latest ./init-host.properties
```
