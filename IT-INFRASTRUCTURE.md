# IT INFRASTRUCTURE
## 1. Network
## 2. Servers
### 2.0 Naming
#### 2.0.1 Fullsize Servers + NAS
Planets of Solar System

    merkur  - VM host
    jupiter - secondary data server
    uranus  - primary data server

#### 2.0.1 Auxiliary Servers + SoC

Dwarf Planets of Solar System

    ceres - printer server

#### 2.0.3 Virtual Machines

Plutoids

*in tribute that pluto was a planet for 76 years*
 
     pluto-a .. pluto-z    - machines to test VM behaviour
     pluto-0 .. pluto-nnn  - machines to develop, test and be productive


### 2.1 Data-Servers

*Data Areas*

    backup         - snapshots and backups
    backup-archive - long-term archive of backups, 
    data           - documents, media data, ... no: executables, configurations, ...
    tmp            - temporary files of all kind. is deleted periodicly, no backup
    vrt            - hd-images, containers, iso-files, gerenated start scripts, ...

#### 2.1.1 Primary Data Server

Name: uranus

Data-Areas:
 * backup
 * data
 * tmp
 * vrt

Current-Configuration (2020.12):
* 2x 1 TB
* RAID 1
* 900 MiB capacity

#### 2.1.2 Backup Data Server
Name: jupiter

Data-Areas:
 * backup-archive

Current-Configuration (2020.12):
* 3x 2 TB
* RAID 5
* 3,6 MiB capacity

### 2.2 VM-Host
Name: merkur

Current-Configuration (2020.12):
* CPU: Core2Quad 4400
* 4 GB RAM

### 2.3 VM-Guests
Names: Pluto-xx (x stands for an hex value)

There are three configurations depending on what the current vm-host can carry at the same time:
* large: 1 VM
* medium: 2 VMs
* small: 4 VMs

see #Configurations global.properties 'vrt.guest.'-section for exact parameters

## 3. Tools
github repo: slag-tools

## 4. Configurations
github repo: slag-configurations
