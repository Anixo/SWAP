# Ejercicios Tema 3 - SWAP

### Ejercicio 3.1
**Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.**  
En Windows disponemos del comando *route*. Su uso es muy simple: **route -p add <red_destino> mask <mascara> <puerta_de_enlace> METRIC 1**  
En Linux tenemos un comando llamado igual que funciona de forma similar. Su uso es **route add -net <red_destino> netmask <mascara> gw <puerta_de_enlace> dev <interfaz>


## Ejercicio 3.2
**Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes.**  
En Linux tenemos los comandos *ufw* y *iptables*, ambos sirven para configurar el cortafuegos.  
En Windows disponemos del Firewall, donde podemos definir nuestras reglas de entrada o salida.  
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/3.2.png)