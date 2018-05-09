# Practica 4 - SWAP

Vamos a configurar nuestros servidores web para que permitan conexiones seguras con HTTPS. También crearemos un servidor nuevo que hará la función de Firewall. Se usará dos redes, la 10.0.0.0/24 para los servidores web y el balanceador y la red 192.168.6.0/24 donde se encontrarán los clientes. El Firewall estará en ambas redes.  

## Configuración HTTPS
Lo primero de todo es habilitar el módulo SSL con los comandos:  
```
$ sudo a2enmod ssl
$ sudo service apache2 restart
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/1_activar_ssl.png)  

Ahora creamos una carpeta en la carpeta de Apache donde guardaremos los certificados generados con el comando *openssl*, el cual nos pedirá datos:  
```
$ sudo mkdir /etc/apache2/ssl
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/2_certificado.png)  

Una vez generados los certificados, debemos indicarle al servicio de Apache dónde están. Ejecutamos:  
```
$ sudo nano /etc/apache2/sites-available/default-ssl.conf
```
Y le indicamos la ruta de los certificados en las opciones, tal y como muestra la imagen:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/3_ssl_site.png)  

Por último, activamos el sitio *default-ssl*:  
```
$ sudo a2ensite default-ssl
$ sudo service apache2 reload
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/4_reinicio_ssl.png)  

Y como vemos, si accedemos tanto por HTTP o por HTTPS al servidor web, nos responde:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/5_curl_https.png)  

Y con todo esto, el servidor web ya acepta peticiones por HTTPS. Vamos a repetir los mismos pasos descritos antes para configurar el segundo servidor web de la misma forma, pero con la diferencia de que en vez de crear el certificado lo copiamos directamente del servidor web principal:  
```
$ sudo scp david10@10.0.0.10:/etc/apache2/ssl/* /etc/apache2/ssl
```
Y tras seguir todos los pasos, el segundo servidor web ya acepta peticiones por HTTP o HTTPS:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/6_curl_secundaria.png)  

Por último, falta configurar **nginx** para que balance las peticiones HTTPS. Creamos una carpeta nueva en la carpeta de **nginx** y copiamos los certificados de el servidor web principal:  
```
$ sudo mkdir /etc/nginx/ssl
$ sudo scp david10@10.0.0.10:/etc/apache2/ssl/* /etc/nginx/ssl
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/7_copia_nginx.png)  

Y editamos la configuración de nuestro balanceador para que permita este tipo de conexiones, tal y como está en la imagen:  
```
$ sudo nano /etc/nginx/conf.d/default.conf
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/8_activar_ssl_nginx.png)  

Reiniciamos **nginx**:  
```
$ sudo systemctl restart nginx
```
Y el balanceador estará configurado correctamente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/9_nginx_https.png)  

Como curiosidad, si intentamos acceder mediante HTTPS con un navegador, como Firefox, nos saltará una advertencia en el cual tendremos que aceptar el certificado. Si lo vemos, veremos que tiene los datos introducidos cuando lo creamos:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/10_extra_certificado.png)  



## Configuración Firewall con IPTABLES en el servidor web principal.
Vamos a configurar nuestro servidor web para que solo tenga abierto los puertos de HTTP y HTTPS. Usaremos el comando *iptables* y haremos que se ejecute desde un script al iniciarse el servidor, ya que si ponemos los comandos a mano al reiniciarlo o apagarlo esta configuración se pierde.  

Para ello creamos un script, yo lo he creado en la carpeta del usuario root:  
```
$ sudo su
$ mkdir /root/scripts
$ nano /root/scripts/iptables.sh
$ chmod +x /root/scripts/iptables.sh
```
Ahora después veremos cómo lo rellenamos, antes agregamos la ejecución del script al siguiente archivo para que surja efecto cada vez que se inicie la máquina:  
```
$ sudo nano /etc/rc.local
```
Y añadimos la línea:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/11_automatico.png)  

Ahora sí vamos a añadir las directivas de *iptables* en el script. Vamos a hacer que solo estén abiertos los puertos de SSH, HTTP y HTTPS. Rellenamos el archivo como el de la imagen, donde se explica lo que hace cada comando:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/12_script_firewall.png)  

Tras rellenarlo, si reiniciamos el servidor vemos que con el siguiente comando se han aplicado las directivas escritas:  
```
$ sudo iptable -L -n -v
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/13_firewall.png)  


## Apartado opcional: Configuración de una máquina como Firewall
Lo ideal es no tocar las directivas del cortafuegos de los servidores web. Por esto, hemos creado una máquina que hará la función solo de Firewall, permitiendo conexiones solo a los puertos 80 y 443. Partimos de un Ubuntu Server recién instalado. Hacemos los mismos pasos que en el apartado anterior, pero ahora el script tendrá el siguiente contenido:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/14_configuracion_firewall.png)  
Lo que hace este script es primero activa el *ip_forwarding* (ya que es un parámetro que no se guarda al apagar la máquina), luego eliminamos todas las reglas que haya del cortafuegos, denegamos todo menos el reenvío, permitimos conexiones de forma local y activamos el reenvío de puertos hacia la IP del balanceador **nginx**.  

Puede que a la hora de probarlo no funcione, así que debemos comentar línea *ssl on* en **nginx** para que redirecciones de forma correcta:  
```
$ sudo nano /etc/nginx/conf.d/default.conf
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/15_quito_ssl.png)  

Comprobamos que la configuración es correcta:  
```
$ sudo iptable -L -n -v
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/16_maquina_firewall_ok.png)  

Y vemos que el servidor Firewall funciona correctamente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P4/img/17_maquina_firewall_curl.png) 
