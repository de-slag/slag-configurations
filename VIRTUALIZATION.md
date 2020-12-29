# VIRTUALIZATION

**Table of contents**
1. [Vorbedingungen](#vorbedingungen)
2. [Ziel](#ziel)
2. [Durchführung](#durchführung)

    3.1. [VM-Host](#vm-host)

## Vorbedingungen
Es werden nur Debian-Stable- und Ubuntu-LTS-Linux-Maschinen virtualisiert. Ein Überblick über die verwendeten Versionen zeigt die folgende Tabelle:

|ab Jahr|Debian|Ubuntu|
|---|---|---|
|2019|10| |
|2020| |20.04|
|2021|11| |
|2024| |24.04|

## Ziel

Die Virtualisierung in diesem Kontext soll helfen, schnell virtuelle Maschinen zu erzeugen, die:
* einfach zu initalisieren sind
* möglichst wenig vor-initiale Konfiguration benötigen.
* der Virtualisierung auf unterschiedlichen Hosts nichts entgegensteht, obwohl alle Maschinen zentral abgelegt werden

Der "Wert" der einzelnen Instanz soll durch die schnelle, einfache Erzeugung vermindert werden um den "behalten" Faktor zu verringern.

## Durchführung

### Hinweise
* Die von der VM verwendeten Dateien .iso, .img müssen in einem von allen Benutzern beschreibbaren Verzeichnis liegen.

### VM-Host
Die Virtualisierung erfolgt auf einem Ubuntu-Server (20.04) mittels KVM / QEMU. Für die Steuerung wird auf *virsh* zurückgegriffen.

### Netzwerk
Damit die VMs im home-Netzwerk verfügbar sind, ist eine Netzwerkbrücke auf dem VM-Host erforderlich.

### Daten-Struktur

    vrt/
      autostart/            # Symlinks auf Start-Scripte von mit dem VM-Host zu startenden Maschinen
      install/
        installed.txt       # Liste mit allen installierten VM-Namen.
        mac-addresses.txt   # Liste mit allen an VMs vergebenen MAC-Adressen
      iso/                  # Iso-Dateien für die Installation eines VM-Vorlage-Images
      images/               # Img-Dateien zum kopieren für die Erzeugung einer neuen VMs
      init/                 # Init-Scripte zur erzeugen neuer VMs
      inst/                 # instance*.properties zum antriggern der Instanziierung einer neuen VM
      vm/
        pluto-4711/         
          hdd.img           # Festplattenimage oder...
          hdd.ovl           # ...Overlayimage
          init.sh           # generiertes Script, das für die Initiierung verwendet wurde
          install.sh        # generiertes Script, das für die Installation verwendet wurde
          start.sh          # generiertes Start Script
          stop.sh           # generiertes Stop Script
          destroy.sh        # generiertes script zum Zerstören der VM
      start/                # Dateien mit Namen zu startender VMs
      stop/                 # Dateien mit Namen zu stoppender VMs
      destroy/              # Dateien mit Namen zu zerstörender VMs
      log/                  # alle Log-Dateien

## Ablauf

*VM-Lifecycle:*
* Betriebssystem installieren
* VM instanziieren
  * Datenstruktur anlegen
  * HD-Image kopieren
  * Scripte generieren/kopieren
  * VM erzeugen
    * MAC-Adresse als verbraucht registrieren
    * VM-Namen als verbraucht registrieren 
  * VM starten
  * VM initialisieren
    * Hostnamen setzen
    * Installation und Konfiguration von Paketen und Anwendungen
  * VM neu starten
  * VM als 'erzeugt' markieren
* VM löschen
  * VM stoppen
  * VM zerstören
  * VM undefinieren 
  * HD-Image löschen
  * In Datenstruktur als gelöscht markieren
* gelöschte VMs aufräumen
  * Datenstrukturen gelöschter VMs löschen

 


|Schritt|Input|Durchführung|Output|
|---|---|---|---|
|Installation OS|Installation-ISO|install-vm-os-wizard.sh|OS-Festpatten-Image|
|Erstellung Init-Script| |host-setup-wizdard.sh|Init-Script|
|Erzeugen VM|OS-Festplatten-Image, Init-Script|create-vm.sh|Lauffähige, vorkonfigurierte VM|




### Installation OS
* Installation eines OS auf einem leeren Image durch slag-tools/bash/install-vm-os-wizard.sh

* Installation einer VM von einem Linux-Installation-ISO per Installation-Start-Script (siehe slag-tools/bash/install-vm-wizard.sh)
* Ausführen des folgenden Scripts zur Vorbereitung der Selbstinitialisierung auf der neuen VM. Das Script sollte unter ~/post-install.sh abgelegt werden.

    #!/bin/bash
    set -euxo pipefail
    ts=$(date '+%Y-%m-%d_%H-%M-%S')
    crontab_backup_name="/etc/crontab.bak-$ts"
    cp /etc/crontab $crontab_backup_name
    mkdir /etc/cron.minutely
    echo "* * * * *       root    cd / && run-parts --report /etc/cron.minutely" >> /etc/crontab

    echo "#!/bin/bash"                                        >  ~/pre-host-init.sh
    echo "ts=$(date '+%Y-%m-%d_%H-%M-%S')                     >> ~/pre-host-init.sh 
    echo "if [ ! -f ~/host-init.sh ] ; then"                  >> ~/pre-host-init.sh 
    echo "  echo '$ts: init script not found' >> ~/init.log"  >> ~/pre-host-init.sh 
    echo "  exit 0"                                           >> ~/pre-host-init.sh 
    echo "fi"                                                 >> ~/pre-host-init.sh
    echo "echo '%ts: init script found. executing...'         >> ~/pre-host-init.sh
    echo "bash -euxo pipefail ~/host-init.sh > ~/init.log"    >> ~/pre-host-init.sh

    ln -s ~/pre-host-init.sh /etc/cron.minutely/pre-host-init
    chmod +x /etc/cron.minutely/pre-host-init

* Herunterfahren der VM und archivieren der IMG-Datei.

### Erstellen des Initialisierungs-Scripts.
* Geführte Erstellung des Initialisierungs-Scripts mittels 'host-setup-wizard.sh'
** Das Init-Script sollte damit schließen:

    rm /etc/cron.minutely/pre-host-init
    hostname=$(cat /etc/hostname)
    if [ -e /mnt/vrt/.exists ] ; then
      cp ~/init.log /mnt/log/init-$hostname.log
    fi
    echo "host: '$hostname', init successful, shutdown"
    systemctl poweroff

* Ablage des erzeugten Init-Scripts in *vrt/init*

### Erzeugung der VM

Beispiel einer *instance.properties*

    # Image Name from images-dir without file extension
    image=ubuntu-server-20-04
    #
    # sizes have to be defined in config file
    size=small
    #
    # a free comment, I.e. purpose
    comment=

* Erzeugung der VM mittels Create-Script unter Verwendung einer Kopie der bei einer Installation erzeugten IMG-Datei.
** Anlage der Datenstruktur für diese VM
* Starten der VM
* hochladen des Init-Scriptes per scp
* warten bis die VM heruntergefahren ist
* prüfen auf Erfolg durch prüfen des init.log

### Starten der VM
* das reguläre Starten einer VM erfolgt durch einfügen einer Datei mit dem VM-Namens in *vrt/start*, z.B. durch:

   touch /mnt/vrt/start/pluto-4711

* ein regelmäßiger Cronjob des VM-Hosts:
** stellt sicher, dass Verzeichnis nicht gelockt ist (Datei: start.lock)
** setzt einen Lock auf das Verzeichnis (Datei: start.lock)
** versucht alle VMs zu starten, außer die mit einer DESTROYED-Datei
** entfern alle Einträge aus *vrt/start*
** entfernt die Lock-Datei (Datei: start.lock)
** schreibt die Aktivitäten in die Log-Datei *vrt/logs/start.log*

### Autostart einer VM
* autostart erfolgt
** durch symlink des VM-eigenen Start-Script nach *vrt/autostart*
** durch symlink des Scripts *slag-tools/bash/vrt/autostart-vms.sh* nach */etc/cron.rebootly/autostart-vms*
*** das Script enthält eine Warteprozedur, falls das nfs-Laufwerk */mnt/vrt* bei Ausführung durch den cronjob noch nicht eingehängt sein sollte

### Stoppen einer VM
* einfügen einer Datei mit Namen der VM in *vrt/stop*, z.B. durch:

   touch /mnt/vrt/stop/pluto-4711

* regelmäßiger Cronjob auf dem VM-Host, der die hier aufgeführten Maschinen stoppt
** robust genug um konkurrierende Ausführungen zu verkraften, kein Lock erforderlich
** entfernen der Dateien der gestoppten Instanzen aus *vrt/stop*
** schreibt alle Aktivitäten in die Datei *vrt/log/stop.log*

### Zerstören einer VM
* einfügen einer Datei mit Namen der VM in *vrt/destroy*, z.B. durch:

   touch /mnt/vrt/destroy/pluto-4711

* manuelle Ausführung um die hier aufgeführten Maschinen zu zerstören
** hierbei wird das Festplatten- oder Overlay-Image aus Platzgründen gelöscht
** ins Verzeichnis der vm (z.B. vrt/vm/pluto-4711) wird eine Datei mit Namen "DESTROYED" eingefügt.
** entfernen der Dateien der zerstörten Instanzen aus *vrt/destroyed*
** robust genug um konkurrierende Ausführungen zu verkraften, kein Lock erforderlich
** schreibt alle Aktivitäten in *vrt/log/destroy.log"

### Aufräumen der zerstörten VMs
* manuelle Ausführung um alle zerstörten Maschinen zu entfernen
** entfernt alle VM-Verzeichnisse (*vrt/vm/...*), die eine DESTROYED-Datei behinhalten
** robust genug um konkurrierende Ausführungen zu verkraften, kein Lock erforderlich
** schreibt alle Aktivitäten in *vrt/log/cleanup.log"

## Glossar

|Begriff|Bedeutung|
|---|---|
|Installation|Starten von einem Linux-Live-ISO mit einer leeren virtuellen Festplatte und durchführen der Linux-Installations-Routine|
|Initialisierung|Einrichten einer VM von einem installierten, unkonfigurierten System mit bestimmten Features durch ein vm-individuelles Init-Script|
