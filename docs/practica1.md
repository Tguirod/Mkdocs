# Practica5.4
En esta practica vamos a realizar una subida de un nginx a docker hub para posteriormente descargarnos nuestro respositorio de dockerhub y instalar una web estatica.

# Configuracion del Docker hub

Tendremos que crearnos una cuenta en docker hub para poder utilizarla . Deberemos conseguir una clave secreta para usarla despues para publicar nuestros repositorios dockers
Tambien tendremos que agregar nuestra clave secreta a las variables del github.

## DockerFile

Tendremos que usar un archivo dockerfile que es donde estara la configuracion para tener servicio de Nginx con la siguiente aplicación web estática

```
FROM ubuntu:24.04

LABEL AUTHOR="Tomas guijarro Rodriuez "
LABEL DESCRIPTION="Esto es una prueba de instalacion"

ENV WORDPRESS_DB_HOST=mysql


RUN apt update && \
    apt install nginx -y && \
    apt install git -y && \
    rm -rf /var/lib/apt/list/*

RUN git clone https://github.com/josejuansanchez/2048 /app && \
    mv /app/* /var/www/html/

EXPOSE 80

CMD [ "nginx","-g", "daemon off;" ]
```

## Docker Compose

Una vez que tengamos subido nuestro repositorio de docker a dockerhub vamos a instalar en nuestra maquina de aws una web estatica de prueba sacando la imagen de nuestro repositorio para comprobar que funciona correctamente.

```
services:
  web:
    image: tguirod275/2048
    container_name: pruebaipestatica
    ports:
      - 80:80
    restart: always
```

## Comprobaciones

Primero vamos a comprobar que efectivamente funciona la subida del repositorioa dockerhub. Como podemos comprobar se suben correctamente.

![](imagenes/1.png)

Tambien si accedemos a docker hub veremos que tenemos el repositorio de docker subido

![](imagenes/2.png)


![](imagenes/3.png)

Ejecutamos el archivo de docker compose y comprobamos que se ejecuta correctamente.

![](imagenes/4.png)

Una vez ejecutado probamos a acceder  via web de nuestra pagina estatica para comprobar que funciona.

![](imagenes/5.png)

