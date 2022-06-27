## Docker comandos y Uso

## Arquitectura y Componentes

**DockerHost** Es el servidor o equipo donde va a correr Docker.

**Docker CLI:** Cliente que usa comandos para manejar docker, manejar imagenes, volumenes y redes

**Rest API:** API para administrar docker desde peticiones GET o POST por ejemplo **docker ps** 
es **GET /containers/json** ideal para Jenkins

**Docker Daemon:** Es el demonio del servidio de docker

**Que son los contenedores docker?** Un contenedor es un proceso que corre sobre el servidor, ocupa menos recursos que levantar todo un sistema
operativo. Ejemplo, si solo necesito Apache no puedo usar un contenedor apache sin necesidad de virtualizar un sistema operativo.
Un Contenedor ejecuta una imagen. Los contenedores pueden contener (Imagenes, Volumes, y Redes)
Los contenedores se puede editar, pero solo de manera temporal. Para que sean Persistentes se hacen Volumenes

## Volumenes en Docker
Los volumenes se utilizan para poner datos permanentes en un contenedor.

Existen 3 tipos 

 **Host:** Se indica un directorio en el docker host.
                  
**Anonimos:** Docker genera un volumen random. 
                  
**Volumenes Nombrados:**  Volumenes que nosotros creamos pero lo maneja docker.

## Que son las imagenes docker?
Es un paquete que tiene la informacion necesaria para que una aplicacion se pueda ejecutar
  -Una imagen esta compuesta por capas **FROM, RUN, CMD** las imagenes son solo de Lectura y no se pueden modificar!
  - Podemos crear imagenes utilizando un fichero de texto llamado **Dockerfile** (Algo asi como una receta)
     Ejemplo        
          
          FROM Centos:7
          
          RUN yum -y install httpd
          
          CMD ["apachectl","-DFOREGROUND"]
          
El **DockerFile** tiene varias capas

***FROM*** 
Establece la imagen base ```FROM <image>[:<tag>][AS<name>]```
Puede aparecer varias veces dentro del dockerfile para crear
multiples imagenes o etapas de copilacion.
Se puede asignar un nombre a la etapa de copilacion con **AS**
los **TAGS** son opcionales, si no se indica se toma por defecto la ultima
version de la imagen.

***RUN*** Se utiliza para correr comandos ``<command>`` 
Utiliza *Shell* por defecto ``/bin/sh -c`` o ``cmd /S /C`` en Windows
Los comandos deben ser *Desatendidos* ejemplo yum **-y** install

***ENTRYPOINT*** Recibe como parametro lo que ponemos en el CMD
Permite configurar un contenedo para que sea ejecutable.

``ENTRYPOINT["executable", "param1", "param2"]``


***CMD*** Es el comando que se usa para levantar nuestra aplicacion, debe levanta en primer plano
para mantener vivo el contenedor.

``CMD["executable", "param1", "param2"]``

*Solo puede haber un CMD en un dockerfile*


Ejemplo
```tsql
syntax=docker/dockerfile:1
FROM node:12-alpine
RUN apk add --no-cache python2 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
**Construyendo Una Imagen**
```tsql 
linux# vim Dockerfile
```

Vamos a trabajar a partir de un ubunto image
Vamos a actualizar e instalar python 3
Copiamos el script python que queremos que ejecute.
*#Podriamos usar el ENTRYPOINT para poder ejecutar el .py*
```ruby                  
FROM ubuntu:lastest

RUN apt update && apt install -y python3

COPY scriptpython.py /scriptpython.py

#ENTRYPOINT ["python3"]

#CMD ["/scriptpython.py"]

CMD ["python3", "/scriptpython.py"]

```

```ruby 
linux# docker build -t NOMBREIMAGE:lastest .

(*el . hace referencia a la ruta donde se encuentra el dockerfile*)
```

Al ejecutar luego el comando 
```ruby 
linux# docker images
```
Deberiamos ver la imagen creada

Si el dockerfile tiene un nombre diferente ejmeplo dockerfilepython

 ```ruby 
linux# docker build -t NOMBREIMAGE:lastest -f dockerfilepython
```
*IMPORTANTE: El directorio donde esta el dockerfile debe estar solo el dockerfile
de lo contrario carga todo lo que esta en el directorio, para evitar eso podemos 
usar el .dockerignore*


**.DOCKERIGNORE**
Ignora ficheros o archivos que no se utilizaran para la construccion de la imagen
de modo que la construccion de la imagen sera mas rapida!

Dentro del .dockerignore agregamos los archivos a omitir en la contruccion del
docker image. *(Archivos que NO queremos que esten en la imagen)*

***COPY/ADD*** Se utilizan para copiar archivos o directorios al contenedor.

``COPY/ADD[--chown=<user>:<group>]<src>....<dest>``

La diferencia entre *COPY* y *ADD* es que con *ADD* se puede copiar un fichero desde una URL.
Incluso si el src es un archivo comprimido lo descomprime en el destino.
  
  
  
  
  
  
 Para descargar Imagenes docker vamos a la web ***Docker Hub***
 
 *Docker hub testeta las imagenes subidas*
 
 *Las imagenes mas utilizadas suelen estar en alpine un SSOO que ocupa entre 5 y 10 Megas*
 
 Para descargar una imagen usamos el comando    ``docker pull ngix ``
          
 ## Redes
 Las redes docker es toda la configuracion de red que utilizan los contenedores para comunicarsen entre si.
 Hay varios tipos.
 
 **Bridge** Red estandar.
 
 **Hosts** Todas las tarjetar de red y el nombre de nuestro docker host se asignaran al contenedor.
 
 **None** No se asigna ninguna red al contenedor
 
 
 ## COMANDOS MAS USADOS EN DOCKER
 
 Ver contenedores activos
 ```ruby 
docker ps
```

 Ver ultimo contenedor creado
 ```ruby 
docker ps -l
```

 Ver contenedores activos y inactivos
 ```ruby 
docker ps -a
```

Crear un contenedor
 ```ruby 
docker run
```
Tenemos varios argumentos que podemos utilizar
 ```ruby 
--name NOMBRE CONTENEDOR
-d CORRE CONTENEDOR SEGUNDO PLANO
-p EXPONE EL PUERTO DEL CONTENEDOR
-network INDICARLE UNA RED AL CONTENEDOR (POR DEFAUL BRIDGE)
--ip ASIGNAR IP AL CONTENEDOR
--hostname NOMBRE PARA EL CONTENEDOR
--memory LIMITE DE MEMORIA PARA EL CONTENEDOR
--cpuset-cpus LIMITE DE NUMERO DE CPUS QUE PODRA USAR EL CONTENEDOR
```

Para acceder al bash de un contener usamos el siguiente comando
 ```ruby 
docker exec -it NOMBRECONTENEDOR bash
```
Ver el consumo de recursos de un contenedor
 ```ruby 
docker stats NOMBRECONTENEDOR
```

Renombrar un contenedor
 ```ruby 
docker rename NOMBREVIEJO NOMBRENUEVO
```

 Eliminar Contenedor
 *Utilize -f para eliminar contenedor que esta en ejecucion*
 ```ruby 
docker rm CONTEDOR
```

 Ver los Docker logs
 *-f permite verlos en tiempo real*
 *-t añade timestamp al log (hora de cada log)*
 ```ruby 
docker logs -f -t
```

 Docker Commit
 *Crear una imagen apartir de un contenedor*
 *No es recomendable*
 ```ruby
 docker commit IDCONTENEDOR NOMBRE
```

Docker Inspect
   *inspecciona toda la informacion de un contenedor, Redes, Volumenes etc*
 ```ruby
 docker inspect CONTENEDOR
```

