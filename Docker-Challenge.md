echo "# Docker Node.js Challenge

## Schritt 1: Projektverzeichnis erstellen
```sh
mkdir node-docker-app
cd node-docker-app
```

## Schritt 2: Node.js-Projekt initialisieren
```sh
npm init -y
```

## Schritt 3: Express.js installieren
```sh
npm install express
```

## Schritt 4: Server-Datei erstellen
```sh
echo 'const express = require("express");
const app = express();
const PORT = 3000;

app.use(express.json());

let books = [
  { id: 1, title: "Docker für Einsteiger", author: "Jacob Menge", year: 2023 },
  { id: 2, title: "Napfgeflüster: Memoiren einer Feinschmecker-Katze", author: "Jacobs Katze", year: 2022 },
];

app.get("/books", (req, res) => res.json(books));
app.get("/books/:id", (req, res) => {
  const book = books.find(b => b.id === parseInt(req.params.id));
  if (!book) return res.status(404).json({ message: "Buch nicht gefunden" });
  res.json(book);
});

app.post("/books", (req, res) => {
  const { title, author, year } = req.body;
  if (!title || !author || !year) {
    return res.status(400).json({ message: "Alle Felder (title, author, year) sind erforderlich" });
  }
  const newBook = { id: books.length + 1, title, author, year };
  books.push(newBook);
  res.status(201).json(newBook);
});

app.listen(PORT, () => console.log(`Server läuft auf Port ${PORT}`));' > server.js
```

## Schritt 5: Dockerfile erstellen
```sh
echo 'FROM node:18
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]' > Dockerfile
```

## Schritt 6: `.dockerignore` Datei erstellen
```sh
echo 'node_modules
npm-debug.log
.DS_Store' > .dockerignore
```

## Schritt 7: Docker-Container bauen
```sh
docker build -t node-docker-app .
```

## Schritt 8: Container starten
```sh
docker run -p 3000:3000 node-docker-app
```

## Schritt 9: API testen
### Abrufen aller Bücher:
```sh
curl -X GET http://localhost:3000/books
```

### Abrufen eines spezifischen Buches (z. B. mit ID 1):
```sh
curl -X GET http://localhost:3000/books/1
```

### Ein neues Buch hinzufügen:
```sh
curl -X POST http://localhost:3000/books -H "Content-Type: application/json" -d '{"title": "Neues Buch", "author": "Max Mustermann", "year": 2024}'
```

## Schritt 10: Docker-Compose nutzen (optional)
```sh
echo 'version: "3"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    command: npm start' > docker-compose.yml
```

Container mit Docker Compose starten:
```sh
docker-compose up
```

## Fazit
Mit diesen Schritten hast du eine einfache Node.js-API gebaut, die in einem Docker-Container läuft. 🚀" > README.md
