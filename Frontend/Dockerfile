# Utiliser l'image officielle Node 20
FROM node:20
FROM node:20 AS build

WORKDIR /app

# Copier seulement les fichiers nécessaires pour npm install
COPY package*.json ./

# Installer les dépendances
# Monter à une version récente de npm
RUN npm install -g npm@latest
RUN pip install gunicorn


# Revenir sur le registre officiel (il est plus stable pour npm@11)
RUN npm config set registry https://registry.npmjs.org/
RUN npm config set fetch-retries 5
RUN npm config set fetch-retry-mintimeout 20000
RUN npm config set fetch-retry-maxtimeout 120000


# Et maintenant installer
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
