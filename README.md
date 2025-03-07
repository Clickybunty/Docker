# 🐳 Docker & Linux Befehle - Schnelle Referenz

## 📌 Docker Befehle

### 🏗 Container verwalten
```sh
docker ps                   # Zeigt alle laufenden Container an
docker ps -a                # Zeigt alle Container (auch gestoppte)
docker run -it ubuntu bash  # Startet neuen Ubuntu-Container mit interaktivem Terminal
docker start <CONTAINER-ID> # Startet einen gestoppten Container erneut
docker stop <CONTAINER-ID>  # Stoppt einen laufenden Container
docker restart <CONTAINER-ID> # Startet einen Container neu
docker rm <CONTAINER-ID>    # Löscht einen Container
docker container prune      # Löscht alle gestoppten Container
```

### 📦 Images verwalten
```sh
docker images               # Listet alle vorhandenen Images auf
docker pull <IMAGE>         # Lädt ein Image aus Docker Hub herunter
docker rmi <IMAGE-ID>       # Löscht ein Image
docker image prune -a       # Löscht ungenutzte Images
docker build -t mein-image . # Erstellt ein Docker-Image aus einem Dockerfile
```

### 🔍 In laufende Container einsteigen
```sh
docker exec -it <CONTAINER-ID> bash  # Öffnet eine interaktive Shell im Container
docker attach <CONTAINER-ID>         # Verbindet sich mit der Konsole eines Containers
exit                                  # Verlässt den Container (läuft weiter)
STRG + P + Q                         # Verlässt den Container ohne ihn zu stoppen
```

### 🌐 Netzwerke & Ports
```sh
docker network ls                     # Zeigt alle Docker-Netzwerke an
docker network create mein-netzwerk    # Erstellt ein neues Docker-Netzwerk
docker run -d -p 8080:80 nginx        # Startet einen Nginx-Container mit Port 8080 auf Port 80 im Container
```

### 📁 Docker-Volumes (Daten speichern)
```sh
docker volume ls                     # Listet alle Docker-Volumes auf
docker volume create mein-volumen     # Erstellt ein neues Volume
docker run -v mein-volumen:/data ubuntu # Bindet ein Volume an /data im Container
docker volume rm mein-volumen         # Löscht ein Volume
```

---

## 🖥 Linux-Befehle für WSL & Docker

### 📂 Dateien & Verzeichnisse verwalten
```sh
ls                  # Zeigt den Inhalt des aktuellen Verzeichnisses an
cd <ORDNER>         # Wechselt in ein anderes Verzeichnis
pwd                 # Zeigt das aktuelle Verzeichnis an
mkdir <NAME>        # Erstellt einen neuen Ordner
rm <DATEI>          # Löscht eine Datei
rm -r <ORDNER>      # Löscht einen Ordner mit allen Dateien
mv <ALT> <NEU>      # Verschiebt oder benennt eine Datei um
cp <QUELLE> <ZIEL>  # Kopiert eine Datei
```

### 🔄 Prozesse & Systembefehle
```sh
top              # Zeigt laufende Prozesse an
ps aux           # Zeigt alle aktiven Prozesse an
kill <PID>       # Beendet einen Prozess mit der ID <PID>
free -h          # Zeigt den Speicherverbrauch an
df -h            # Zeigt den Speicherplatz an
```

### 🔑 Berechtigungen & Nutzerverwaltung
```sh
sudo <BEFEHL>            # Führt einen Befehl mit Admin-Rechten aus
chmod 777 <DATEI>        # Ändert Dateiberechtigungen (777 = alles erlaubt)
chown user:group <DATEI> # Ändert den Besitzer einer Datei
```

---

## 🎯 Zusammenfassung
✅ **Docker** → Container starten, verwalten, Netzwerke und Volumes nutzen  
✅ **Linux** → Dateien bearbeiten, Prozesse kontrollieren, Rechte setzen  

🚀 **Diese Datei ist deine schnelle Referenz für Docker & Linux in WSL!**
