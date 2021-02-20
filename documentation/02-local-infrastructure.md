# Local infrastructure

## Data Storage

### Overview

|Data Area|Purpose|Comment|
|---|---|---|---|
|data|active data (documents, media data, no executables, no configurations)| |
|backup|backups of all kind| |
|tmp|temporary files of all kind| |
|vrt|VM instance files, logs, generated logic| |
|backup-archive|swap for backups, long term backups| |
|logic|executables|deprecated -> slag-tools|
|test|test data, logic and configurations|deprecated -> slag-tools, slag-configurations|


## Machines

here also: separate of concern

### Location: Oslo

*jupiter*

* purpose: data storage hosting 
* configuration
** 2x 3TiB @RAID5
** capacity: 3.6 TB

### Location: London

*uranus*

* purpose: data storage hosting
* configuration
** NAS
** 2x 1TiB @ RAID1
** capacity: 900 GB

*ceres*

* purpose: backup and other scheduled processes for data storage
* configuration:
** Raspberry pi 2

*merkur*
* vm host
* configuration:
** cpu: Core2Quad Q4400
** ram: 4 GiB
** hdd: 40 GB for host OS

## Naming Stragety:

* Fullsize-Servers + NAS: Planets of solar system
* Auxiliary Servers + SoC: Dwarf planets of solar system
* Virtual machines: Plutoids
** Suffix -a .. -z: machines to test vm behaviour
** Suffix -0 .. -nnn: machines to develop, test and be productive
* Notebooks, PCs and other End-User-Machines: Moons of solar system

