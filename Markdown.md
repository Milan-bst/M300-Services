# M300-Services

## Persönlicher Wissenstand

### Linux

Mit Linux habe ich bereits einiges an Erfahrung. Ich habe im meinem Basislehrjahr beim ZLI angefangen mit Linux zu arbeiten und seit dem habe ich immer mal wieder damit zu tun gehabt im Geschäft da wir diverse Agents mit Testing Pipelines haben welche auf Linux laufen.

### Virtualisierung 

Mit Virtualisierung hab ich ebenfalls schon viel Erfahrung da ich bereits am Anfang meiner Ausbildung mit VMware gearbeitet habe dort meist mit Linux VMs und auch Windows Server VMs. Ebenfalls hatte ich erst letztens einen ÜK, welcher nur um Virtualisierung mit Exsi und Proxmox ging.

## Vagrant Umgebung

Um eine Vagrant VM mit einer Box zu erstellen muss man zuerst in einem Ordner seiner Wahl das Vagrantfile erstellen für die Konfigurationen der VM dies geht mit dem command "vagrant init [VM-Aus-Cloud](https://app.vagrantup.com/ubuntu/boxes/)". Nachdem man dieses Config File erstellt hat dieses verändern. Die Config Files sehen je nach VM anders aus. Die wichtigstens änderungen, welche man vornehmen kann ist die IP adresse, die menge an RAM und Cores sowie commands welche beim Einrichten bereits in der Shell ausgeführt werden sollen beim erstellen der VM. Sobald mann alle Konfigurationen vorgenommen hat kann man mit dem Befehl vagrant up die VM erstellen. Wenn dies gemacht wurde, benutzt man den command vagrant ssh um sich mit dieser VM zu verbinden.

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