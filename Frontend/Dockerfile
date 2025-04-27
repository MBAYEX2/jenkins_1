# Utiliser l'image officielle Node 20
FROM node:20

WORKDIR /app

# Copier seulement les fichiers nécessaires pour npm install
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier tout le reste de ton code
COPY . .


RUN npm run build

# Production Stage
FROM nginx:stable-alpine AS production

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80


# Definz the command to run your app
CMD ["nginx","-g","daemon off;"]
