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

|Data Area|Purpose|Server|Comment|
|---|---|---|---|
|data|active data (documents, media data, no executables, no configurations)|uranus| |
|backup|backups of all kind|uranus| |
|logic|executables|uranus|deprecated -> slag-tools|
|test|test data, logic and configurations|uranus|deprecated -> slag-tools, slag-configurations|
|tmp|temporary files of all kind|jupiter| |
|vrt|VM instance files, logs, generated logic|uranus| |
|backup-archive|swap for backups, long term backups|jupiter| |

#### 2.1.1 Primary Data Server
Name: uranus

Current-Configuration (2020.12):

|Parameter|Value|
|---|---|
|Base Storage Size|1 TB|
|Base Storage Count|2|
|RAID|1|
|Capacity|~900 GiB|

#### 2.1.2 Secondary Data Server
Name: jupiter

|Parameter|Value|
|---|---|
|Base Storage Size|1 TiB|
|Base Storage Count|2|
|RAID|1|
|Capacity|~900 GB|

Data-Areas:
 * backup-archive

Current-Configuration (2020.12):
|Parameter|Value|
|---|---|
|Base Storage Size|2 TiB|
|Base Storage Count|3|
|RAID|5|
|Capacity|3.6 TB|

### 2.2 VM-Host
Name: merkur

Current-Configuration (2020.12):
|Parameter|Value|
|---|---|
|CPU|Core2Quad 4400|
|RAM|4 GiB|
|HDD|40 GB (for OS)|

### 2.3 VM-Guests

|Size|CPUs|RAM|HDD-Image|
|---|---|---|---|
|small|1|512 MiB|8 GB|
|medium|2|1 GiB|8 GB|
|large|4|2 GiB|8 GB

## 3. Tools
github repo: slag-tools

## 4. Configurations
github repo: slag-configurations
