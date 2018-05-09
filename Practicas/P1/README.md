# Practica 1 - SWAP

## Punto de partida
El punto de partida para la realización de esta práctica es tener dos máquinas con Ubuntu Server instalado. Durante el proceso de la instalación, le hemos asignado los siguientes nombres a las máquinas:

![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/1_nombre10.png)  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/2_nombre20.png)

También hemos llamado al usuario de la siguiente forma en cada máquina:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/3_usuario10.png)  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/4_usuario20.png)

Y durante el proceso de la instalación, hemos instalado los siguientes programas:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/5_programas.png)  

El resto de pasos hay que seguir los pasos normales de instalación y las máquinas ya estarían listas.

## Configuración de la red
Procedemos a configurar la red de ambas máquinas. Para ello a ejecutamos el siguiente comando para editar de forma manual la configuración:
~~~
$ nano /etc/network/interfaces
~~~
Y editamos el archivo de una máquina de la siguiente forma:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/6_red10.png)  
Hacemos lo mismo para la segunda máquina:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/7_red20.png)  
Reiniciamos el servicio de red y las tarjetas de red para que surjan efecto los cambios:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/8_reinicio.png)  
Por último, comprobamos que Apache se está ejecutando con el comando de la imagen:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/9_apacherun.png)  

## Comando Curl
Vamos a  comprobar que ambos servidores tienen conectividad, hemos creado un pequeño archivo html en cada uno llamado *hola.html* que contiene un pequeño mensaje.  
Para ver que van, ejecutamos el comando curl en cada servidor hacia el otro servidor para acceder a este archivo:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/10_curl10.png)  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/11_curl20.png)  

## SSH
Por último, vamos a comprobar que podemos acceder de un servidor a otro a través de SSH, y creamos un archivo para ver que funciona correctamente:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/12_ssh10.png)  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/13_ssh10comprobacion.png)  
Hacemos lo mismo desde el otro servidor:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/14_ssh20.png)  
![imagen](https://github.com/Anixo/SWAP/blob/master/Practicas/P1/img/15_ssh20comprobacion.png)  
