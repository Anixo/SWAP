# Ejercicios Tema 6 - SWAP

### Ejercicio 6.1
**Aplicar con iptables una política de denegar todo el tráfico en una de las máquinas de prácticas. Comprobar el funcionamiento.  
Aplicar con iptables una política de permitir todo el tráfico en una de las máquinas de prácticas. Comprobar el funcionamiento.**  
Permitimos todo el tráfico con los comandos (los 4 primeros borran todas las reglas, los 3 siguientes permiten todo y el último muestra la configuración):  
```
$ iptables –F
$ iptables -X
$ iptables -Z
$ iptables -t nat -F
$ iptables −P INPUT ACCEPT
$ iptables −P OUTPUT ACCEPT
$ iptables −P FORWARD ACCEPT
$ iptables -L -n -v
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/6.1.1.png)  
Si hacemos, por ejemplo, un ping funciona:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/6.1.2.png)  
Denegamos todo el tráfico con los comandos (los 4 primeros borran todas las reglas, los 3 siguientes deniegan todo y el último muestra la configuración):  
```
$ iptables –F
$ iptables -X
$ iptables -Z
$ iptables -t nat -F
$ iptables −P INPUT DROP
$ iptables −P OUTPUT DROP
$ iptables −P FORWARD DROP
$ iptables -L -n -v
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/6.1.3.png)  
Si hacemos, por ejemplo, un ping nos lo deniega:  
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/6.1.4.png)  

### Ejercicio 6.2
**Comprobar qué puertos tienen abiertos nuestras máquinas, su estado, y qué programa o demonio lo ocupa.**  
Usamos el comando:  
```
$ sudo netstat -tulpn
```
![imagen](https://github.com/Anixo/SWAP/blob/master/Ejercicios/img/6.2.png)  


### Ejercicio 6.3
**Buscar información acerca de los tipos de ataques más comunes en servidores web (p.ej. secuestros de sesión). Detallar en qué consisten, y cómo se pueden evitar.**  
Algunos de los ataques más comunes son:  
* ***Ataque por inyección:*** Técnica para modificar una cadena de consulta de base de datos mediante la inyección de código en la consulta, para intentar obtener acceso a las tablas de la base de datos.
* ***DDos:*** Consiste en inundar un sitio con solicitudes, por lo que ese sitio llegará a un punto donde se colapse y no de servicio. Por lo general, se dirigen a puertos específicos, rangos de IP o redes completas.
* ***Fuerza bruta:*** Se busca todas las combinaciones posibles de nombre de usuario más contraseña en una página web. Los ataques de fuerza bruta buscan contraseñas débiles para ser descifradas y tener acceso de forma fácil.
* ***Cross Site Scripting***: Conocido como XSS, consiste en inyectar scripts maliciosos en los sitios web. Debido a que estos scripts parecen provenir de sitios web de confianza, el navegador de los usuarios casi siempre ejecuta la secuencia de comandos. El XSS generalmente se utiliza para obtener la cuenta de un usuario.