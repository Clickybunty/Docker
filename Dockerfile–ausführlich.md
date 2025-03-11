# **Dockerfile – Eine ausführliche Erklärung**

## **Einleitung**
Ein `Dockerfile` ist eine textbasierte Datei, die Anweisungen enthält, um ein Docker-Image zu erstellen. Docker nutzt diese Datei, um eine wiederholbare und konsistente Umgebung für Anwendungen bereitzustellen.

Dockerfiles ermöglichen es, Software und deren Abhängigkeiten in einem Container zu kapseln, sodass sie auf jedem System mit Docker einheitlich ausgeführt werden kann.

---

## **Bedeutung der Dockerfile-Anweisungen**
In einem Dockerfile sind verschiedene Anweisungen enthalten, die definieren, wie das Image aufgebaut wird und wie der Container läuft.

### **Beispiele:**
```dockerfile
FROM ubuntu:latest   # Nutzt Ubuntu als Basis-Image
FROM node:20   # Nutzt Node.js Version 20
```

### **Wichtige Dockerfile-Anweisungen und ihre Bedeutung**

| **Anweisung** | **Beschreibung** |
|--------------|----------------|
| `FROM` | Definiert das Basis-Image für den Container |
| `LABEL` | Fügt Metadaten (z. B. Autor) hinzu |
| `RUN` | Führt Befehle während des Build-Prozesses aus |
| `COPY` oder `ADD` | Kopiert Dateien in das Image |
| `WORKDIR` | Legt das Arbeitsverzeichnis im Container fest |
| `ENV` | Setzt Umgebungsvariablen |
| `CMD` oder `ENTRYPOINT` | Definiert den Standardbefehl für den Container |
| `EXPOSE` | Definiert offene Ports im Container |
| `VOLUME` | Erstellt einen Speicherbereich für persistente Daten |

---

## **1. Das Basis-Image mit `FROM` definieren**
Jedes Dockerfile beginnt mit einer `FROM`-Anweisung, die das Basis-Image bestimmt.

**Beispiele:**
```dockerfile
FROM ubuntu:latest  # Nutzt Ubuntu als Basis-Image
FROM node:20  # Nutzt Node.js Version 20
```

### **Welche Basis-Images sollte man wählen?**
- **`ubuntu:latest`** → Volle Kontrolle über das System, aber groß.
- **`node:20`** → Enthält direkt Node.js 20, ideal für JavaScript-Apps.
- **`alpine`** → Sehr schlankes Linux-Image, benötigt oft zusätzliche Konfiguration.

💡 **Falls du immer die neueste Version willst, nutze:**
```dockerfile
FROM node:lts
```
Das stellt sicher, dass dein Container **automatisch** die aktuelle LTS-Version von Node.js verwendet.

---

## **2. Metadaten mit `LABEL` hinzufügen**
`LABEL` wird genutzt, um Informationen über das Image hinzuzufügen, z. B. den Autor.
```dockerfile
LABEL maintainer="Stevan Menicanin"
```

---

## **3. Befehle ausführen mit `RUN`**
`RUN` führt Shell-Befehle während des Builds aus, um das Image zu konfigurieren.

**Beispiel:**
```dockerfile
RUN apt update && apt install -y curl
```

Für Node.js:
```dockerfile
RUN npm install -g pm2
```

### **Unterschied zwischen `RUN`, `CMD` und `ENTRYPOINT`**
| **Anweisung** | **Wann wird sie ausgeführt?** |
|--------------|----------------------------|
| `RUN` | Während des Image-Builds |
| `CMD` | Beim Container-Start, kann überschrieben werden |
| `ENTRYPOINT` | Standardbefehl, kann nur schwer überschrieben werden |

---

## **4. Dateien in den Container kopieren**
Zum Kopieren von Dateien gibt es `COPY` und `ADD`.

**Beispiel mit `COPY`:**
```dockerfile
COPY app.js /usr/src/app/
```

`ADD` kann zusätzlich Archive entpacken:
```dockerfile
ADD mein-archiv.tar.gz /data/
```

---

## **5. Arbeitsverzeichnis mit `WORKDIR` setzen**
Das `WORKDIR` definiert den Standardordner, in dem Befehle ausgeführt werden.
```dockerfile
WORKDIR /usr/src/app
```

---

## **6. Umgebungsvariablen setzen mit `ENV`**
```dockerfile
ENV NODE_ENV=production
```

Dies kann später im Container mit `process.env.NODE_ENV` genutzt werden.

---

## **7. Standardbefehl mit `CMD` oder `ENTRYPOINT` definieren**
`CMD` setzt den Standardbefehl, der beim Starten des Containers ausgeführt wird:
```dockerfile
CMD ["node", "app.js"]
```

`ENTRYPOINT` wird genutzt, wenn der Befehl nicht überschreibbar sein soll:
```dockerfile
ENTRYPOINT ["python"]
```

---

## **8. Ports freigeben mit `EXPOSE`**
`EXPOSE` informiert darüber, welche Ports der Container nutzt:
```dockerfile
EXPOSE 8080
```

---

## **9. Volumes für persistente Daten nutzen**
```dockerfile
VOLUME /app/data
```
Hier können externe Daten gespeichert werden, die auch nach Neustart erhalten bleiben.

**Wann nutzt man Volumes?**
- Wenn **Daten über Container-Neustarts hinweg gespeichert** werden sollen.
- Für **Datenbanken**, damit Inhalte bei Updates erhalten bleiben.
- Wenn **mehrere Container** auf dieselben Daten zugreifen sollen.

---

## **10. Container mit `docker build` und `docker run` nutzen**
Nachdem das `Dockerfile` erstellt wurde, wird das Image gebaut:
```bash
docker build -t mein-image .
```

Den Container starten:
```bash
docker run -d -p 8080:8080 mein-image
```

---

## **Zusammenfassung**
✔ **`FROM`** definiert das Basis-Image.  
✔ **`RUN`** installiert Pakete während des Builds.  
✔ **`COPY` / `ADD`** fügt Dateien in den Container ein.  
✔ **`WORKDIR`** legt das Arbeitsverzeichnis fest.  
✔ **`CMD` / `ENTRYPOINT`** setzen den Standardbefehl.  
✔ **`EXPOSE`** gibt Ports frei.  
✔ **`VOLUME`** speichert persistente Daten.  
✔ **Nutze Alpine für kleine Container, LTS für langfristige Stabilität.**

---

# **Dockerfile – Beispiel mit Erklärungen**

## **Einleitung**
Ein `Dockerfile` ist eine textbasierte Datei, die Anweisungen enthält, um ein Docker-Image zu erstellen. Dieses Image kann dann genutzt werden, um Container zu starten. In diesem Beispiel erstellen wir ein **Docker-Image für eine einfache Node.js-Webanwendung** und erklären die wichtigsten Anweisungen.

---

## **Beispiel: Dockerfile für eine Node.js-Anwendung**

```dockerfile
# 1. Basis-Image wählen
FROM node:20

# 2. Metadaten hinzufügen
LABEL maintainer="Stev"

# 3. Arbeitsverzeichnis im Container festlegen
WORKDIR /usr/src/app

# 4. Abhängigkeiten in den Container kopieren
COPY package.json package-lock.json ./

# 5. Abhängigkeiten installieren
RUN npm install

# 6. Anwendungscode in den Container kopieren
COPY . .

# 7. Port definieren, auf dem die Anwendung läuft
EXPOSE 3000

# 8. Standardbefehl zum Starten der Anwendung
CMD ["node", "server.js"]
```

---

## **Erklärung der einzelnen Schritte**

### **1. Basis-Image wählen (`FROM`)
```dockerfile
FROM node:20
```
Hier wird `node:20` als Basis-Image verwendet. Es enthält bereits Node.js und npm, sodass keine zusätzliche Installation nötig ist.

### **2. Metadaten hinzufügen (`LABEL`)
```dockerfile
LABEL maintainer="Stevan Menicanin"
```
Diese Zeile fügt Informationen über den Ersteller des Docker-Images hinzu. Dies ist nützlich für Wartung und Dokumentation.

### **3. Arbeitsverzeichnis festlegen (`WORKDIR`)
```dockerfile
WORKDIR /usr/src/app
```
Setzt das Arbeitsverzeichnis im Container auf `/usr/src/app`, sodass alle nachfolgenden Befehle in diesem Verzeichnis ausgeführt werden.

### **4. Nur die benötigten Dateien kopieren (`COPY`)
```dockerfile
COPY package.json package-lock.json ./
```
Kopiert nur `package.json` und `package-lock.json` in den Container. Dadurch wird sichergestellt, dass npm-Installationen nicht erneut ausgeführt werden müssen, wenn sich der Code nicht geändert hat.

### **5. Abhängigkeiten installieren (`RUN`)
```dockerfile
RUN npm install
```
Führt `npm install` aus, um alle Abhängigkeiten zu installieren.

### **6. Anwendungscode in den Container kopieren (`COPY`)
```dockerfile
COPY . .
```
Kopiert den gesamten Quellcode aus dem aktuellen Verzeichnis in den Container.

### **7. Offenen Port definieren (`EXPOSE`)
```dockerfile
EXPOSE 3000
```
Definiert Port 3000 als offenen Port, auf dem die Anwendung läuft.

### **8. Startbefehl für den Container (`CMD`)
```dockerfile
CMD ["node", "server.js"]
```
Legt fest, dass beim Starten des Containers die Datei `server.js` mit `node` ausgeführt wird.

---

## **Den Container erstellen und starten**

1️⃣ **Docker-Image bauen:**
```bash
docker build -t mein-node-app .
```

2️⃣ **Container starten:**
```bash
docker run -d -p 3000:3000 mein-node-app
```

3️⃣ **Container-Logs anzeigen:**
```bash
docker logs <CONTAINER_ID>
```

4️⃣ **Container stoppen:**
```bash
docker stop <CONTAINER_ID>
```

---

## **Zusammenfassung**
✔ **`FROM`** definiert das Basis-Image.  
✔ **`LABEL`** speichert Metadaten.  
✔ **`WORKDIR`** legt das Arbeitsverzeichnis fest.  
✔ **`COPY`** kopiert Dateien in den Container.  
✔ **`RUN`** installiert Abhängigkeiten.  
✔ **`EXPOSE`** definiert offene Ports.  
✔ **`CMD`** legt den Startbefehl fest.  

Mit diesem Wissen kannst du nun eigene `Dockerfiles` schreiben und anpassen! 🚀

