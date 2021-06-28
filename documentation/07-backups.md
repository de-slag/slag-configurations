# Backups

For all this backups it is very strongly recommendet to have a local backup drive or be connected by cable LAN. Otherwise this processes will take a multiple amount of time and will exhaust network resources.

## 1. Full disk or partition backup

Status: **untested** - intented to be executed on a Live Ubuntu 20.04

### Steps

Set german keymap (if any):

    setxkmap de
    
Accelerate to root rights:

    sudo -i
    
List drives and partitions and find out correct backup source:

    fdisk -l


Paste this to a script, i.e. `/tmp/backup.sh`

    #!/bin/bash
    
    ## constants - do not change    
    readonly TS=$(date +%s)
    
    ## configuration  
    
    # All of the following configurations has to be setted!
    
    # Use here the result from selection by 'fdisk -l'
    BACKUP_SOURCE=/dev/sda
    
    # This is the local mountpoint for nfs share. You can leave this, if it is ok.
    LOCAL_BACKUP_MOUNTPOINT=/mnt/backup
    
    # This is the name of server with the nfs share, it can also be an ip address or a simple host name (i.e. 'jupiter')
    BACKUP_HOST=jupiter.local.ip
    
    # This is the nfs share on server
    BACKUP_DRIVE=/media/raid_a/backup
    
    # This is the file name where backup is stored in, change this to whatever you want.
    BACKUP_FILE_NAME=sda-$TS.img
       
    ## script - do not change
    
    # fail on any errors
    set -exuo pipefail
    
    echo $BACKUP_SOURCE
    echo $LOCAL_BACKUP_MOUNTPOINT
    echo $BACKUP_HOST
    echo $BACKUP_DRIVE
    echo $BACKUP_FILE_NAME
    
    apt install nfs-common
    mkdir /mnt/backup
    mount $BACKUP_HOST:BACKUP_DRIVE $LOCAL_BACKUP_MOUNTPOINT
     
    dd if=$BACKUP_SOURCE of=$LOCAL_BACKUP_MOUNTPOINT/$BACKUP_FILE_NAME bs=10M status=progress
    
    shutdown -h now
    

## 1. preparation

* set german keyboard layout
* login to root console
* install nfs-common (if any)
* create mountpoint (if any)
* mount network dir (if any)

##
    setxkmap de
    sudo -i
    apt install nfs-common
    mkdir /mnt/backup
    mount host.local.dns:/media/raid_a/backup /mnt/backup

## 2. full disk dump

* list all disks and partitions: `fdisk -l`
* pick correct disk or partition
* run backup

## 

    dd if=/to-be-backuped of=/mnt/backup/backup-file.img bs=10M status=progress


