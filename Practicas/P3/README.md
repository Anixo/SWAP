# Practica 3 - SWAP

Vamos a proceder a configurar nuestro balanceador con un software específico. Usaremos **nginx**, **haproxy** y como tarea adicional **pound**. Todos estos balanceadores lo instalaremos en un Ubuntu Server, el cual repartirá la carga a dos servidores web configurados en prácticas anteriores.  

## Configuración de Nginx
Para crear nuestro balanceador con **nginx** lo primero de todo es instalarlo:  
```
$ sudo apt-get install nginx
```
Y lo iniciamos:
```
$ sudo systemctl start nginx
```

Procedemos ahora a configurar el balanceador para que reparta la carga a nuestros servidores web. Abrimos el archivo de configuración siguiente:  
```
$ sudo nano /etc/nginx/conf.d/default.conf
```
Y añadimos la configuración tal y como muestra la imagen:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/1_nginx_conf.png)  

Para que nginx funcione como balanceador y no como servidor, debemos modificar el siguiente archivo:  
```
$ sudo nano /etc/nginx/nginx.conf
```
Y comentar la última línea del *include*:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/2_nginx_balanceador.png)  

Reiniciamos nuestro balanceador para que surja efecto los cambios:  
```
$ sudo systemctl restart nginx
```
Y si hacemos *curl* vemos como el balanceador hace su trabajo de forma correcta. Por defecto, nginx usa el algoritmo de **round-robin** para balancear:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/3_nginx_peticion.png)  

Si queremos que use, por ejemplo, un algoritmo de ponderación, basta con modificar el archivo *default.cfg* añadiendo el atributo *weight* junto con un peso (en la imagen el servidor 10.0.0.10 tiene el doble de prioridad):  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/4_nginx_ponderacion.png)  

Reiniciamos el balanceador, y si hacemos peticiones vemos que nos responde con el algoritmo de ponderación:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/5_gninx_peticion_ponderacion.png)  

## Configuración de haproxy
Si queremos usar el balanceador **haproxy**, ejecutamos el siguiente comando para instalarlo:  
```
$ sudo apt-get install haproxy
```
Para configurarlo, abrimos el archivo:  
```
$ sudo nano /etc/haproxy/haproxy.cfg
```
Y lo editamos con la configuración de la imagen:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/6_haproxy_conf.png)  

Y reiniciamos el servicio:
```
$ sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
```

Le hacemos un par de peticiones y vemos que responde como se espera:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/7_haproxy_peticion.png)  

Si, como en nginx, queremos que use un algoritmo de ponderación, editamos su configuración con el atributo *weight* junto con un valor:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/8_haproxy_ponderacion.png)  

Y tras reiniciar el servicio, vemos que hace bien las peticiones:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/9_haproxy_peticion_ponderacion.png)  

## Tarea opcional: Configuración de Pound
Configurar un servidor con pound es muy simple. Lo instalamos con el comando:  
```
$ sudo apt-get install pound
```
Y editar su archivo de configuración:  
```
$ sudo nano /etc/pound/pound.cfg
```
En el cual escribimos lo siguiente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/10_pound_conf.png)  

Ahora ejecutamos el siguiente comando para que se pueda iniciar:  
```
$ sed -i -e "s/^startup=0/startup=1/" /etc/default/pound
```
Abrimos el siguiente archivo para activar los logs de pound:  
```
$ sudo nano /etc/rsyslog.d/50-default.conf
```
Y editamos las líneas subrayadas de rojo dejándolas tal y como está en la imagen:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/11_pound_log.png)  

Reiniciamos el servicio:
```
$ sudo systemctl restart pound
```
Y vemos que funciona correctamente como balanceador:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/12_pound_peticiones.png)  

## Apache Benchmark
Vamos a poner a una prueba de estrés a nuestros balanceadores para ver cuál de ellos tiene mejor prestaciones. Usaremos el programa Apache Benchmark con 100.000 peticiones de 100 en 100. El parámetro *-g archivo.tsv* nos servirá para guardar estos valores.  

Empezamos con **nginx**. Ejecutamos:  
```
$ ab -g nginx.tsv -n 100000 -c 100 http://10.0.0.30/hola.html
```
Y obtenemos como salida la imagen siguiente, viendo con gran detalle el resultado de la ejecución del comando:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/13_ab_nginx.png)  

Repetimos el mismo paso para **haproxy**:  
```
$ ab -g haproxy.tsv -n 100000 -c 100 http://10.0.0.30/hola.html
```
Y obtenemos:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/14_ab_haproxy.png)  

Por último, hacemos lo mismo con **pound**:  
```
$ ab -g pound.tsv -n 100000 -c 100 http://10.0.0.30/hola.html
```
Obteniendo como resultado lo siguiente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/15_ab_pound.png)  

Si observamos el campo *Time per request* vemos que haproxy es el que mejor resultado tiene, e incluso tiene una mayor tasa de transferencia. Con la siguiente gráfica, se ve más claro que haproxy es el que mejor prestaciones nos ha dado:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P3/img/16_grafica.jpg)
