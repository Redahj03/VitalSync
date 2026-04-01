# Multi-stage build: le 1er stage sert à installer les dépendances et exécuter les tests (surface d'attaque réduite),
# le 2e stage ne contient que le strict nécessaire pour exécuter l'API (image finale plus légère).

FROM node:20-alpine AS test
WORKDIR /app

COPY package.json ./
RUN npm install

COPY . .
RUN npm test

FROM node:20-alpine AS prod
ENV NODE_ENV=production
WORKDIR /app

COPY package.json ./
RUN npm install --omit=dev

COPY server.js ./server.js

EXPOSE 3000
CMD ["node", "server.js"]

