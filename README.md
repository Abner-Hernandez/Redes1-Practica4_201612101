# Definición de las redes


![definicion de las redes](Images/direccionredes.PNG?raw=true "Title")

# Topologia de red
La topologia a Utilizar es de tipo árbol la cual cuenta con la siguiente configuracion.

1. Router C3725
   - Conexión Interfaz f0/0 con Switch1 (8 puertos) en puerto ethernet 0
   - Configuración Sub Interfaz f0/0.10 Dirección de Red 192.168.12.0/26 Dirección IP 192.168.11.254/24
   - Configuración Sub Interfaz f0/1.20 Dirección de Red 192.168.12.64/26 Dirección IP 192.168.21.254/24
2. EtherSwitch Router 16 puertos fastethernet (ESW1)
   - Conexión puerto fastethernet 1/0 con EtherSwitch Router2 en puerto fastethernet 1/0 Port Chanel 1
   - Conexión puerto fastethernet 1/1 con EtherSwitch Router2 en puerto fastethernet 1/1 Port Chanel 1
   - Conexión puerto fastethernet 1/2 con EtherSwitch Router3 en puerto fastethernet 1/0 Port Chanel 2
   - Conexión puerto fastethernet 1/3 con EtherSwitch Router3 en puerto fastethernet 1/1 Port Chanel 2
3. EtherSwitch Router2 16 puertos fastethernet (ESW2)
   - Conexión puerto fastethernet 1/0 con EtherSwitch Router en puerto fastethernet 1/0 Port Chanel 1
   - Conexión puerto fastethernet 1/1 con EtherSwitch Router en puerto fastethernet 1/1 Port Chanel 1
   - Conexión puerto fastethernet 1/2 con EtherSwitch Router3 en puerto fastethernet 1/2 Port Chanel 3
   - Conexión puerto fastethernet 1/3 con EtherSwitch Router3 en puerto fastethernet 1/3 Port Chanel 3
   - Conexión puerto fastethernet 1/4 con Switch1 en puerto ethernet 0
   - Conexión puerto fastethernet 1/5 con Switch2 en puerto ethernet 1
4. EtherSwitch Router3 16 puertos fastethernet (ESW3)
   - Conexión puerto fastethernet 1/0 con EtherSwitch Router en puerto fastethernet 1/2 Port Chanel 1
   - Conexión puerto fastethernet 1/1 con EtherSwitch Router en puerto fastethernet 1/3 Port Chanel 1
   - Conexión puerto fastethernet 1/2 con EtherSwitch Router2 en puerto fastethernet 1/2 Port Chanel 3
   - Conexión puerto fastethernet 1/3 con EtherSwitch Router2 en puerto fastethernet 1/3 Port Chanel 3
   - Conexión puerto fastethernet 1/4 con Switch1 en puerto ethernet 0
   - Conexión puerto fastethernet 1/5 con Switch2 en puerto ethernet 1
5. Switch1 8 puertos ethernet
   - Conexión puerto ethernet 0 con EtherSwitch Router2 en puerto fastethernet 1/4 
   - Conexión puerto ethernet 1 con EtherSwitch Router2 en puerto fastethernet 1/5 
6. Switch1 8 puertos ethernet
   - Conexión puerto ethernet 0 con EtherSwitch Router3 en puerto fastethernet 1/4 
   - Conexión puerto ethernet 1 con EtherSwitch Router3 en puerto fastethernet 1/5 
7. VPCS
   - PC1 direccion ip 192.168.11.20 máscaras de red 255.255.255.192 máscaras de subred 192.168.11.20
   - PC2 direccion ip 192.168.21.10 máscaras de red 255.255.255.192 máscaras de subed 192.168.21.10
   - PC3 direccion ip 192.168.21.20 máscaras de red 255.255.255.192 máscaras de subred 192.168.21.20
8. Maquina Virtual
   - Tiny_Linux direccion ip 192.168.11.10 mascara subred 255.255.255.0 gateway 192.168.11.254
   

![Network's Topology](Images/topologia.PNG?raw=true "Title")

# Desarrollo Topologia
Herramientas de desarrolo para la topologia
1. Software GNS3 en su version 2.2.12
2. VMware workstation
3. Tiny Linux SO
4. Wireshark

## Configuración Router C3725
El Software GNS3 en su version 2.2.12 por default no trae el router C3725 y es necesario agregar el dispositivo para esto es necesario abrir las preferencias del programa luego seleccionar la seccion de Dynamips IOS Routers y seleccionar new.


![add router](Images/preferences.PNG?raw=true "Title")


Luego seleccionamos new image luego en browse para buscar la imagen en nuestro ordenador.


![search image](Images/selectimage.PNG?raw=true "Title")


Continuamos la instalacion hasta el siguiente punto, luego es necesario pulsar el boton Idle-PC Finder el cual buscara un Idle optimo para nuestro equipo y luego continuamos hasta finalizar la instalacion.


![search idle](Images/Idlepc.PNG?raw=true "Title")


Para la configuracion de las Sub Interfacez, primero es necesario entrar en el modo configuracion del router para este caso necesitamos hacer uso del comando 'configure terminal' luego pasamos a crear la sub interfaz con 'interface fastEthernet 0/0.10' luego le asignamos una direccion ip mas su mascara de red 'ip address 192.168.11.254 255.255.255.0' en este caso son dos sub interfacez, una para cada vlan 'interface fastEthernet 0/0.20', 'ip address 192.168.21.254 255.255.255.0'.


![Configure interface](Images/configuresubinterfacez.PNG?raw=true "Title")


Para verificar la configuracion realizadas en los puertos del router usamos 'show ip interface brief'.


![verificate Configuration router](Images/configuredinterfaces.PNG?raw=true "Title")


Para poder guardar las configuraciones realizadas al router tenemos que salir primero del modo configuracion para esto usamos 'exit' luego guardamos con 'write'.


![verificate Configuration router](Images/saveConfigRouter.PNG?raw=true "Title")


## EtherSwitch Router
El Software GNS3 en su version 2.2.12 por default no trae el EtherSwitch Router y es necesario agregar el dispositivo, la instalacion del EtherSwitch Router es similar a la instalacion del router c3725.


Para la configuracion primero hay que caparlos a modo switch para esto es necesario indicarle al router que no estara en modo ruteo entramos a modo configuracion 'configure terminal', luego indicamos el modo no ruteo 'no ip routing'. tambien aplicamos las configuraciones para todos los puertos, primero indicamos el rango de las interfacez a configurar en este caso 'interfaz range fastethernet 1/0 - 15' luego seteamos las configuraciones en este caso 'speed 100', 'duplex full' luego levantamos las interfacez 'no shutdown', salimos 'exit' y guardamos 'wr', la configuracion aplica para los 3 EtherSwitch Router.


![Configuration etherswitch](Images/confinterfaces.PNG?raw=true "Title")


## Configuracion PortChanel
Para configurar un portchanel es necesario entrar primero en modo configuracion 'configure terminal' luego seleccionamos las interfacez que pertenecerán al portchanel para nuestra topologia tenemos 3 portchanel 'interfaz range fastethernet 1/1 - 1' luego indicamos el portchanel y lo habilitamos 'channel-group 1 mode on' finalizamos con 'end, cabe aclarar que los portchanel se hacen en cada extremo de cada route y con el mismo nombre de portchanel.


![portchanel](Images/confportchanel.PNG?raw=true "Title")


## Configuracion VLAN
Para nuestra red manejaremos 2 vlans una para VENTAS y otra para CONTABILIDAD, primero configuramos las interfacez de los EtherSwitch en modo trunk ya que estos estan conectaodos entre si y aun router, para esto entramos primero en modo configuracion 'configure terminal' luego selecionamos el rango de interfacez a configurar en este caso seleccionamos todos los puertos 'interfaz range fastethernet 1/0 - 15', colocamos los puertos en modo trunk 'switchport mode trunk' salimos 'exit', la configuracion es para los 3 EtherSwitch Router.


![mode trunk](Images/confmodetrunk.PNG?raw=true "Title")


Tenemos que configurar un EtherSwitch Router en la configuracion de vtp en modo servidor para poder guardar las vlans y replicarlas a los clientes, primero entramos a la base de datos de las vlans 'vlan database' luego definimos un dominio 'vtp domain redes1_201612101' y un password 'vtp password redes1_201612101' es necesario definir el modo del vtp a utilizar en este caso usaremos 'vtp v2-mode', definimos al EtherSwitch Roueter como server 'vtp server', salimos 'exit'.


![mode server](Images/confvlaserver.PNG?raw=true "Title")


Creamos las VLANS que definimos que trabajaremos primero ingresamos a la base de datos de las vlans 'vlan database' luego definimos la primera vlan 'vlan 10 name VENTAS'
luego la segunda 'vlan 20 name CONTABILIDAD' salimos 'exit'.


![create vlans](Images/createvlans.PNG?raw=true "Title")


Es necesario que el EtherSwitch Router2 y 3 esten en vtp cliente para poder replicar las vlans primero entramos a la base de datos de las vlans 'vlan database' luego ingresamos el dominio 'vtp domain redes1_201612101' y el password 'vtp password redes1_201612101' es necesario definir el modo del vtp a utilizar en este caso usaremos 'vtp v2-mode', definimos los EtherSwitch Roueter como clientes 'vtp client', salimos 'exit'.


![mode client](Images/confvlaclient.PNG?raw=true "Title")


ahora podemos ver las como los clientes cuentan con las vlans definidas en el server con el comando 'sh vlan-switch'.

![sh vlan](Images/shvlan.PNG?raw=true "Title")


## Configuración Switch 8 puertos
Necesitamos configurar 2 puertos en modo trunk para cada switch en modo trunk los cuales estan conectados a un EtherSwtich para esto primero damos clic derecho sobre el switch luego pulsamos sobre configure.


![conf switch](Images/confswitch.PNG?raw=true "Title")


Definimos el el acceso de los puertos ethernet 0 y un en modo trunk para este caso ingresamos el puerto y el numero de vlan cabe aclarar que usaremos vlan 1 ya que nos permitira el paso para ambas vlans 10 y 20 y agregamos el modo trunk en este caso seria dotIq.


![conf switch trunk](Images/swmodetrunk.PNG?raw=true "Title")


Definimos tambien el modo acceso para nuestros host para esto agregamos el numero de puerto, numero de vlan que sera definida para el puerto y access.


![conf switch access](Images/swmodeaccess.PNG?raw=true "Title")


## Configuración VPC
La configuracion a realizar en las tarjetas ethernet de nuestros vpc se realiza con el siguiente comando colocando 'ip [direccion ip] [mask] [gateway]' un ejemplo de ip utilizada para este caso seria 'ip 192.168.11.10 255.255.255.192 192.168.11.254', para poder ver los cambios podemos usar 'show ip'


![verificate Configuration vpc ip](Images/ip.PNG?raw=true "Title")


![verificate Configuration vpc showip](Images/showip.PNG?raw=true "Title")


Para guardar la configuracion de cada vpc es necesario que a cada configuracion le demos 'save'


## Configuración Maquina Virtual (Tiny Linux)
Para la maquina virtual necesitamos configurar el adaptador de red de la maquina y para esto necesitamos primero agregar una red virtual, para esto es necesario entrar al editor de redes virtuales y agregar una red luego cambiar el gateway al que usaremos en nuestro caso sera 192.168.11.254 y la mascara de red 255.255.255.0.


![verificate Configuration MV](Images/vnet.PNG?raw=true "Title")


Para configurar la red en nuestra maquina virtual es necesario abrir panel de control ir a la seccion de redes, esto nos mostrara un recuadro en el cual podremos configurar los parametros de red que necesitamos en este caso ingresamos la direccion ip y gateway, en este caso utilizar ip 192.168.11.10  gateway 192.168.11.254 mascara 255.255.255.0.


![verificate Configuration router](Images/tiny_linux.PNG?raw=true "Title")

## Captura de paquetes
Para capturar paquetes es necesario realizar un ping de forma extendida mientras capturamos en nuestro caso lo hacemos desde tiny linux. hacia el VPC3, despues en GNS3 hacemos click izquierdo la interfaz que queremos capturar y seleccionamos capturar, se nos abrira un dialogo donde se nos indicara el puerto a capturar, pulsamos ok y se abrira Wireshark mostrandonos los paquetes request que envia nuestra maquina tiny linux y la respuesta de la VPC3, con doble clic podremos expandir la informacion de dichos paquetes y ver la informacion que tienen.


![dialog capture](Images/startcapt.PNG?raw=true "Title")


![dialog capture](Images/capturing.PNG?raw=true "Title")



## Diagrama de identificación de rutas principales
Para poder identificar las rutas principales hacemos uso de spanning tree protocol ya que este nos indica que puertos son bloqueados, para evitar loops ademas de la captura de paquetes.

Primero verificamos en los 3 EtherSwitch que puertos son bloqueados para evitar loops, usamos el comando 'sh spanning-tree brief
   - EtherSwitch Router (ESW1) podemos identificar que este EtherSwitch es el root
   
   
   ![spanning root](Images/spanningtreeroot.PNG?raw=true "Title")
   
   
   - EtherSwitch Router2 (ESW2)
   
   
   ![spanning sw1p1](Images/spanningtreesw1p1.PNG?raw=true "Title")
   ![spanning sw1p2](Images/spanningtreesw1p2.PNG?raw=true "Title")
   
   
   - EtherSwitch Router3 (ESW3)
   
   
   ![spanning sw1p1](Images/spanningtreesw2p1.PNG?raw=true "Title")
   ![spanning sw1p2](Images/spanningtreesw2p2.PNG?raw=true "Title")
   
Para la captura la realizaremos en los dos puertos del EtherSwitch Root (ESW1) esto para corroborar los caminos principales. seleccionamos con clic derecho y pulsamos capturar podremos ver que los dos puertos que van del ESW1 hacia ESW2 y ESW3 son los cuales tienen trafico a la hora de realizar ping, ya que podemos ver el icmp request y reply que realiza los equipos al realizar ping con esto ya podemos crear nuestro diagrama de caminos principlaes y agregar quie es el ESW root, cabe aclarar que es necesario siempre que el ESW root tiene que estar adyacente siempre al router que usemos.

Captura ESW1 a ESW2


![SW1-SW2](Images/camino0.PNG?raw=true "Title")


Captura ESW1 a ESW3


![SW1-SW2](Images/camino1.PNG?raw=true "Title")


Diagrama caminos principales y ESW root


![ruta principal](Images/rutaprincipal.PNG?raw=true "Title")


# Verificando conexion
Para poder verificar la conexion de VPC con la maquina linux en la consola de nuestra VPC executamos 'ping 192.168.11.20' (VLAN 10 a VLAN 10)


![ping vlan 10 10](Images/pingtinyvlan10.PNG?raw=true "Title")


Para poder verificar la conexion de VPC con la maquina linux en la consola de nuestra VPC executamos 'ping 192.168.21.10' (VLAN 10 a VLAN 20)


![ping vlan 10 20](Images/pingtinyvlan20.PNG?raw=true "Title")


Para poder verificar la conexion de VPC1 con otra VPC en la consola de nuestra VPC executamos 'ping 192.168.11.20' y 'ping 192.168.11.20'


![ping vpc 10 20](Images/pingvpc1.PNG?raw=true "Title")


Para poder verificar la conexion de VPC2 con otra VPC en la consola de nuestra VPC executamos 'ping 192.168.11.20' y 'ping 192.168.11.10'


![ping vpc 10 20](Images/pingvpc2.PNG?raw=true "Title")


Para poder verificar la conexion de VPC3 con otra VPC en la consola de nuestra VPC executamos 'ping 192.168.11.10' y 'ping 192.168.11.10'


![ping vpc 10 20](Images/pingvpc3.PNG?raw=true "Title")
