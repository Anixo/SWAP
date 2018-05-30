# Ejercicios Tema 5 - SWAP

### Ejercicio 5.1
**Buscar información sobre cómo calcular el número de conexiones por segundo.
Para empezar, podéis revisar las siguientes webs: http://bit.ly/1ye4yHz http://bit.ly/1PkZbLJ **  
Una fórmula aproximada es la siguiente (fuente en http://priocept.com/2009/08/21/calculating-http-server-load/):  
Tomaremos algunos datos de ejemplo:  
* 20000 usuarios por hora.
* Cada petición son 30 segundos como mucho.
* Cada petición tiene 3 pasos.
* Hay 2 peticiones HTTP por petición.
Peticiones por segundo, H = 3 x 2 x 20000/(60x60) = 33.333  


### Ejercicio 5.2
**Revisar los análisis de tráfico que se ofrecen en: http://bit.ly/1g0dkKj  
Instalar wireshark y observar cómo fluye el tráfico de red en uno de los servidores web mientras se le hacen peticiones HTTP.**  
Hemos instalado Wireshark y capturado paquetes HTTP:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/5.2.png)  


### Ejercicio 5.3
**Buscar información sobre características, disponibilidad para diversos SO, etc de herramientas para monitorizar las prestaciones de un servidor.  
Para empezar, podemos comenzar utilizando las clásicas de Linux: top, vmstat, netstat.**  
Aparte de los comandos ya menciones, también tenemos *tcpdump*. Si buscamos algún software, he encontrado varios como:  
* (OpenNMS)[https://opennms.org/en]
* (SolarWinds)[https://www.solarwinds.com/free-tools/server-health-monitor]
* (Spiceworks)[https://community.spiceworks.com/products/114-spiceworks]