# Benni_Sofiane_SRD3_2022_Docker


L’objectif de ce projet est d’intégrer le déploiement d’une API Rest avec Docker afin de pouvoir l’intégrer dans un système d’intégration continue.

1. Récupérer le projet API REST qui comporte une partie backend (API rest) et une partie
frontend (reactJS) : deux dossiers backend et frontend.

2. Créer un/des fichier(s) DockerFile(s) pour builder notre image pour les serveurs de production.

3. Créer un fichier docker-compose.yml pour démarrer nos containers, nos services.
 
4. Modifier la documentation qui va expliquer le contenu de ces deux fichiers et de l’architecture dans un fichier README.md :

a. DockerFile(s) pour builder notre image
b. Docker-compose pour démarrer le(s) container(s) (Bien décrire l’ensemble des différentes étapes d’exécution des commandes associés aux services)
c. Architecture de l’application


# DockerFile(s) afin de builder notre image pour les serveurs de prod :

Frontend : 

```
FROM node:alpine as builder
WORKDIR /frontend
COPY ./package.json /frontend
RUN npm install
COPY . .
CMD [ "npm", "run", "start" ]

```

Backend :

```
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 8080

CMD ["npm","start"] 

```

# Docker-compose.YML :

```
version: "3.1"
services:
  mongodb:
    image: "mongo"
    ports:
      - "3306:3306"
    volumes:
      - data:/data/db
  backend:
    build: ./backend
    ports:
      - "8080:8080"
    depends_on:
      - mongodb
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    stdin_open: true
    tty: true
    depends_on:
      - backend
volumes:
  data:
```
# Un schéma récapitule l'ensemble de l'architecture du projet :

![image](https://user-images.githubusercontent.com/105608933/168494720-12952384-c3a6-406e-a555-0fb47fc24647.png)

