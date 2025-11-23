# Hello DevOps – Node.js Express projekt

Ez a projekt egy egyszerű Node.js + Express alkalmazást valósít meg, amelyet GitHub Actions segítségével automatikusan buildelünk, tesztelünk és Docker image-ként publikálunk a GitHub Container Registry-be (GHCR).  
A projekt célja a CI folyamat gyakorlása és bemutatása.

---

## Projekt tartalma

A projekt egy egyszerű Express alkalmazást tartalmaz az alábbi végpontokkal:

- `/` – visszaadja: „Üdvözlet DevOps!”
- `/egészség` – egyszerű állapotellenőrző endpoint

A projekt futtatható lokálisan és Docker konténerben is.

---

## CI pipeline – GitHub Actions

A projekt egy automatikus CI pipeline-t tartalmaz, melynek feladata:

1. A kód letöltése  
2. Node.js környezet előkészítése  
3. Függőségek telepítése (`npm ci`)  
4. Tesztek futtatása (ha vannak)  
5. Bejelentkezés a GitHub Container Registry-be  
6. Docker image buildelése  
7. Docker image feltöltése (push) a registrybe

A workflow fájl helye:  
`.github/workflows/nodejs.yml`

---

## Docker image (GHCR)

A CI pipeline futása során létrejön és feltöltődik a Docker image a GitHub Container Registry-be.

### Image neve:
ghcr.io/kriszta360/hello-devops:latest

### Letöltés:
docker pull ghcr.io/kriszta360/hello-devops:latest

### Futtatás:
docker run -p 8080:8080 ghcr.io/kriszta360/hello-devops:latest

Az alkalmazás ezután itt érhető el:  
http://localhost:8080

---

## Dockerfile

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 8080

CMD ["npm", "start"]
```
## Projekt fájlstruktúra
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

## Lokális futtatás
npm install
npm start
Az alkalmazás ezt követően a http://localhost:8080
címen érhető el.
 Opcionális feladat (3.2 – CI + Free Registry)

## Ez a projekt a DevOps beadandó „3.2 – CI pipeline + free registry” opcionális feladatát valósítja meg, amely tartalmazza:
CI pipeline használatát
Docker image buildet
Docker image feltöltést GHCR-be
Publikus image szolgáltatást

Készült: "DevOps" beadandó feladathoz
A projekt célja egy működő CI/CD gyakorlat bemutatása, beleértve a verziókezelést, a pipeline működését, a konténerizálást és a registry használatát.

Kérdések esetén
Ha bárki szeretné futtatni vagy elemezni a projektet, minden szükséges információ megtalálható fentebb.
