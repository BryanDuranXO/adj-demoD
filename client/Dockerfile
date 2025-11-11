#etapa 1 (build): compilación de los recursos
FROM node:20 AS build
#establecer el directorio del trabajo
WORKDIR /app
#package-lock.json, package.json
COPY package*.json
#función para generar el node_modules
RUN npm install

# /client/* -> app
COPY . .

#Traer argumentos (LLEGAN DEL COMPOSE)
ARG API_HOST
ARG API_BASE
ARG API_PORT

#establecer las variables de entorno en base a los argumentos
#(ESTOS VAN AL .ENV)
ENV VITE_API_HOST=$API_HOST
ENV VITE_API_BASE=$API_BASE
ENV VITE_API_PORT=$API_PORT
#generar la carpeta /dist
RUN npm run build

#etapa 2: construcción de la imagen
FROM nginx:alpine
#servir todos los recursos generados en la etapa anterior en la imagen
COPY --from=build /app/dist /usr/share/nginx/html

#exponer el puerto
EXPOSE 8080

#levantar el nginx
#nginx -g daemon off;
CMD ["nginx", "-g", "daemon off;"]