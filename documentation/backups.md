# Backups

For all this backups it is very strongly recommendet to have a local backup drive or be connected by cable LAN. Otherwise this processes will take a multiple amount of time and will exhaust network resources.

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


