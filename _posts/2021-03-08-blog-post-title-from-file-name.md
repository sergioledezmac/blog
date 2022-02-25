## Comandos Chmod y Chowm

## Comandos chowm
El comando chown de Linux es una abreviatura para “change owner” (cambiar de propietario)
y se usa para cambiar la propiedad de los archivos, directorios y enlaces.

Ejemplo

ls -l

-rw-r--r-- 1 root root 0 Feb 20 17:35 archivo.txt

```tsql
chown sergio archivo.txt
```

-rw-r--r-- 1 sergio root 0 Feb 20 17:35 archivo.txt


## De manera similar a los archivos, podemos cambiar la propiedad y el grupo de los directorios
Para cambiar solamente el grupo, puedes usar:
```tsql
chown :grupo /Carpeta
```

Para cambiar el propietario y el grupo del archivo, puedes usar:
```tsql
chown sergio:grupo /Carpeta
```

---

## Comandos Chmod
El comando chmod se usa para cambiar los permisos de archivos o directorios.

Para ver los permisos de un archivo o carpeta usamos el comando ll o ls -l

Para cambiar los permisos vamos a usar las letras
u de usuario
g de grupo 
o de otros

Ejemplo vamos hacer un ll para revisar los permisos de una carpeta
drwx------ 2 root root   4096 Feb 14 08:33 test
La d es de directorio ese datos no nos interesa.
rwx: (read,write,excute) Los siguientes tres caracteres representan los permisos para el propietario del archivo: en este caso, el propietario puede leer escribir y ejecutar el archivo.

EJEMPLOS

```tsql
drwx rwx rwx 2 root root   4096 Feb 14 08:33 test
 │    │   │
 │    │   │
 │    │   | _________ Otro
 │    |____________ Grupo
 │ 
 │________________  Usuario
```
drwxrwxrwx 2 root root   4096 Feb 14 08:33 test
vamos a quitar todos los permisos de escritura, lectura y ejecucion de la carpeta test al usuario otro
```tsql
chmod o-rwx  test
```
drwxrwx--- 2 root root   4096 Feb 14 08:33 test

Ahora vamos a dar solo permisos de lectura y ejecucion al grupo y quitamos el permiso de escritura.
```tsql
chmod -R g-w  test
```
El -R es de (recursivo). Esta opción te permite cambiar permisos o propietarios de todos los archivos y subdirectorios dentro de un directorio específica.
drwxr-x--- 2 root root   4096 Feb 14 09:39 test
.
