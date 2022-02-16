# Docker

Es una imagen, un framework hecho por capas.
Cada capa es inmutable 

Las capas estan montadas una encima de otra
Las capas son solo de lectura.

El contenerder tiene una capa fina de Lectura  escritura. 
Las Capas de imagen, lo que se ha dicho, son inmutables y una encima de otra.


Cada contenedor esta encima del kernel, ve el kernel.

# Generar imgane
    from ubuntu:latest
    run apt-get update -y
    run apt-get install -y python-pip python-dev
    workdir /app
    wnv debug=true
    expose8-
    volume /data
    copy ./app
    run pip install -r requierments.txt
    entrypoint ["python"]
    cmd ["app.py"]


### Descargar contenedo
 [Ubuntu_16.04]: docker pull ubuntu:16.04
                docker search ubuntu:xxxxx

### Ejecutar un contenedor con un comando
 docker container run ubuntu:16.04 ls / 
 #### Comprobar los procesos que se han ejecutado 
 docker container -ls -a
### Ejecutar un contenedor en segundo plano
 docker container run --detach ubuntu:16.04 top -b
 #### Comprobar los procesos que se estan ejecutando
 docker container ls 
### Parar un contenedor
 docker container stop [id unico | container id | nombre]
### Ver logs del contenedor
 docker container logs [follow] [id unico | container id | nombre]

## TRANSPORTAR UN CONTENEDOR/IMAGEN | docker image save --help
docker image save --output [nombre_fichero] [nombre_imagen]
docker image load --input [nombre_fichero]

## Construir docker.
Necesitaremos construir dockerfile que contiene
 - Comandos necesarios para crear nuestra imagen para
   > Crera la arquitectura
   > incorporar dependencias
   > instalar programas que necesitemos
   > agregar codigo fuente
   > realizar config necesaria para el sistema

 dockerfile + docker image build = image
### Una imagen
 > Tiene una capa origen (layer form) 
 > y por encima otras capas
De forma que, cada linea de comando en el dockerfile es una nueva capa por encima

### Dockerfile
 - Creamos una carpeta para la imagen
 - En esa carpeta creamos un fichero de texto
 - En el fichero de texto comenzamos a escrbir
   > Primera linea (siempre): FROM [imagen_base]
   Que nose va a indicar qué imagen vamos a utilizar de base para esta imagen, por ejemplo
    FROM ubuntu:16.04
   > ENTRYPOINT : permite configurar el contenedor par que se pueda iniciar como ejecutable. Podemos poner un comando primero nada mas ejecutar:
   ENTRYPOINT echo "bienvenido al curso docker" 

### Crear la imagen
docker image build --tag [nombre_imagen] .

## Volumenes 
docker container run --name ejemplo-contenedor -v /home/ubuntu/ejemplo/datos/:/ejemplo -it ubuntu:16.04 /bin/bash
-v para identificar que estamos montando un volumen de datos y especificamos la ruta y donde se monta.

### Creamos un contenedor de almacenamiento de datos
docker container run --name datos -v ./datos busybox

### Creamos otro contenedor como aplicacion que va a utilizar al contenedor "datos" como volumen de datos:
docker container run --name app1 --volumes-from datos -it ubuntu:16.04 /bin/bash



## Escuchar puertos (sin salida :( )
([para escuchar] apt-get update && apt-get install -y netcat ; \
                    nc -lp 8080 -v )

[mapear_puertos] : -p 5432:5432 (si es postgres)
