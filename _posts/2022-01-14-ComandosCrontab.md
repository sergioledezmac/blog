## Que es y como usar Comandos Crontab

Crontab es útil para realizar varias operaciones como manejar copias de seguridad automatizadas, 
rotar archivos de registro, sincronizar archivos entre máquinas remotas y borrar carpetas temporales, etc.

Crontab tiene seis campos y los primeros cinco (1-5) campos definen la fecha y hora de ejecución.
El último campo, es decir, el sexto campo, podría ser un nombre de usuario y / o tarea / trabajo / comando / script que se ejecutará

```tsql
* * * * *  *
│ │ │ │ │
│ │ │ │ │
│ │ │ │     | _________   Día de la semana (0 - 6) (0 es domingo, o use nombres)
│ │ │ |____________ Mes (1 - 12), * significa todos los meses
│ │ |______________  Día del mes (1 - 31), * significa todos los días
│ |________________  Hora (0 - 23), * significa cada hora
|___________________ Minuto (0 - 59), * significa cada minuto

```

## Ejemplos
1. Un Cron para que se ejecute a las 5 a. M. Todos los días
```tsql
0 5 * * * /scripts/job.sh
```


2.Un Cron para que se ejecute dos veces al día a las 6 a. M. Y a las 6 p. M.
```tsql
0 6,18 * * * /scripts/job.sh
```

3. Un Cron para que se ejecute cada minuto
```tsql
* * * * * /scripts/job.sh
```
4. Un Cron para que se ejecute en cada Lunes a las 7 pm.
```tsql
0 19 * * mon /scripts/job.sh
```
5. Un Cron para que se ejecute cada 15 minutos.
```tsql
*/10 * * * * /scripts/job.sh
```
## Donde se ejecutan
El servicio cron (demonio) se ejecuta en segundo plano y comprueba constantemente (cada minuto) el /etc/crontab archivo, y /etc/cron.*/ directorios. 

También comprueba el /var/spool/cron/ directorio


## Algunos Comandos

Enumerar todos los trabajos de Cron sin abrir el archivo de configuración crontab usando el siguiente comando

crontab -l

Si no hay ningún trabajo existente, devolverá la salida como

[geekflare@localhost ~]# crontab -l
no crontab for geekflare


Si el usuario ya ha agregado algunos de los trabajos, se mostrará de la siguiente manera.
```tsql
[geekflare@localhost ~]# crontab -l
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
0 */1 * * * /home/account/scripts/updateAccountStatuses.sh
0 */1 * * * /home/account/scripts/reActivateAccountStatus.sh
[geekflare@localhost ~]#
```


## List Cron para un usuario particular
Para enumerar los trabajos programados de otro usuario, use la opción como -u (Usuario) y -l (Lista).
```tsql
crontab -u another_username -l
```
Ejemplo: crontab -u geekflare -l

## Agregar / modificar entradas de Crontab
Para editar la entrada crontab, podemos usar -e opción como se muestra a continuación.
```tsql
crontab -e
```
El comando anterior abrirá los editores vi donde especificará los detalles del trabajo y guardará el archivo.
Una vez guardado, puede verificar si cron está configurado o no con ```tsqlcrontab -l ``` .
