# M300-Services

## Persönlicher Wissenstand

### Linux

Mit Linux habe ich bereits einiges an Erfahrung. Ich habe im meinem Basislehrjahr beim ZLI angefangen mit Linux zu arbeiten und seit dem habe ich immer mal wieder damit zu tun gehabt im Geschäft da wir diverse Agents mit Testing Pipelines haben welche auf Linux laufen.

### Virtualisierung 

Mit Virtualisierung hab ich ebenfalls schon viel Erfahrung da ich bereits am Anfang meiner Ausbildung mit VMware gearbeitet habe dort meist mit Linux VMs und auch Windows Server VMs. Ebenfalls hatte ich erst letztens einen ÜK, welcher nur um Virtualisierung mit Exsi und Proxmox ging.

## Wissenszuwachs

So gut wie alles was ich unten dokumentiert habe ist Neuland für mich gewesen. In diesem Modul konnte ich bis jetzt schon vieles Lernen unter anderem finde ich das Provisioning von VMs mit dem VagrantFile sehr nützlich und interessant. Man kann damit vieles vereinfachen indem man bereits die gesamte VM nach seinen vorstellungen Konfigurieren kann bevor man das erste mal darauf connected. Um es noch genauer zu beschreiben sind die Punkte, welche ich bis jetzt erlernen konnte die folgenden.

- Erstellen von Vagrant VMs mit Virtual Box
- Erstellen von Vagrant VMs mit Boxes
- Umgang mit GitHub
- SSH-Key erstellen und einbinden
- VM Konfigurieren mit hilfe von Vagrant File
- Text in Config files schreiben mit Ruby
- Port forwarding im VagrantFile einrichten


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

## 20 - Infrastruktur
Sachen, welche ich in diesem Kapitel erledigt habe:
- Lesen der Dokumente
- Dokumentation im .md File angefangen zu schreiben

## 25 - Infrastruktur-Sicherheit

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
    !                    ! Nat: 187           !                     !
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

### Netzwerkplan
![Netzwerkplan](https://github.com/Milan-bst/M300-Services/blob/main/Netzwerkplan.jpg)

### Erklärung der Commands aus dem VagrantFile

```
sudo ufw --force enable
``` 
\
Mit diesem Command wird die ufw Firewall dazu gezwungen sich zu aktivieren auch falls dies von etwas blockiert werden sollte.
```
sudo ufw allow 22
``` 
\
Mit diesem Command wird der Port 22 auf der Firewall freigegeben.
```
sudo ufw allow 2222
``` 
\
Mit diesem Command wird der Port 2222 auf der Firewall freigegeben.
```
sudo systemctl start ssh
``` 
\
Mit diesem Command wird der Dienst ssh gestartet
```
sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
``` 
\
Mit diesem Command sucht man im File "/etc/ssh/sshd_config" nach ".*PasswordAuthentication.*" und ersetzt dies durch "PasswordAuthentication yes". 
- "/g" steht für global. Das heisst es ersetzt den Gesuchten Teil überall im genannten File und nicht nur an einem Ort 
```
sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
``` 
\
Mit diesem Command sucht man im File "/etc/ssh/sshd_config" nach ".*ChallengeResponseAuthentication.*" und ersetzt dies durch "ChallengeResponseAuthentication yes". 
- "/g" steht für global. Das heisst es ersetzt den Gesuchten Teil überall im genannten File und nicht nur an einem Ort 
```
sudo chown -c vagrant /var/mail
``` 
\
Mit diesem Command werden die Rechte für den Ordner /var/mail an de User vagrant gegeben.
```
sudo chmod -R 700 /var/mail
``` 
\
Mit diesem Command setzt man die Rechte auf 700 --> User hat Read/Write/Execute. Group und Global haben keine Berechtigungen.
```
sudo apt-get -y install nginx
``` 
\
Mit diesem Command wird nginx für den Reverse-Proxy installiert.
```
sudo unlink /etc/nginx/sites-enabled/default
``` 
\
Mit diesem Command wird der Virtuelle Host unlinked.
```
sudo touch /etc/nginx/sites-available/reverse-proxy.conf
``` 
\
Mit diesem Command erstellt man das File für die Konfigurationen des Reverse-Proxy.
```
	cat <<%EOF% | sudo tee -a /etc/nginx/sites-available/reverse-proxy.conf
	server {
		listen 187;
		location / {
			proxy_pass http://127.0.0.1;
		}
	}
%EOF%
``` 
\
Dieser Abschnitt fügt den Inhalt im File "/etc/nginx/sites-available/reverse-proxy.conf" hinzu. Der Inhalt sagt, dass der Reverse Proxy über Port 187 für den Localhost (127.0.0.1) kommuniziert.
``` 
sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
``` 
\
Mit diesem Command erstellt man einen Symlink von "/etc/nginx/sites-available/reverse-proxy.conf" nach "/etc/nginx/sites-enabled/reverse-proxy.conf".
``` 
sudo systemctl stop apache2
``` 
\
Dieser Command stopt den Dienst "apache2".
``` 
sudo systemctl restart nginx
``` 
\
Dieser Command startet den Dienst "nginx" neu. 
``` 
sudo reboot
``` 
\
Dieser Command startet die VM neu.  

## Überprüfen der Funktionen

### Zugriff mit Putty überprüfen

In dem Vagrant File habe ich den Port 22 auf den Port 2222 weitergeleitet und sollte somit in der Lage sein von meinem Laptop aus mit dem Port 2222 auf die Vagrant VM zuzugreifen was auch funktioniert hat. Diese Portweiterleitung kann man auch überprüfen indem man im GitBash "Vagrant Port" eingibt. Dieser Command gibt alle Portweiterleitungen aus, welche für die VM konfiguriert wurden. Dies hat auch korrekt funktioniert.
![Putty-Bild](https://github.com/Milan-bst/M300-Services/blob/main/putty.JPG)

### Benutzer Rechte überprüfen

Hier habe ich überprüft ob die Rechte korrekt gesetzt wurden indem ich einmal mit dem user Vagrant auf das verzeichnis /var/mail zugreife und einmal mit dem benutzer milbes, welchen ich selbst erstellt hatte. Das ergebnis war wie erwartet: Mit dem Vagrant benutzer hat es funktioniert mit dem milbes benutzer jedoch nicht.
![Putty-Bild](https://github.com/Milan-bst/M300-Services/blob/main/cd.JPG)

### Firewall status & Regeln überprüfen

Hier habe ich überprüft ob die ufw firewall überhaupt aktiv ist und ob die korrekten Regeln erstellt wurden. Das heisst es sollten der Port 22 sowie der Port 2222 freigegeben sein. Dies ist auch der Fall. Jedoch sind auch noch andere Ports freigegeben aber dies nur weil ich an der Firewall zuvor einige Commands ausprobiert habe.
![Putty-Bild](https://github.com/Milan-bst/M300-Services/blob/main/ufw.JPG)

### Reverse Proxy überprüfen

Hier habe ich überprüft ob der Reverse Proxy korrekt funktioniert. Dies geht in dem man den Command "sudo service nginx configtest" wenn der Reverse Proxy funktioniert sollte ein ok erscheinen oder es zumindest keine Fehlermeldung geben, was auch genauso war.
![Putty-Bild](https://github.com/Milan-bst/M300-Services/blob/main/nginx.JPG)