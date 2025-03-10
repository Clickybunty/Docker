# 🚀 Vite + Docker: Setup & Deployment Anleitung

Diese Anleitung zeigt, wie du eine Vite-React-App mit Docker in WSL hostest.

## 📌 Voraussetzungen
- **WSL2 mit Ubuntu 22.04/24.04**
- **Docker installiert** (`docker --version` zum Prüfen)

---

## ✅ **1. Projekt vorbereiten**

### 📂 **1.1. Ins Projektverzeichnis wechseln**
```bash
cd /home/stev/GPTBot/project
```

### 🛠 **1.2. `vite.config.ts` anpassen**
Öffne `vite.config.ts` und stelle sicher, dass der Server auf `0.0.0.0` lauscht:
```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  optimizeDeps: {
    exclude: ['lucide-react'],
  },
  server: {
    host: "0.0.0.0", // Erforderlich für Docker
    port: 5173,
    strictPort: true // Erzwingt die Nutzung von Port 5173
  }
});
```

---

## ✅ **2. Docker-Container bereinigen & vorbereiten**
Falls bereits laufende Container existieren, bereinige das System:

### 🗑 **2.1. Laufende Container anzeigen**
```bash
docker ps
```

### 🛑 **2.2. Alle laufenden Container stoppen und entfernen**
```bash
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
```

### 🗑 **2.3. Ungenutzte Images bereinigen**
```bash
docker image prune -a
```

---

## ✅ **3. Docker-Container erstellen & starten**

### 📜 **3.1. `Dockerfile` erstellen**
Falls noch nicht vorhanden, erstelle eine Datei namens `Dockerfile` mit folgendem Inhalt:
```dockerfile
# Basis-Image mit Node.js 18
FROM node:18

# Arbeitsverzeichnis im Container setzen
WORKDIR /app

# package.json und package-lock.json kopieren & Abhängigkeiten installieren
COPY package.json package-lock.json ./
RUN npm install

# Den gesamten Code in das Container-Verzeichnis kopieren
COPY . .

# Port für Vite-Server freigeben
EXPOSE 5173

# Startbefehl für Vite-Server
CMD ["npm", "run", "dev"]
```

### 🏗 **3.2. Docker-Image bauen**
```bash
docker build -t mein-projekt .
```

### 🚀 **3.3. Docker-Container starten**
```bash
docker run -d -p 5173:5173 mein-projekt
```

---

## ✅ **4. Debugging & Überprüfung**

### 📊 **4.1. Laufende Container überprüfen**
```bash
docker ps
```

### 🔍 **4.2. Logs des Containers anzeigen**
```bash
docker logs $(docker ps -q)
```
Falls Fehler auftreten, prüfe die Logs und passe `vite.config.ts` entsprechend an.

### 🌐 **4.3. App im Browser testen**
Öffne:
```
http://localhost:5173/
```
Falls die Seite nicht lädt, Cache leeren (**F12** → Rechtsklick auf **Neu laden** → **„Cache leeren und vollständig neu laden“**).

### 🔄 **4.4. Container stoppen & löschen (falls nötig)**
```bash
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
docker image prune -a
```

---

## ✅ **5. Nützliche Befehle**

### 📋 **Befehlshistorie anzeigen**
```bash
history
```

### 📂 **Images & Container verwalten**
```bash
docker images       # Listet alle Images
```
```bash
docker ps -a        # Listet alle Container (auch gestoppte)
```

🚀 **Jetzt läuft deine Vite-App erfolgreich in Docker!** 🎉
