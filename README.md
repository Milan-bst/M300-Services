# M300-Services

## Persönlicher Wissenstand

### Linux

Mit Linux habe ich bereits einiges an Erfahrung. Ich habe im meinem Basislehrjahr beim ZLI angefangen mit Linux zu arbeiten und seit dem habe ich immer mal wieder damit zu tun gehabt im Geschäft da wir diverse Agents mit Testing Pipelines haben welche auf Linux laufen.

### Virtualisierung 

Mit Virtualisierung hab ich ebenfalls schon viel Erfahrung da ich bereits am Anfang meiner Ausbildung mit VMware gearbeitet habe dort meist mit Linux VMs und auch Windows Server VMs. Ebenfalls hatte ich erst letztens einen ÜK, welcher nur um Virtualisierung mit Exsi und Proxmox ging.

## Wissenszuwachs

So gut wie alles was ich unten dokumentiert habe ist Neuland für mich gewesen. In diesem Modul konnte ich bis jetzt schon vieles Lernen unter anderem finde ich das Provisioning von VMs mit dem VagrantFile sehr nützlich und interessant. Man kann damit vieles vereinfachen indem man bereits die gesamte VM nach seinen vorstellungen Konfigurieren kann bevor man das erste mal darauf connected.

## 10 - Toolumgebung 
Sachen, welche ich in diesem Kapitel erledigt habe:
- GitHub Account konfiguriert
  - Repository erstellt
  - SSH Key generiert und eingebunden \
    In der Git Bash: 
    ```
    ssh-keygen -t rsa -b 4096 -C "milan.beste@gmail.com"
    ```
  - Repository auf mein Desktop geklont
- Git Client installiert und konfiguriert
- VM auf VirtualBox erstellt und konfiguriert
- Virtual Box installiert (für Vagrant)
- Vagrant installiert
- Visual Studio Code mit Github verbunden, Extensions installiert und als Mardown Editor ausgewählt 

### Vagrant Umgebung

Um eine Vagrant VM mit einer Box zu erstellen muss man zuerst in einem Ordner seiner Wahl das Vagrantfile erstellen für die Konfigurationen der VM. Dies geht mit dem command "vagrant init [VM-Aus-Cloud](https://app.vagrantup.com/ubuntu/boxes/)". Nachdem dieses Config File erstellt ist kann man dieses verändern. Die Config Files sehen je nach VM anders aus. Die wichtigstens änderungen, welche man vornehmen kann ist die IP adresse, die menge an RAM und Cores sowie commands welche beim Einrichten bereits in der Shell ausgeführt werden sollen beim erstellen der VM. Sobald mann alle Konfigurationen vorgenommen hat kann man mit dem Befehl vagrant up die VM erstellen. Wenn dies gemacht wurde, benutzt man den command vagrant ssh um sich mit dieser VM zu verbinden.

    +---------------------------------------------------------------+
    ! Notebook - Schulnetz 10.x.x.x und Privates Netz 192.168.1.1   !                 
    ! Port: 8080 (192.158.1.2:80)                                   !	
    !                                                               !	
    !                    +--------------------+                     !
    !                    ! Web Server         !                     !
    !                    ! Host: web01        !                     !
    !                    ! IP: 192.168.1.2    !                     !
    !                    ! Port: 80           !                     !
    !                    ! Nat: 8080          !                     !
    !                    +--------------------+                     !
    !                                                               !	
    +---------------------------------------------------------------+

#### Basic Vagrant Befehle

Befehl | Erklärung | Beispiel
------------ | ------------- | -------------
`vagrant init` | Initalisiert im aktuellen Verzeichnis eine Vagrant Umgebung und falls noch kein Vagrant File vorhanden ist erstellt es dieses | `vagrant init ubuntu/xenial64`
`vagrant up` | Fährt diese VM hoch, in welchem Verzeichnis man das ausführt | `vagrant up --provider virtualbox` --> oder nur vagrant up
`vagrant ssh` | Baut eine SSH Verbindung mit dem VM auf | -
`vagrant status` | Zeigt den Status der VM an| -
`vagrant port` | Zeigt Port von PortForwarding an | -
`vagrant halt` | Stoppt VM | -
`vagrant destroy` | Löscht VM | -
`vagrant -v` | Zeigt Vagrant Version an | -

## 20 - Infrastruktur

## Vagrant Umgebung

