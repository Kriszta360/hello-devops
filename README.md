
# Hello DevOps – Node.js Express projekt

Ez a projekt egy egyszerű Node.js + Express alkalmazást valósít meg, amelyet GitHub Actions segítségével automatikusan buildelünk, tesztelünk és Docker image-ként publikálunk a GitHub Container Registry-be (GHCR).  
A projekt célja a DevOps alaplépések bemutatása: kódkészítés, verziókövetés, build, konténerizálás és CI pipeline használata.

---

## Projekt tartalma

Az alkalmazás egy egyszerű Express szerver, amely két végpontot biztosít:

- `/` – visszaadja: **„Üdvözlet DevOps!”**
- `/egészség` – egyszerű állapotellenőrző („health check”) endpoint

Az app futtatható:
- lokálisan  
- Docker konténerben  
- GitHub Actions által buildelt image-ből (GHCR)

---

## Build

Ez egy egyszerű Node.js alkalmazás, nincs külön fordítási lépés.  
A „build” a függőségek telepítését jelenti:

```bash
npm ci
```

---

## Lokális futtatás

```bash
npm start
```

Az alkalmazás ezután a böngészőben elérhető a következő címen:  
**http://localhost:8080**

---

## Dockerizálás – helyi image build

Helyi Docker image készítése:

```bash
docker build -t hello-devops:latest .
```

Konténer futtatása helyi image-ből:

```bash
docker run -p 8080:8080 hello-devops:latest
```

A futó konténerben az alkalmazás itt érhető el:  
**http://localhost:8080**

---

# CI pipeline – GitHub Actions  
*(Kötelezően választandó feladatrész: 3.2 – CI pipeline + free registry)*

A projekt tartalmaz egy automatikus CI pipeline-t, amely az alábbi lépéseket végzi:

1. Kód letöltése  
2. Node.js környezet beállítása  
3. Függőségek telepítése (`npm ci`)  
4. Tesztek futtatása (ha vannak)  
5. Bejelentkezés a GitHub Container Registry-be  
6. Docker image build  
7. Docker image push a registry-be

A pipeline fájl helye:

```
.github/workflows/nodejs.yml
```

---

# Docker image (GHCR)

A CI pipeline futásakor automatikusan létrejön és feltöltésre kerül a Docker image a GitHub Container Registry-be.

### Image neve:
```
ghcr.io/kriszta360/hello-devops:latest
```

### Letöltés:
```bash
docker pull ghcr.io/kriszta360/hello-devops:latest
```

### Futtatás:
```bash
docker run -p 8080:8080 ghcr.io/kriszta360/hello-devops:latest
```

Az alkalmazás ezután elérhető:  
**http://localhost:8080**

---

# Dockerfile

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 8080

CMD ["npm", "start"]
```

---

# Projekt fájlstruktúra

```
hello-devops/
│
├── alkalmazás.js
├── package.json
├── package-lock.json
├── Dockerfile
├── README.md
└── .github/
    └── workflows/
        └── nodejs.yml
```

---

# Git – trunk-based development

A projekt trunk-based alapon készült:

- **main** branch → trunk  
- több egymásra épülő commit  
- legalább egy **feature branch** (például `feature-uj-udvozles`)  
- commit üzenetek érthetőek és funkcióhoz kötődnek

---

# Követelmények teljesítése (A tanár úr számára rövid összefoglaló.)

| Feladat | Teljesítve |
|--------|:----------:|
| „Hello world” app HTTP endpointtal | ✔ |
| Build folyamat dokumentálva | ✔ |
| Git repository + main branch + feature branch | ✔ |
| Több commit, értelmes commit üzenetek | ✔ |
| Dockerfile, helyi image build + run | ✔ |
| CI pipeline működik | ✔ |
| Registry-be pusholt image (GHCR) | ✔ |
| README teljes és részletes | ✔ |

---

# Készült: DevOps beadandó feladathoz  
A projekt célja a teljes DevOps folyamat bemutatása:  
**kódkészítés → verziókezelés → build → konténerizálás → CI pipeline → registry.**

---

## Kérdések esetén  
A projekt minden szükséges információt tartalmaz a futtatáshoz, elemzéshez vagy ellenőrzéshez.
