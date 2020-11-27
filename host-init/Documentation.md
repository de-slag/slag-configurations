# How To

* copy the following lines to **pre-init.sh** on host
```
sudo apt-get -y install git
git clone https://github.com/de-slag/slag-tools.git
git clone https://github.com/de-slag/slag-configurations.git
ln -s slag-tools/host-init/init-host.sh.latest ./init-host.sh
ln -s slag-configurations/host-init/init-host.properties.latest ./init-host.properties
```
* run **pre-init.sh**:
```
bash pre-init.sh
```
