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
  - Podemos crear imagenes utilizando un fichero de texto llamdo **Dockerfile**
     Ejemplo        
          ```tsql
          FROM Centos:7
          RUN yum -y install httpd
          CMD ["apachectl","-DFOREGROUND"]
          ```
          
          
 ## Redes
 Las redes docker es toda la configuracion de red que utilizan los contenedores para comunicarsen entre si.
 Hay varios tipos.
 
 **Bridge** Red estandar.
 
 **Hosts** Todas las tarjetar de red y el nombre de nuestro docker host se asignaran al contenedor.
 
 **None** No se asigna ninguna red al contenedor
