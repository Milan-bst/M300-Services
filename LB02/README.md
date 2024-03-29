# M300-Services - LB02

## Persönlicher Wissenstand

### Linux

Mit Linux habe ich bereits einiges an Erfahrung. Ich habe im meinem Basislehrjahr beim ZLI angefangen mit Linux zu arbeiten und seit dem habe ich immer mal wieder damit zu tun gehabt im Geschäft da wir diverse Agents mit Testing Pipelines haben welche auf Linux laufen.

### Virtualisierung 

Mit Virtualisierung hab ich ebenfalls schon viel Erfahrung da ich bereits am Anfang meiner Ausbildung mit VMware gearbeitet habe dort meist mit Linux VMs und auch Windows Server VMs. Ebenfalls hatte ich erst letztens einen ÜK, welcher nur um Virtualisierung mit Exsi und Proxmox ging.

### Vagrant

Mit Vagrant hatte ich vor der LB01 nicht viel Erfahrung, jedoch konnte ich in dieser LB bereits viel über Vagrant lernen sprich die wichtigstens Commands, für welche zwecke man es benutzen kann, wie die Vagrant Files zu strukturieren sind und noch einiges mehr.

### Docker / Kubernetes

Ich habe mit Docker schon im Geschäft einmal gearbeitet jedoch nicht wirklich lange da ich nur einge Applikationen in Docker Containern getestet habe jedoch war dies bereits schon sehr lehrreich darüber wie Docker funktionieren und wie Docker files aufgebaut sind.

## Wissenszuwachs

In dieser LB konnte ich einiges lernen die wichtigsten Dinge meiner Meinung nach sind die Folgenden: Ich konnte mein Wissen über Docker erweitern um es genauer zu sagen mein Wissen über die Befehle um Docker Container zu verwalten und ebenfalls mein Wissen für die Commands, welche in den Dockerfiles verwendet werden. Mittlerweile muss ich auch sagen, dass ich Markdown besser finde als Word wenn es um Dokumentationen geht, welche mit der IT zu tun haben da es einfach deutlich angenehmer ist mit Markdown zu schreiben da die Formatierung grösstenteils automatisch gemacht wird.

## Docker

Docker ist eine Freie Software zur Isolierung von Anwendungen mit Hilfe von Containervirtualisierung. Docker vereinfacht die Bereitstellung von Anwendungen, weil sich Container, die alle nötigen Pakete enthalten, leicht als Dateien transportieren und installieren lassen.

### Docker-Commands
| Befehl            | Funktion                                             |
| -------------     | ---------------------------------------------------- | 
| ```docker pull```      | Holt ein Image. |
| ```docker run```      | Startet eine VM mit dem ausgewähltem Image. |
| ```docker ps```      | Zeigt die laufende Maschinen an. |
| ```docker version```      | Zeigt die Docker Version von Echo-Client und des Server an. |
| ```docker images```        | Listet alle vorhandenen Docker Images auf. |
| ```docker exec```       | Führt einen Befehl in einem laufenden Container aus. |
| ```docker search```    | Durchsucht das Docker Hub nach Images. |
| ```docker attach```      | Hängt etwas an einen laufenden Container an. |
| ```docker commit```   | Erstellt ein neues Image mit den Änderungen, die an einem Container vorgenommen worden sind. |
| ```docker stop```   | Haltet die gewünschte Maschine an. |

### Dockerfile

#### Was ist ein Dockerfile?
Ein Dockerfile ist eine Textdatei, in dem die "Befehle" definiert sind, anhanden von denen ein Container erstellt wird

#### Anweisungen im Dockerfile
Anweisung | Erklärung 
------------ | ------------- | 
`FROM` | Gibt an welches "Base" Image verwendet werden soll
`ADD` | Kopiert die Datei, aus dem Build Context oder von URLs in das Image
`CMD` | Führt die angegebenen Anweisungen aus, wenn der Container gestartet wurde
`COPY` | Kopiert Dateien aus dem Build Context in das Image
`ENTRYPOINT` | Legt eine ausführbare Datei fest, welche beim Start des Containers laufen soll
`ENV` | Setzt Umgebungsvariablen im Image
`HEASLTCHECK` | Docker prüft den Status der Anwendungen in einem Conatainer
`MAINTAINER` | Setzt die "Autor-Metadaten" des Image auf den angegebenen Wert
`RUN` | Führt die angegebenen Anweisungen im Container aus und bestätigt das Ergebnis
`SHELL` | Erlaubt der Shell für folgenden RUN-BEfehk zu setzen.
`USER` | Setzt User, welcher in folgeden RUN-, CMD- oder ENTRYPOINT-ANweisungen genutzt werden soll.
`VOLUME` | Deklariert die angegebene Datei oder das Verzeichnis als Volumen
`WORKDIR` | Setzt Arbeitsverzeichnis für alle folgende RUN-, CMD-, ENTRYPOINT-, ADD oder COPY-Anweisung

### Netzwerkplan

![Netzwerkplan](https://github.com/Milan-bst/M300-Services/blob/main/LB02/Netzwerkplan.png)

### Webserver mit Docker

Damit ich einen Webserver mit Apache aufsetzen kann musste ich die folgenden Schritte durchlaufen.

#### Erstellen des Dockerfiles
```
#
#	Einfache Apache Umgebung
#
FROM ubuntu:14.04
MAINTAINER Marcel mc-b Bernet <marcel.bernet@ch-open.ch>

RUN apt-get update
RUN apt-get -q -y install apache2 

# Konfiguration Apache
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

RUN mkdir -p /var/lock/apache2 /var/run/apache2

EXPOSE 80

VOLUME /var/www/html

CMD /bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"
```

Mit diesem Befehl habe ich das Image dann erstellen lassen. 
```
docker build [VerzeichnisDesDockerfiles]
```
Danach das Image umbennen, damit es einfacher zu finden ist.
```
docker tag <Image ID> <Neuer Name>
```
Jetzt kann der Container gestartet werden (hier wird der Port 8080 weitergeleitet):
```
docker run --rm -d -p 8080:80 -v /web:/var/www/html --name <Container Name> <Image ID>
```
Zugriff auf die Shell:
```
docker exec -it Webserver /bin/bash
```
Index File in den Containier kopieren
```
docker cp /SpeicherortDerDatei/index.html ContainereName:/var/www/html/
```

Inhalt des Index File:
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>M300</title>
</head>
<body>
<h1>LB02 GitHub Repository</h1>
<a>Das LB02 GitHub Repository ist unter</a>
<a href="https://github.com/Milan-bst/M300-Services/tree/main/LB02">folgendem Link</a>
<a>erreichbar</a>
</body>
</html>
```
In diesem File habe ich den Link zu meinem GitHub Repository der LB02 eingefügt. Nach den Anpassungen sieht die Website nun folgendermassen aus.

![Website](https://github.com/Milan-bst/M300-Services/blob/main/LB02/Website.JPG)

### Monitoring

Um die Container sicherer zu machen und Überwachen zu können verwenden wir ein security tool von Google welches für das Monitoring von Containern entwickelt wurde.

Um diesen Service einzurichten muss ich diesen Command eingeben, welcher einen neuen Container erstellt, in welchem die Webappliktion läuft.
```
docker run -d --name cadvisor -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -p 8010:8080 google/cadvisor:latest
```

_Hierbei ist wichtig das man nicht den normalen Port 8080 verwendet da dieser bereits vom Webserver benutzt wird. In diesem Fall bin ich deshalb auf den Port 8010 ausgwichen_

Nachdem ich diesen Command eingegeben habe ist das Webinterface über 127.0.0.1:8010 zu erreichen und sieht folgender massen aus.

![Website](https://github.com/Milan-bst/M300-Services/blob/main/LB02/Website2.JPG)

### Container Sicherheit

Damit nicht jeder User root rechte in den Containern hat, muss man folgende Zeilen in das Dockerfile hinzufügen.
```
RUN useradd -ms /bin/bash NeuerUserName

USER NeuerUserName

WORKDIR /homeNeuerUserName
```

#### Ready-Only

Wenn Docker mit der Option Ready-Only gestartet wird, können keine Änderungen am Dateisystem vorgenommen werden (Auch nicht mit sudo). Der Command um einen Docker im Read-Only Modus zu starten sieht folgender massen aus:
```
docker run --read-only -d -t --name NameDesContainer Image
```

### Dockerfiles
Ein weiterer Aspekt der Sicherheit von Containern ist das Dockerfile selbst, wenn man diese nicht selbst schreibt, kann man sich nie genau sicher sein was man für einen Container in sein Netzwerk einbindet. Deshalb kann es immer sein dass sich dort eine Art von Mallware oder gar ein Virus versteckt.

## Docker Hub

### Image bereitstellen

Am Anfang muss man einen Container erstellen, mit dem Image, welches auf die Docker Cloud gepusht werden soll. Wenn dies gemacht wurde muss folgender Befehl im CMD ausgeführt werden.
```
docker commit [Container ID des oben erstellten Containers] [Dockerhub Username]/[Gewünschter Name]:[Tag]
```
Wenn dies erledigt wurde kann man mit folgendem Befehl das Ganze auf Docker Hub pushen:
```
docker push [Docker Hub Username]/[GewünschterName]:[Tag]
```

### Image Pullen
Sobald das Image hochgeladen wurde kann man es wie jedes andere Image auf Docker Hub herunterladen mit dem folgenden Command:
```
docker pull [Docker Hub Username]/[GewünschterName]:[Tag]
```

## Kubernetes
Hier muss ein deployment File erstellt werden, welches ich deployment.yaml genannt habe, mit dem folgdenen Inhalt.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: Kubernetes-M300
  labels:
    app: Kubernetes-M300
spec:
  replicas: 5
  selector:
    matchLabels:
      app: Kubernetes-M300
  template:
    metadata:
      labels:
        app: Kubernetes-M300
    spec:
      containers:
      - name: Kubernetes-M300
        image: kub
        imagePullPolicy: Always
        ports:
        - containerPort: 80
```

Sobald dieses File erstellt wurde muss man es auch ausführen und somit die "Container", welche dort definiert wurde erstellen. Der Command dafür ist folgender:
```
kubectl apply -f deployment.yaml
```

## Service

Hier werde ich noch einen Loadbalancer für Apache Webserver aufsetzten

```
apiVersion: v1
kind: Service
metadata:
  name: Loadbalancer
  annotations:
    service.beta.kubernetes.io/linode-loadbalancer-throttle: "4"
  labels:
    app: M300
spec:
  type: LoadBalancer
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: M300
  sessionAffinity: None
  ```

Hier gilt es zu beachten das alle bei "app" den gleichen Eintrag haben müssen da der Loadbalancer sonst einige Probleme haben wird. Sobald man dieses File erstellt hat kann man das ganze wieder mit dem folgenden Command ausführen.
```
kubectl apply -f FileName.yaml
```

