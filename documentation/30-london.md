## NFS

    # unfornunately there is a problem by only using simple hostname, i.e. 'uranus', so full host qualifier is needed here 
    uranus.fritz.box:/volume1/data            /mnt/data            nfs defaults,nofail 0 9
    uranus.fritz.box:/volume1/backup          /mnt/backup          nfs defaults,nofail 0 9
    uranus.fritz.box:/volume1/vrt             /mnt/vrt             nfs defaults,nofail 0 9
    uranus.fritz.box:/volume1/tmp             /mnt/tmp             nfs defaults,nofail 0 9
