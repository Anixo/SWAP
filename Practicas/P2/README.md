# Practica 2 - SWAP

## Acceso mediante SSH sin contraseña
Antes de nada, vamos a configurar las máquinas para que al acceder mediante SSH no no pida escribir cada vez la contraseña. Así es más rápido la conexión y nos permite automatizar las tareas que requieran esta conexión.  
Para ello escribimos en la terminal el siguiente comando para generar las claves pública-privada y seguir los pasos que se indican:  
~~~
$ ssh-keygen -b 4096 -t rsa
~~~
Y ahora copiamos la clave de una máquina a la otra con el siguiente comando:  
~~~
$ ssh-copy-id david10@10.0.0.10
~~~
Y listo, ya podemos acceder de una máquina a otra sin que nos pida la contraseña:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/1_ssh.png)  
E incluso se puede mandar comando directamente desde un comando, como por ejemplo:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/2_ssh_comando.png)  

## Comando TAR con SSH
Si queremos enviar archivos entre distintas máquinas, un comando que podemos usar es tar junto con SSH. Existen mejores herramientas como rsync, que veremos más adelante.  
Para enviar archivos entre máquinas se debe ejecutar:  
~~~
$ tar czf - <directorio_a_copiar> | ssh <usuario>@<equipo_destino> 'cat > ~/tar.tgz'
~~~
Como se muestra a continuación el archivo se envía...  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/3_tar_envio.png)  
...y vemos que se ha copiado todo correctamente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/4_tar_recibo.png)  

## Comando RSYNC
Rsync es una herramienta útil para copiar grandes cantidades de archivos. Se puede instalar ejecutando el comando:
~~~
$ sudo apt-get install rsync
~~~
Vamos a copiar el contenido web de la máquina principal en la secundaria. Para ello antes cambiamos los permisos de la carpeta:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/5_rsync_permisos.png)  
Y ahora tan solo ejecutamos el comando de rsync:  
~~~
$ rsync -avz --delete --exclude=**/stats --exclude=**/error --exclude=**/files/pictures -e ssh david10@10.0.0.10:/var/www/ /var/www/
~~~
Con *--exclude* excluimos carpetas no deseadas y con *--delete* se hace que el clonado sea idéntico.  
En la siguiente imagen vemos su ejecución:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/6_rsync.png)  

## Ejecución de tareas programadas con CRONTAB
Cron ejecuta tareas de forma automática indicándole en que momento queremos que se ejecuten estas tareas.  
En el archivo */etc/crontab* vamos a añadir una regla para hacer que el comando escrito anterior de rsync se ejecute cada hora. Abrimos el archivo con:  
~~~
$ sudo nano /etc/crontab
~~~
Y tan solo debemos poner la nueva regla tal y como aparece en la imagen:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P2/img/7_cron.png)  
