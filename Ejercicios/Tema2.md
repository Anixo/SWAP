# Ejercicios Tema 2 - SWAP

### Ejercicio 2.1
**Calcular la disponibilidad del sistema si tenemos dos réplicas de cada elemento (en total 3 elementos en cada subsistema).**  
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/2.1.png)  
* Web con 3 = 97.75% + ((1-97.75%)x85%) = 99.6625%
* App con 3 = 99% + ((1-99%)x90%) = 99.9%
* Database con 3 = 99.9999% + ((1-99.999%)x99.9%) = 99.9999999%
* DNS con 3 = 99.96% + ((1-99.96%)x98%) = 99.9992%
* FireWall con 3 = 97.75% +((1-97.75%)x85%) =  99.6625%
* Switch con 3 = 99.99% + ((1-99.99%)x99%) = 99.9999%
* Data Center con 3 = 99.99% + ((1-99.99%)x99.99%) = 99.999999%
* ISP con 3 = 99.75% + ((1-99.75%)x95%) = 99.9875%
Disponibilidad = 99.6625% x 99.9% x 99.9999999% x 99.9992% x 99.6625% x 99.9999% x 99.999999% x 99.9875% = 99.213155%


### Ejercicio 2.2
**Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.  
Como ejemplo, examina PM2 (https://github.com/Unitech/pm2) que sirve para administrar clústeres de NodeJS.**  
He encontrado los siguientes:  
* Junos OS
* ***Akka:*** conjunto de bibliotecas de código abierto para diseñar sistemas flexibles y escalables que abarcan núcleos y redes de procesadores.
* ***Play! Framework:*** Framework para aplicaciones web con Java.
* ***Ultra Monkey:*** Gran disponibilidad para servicios en red. Es un balanceador de carga.


### Ejercicio 2.3
**¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar herramientas y aprender a usarlas...¡o recordar cómo usarlas!**  
Existen multitud de programas que sirve para monitorizar. Por ejemplo, para la la CPU tenemos programas como *Core Temp*, *AIDA64*...  
En Windows tenemos el Administrador de Tareas, el cual se encarga de mostrar toda la carga del sistema.  
También existen comandos para Linux para monitorizar todo esto, como *top*, *vmstat*, *tcpdump*, *netstat*...  


### Ejercicio 2.4
**Buscar ejemplos de balanceadores software y hardware(productos comerciales).  
Buscar productos comerciales para servidores de aplicaciones.  
Buscar productos comerciales para servidores de almacenamiento.**  
Ejemplos de balanceadores software y hardware:  
* [Software de Barracuda](https://www.barracuda.com/products/loadbalancer)
* [Software de Amazon](https://aws.amazon.com/es/elasticloadbalancing/details/)
* [Balanceador hardware de Kemp](https://kemptechnologies.com/es/server-load-balancing-appliances/product-matrix.html)
Ejemplos productos de almacenamiento:
* [Productos HP para almacenamiento](https://www.it-market.com/en/hewlett-packard/hp-storage/hp-storage-accessories?cat=452&next_page=2)