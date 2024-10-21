# Aplicación Node.js con Docker - Ejemplo de Contenedorización
![image](https://github.com/user-attachments/assets/1932fa49-f714-4578-9fc7-f2105f24f6a2)


Este proyecto es un ejemplo sencillo de cómo crear una aplicación Node.js, contenerizarla usando Docker, y luego subir la imagen a Docker Hub. La aplicación simplemente responde con un mensaje cuando se accede a ella a través del navegador.

## Requisitos

Antes de comenzar, asegúrate de cumplir con los siguientes requisitos:

1. **Docker** instalado en tu sistema.
2. **Node.js** y **npm** instalados en tu máquina.
3. Una cuenta en [Docker Hub](https://hub.docker.com/).

### 1. Instalación de Docker

En esta guía vamos a utilizar Docker en un sistema **Debian 12**. Si aún no tienes Docker instalado, sigue los siguientes pasos.

#### 1.1. Actualiza el sistema e instala dependencias:

# bash
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y



# 1.2. Agrega la clave GPG de Docker:
bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 1.3. Agrega el repositorio de Docker:
bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 1.4. Instala Docker:
bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y

# 1.5. Verifica la instalación de Docker:
bash
docker --version
Captura sugerida: Toma una captura de la salida del comando docker --version, mostrando que Docker está instalado correctamente.


# 2. Creación de la Aplicación Node.js

# 2.1. Instala Node.js y npm
Si aún no lo has hecho, instala Node.js y npm:

# bash
sudo apt install nodejs npm -y

# 2.2. Crea la estructura de carpetas
Crea una carpeta para tu proyecto y entra en ella:

bash
mkdir -p ~/mi-docker-app/src
cd ~/mi-docker-app

# 2.3. Crea el archivo server.js
Dentro de la carpeta src, crea el archivo server.js con el siguiente contenido:

bash
nano src/server.js
Y pega el siguiente código:

javascript

const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send("Welcome to my awesome app!");
});

app.listen(3000, function () {
    console.log("app listening on port 3000");
});
2.4. Crea el archivo package.json
Ahora crea el archivo package.json en la raíz del proyecto:

bash
nano package.json
Y agrega lo siguiente:

json

{
  "name": "my-app",
  "version": "1.0",
  "dependencies": {
    "express": "4.18.2"
  }
}


# 2.5. Instala las dependencias de Node.js
Ejecuta el siguiente comando para instalar las dependencias:

bash
npm install
Captura sugerida: Toma una captura después de la instalación de dependencias mostrando la salida de npm install, donde se vean los módulos instalados.

# 3. Crear el archivo Dockerfile
Vamos a crear un archivo Dockerfile para contenerizar nuestra aplicación.

# 3.1. Crea el archivo Dockerfile
bash
nano Dockerfile
Y agrega el siguiente contenido:

Dockerfile

# Utilizar la imagen oficial de Node.js como base
FROM node:19-alpine

# Crear directorio /app y copiar archivos
WORKDIR /app
COPY package.json /app/
COPY src /app/

# Instalar dependencias
RUN npm install

# Exponer el puerto 3000
EXPOSE 3000

# Comando para ejecutar la aplicación
CMD ["node", "server.js"]
# 4. Construcción de la imagen Docker
Ahora vamos a construir la imagen Docker de nuestra aplicación.

# 4.1. Ejecuta el comando para construir la imagen:
bash
docker build -t node-app:1.0 .
Captura sugerida: Toma una captura del terminal mostrando el progreso de la construcción de la imagen Docker.

# 4.2. Verifica que la imagen ha sido creada:
bash
docker images
Captura sugerida: Toma una captura mostrando el resultado de docker images donde aparezca la imagen node-app:1.0.

# 5. Ejecutar la aplicación en Docker
Ejecuta tu aplicación utilizando Docker con el siguiente comando:

bash
docker run -d -p 3000:3000 node-app:1.0

# 5.1. Verifica que el contenedor está en ejecución:
bash
docker ps
Captura sugerida: Toma una captura de la salida de docker ps mostrando el contenedor en ejecución.

# 5.2. Acceder a la aplicación
Abre un navegador web y visita http://localhost:3000. Deberías ver el mensaje "Welcome to my awesome app!".

Captura sugerida: Toma una captura de pantalla del navegador mostrando la aplicación en ejecución con el mensaje de bienvenida.

# 6. Modificar la aplicación
Vamos a cambiar el mensaje de bienvenida en la aplicación.

# 6.1. Edita el archivo server.js
Abre el archivo server.js y cambia el mensaje de bienvenida:

bash
nano src/server.js
Modifica el contenido para que quede así:

javascript
app.get('/', (req, res) => {
    res.send("Welcome to my awesome app v2!");
});

# 6.2. Reconstruye la imagen:
Detén el contenedor actual:

bash
docker stop <container_id>
Luego, vuelve a construir la imagen con:

bash
docker build -t node-app:1.0 .

# 6.3. Ejecuta la nueva versión:
bash
docker run -d -p 3000:3000 node-app:1.0

# 7. Subir la imagen a Docker Hub
# 7.1. Inicia sesión en Docker Hub:
bash
docker login
Ingresa tu nombre de usuario y contraseña cuando se te pida.

# 7.2. Etiqueta la imagen:
bash
docker tag node-app:1.0 usuariodocker/node-app:1.0

# 7.3. Sube la imagen a Docker Hub:
bash
docker push usuariodocker/node-app:1.0
Captura sugerida: Toma una captura de la salida de docker push mostrando que la imagen ha sido subida exitosamente.

# 8. Verificar la imagen en Docker Hub
Para verificar que tu imagen está en Docker Hub, abre tu cuenta en Docker Hub y busca tu repositorio node-app.

Captura sugerida: Toma una captura de pantalla de tu repositorio en Docker Hub mostrando la imagen node-app:1.0.

# 9. Comandos de Docker utilizados
Aquí está un resumen de los comandos de Docker que utilizamos en este proyecto:

Construir la imagen Docker:

bash
docker build -t node-app:1.0 .
Ver imágenes Docker:

bash
docker images
Ejecutar el contenedor:

bash
docker run -d -p 3000:3000 node-app:1.0
Detener el contenedor:

bash
docker stop <container_id>
Subir la imagen a Docker Hub:

bash
docker push usuariodocker/node-app:1.0
¡Gracias por seguir esta guía! Si tienes algún problema o duda, siéntete libre de abrir un issue.

