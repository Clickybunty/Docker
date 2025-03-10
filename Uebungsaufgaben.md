# Praktische Übung: Web-Anwendung mit Docker erstellen und verwalten

## Aufgabe:

Erstelle eine einfache mehrseitige Website und betreibe sie in einem Docker-Container. Du sollst dann auch eine kleine Änderung an der Website vornehmen und den Container verwalten.

## Schritte:

### 1. Projektordner anlegen
Erstelle einen neuen Ordner für dein Projekt und wechsle in diesen:
```bash
mkdir docker-webprojekt
cd docker-webprojekt
```

### 2. Website-Dateien erstellen

a) Erstelle eine `index.html` Datei:
```bash
echo '<!DOCTYPE html>
<html>
<head>
    <title>Meine Docker-Website</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Willkommen zu meiner Docker-Übung</h1>
        <nav>
            <a href="index.html">Startseite</a> |
            <a href="ueber.html">Über Docker</a>
        </nav>
    </header>
    <main>
        <p>Diese Seite wird über einen Docker-Container bereitgestellt.</p>
        <p>Besuche die "Über Docker"-Seite für mehr Informationen.</p>
    </main>
    <footer>
        <p>Docker-Übung 2025</p>
    </footer>
</body>
</html>' > index.html
```

b) Erstelle eine `ueber.html` Datei:
```bash
echo '<!DOCTYPE html>
<html>
<head>
    <title>Über Docker</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Über Docker</h1>
        <nav>
            <a href="index.html">Startseite</a> |
            <a href="ueber.html">Über Docker</a>
        </nav>
    </header>
    <main>
        <h2>Was ist Docker?</h2>
        <p>Docker ist eine Open-Source-Plattform für die Entwicklung, den Versand und die Ausführung von Anwendungen.</p>
        <p>Docker ermöglicht es, Anwendungen von der Infrastruktur zu trennen, sodass Software schnell bereitgestellt werden kann.</p>
    </main>
    <footer>
        <p>Docker-Übung 2025</p>
    </footer>
</body>
</html>' > ueber.html
```

c) Erstelle eine `style.css` Datei:
```bash
echo 'body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
    line-height: 1.6;
    color: #333;
}

header {
    background-color: #f4f4f4;
    padding: 10px;
    margin-bottom: 20px;
    border-radius: 5px;
}

nav {
    margin-top: 10px;
}

main {
    padding: 10px;
}

footer {
    margin-top: 20px;
    text-align: center;
    font-size: 0.8em;
    color: #777;
}

a {
    color: #0066cc;
    text-decoration: none;
}

a:hover {
    text-decoration: underline;
}' > style.css
```

### 3. Starte einen Container mit deiner Website
```bash
docker run -d -p 8080:80 \
  -v $(pwd)/index.html:/usr/share/nginx/html/index.html \
  -v $(pwd)/ueber.html:/usr/share/nginx/html/ueber.html \
  -v $(pwd)/style.css:/usr/share/nginx/html/style.css \
  --name meine-website nginx
```

### 4. Überprüfe deine Website
Öffne einen Browser und gehe zu:
- http://localhost:8080 (Startseite)
- http://localhost:8080/ueber.html (Über-Seite)


Aktualisiere die Seite in deinem Browser und beobachte die Änderung.

### 5. Untersuche den Container
```bash
# Container-Logs anzeigen
docker logs meine-website

# Verbindung zum Container herstellen
docker exec -it meine-website bash

# Im Container: Überprüfe, ob die Dateien korrekt gemountet wurden
ls -la /usr/share/nginx/html/

# Container verlassen
exit
```

### 6. Stoppe und entferne den Container
```bash
docker stop meine-website
docker rm meine-website
```

### 7. Bonus-Aufgabe: Erstelle ein eigenes Docker-Image

Erstelle eine Dockerfile-Datei:
```bash
echo 'FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
COPY ueber.html /usr/share/nginx/html/
COPY style.css /usr/share/nginx/html/
EXPOSE 80' > Dockerfile
```

Baue dein eigenes Image:
```bash
docker build -t meine-eigene-website .
```

Starte einen Container von deinem eigenen Image:
```bash
docker run -d -p 8081:80 --name mein-eigenes-image meine-eigene-website
```

Überprüfe, ob die Website unter http://localhost:8081 funktioniert.

## Lösung überprüfen:

Deine Lösung ist korrekt, wenn:
1. Deine Website unter http://localhost:8080 aufrufbar ist
2. Du die Dateien im Container anzeigen kannst
3. Du den Container erfolgreich gestoppt und entfernt hast
4. (Bonus) Deine eigene Website über das selbst erstellte Image unter http://localhost:8081 läuft


---


# Docker Hello World: Grundlagen-Anleitung

Diese Anleitung zeigt dir die grundlegenden Schritte, um mit Docker zu arbeiten - vom Starten eines Webservers bis zum Löschen des Containers.

## Docker Installation auf WSL

### Option 1: Docker Desktop
1. Lade [Docker Desktop für Windows](https://www.docker.com/products/docker-desktop/) herunter und installiere es
2. Aktiviere in den Einstellungen "Use the WSL 2 based engine"
3. Aktiviere unter "Resources > WSL Integration" deine WSL-Distribution
4. Docker ist nun in deiner WSL-Distribution verfügbar

### Option 2: Direkte Installation in WSL
```bash
# Docker mit einem Befehl installieren
curl -fsSL https://get.docker.com | sudo sh

# Docker starten
sudo service docker start

```

## Grundlegende Docker-Befehle

### 1. Testen, ob Docker funktioniert
```bash
docker run hello-world
```
**Erklärung:** Dieser Befehl lädt das kleine "hello-world" Image herunter und führt es aus. Docker wird:
1. Lokal nach dem Image "hello-world" suchen
2. Es von Docker Hub herunterladen, wenn es nicht gefunden wird
3. Einen Container aus diesem Image erstellen
4. Den Container starten, der eine Erfolgsmeldung ausgibt
5. Den Container beenden, nachdem die Meldung ausgegeben wurde

### 2. Webserver starten
```bash
docker run -d -p 8080:80 --name mein-webserver nginx
```
**Erklärung:** Dieser Befehl startet einen NGINX-Webserver-Container.
- `docker run`: Befehl zum Erstellen und Starten eines Containers
- `-d`: (detached) Führt den Container im Hintergrund aus, so dass du das Terminal weiter benutzen kannst
- `-p 8080:80`: (port) Leitet Anfragen vom Host-Port 8080 an den Container-Port 80 weiter
  - 80 ist der Standard-HTTP-Port, auf dem NGINX im Container läuft
  - 8080 ist der Port, über den du von außen auf den Webserver zugreifst
- `--name mein-webserver`: Gibt dem Container einen benutzerdefinierten Namen, damit du ihn leichter verwalten kannst
- `nginx`: Der Name des Images, das verwendet werden soll (offizielles NGINX-Image)

### 3. Überprüfen, ob der Container läuft
```bash
docker ps
```
**Erklärung:** Zeigt alle laufenden Container an.
- Die Ausgabe enthält wichtige Informationen:
  - CONTAINER ID: Eindeutige ID des Containers
  - IMAGE: Das verwendete Image
  - COMMAND: Der Startbefehl des Containers
  - CREATED: Wann der Container erstellt wurde
  - STATUS: Aktueller Status des Containers
  - PORTS: Port-Weiterleitungen
  - NAMES: Name des Containers (hier: "mein-webserver")

### 4. Auf den Webserver zugreifen
Öffne einen Browser und gehe zu: http://localhost:8080

**Erklärung:** Die Anfrage wird folgendermaßen verarbeitet:
1. Dein Browser sendet eine Anfrage an localhost (deinen Computer) auf Port 8080
2. Docker fängt diese Anfrage ab, da es Port 8080 überwacht
3. Docker leitet die Anfrage an Port 80 im NGINX-Container weiter
4. NGINX im Container verarbeitet die Anfrage und sendet die Standard-Willkommensseite zurück
5. Diese Seite wird in deinem Browser angezeigt

### 5. Verbindung zum Container herstellen
```bash
docker exec -it mein-webserver bash
```
**Erklärung:** Dieser Befehl öffnet eine Bash-Shell im laufenden Container.
- `docker exec`: Führt einen Befehl in einem laufenden Container aus
- `-i`: (interactive) Behält STDIN offen, so dass du Befehle eingeben kannst
- `-t`: (tty) Weist ein Pseudo-Terminal zu, das die Ausgabe formatiert
- `mein-webserver`: Der Name des Containers, mit dem du dich verbinden möchtest
- `bash`: Der Befehl, den du im Container ausführen möchtest (hier: eine Bash-Shell starten)

Im Container kannst du folgendes ausprobieren:
```bash
# Webserver-Verzeichnis anzeigen
ls -la /usr/share/nginx/html/
# Dieser Befehl zeigt den Inhalt des Webroot-Verzeichnisses an, wo NGINX die Webseiten speichert

# Webserver-Konfiguration anzeigen
cat /etc/nginx/conf.d/default.conf
# Zeigt die Standardkonfiguration von NGINX an

# Container verlassen
exit
# Beendet die Shell-Sitzung im Container (der Container läuft weiter)
```

### 6. Container-Logs anzeigen
```bash
docker logs mein-webserver
```
**Erklärung:** Zeigt die Logs (Standardausgabe und Standardfehlerausgabe) des Containers an.
- Dies ist nützlich, um Probleme zu diagnostizieren oder zu sehen, was im Container passiert
- Bei NGINX siehst du hier die Zugriffslogs und Fehlermeldungen

Für kontinuierliche Logausgabe (wie `tail -f`):
```bash
docker logs -f mein-webserver
```
- `-f`: (follow) Zeigt neue Logeinträge an, sobald sie entstehen (Beenden mit Strg+C)

### 7. Container stoppen
```bash
docker stop mein-webserver
```
**Erklärung:** Stoppt den laufenden Container.
- Der Befehl sendet ein SIGTERM-Signal an den Hauptprozess im Container
- Nach einer kurzen Wartezeit (standardmäßig 10 Sekunden) wird SIGKILL gesendet, falls der Container nicht reagiert
- Der Container wird gestoppt, aber nicht gelöscht - alle Daten bleiben erhalten
- Nach dem Stoppen ist der Webserver unter http://localhost:8080 nicht mehr erreichbar

### 8. Container wieder starten
```bash
docker start mein-webserver
```
**Erklärung:** Startet einen gestoppten Container wieder.
- Alle Daten und Einstellungen des Containers bleiben erhalten
- Port-Weiterleitungen werden wiederhergestellt
- Der Webserver ist wieder unter http://localhost:8080 erreichbar

### 9. Container löschen (muss erst gestoppt sein)
```bash
docker stop mein-webserver
docker rm mein-webserver
```
**Erklärung:**
- `docker stop`: Stoppt den Container, wie bereits erklärt
- `docker rm`: Entfernt/löscht den Container
  - Dies löscht den Container und alle nicht in Volumes gespeicherten Daten permanent
  - Das zugrunde liegende Image (nginx) bleibt auf dem System
  - Nach dem Löschen ist der Name "mein-webserver" wieder verfügbar

## Weitere nützliche Befehle

### Images verwalten
```bash
# Alle lokal vorhandenen Images anzeigen
docker images
```
**Erklärung:** Listet alle auf deinem System heruntergeladenen Docker-Images auf.
- Die Ausgabe zeigt:
  - REPOSITORY: Name des Images
  - TAG: Version des Images (z.B. "latest")
  - IMAGE ID: Eindeutige ID des Images
  - CREATED: Erstellungszeitpunkt
  - SIZE: Größe des Images

```bash
# Ein bestimmtes Image herunterladen
docker pull ubuntu
```
**Erklärung:** Lädt das angegebene Image von Docker Hub herunter.
- `pull`: Nur das Image herunterladen, ohne einen Container zu starten
- Standardmäßig wird das Tag "latest" verwendet (neueste Version)
- Du kannst auch eine spezifische Version angeben: `docker pull ubuntu:20.04`

```bash
# Ein ungenutztes Image löschen
docker rmi nginx
```
**Erklärung:** Entfernt ein Image von deinem lokalen System.
- `rmi`: Remove Image
- Das Image kann nur gelöscht werden, wenn keine Container davon abhängen
- Mit `-f` kannst du das Löschen erzwingen, aber das kann Probleme verursachen

### Container verwalten
```bash
# Alle Container anzeigen (auch gestoppte)
docker ps -a
```
**Erklärung:** Zeigt alle Container an, nicht nur laufende.
- `-a`: (all) Zeigt sowohl laufende als auch gestoppte Container an
- Dies ist nützlich, um Container zu finden, die du vielleicht vergessen hast zu löschen

```bash
# Alle gestoppten Container löschen
docker container prune
```
**Erklärung:** Entfernt alle gestoppten Container, um Speicherplatz freizugeben.
- Du wirst um Bestätigung gebeten, bevor die Container gelöscht werden
- Dies ist nützlich, um "Containerleichen" zu beseitigen

### Einfache Webseite erstellen

Wenn du eine eigene Webseite anzeigen möchtest:

1. Erstelle eine `index.html` Datei:
```bash
echo '<html><body><h1>Meine Docker-Webseite</h1></body></html>' > index.html
```
**Erklärung:** Erstellt eine einfache HTML-Datei im aktuellen Verzeichnis.
- `echo`: Gibt den Text aus
- `>`: Leitet die Ausgabe in die Datei um (überschreibt bestehende Dateien)
- Die HTML-Datei enthält nur einen Titel "Meine Docker-Webseite"

2. Starte einen Container mit der eigenen Webseite:
```bash
docker run -d -p 8080:80 -v $(pwd)/index.html:/usr/share/nginx/html/index.html --name meine-webseite nginx
```
**Erklärung:** Startet einen NGINX-Container und ersetzt die Standard-Webseite durch deine eigene.
- `-d -p 8080:80 --name meine-webseite`: Wie bereits erklärt
- `-v $(pwd)/index.html:/usr/share/nginx/html/index.html`: Bindet deine lokale index.html-Datei an die im Container
  - `-v`: (volume) Erstellt eine Verbindung zwischen Host und Container
  - `$(pwd)`: Gibt das aktuelle Arbeitsverzeichnis zurück
  - `/usr/share/nginx/html/index.html`: Pfad zur Standardwebseite im Container
  - Änderungen an der lokalen Datei werden sofort im Container sichtbar

3. Öffne im Browser: http://localhost:8080
   - Du solltest jetzt deine eigene Webseite sehen anstelle der NGINX-Standardseite

Das war's! Du hast einen Docker-Container mit einem Webserver gestartet, dich mit ihm verbunden und gelernt, wie man ihn verwaltet. Dies sind die wichtigsten Grundlagen für den Einstieg in Docker.


