# Ejercicios Tema 4 - SWAP

### Ejercicio 4.1
**Buscar información sobre cuánto costaría en la actualidad un mainframe. Comparar precio y potencia entre esa máquina y una granja web de unas prestaciones similares.**  
Un mainframe muy potente y actual es el IBM Z13, capaz de soportar 8000 servidores virtuales en uno solo. Su precio ronda desde los 64000€ a los 1700000€. Soporta desde 256GB de RAM hasta un máximo de 10TB.  
Su precio comparado con cualquier servidor de una granja web es muy elevado, ya que en una granja web con varios servidores, como (este de HP)[https://www.amazon.es/Hewlett-Packard-Enterprise-ProLiant-E5-2660V4/dp/B01DQT1WDA/ref=sr_1_2?s=computers&ie=UTF8&qid=1527699378&sr=1-2&keywords=server+hp] que ronda los 13000€, sale más barato.   

### Ejercicio 4.2
**Buscar información sobre precio y características de balanceadores hardware específicos. Compara las prestaciones que ofrecen unos y otros.**  
Algunos balanceadores hardware que he encontrado son:  
* *A10 networks, Serie AX, modelo AX 2500*: dispone de 8 puerto GbE.
* *Empresa LoadBalancer, modelo Enterprise 10G *: precio 6395€, dispone de 2x1GbE + 2x10GbE, 16GB de RAM, procesador Quad Core Intel Xeon


### Ejercicio 4.3
**Buscar información sobre los métodos de balanceo que implementan los dispositivos recogidos en el ejercicio 4.2**  
Los balanceadores anteriores implementan los métodos:  
* *A10 networks, Serie AX, modelo AX 2500*: Round Robin, conexiones mínimas, Round Robin ponderado, conexiones mínimas ponderadas.
* *Empresa LoadBalancer, modelo Enterprise 10G *: Round Robin, Round Robin ponderado, última conexión, última conexión ponderada.


### Ejercicio 4.4
**Instala y configura en una máquina virtual el balanceador ZenLoadBalancer.**  
-


### Ejercicio 4.5
**Probar las diferentes maneras de redirección HTTP. ¿Cuál es adecuada y cuál no lo es para hacer balanceo de carga global? ¿Por qué?**  
-


### Ejercicio 4.6
**Buscar información sobre los bloques de IP para los distintos países o continentes.
Implementar en JavaScript o PHP la detección de la zona desde donde se conecta un usuario.**  
La empresa (Iana)[https://www.iana.org/numbers] es la encargada de repatir los diferentes bloques IP en los distintos países del mundo. En el enlace, se puede ver qué organización es la encargada de cada continente.  


### Ejercicio 4.7
**Buscar información sobre métodos y herramientas para implementar GSLB.**  
Un método para el GSLB es usar el producto que ofrece la siguiente empresa en este enlace: https://www.incapsula.com/global-server-load-balancing.html  