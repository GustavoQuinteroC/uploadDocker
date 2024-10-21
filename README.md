# Aplicación Node.js con Docker - Ejemplo de Contenedorización

Este proyecto es un ejemplo sencillo de cómo crear una aplicación Node.js, contenerizarla usando Docker, y luego subir la imagen a Docker Hub. La aplicación simplemente responde con un mensaje cuando se accede a ella a través del navegador.

## Requisitos

Antes de comenzar, asegúrate de cumplir con los siguientes requisitos:

1. **Docker** instalado en tu sistema.
2. **Node.js** y **npm** instalados en tu máquina.
3. Una cuenta en [Docker Hub](https://hub.docker.com/).

### 1. Instalación de Docker

En esta guía vamos a utilizar Docker en un sistema **Debian 12**. Si aún no tienes Docker instalado, sigue los siguientes pasos.

#### 1.1. Actualiza el sistema e instala dependencias:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
