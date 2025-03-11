# Docker Workflow Anleitung

## 1. Erstellen des Projektverzeichnisses
```bash
mkdir meine-docker-webseite
cd meine-docker-webseite
```
- Erstellt ein neues Verzeichnis `meine-docker-webseite`.
- Wechsel in dieses Verzeichnis.

## 2. Erstellung der `index.html` Datei
```bash
echo '<!DOCTYPE html>
<html>
<head>
    <title>Meine Docker-Webseite</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 30px; background-color: #f0f8ff; }
        .container { max-width: 800px; margin: 0 auto; background-color: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        h1 { color: #0066cc; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Hallo Docker!</h1>
        <p>Dies ist meine erste Webseite in einem Docker-Container.</p>
        <p>Heutiges Datum: <span id="datum"></span></p>
    </div>
    <script>
        document.getElementById("datum").textContent = new Date().toLocaleDateString();
    </script>
</body>
</html>' > index.html
```
- Erstellt eine einfache HTML-Seite mit Begrüßungstext und Datumsausgabe.

## 3. Erstellung der `Dockerfile` Datei
```bash
echo 'FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]' > Dockerfile
```
- Basis-Image: `nginx:alpine`.
- Kopiert `index.html` in das Webserver-Verzeichnis.
- Exponiert Port `80`.
- Startet `nginx` im Vordergrund.

## 4. Build und Start des Containers
```bash
docker build -t meine-webseite .
docker run -d -p 8080:80 --name mein-webserver meine-webseite
```
- Erstellt ein Docker-Image `meine-webseite`.
- Startet den Container und mappt Port `8080` auf `80`.
- Webseite erreichbar unter `http://localhost:8080`.

## 5. Aktualisierung der Webseite
```bash
echo '<!DOCTYPE html>
<html>
<head>
    <title>Meine Docker-Webseite</title>
    <style>
        h1 { color: #cc0066; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Hallo Docker! (Aktualisiert)</h1>
        <p>Dies ist meine erste Webseite in einem Docker-Container.</p>
        <p>Heutiges Datum: <span id="datum"></span></p>
        <p><strong>Diese Zeile wurde hinzugefügt!</strong></p>
    </div>
</body>
</html>' > index.html
```
- Die HTML-Datei wird aktualisiert.

## 6. Neustart mit neuer Version
```bash
docker stop mein-webserver
docker rm mein-webserver
docker build -t meine-webseite:v2 .
docker run -d -p 8080:80 --name mein-webserver-v2 meine-webseite:v2
```
- Stoppt und löscht den alten Container.
- Erstellt ein neues Image `v2` und startet einen neuen Container.

## 7. Überprüfung des Containers
```bash
docker logs mein-webserver-v2
docker exec -it mein-webserver-v2 sh
ls -la /usr/share/nginx/html/
cat /etc/nginx/conf.d/default.conf
exit
```
- Zeigt Logs und überprüft die Konfiguration.

## 8. Bereinigung (optional)
```bash
docker stop mein-webserver-v2
docker rm mein-webserver-v2
docker images
docker rmi meine-webseite meine-webseite:v2
```
- Stoppt und entfernt den Container und das Image.

## 9. Erstellen eines dynamischen Startskripts
```bash
echo '#!/bin/sh
DATE=$(date)
HOSTNAME=$(hostname)
echo "<div class=\"dynamic-info\">
<p>Container-ID: $HOSTNAME</p>
<p>Container gestartet am: $DATE</p>
</div>" > /tmp/dynamic-content.html
sed -i "s|</div>|$(cat /tmp/dynamic-content.html)\n</div>|" /usr/share/nginx/html/index.html
nginx -g "daemon off;"' > start.sh
chmod +x start.sh
```
- Fügt Container-Informationen zur HTML-Seite hinzu.

## 10. Neues `Dockerfile` für dynamische Inhalte
```bash
echo 'FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY index.html .
COPY start.sh /start.sh
ENV ERSTELLER="Docker-Kursteilnehmer"
RUN echo "<p>Erstellt von: $ERSTELLER</p>" >> index.html
EXPOSE 80
CMD ["/start.sh"]' > Dockerfile.dynamic
```
- Setzt dynamische Inhalte durch ein Startskript.

## 11. Build und Start des neuen Containers
```bash
docker build -t dynamische-webseite -f Dockerfile.dynamic .
docker run -d -p 8081:80 --name dynamischer-container dynamische-webseite
```
- Erstellt und startet den neuen dynamischen Container.
- Webseite erreichbar unter `http://localhost:8081`.

