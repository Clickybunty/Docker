# Docker Compose Zusammenfassung

## 1. Einleitung
Docker Compose ist ein Tool zur Definition und Verwaltung mehrerer Container in einer Anwendung. Es ermöglicht die einfache Orchestrierung von Containern mit einer YAML-Datei (`docker-compose.yml`).

---

## 2. Grundlegende Befehle

### **Allgemeine Befehle**
- `docker-compose version` – Zeigt die aktuelle Version von Docker Compose.
- `docker-compose help` – Listet alle verfügbaren Befehle auf.

### **Konfiguration und Build**
- `docker-compose config` – Prüft die `docker-compose.yml` auf Fehler.
- `docker-compose build` – Erstellt Container-Images basierend auf der Konfiguration.
- `docker-compose pull` – Lädt die benötigten Images aus Docker Hub.
- `docker-compose push` – Lädt selbst erstellte Images in eine Registry hoch.

### **Container-Verwaltung**
- `docker-compose up -d` – Startet alle definierten Container im Hintergrund.
- `docker-compose ps` – Zeigt eine Liste aller laufenden Container.
- `docker-compose stop` – Stoppt laufende Container.
- `docker-compose start` – Startet zuvor gestoppte Container neu.
- `docker-compose restart` – Startet Container neu.
- `docker-compose pause` – Pausiert laufende Container.
- `docker-compose unpause` – Setzt pausierte Container fort.
- `docker-compose logs` – Zeigt Logs der Container.
- `docker-compose exec <service> sh` – Öffnet eine interaktive Shell im Container.
- `docker-compose kill` – Erzwingt das Stoppen aller Container.
- `docker-compose rm -f` – Löscht gestoppte Container.
- `docker-compose down` – Stoppt und entfernt Container, Netzwerke und Volumes.

### **Netzwerk & Skalierung**
- `docker-compose port <service> <port>` – Zeigt die Zuordnung von Container-Ports zu Host-Ports.
- `docker-compose run <service> sh` – Erstellt und startet einen neuen Container des Services.
- `docker-compose scale <service>=<anzahl>` – Skaliert die Anzahl der laufenden Container.

---

## 3. Beispiel `docker-compose.yml`

```yaml
version: '3.8'

services:
  webserver:
    image: nginx:alpine
    container_name: webserver
    restart: unless-stopped
    ports:
      - "8080:80"

  db:
    image: mysql:5.7
    container_name: Mysqldb
    restart: unless-stopped
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: my_database
      MYSQL_USER: my_user
      MYSQL_PASSWORD: user_password

volumes:
  db_data:
```

---

## 4. Wichtige Erkenntnisse
- **Container können mit wenigen Zeilen Code orchestriert werden.**
- **`docker-compose up -d`** reicht aus, um eine komplette Umgebung zu starten.
- **Volumen & Netzwerke** können in der `docker-compose.yml` definiert werden, um persistente Daten zu sichern.
- **Skalierung mit `scale`** ermöglicht das Hochfahren mehrerer Instanzen eines Services.
- **Docker Compose erleichtert die Entwicklung** von Anwendungen mit mehreren Containern (z. B. Webserver + Datenbank).

---

## 5. Nächste Schritte
- **Docker Swarm kennenlernen**, um Container über mehrere Hosts hinweg zu verwalten.
- **Eigene Compose-Dateien optimieren**, z. B. mit `.env`-Dateien für Umgebungsvariablen.
- **Automatisierung mit CI/CD**, um Docker Compose in Deployment-Pipelines zu integrieren.

