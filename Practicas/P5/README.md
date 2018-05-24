# Practica 5 - SWAP

En esta práctica vamos a hacer la copia de una base de datos de la máquina principal a la secundaria, tanto de forma manual como de forma automática(la máquina secundaria siempre estará actualizada).  

## Crear una base de datos
Lo primero es crear una pequeña base de datos con un tabla que contiene varias tuplas. Accedemos a MYSQL con el comando:  
```
$ mysql -u root -p
```
Y creamos la base de datos con una tabla e insertamos algunas tuplas:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P5/img/1_mysql_insertar.png)  
Como vemos se han insertado correctamente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/2_mysql_datos.png)  


## Copiar los datos de una máquina a otra
Para copiar la base de datos, debemos bloquear las tablas para que no cambien con el siguiente comando en MYSQL:  
```
> flush tables with read lock;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/3_parar_cambios.png)  
Salimos de MYSQL y creamos la copia de la base de datos con:  
```
$ mysqldump <base_de_datos> -u root -p > <nombre_archivo>.sql
```
Y como muestra la imagen con el comando *ls* el archivo se ha creado:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/4_backup.png)  
Volvemos a permitir cambios en MYSQL con el comando:
```
> unlock tables;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/5_activar_cambios.png)  
Y copiamos el archivo **sql** generado en la máquina secundaria:
```
$ scp <usuario>@<ip_maquina_principal>:<ruta_archivo_sql> <destino>
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/6_copia.png)  


## Restaurar los datos en la máquina secundaria
Queda restaurar estos datos en la máquina secundaria. Antes de nada, en la máquina secundaria debemos crear la base de datos, que se llamará igual que en la máquina principal. Ejecutamos en MYSQL:  
```
> create database usuarios;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/7_db.png)  
Y procedemos a volcar los datos del archivo **sql** en esta base de datos creada con el comando:  
```
$ mysql -u root -p usuarios < <ruta_archivo_sql>
```
Como se puede mostrar en la siguiente imagen, el volcado se ha hecho correctamente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/8_volcado.png)  


## Replicación mediante maestro-esclavo
### Configuración del maestro
Vamos a configurar la máquina principal como el maestro. Para ello debemos modificar un archivo de configuración:  
```
$ sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Y modificamos lo siguiente. Debemos comentar la línea:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/9_config_maestro.png)  
Y descomentar:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/10_config_maestro.png)  
Reiniciamos el servicio de MYSQL y listo:  
```
$ sudo /etc/init.d/mysql restart
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/11_reinicio_maestro.png)  

### Configuración del esclavo
Vamos a configurar ahora la máquina secundaria como el esclavo. Para ello debemos modificar el mismo archivo de configuración que en el maestro y editar lo mismo, salvo que ahora debemos dejar el parámetro de la imagen tal y como sale:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/12_config_esclavo.png)  
Y reiniciamos:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/13_reinicio_esclavo.png)  

## Conectar maestro con esclavo
Vamos a crear un usuario en el maestro que servirá para cuando el esclavo se conecte al maestro para actualizarse. Ejecutamos los siguientes comandos dentro de MYSQL:  
```
> create user esclavo identified by 'esclavo';
> grant replication slave on *.* to 'esclavo'@'%' identified by 'esclavo';
> flush privileges;
> flush tables;
```
Y cerramos de forma temporal los cambios:  
```
> flush tables with read lock;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/14_usuario.png)  
Si ejecutamos en el maestro el siguiente comando dentro de MYSQL, obtendremos información que nos será necesaria para asociar al esclavo un maestro:  
```
> show master status;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/15_maestro_estado.png)  
Vamos a la máquina esclava y dentro de MYSQL ejecutamos los comandos siguientes para que empiece a replicar del maestro. Los datos que ponemos son los referentes a los que tiene el maestro:  
```
> CHANGE MASTER TO MASTER_HOST='10.0.0.10', MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=980, MASTER_PORT=3306;
> start slave;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/16_esclavo_maestro.png)  
Y por último, en el maestro desbloqueamos las tablas con:  
```
> unlock tables;
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/17_desbloqueo.png)  

## Comprobación
Tras todo esto, la configuración maestro-esclavo estará lista. Para comprobarlo, si ejecutamos en MYSQL:
```
> show slave status\G;
```
El parámetro que se muestra en la imagen debe ser distinto de *null*, lo que significa que es correcto.  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/18_comrobacion.png)  
Y si ahora insertamos una tupla en el maestro:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/19_escribo_maestro.png)  
Este aparece de forma automática en el esclavo:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P6/img/20_actualiza_esclavo.png)  
