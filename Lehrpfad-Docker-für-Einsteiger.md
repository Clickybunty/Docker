# Einleitung: Warum Docker?
Docker ist eine Plattform zur Containerisierung von Anwendungen. Statt Software in unterschiedlichen Umgebungen mit verschiedenen Abhängigkeiten manuell einzurichten, kannst du mit Docker Anwendungen in Containern kapseln. Diese Container sind leicht, portabel und laufen auf jedem System, das Docker unterstützt.

## Vorteile von Docker:

- **Portabilität:** Einmal erstellte Container laufen überall gleich – ob lokal, in der Cloud oder auf Servern.
- **Isolation:** Container laufen unabhängig voneinander und beeinflussen sich nicht gegenseitig.
- **Effizienz:** Docker nutzt weniger Ressourcen als virtuelle Maschinen, da es den Host-Kernel teilt.
- **Schnelle Bereitstellung:** Anwendungen können in Sekunden gestartet und gestoppt werden.

# Lektion 1: Docker installieren und erste Befehle

## 1. Docker installieren
Je nach Betriebssystem gibt es unterschiedliche Installationsschritte. Hier die offiziellen Links für dein System:

- **Windows & Mac:** Docker Desktop herunterladen und installieren.
- **Linux (Ubuntu, Debian, etc.):**

```bash
sudo apt update
sudo apt install docker.io
```

Danach sicherstellen, dass Docker läuft:

```bash
sudo systemctl enable --now docker
```

## 2. Überprüfung der Installation
Teste, ob Docker erfolgreich installiert wurde, indem du folgendes eingibst:

```bash
docker --version
```

Es sollte eine Versionsnummer von Docker ausgegeben werden.

## 3. Der erste Container: "Hello World"
Starte deinen ersten Docker-Container:

```bash
docker run hello-world
```

Dieser Befehl:

1. Lädt das offizielle `hello-world` Image von Docker Hub herunter (wenn es nicht lokal vorhanden ist).
2. Erstellt einen Container und führt ihn aus.
3. Gibt eine Begrüßungsnachricht aus, um zu bestätigen, dass Docker funktioniert.

## 4. Die wichtigsten Docker-Befehle kennenlernen
Nachdem du deinen ersten Container gestartet hast, sind hier einige grundlegende Befehle:

**Laufende Container anzeigen:**

```bash
docker ps
```

**Alle Container (auch gestoppte) anzeigen:**

```bash
docker ps -a
```

**Container stoppen:**

```bash
docker stop <CONTAINER_ID>
```

**Container entfernen:**

```bash
docker rm <CONTAINER_ID>
```

**Heruntergeladene Images anzeigen:**

```bash
docker images
```

**Ein Image entfernen:**

```bash
docker rmi <IMAGE_ID>
```

# Lektion 2: Eigene Container erstellen und verwalten
Nachdem du die grundlegenden Docker-Befehle kennengelernt hast, geht es jetzt darum, eigene Container zu erstellen und sie nach deinen Bedürfnissen anzupassen.

## 1. Docker-Images mit Dockerfile erstellen
Ein Docker-Image ist die Blaupause für einen Container. Um ein eigenes Image zu erstellen, brauchst du eine `Dockerfile`.

### Beispiel: Ein einfaches Dockerfile
Erstelle einen neuen Ordner und wechsle in ihn:

```bash
mkdir mein-docker-projekt && cd mein-docker-projekt
```

Erstelle eine Datei mit dem Namen `Dockerfile`:

```bash
touch Dockerfile
```

Öffne die Datei in einem Editor und füge folgende Zeilen hinzu:

```dockerfile
# Basis-Image (z. B. eine minimalistische Linux-Distribution)
FROM ubuntu:latest 

# Autor des Docker-Images (optional)
LABEL maintainer="Stevan Menicanin"

# Shell-Befehl, der beim Erstellen des Containers ausgeführt wird
RUN apt update && apt install -y curl

# Standardbefehl, wenn der Container gestartet wird
CMD ["bash"]
```

## 2. Ein eigenes Image bauen
Um ein Image aus dem Dockerfile zu erstellen, verwende den folgenden Befehl (im Verzeichnis, in dem das Dockerfile liegt):

```bash
docker build -t mein-ubuntu-image .
```

### Erklärung:
- `docker build` – Baut ein neues Docker-Image.
- `-t mein-ubuntu-image` – Gibt dem Image den Namen „mein-ubuntu-image“.
- `.` – Sagt Docker, dass das Dockerfile im aktuellen Verzeichnis liegt.

Nach erfolgreichem Bau kannst du dein Image in der Liste der Docker-Images sehen:

```bash
docker images
```

## 3. Container aus eigenem Image starten
Jetzt kannst du dein eigenes Image verwenden, um einen Container zu starten:

```bash
docker run -it mein-ubuntu-image
```

### Erklärung:
- `-it` – Öffnet eine interaktive Shell im Container.
- `mein-ubuntu-image` – Name des erstellten Images.

Im Container hast du jetzt eine Ubuntu-Umgebung, in der du Befehle ausführen kannst. Beende den Container mit:

```bash
exit
```

## 4. Container verwalten und persistente Daten speichern
Standardmäßig sind Änderungen in einem Container nicht dauerhaft. Um Daten zu speichern, gibt es zwei Lösungen:

### **Volumes verwenden**
Volumes sind Docker-spezifische Speicherbereiche, die von Containern genutzt werden können.

```bash
docker run -v mein-volume:/data -it mein-ubuntu-image
```

Der Container hat nun Zugriff auf das Volume `/data`.

### **Ordner vom Host als Volume einbinden**
Statt ein internes Volume zu nutzen, kannst du auch einen Ordner deines Computers mit dem Container teilen:

```bash
docker run -v $(pwd)/meine-daten:/data -it mein-ubuntu-image
```

Alles, was im Container im Ordner `/data` gespeichert wird, erscheint auch auf deinem Host im Ordner `meine-daten`.

## **Zusammenfassung**
- Ein `Dockerfile` definiert die Basis eines eigenen Images.
- `docker build -t mein-ubuntu-image .` erstellt ein Image.
- `docker run -it mein-ubuntu-image` startet einen Container aus diesem Image.
- Volumes ermöglichen das Speichern von Daten über Container-Neustarts hinweg.


## 3. Docker Volumes – Gemeinsamer Zugriff und Nutzung für Datenbanken
Docker-Volumes sind der bevorzugte Weg, um Daten zwischen Containern zu teilen und zu persistieren, da sie nicht gelöscht werden, wenn ein Container entfernt wird. Sie werden häufig für Datenbanken und gemeinsame Speicherbereiche genutzt.

### 1. Wie funktionieren Docker Volumes?
Docker-Volumes sind unabhängig von Containern und bleiben bestehen, selbst wenn der Container gelöscht wird. Mehrere Container können gleichzeitig auf dasselbe Volume zugreifen.

Ein einfaches Volume erstellen:

```bash
docker volume create mein-daten-volume
```

Liste aller Volumes:

```bash
docker volume ls
```

Zeige Details zu einem Volume an:

```bash
docker volume inspect mein-daten-volume
```

### 2. Mehrere Container greifen auf dasselbe Volume zu
Starte den ersten Container:

```bash
docker run -dit --name container1 -v mein-daten-volume:/data ubuntu
```

Starte den zweiten Container mit dem gleichen Volume:

```bash
docker run -dit --name container2 -v mein-daten-volume:/data ubuntu
```

Jetzt haben beide Container Zugriff auf das Volume `/data`. Änderungen in einem Container sind auch im anderen sichtbar.

### 3. Docker Volumes für Datenbanken nutzen
Datenbanken speichern ihre Daten standardmäßig innerhalb des Containers. Dies ist problematisch, weil die Daten verloren gehen, wenn der Container gelöscht wird.

Beispiel: MySQL mit Volume:

```bash
docker run -d --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=geheim \
  -v mysql-daten:/var/lib/mysql \
  mysql:latest
```

### 4. Auf die Datenbank mit einem zweiten Container zugreifen

```bash
docker run -d --name adminer-container \
  --link mysql-container:mysql \
  -p 8080:8080 adminer
```

Dann Adminer im Browser öffnen:

[http://localhost:8080](http://localhost:8080)

### 5. Gemeinsames Volume für mehrere Datenbank-Container
Mehrere MySQL-Container können dieselben Daten nutzen, indem sie auf dasselbe Volume zugreifen.

```bash
docker run -d --name mysql1 -e MYSQL_ROOT_PASSWORD=geheim -v shared-db:/var/lib/mysql mysql:latest
```

```bash
docker run -d --name mysql2 -e MYSQL_ROOT_PASSWORD=geheim -v shared-db:/var/lib/mysql mysql:latest
```

## **Zusammenfassung**
- Docker Volumes bleiben erhalten, auch wenn ein Container gelöscht wird.
- Mehrere Container können ein Volume teilen, indem sie es mit `-v` einbinden.
- Datenbanken nutzen Volumes, um Daten dauerhaft zu speichern.
- Adminer oder andere Clients können auf die Datenbank zugreifen.
- Mehrere MySQL-Container können dasselbe Volume verwenden, um Daten gemeinsam zu nutzen.
