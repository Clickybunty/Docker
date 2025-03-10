# 📜 Umfangreiche Liste aller Docker-Befehle mit Erklärungen

## 📌 Grundlegende Docker-Befehle

### 🔍 **1. Docker Version & Systeminformationen anzeigen**
```bash
docker --version
```
📌 Zeigt die installierte Docker-Version an.

```bash
docker info
```
📌 Zeigt detaillierte Informationen über die Docker-Installation, wie Anzahl der Container, Images, Laufzeit usw.

---

## 🏗 **Docker-Container verwalten**

### 🏃 **2. Einen neuen Container starten**
```bash
docker run IMAGE
```
📌 Erstellt und startet einen neuen Container basierend auf dem angegebenen Image.

#### **Zusätze für `docker run`**
- `-d` → Detached Mode (**Container läuft im Hintergrund**).
- `-it` → Interaktiver Modus mit Terminal (z. B. für Bash).
- `--rm` → Löscht den Container nach dem Beenden automatisch.
- `--name NAME` → Gibt dem Container einen benutzerdefinierten Namen.
- `-p HOSTPORT:CONTAINERPORT` → Verbindet einen lokalen Port mit dem Container.

```bash
docker run -d -p 8080:80 --name mein-container nginx
```
📌 Startet einen **Nginx-Server**, der über `http://localhost:8080` erreichbar ist.

---

### ⏯ **3. Laufende Container anzeigen, stoppen und starten**
```bash
docker ps
```
📌 Zeigt alle laufenden Container an.

```bash
docker ps -a
```
📌 Zeigt alle Container, auch gestoppte.

```bash
docker stop CONTAINER_ID
```
📌 Stoppt einen laufenden Container.

```bash
docker start CONTAINER_ID
```
📌 Startet einen gestoppten Container erneut.

```bash
docker restart CONTAINER_ID
```
📌 Startet einen Container neu.

---

### 🗑 **4. Container entfernen**
```bash
docker rm CONTAINER_ID
```
📌 Entfernt einen gestoppten Container.

```bash
docker rm $(docker ps -aq)
```
📌 Löscht **alle gestoppten Container**.

```bash
docker stop $(docker ps -q) && docker rm $(docker ps -aq)
```
📌 Stoppt und löscht **alle Container**.

---

## 📦 **Docker-Images verwalten**

### 🔍 **5. Docker-Images anzeigen & herunterladen**
```bash
docker images
```
📌 Zeigt alle lokalen Docker-Images an.

```bash
docker pull IMAGE_NAME
```
📌 Lädt ein Docker-Image aus Docker Hub herunter.

---

### 🗑 **6. Docker-Images löschen**
```bash
docker rmi IMAGE_ID
```
📌 Entfernt ein bestimmtes Docker-Image.

```bash
docker image prune -a
```
📌 Löscht **alle nicht verwendeten Images**, um Speicherplatz freizugeben.

---

## 🏗 **Docker-Images erstellen**

### 🏗 **7. Ein eigenes Docker-Image bauen**
```bash
docker build -t IMAGE_NAME .
```
📌 Erstellt ein neues Image basierend auf einem `Dockerfile` im aktuellen Verzeichnis.

#### **Zusätze für `docker build`**
- `-t` → Gibt dem Image einen Namen (Tagging).
- `.` → Bedeutet, dass das `Dockerfile` im aktuellen Verzeichnis verwendet wird.

Beispiel:
```bash
docker build -t mein-image .
```
📌 Erstellt ein Image mit dem Namen `mein-image` aus dem `Dockerfile`.

---

## 🔄 **In laufende Container einsteigen**

### 🔍 **8. In einen laufenden Container einloggen**
```bash
docker exec -it CONTAINER_ID bash
```
📌 Öffnet eine interaktive Shell im Container.

```bash
docker attach CONTAINER_ID
```
📌 Verbindet das Terminal mit dem laufenden Container (wie `exec`, aber ohne neues Bash-Fenster).

```bash
docker logs CONTAINER_ID
```
📌 Zeigt die Logs eines Containers an.

```bash
docker logs -f CONTAINER_ID
```
📌 Zeigt Live-Logs des Containers an (nützlich für Fehleranalyse).

---

## 🌍 **Netzwerk & Ports in Docker**

### 🌍 **9. Netzwerk in Docker verwalten**
```bash
docker network ls
```
📌 Zeigt alle Docker-Netzwerke an.

```bash
docker network create MEIN_NETZWERK
```
📌 Erstellt ein neues benutzerdefiniertes Docker-Netzwerk.

```bash
docker network inspect MEIN_NETZWERK
```
📌 Zeigt Details zu einem bestimmten Netzwerk.

```bash
docker network rm MEIN_NETZWERK
```
📌 Löscht ein Docker-Netzwerk.

---

## 📁 **Docker-Volumes (Daten persistent speichern)**

### 📂 **10. Volumes erstellen & verwalten**
```bash
docker volume create MEIN_VOLUME
```
📌 Erstellt ein neues Docker-Volume.

```bash
docker volume ls
```
📌 Listet alle Docker-Volumes auf.

```bash
docker volume rm MEIN_VOLUME
```
📌 Löscht ein bestimmtes Volume.

```bash
docker volume prune
```
📌 Löscht **alle unbenutzten Volumes**.

---

## 🚀 **Zusätzliche nützliche Befehle**

### 🔍 **11. Prozesse und Status prüfen**
```bash
history
```
📌 Zeigt die Befehls-Historie an.

```bash
sudo netstat -tulnp | grep PORT
```
📌 Zeigt, welcher Prozess einen bestimmten Port belegt.

```bash
docker system prune -a
```
📌 Löscht **alle unbenutzten Images, Container, Volumes und Netzwerke**, um Speicherplatz freizugeben.

```bash
docker stats
```
📌 Zeigt die Echtzeit-Ressourcennutzung der laufenden Container an.

```bash
docker top CONTAINER_ID
```
📌 Zeigt die aktiven Prozesse in einem laufenden Container.

---

## 🎯 **Fazit**
Diese Befehlsübersicht hilft dir, Docker effektiv zu verwalten. Egal ob Container, Images, Netzwerke oder Volumes – du hast jetzt die wichtigsten Befehle immer griffbereit! 🚀
