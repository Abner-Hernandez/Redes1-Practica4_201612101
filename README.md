# Definición de las redes


![definicion de las redes](Images/direccionredes.PNG?raw=true "Title")

# Topologia de red
La topologia a Utilizar es de tipo árbol la cual cuenta con la siguiente configuracion.

1. Router C3725
   - Conexión Interfaz f0/0 con Switch1 (8 puertos) en puerto ethernet 0
   - Configuración Sub Interfaz f0/0.10 Dirección de Red 192.168.12.0/26 Dirección IP 192.168.11.254/24
   - Configuración Sub Interfaz f0/1.20 Dirección de Red 192.168.12.64/26 Dirección IP 192.168.21.254/24
2. Ethernet Switch 16 puertos fastethernet
   - Conexión puerto fastethernet 1/0 con Ethernet Switch2 en puerto fastethernet 1/0 Port Chanel 1
   - Conexión puerto fastethernet 1/1 con Ethernet Switch2 en puerto fastethernet 1/1 Port Chanel 1
   - Conexión puerto fastethernet 1/2 con Ethernet Switch3 en puerto fastethernet 1/0 Port Chanel 2
   - Conexión puerto fastethernet 1/3 con Ethernet Switch3 en puerto fastethernet 1/1 Port Chanel 2
3. Ethernet Switch2 16 puertos fastethernet
   - Conexión puerto fastethernet 1/0 con Ethernet Switch en puerto fastethernet 1/0 Port Chanel 1
   - Conexión puerto fastethernet 1/1 con Ethernet Switch en puerto fastethernet 1/1 Port Chanel 1
   - Conexión puerto fastethernet 1/2 con Ethernet Switch3 en puerto fastethernet 1/2 Port Chanel 3
   - Conexión puerto fastethernet 1/3 con Ethernet Switch3 en puerto fastethernet 1/3 Port Chanel 3
4. Ethernet Switch3 16 puertos fastethernet
   - Conexión puerto fastethernet 1/0 con Ethernet Switch en puerto fastethernet 1/2 Port Chanel 1
   - Conexión puerto fastethernet 1/1 con Ethernet Switch en puerto fastethernet 1/3 Port Chanel 1
   - Conexión puerto fastethernet 1/2 con Ethernet Switch2 en puerto fastethernet 1/2 Port Chanel 3
   - Conexión puerto fastethernet 1/3 con Ethernet Switch2 en puerto fastethernet 1/3 Port Chanel 3
5. VPCS
   - PC1 direccion ip 192.168.11.20 máscaras de red 255.255.255.192 máscaras de subred 192.168.11.20
   - PC2 direccion ip 192.168.21.10 máscaras de red 255.255.255.192 máscaras de subed 192.168.21.10
   - PC3 direccion ip 192.168.21.20 máscaras de red 255.255.255.192 máscaras de subred 192.168.21.20
6. Maquina Virtual
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

# Verificando conexion
Para poder verificar la conexion de VPC con la maquina linux en la consola de nuestra VPC executamos 'ping 192.168.12.192'


![verificate Configuration router](Images/pc1topc6.PNG?raw=true "Title")


Para poder verificar la conexion de VPC con otra vpc en la consola de nuestra VPC executamos 'ping 192.168.12.80'


![verificate Configuration router](Images/pc1topc5.PNG?raw=true "Title")


Para poder verificar la conecion de nuestra maquina virtual con alguna de nuestras VPC hacemos ping con alguna de ellas en este caso 'ping 192.168.12.5'


![verificate Configuration router](Images/pc6topc1.PNG?raw=true "Title")


## Captura de paquetes
Para capturar paquetes es necesario realizar un ping de forma extendida mientras capturamos en nuestro caso lo hacemos desde tiny linux. hacia el VPC3, despues en GNS3 hacemos click izquierdo la interfaz que queremos capturar y seleccionamos capturar, se nos abrira un dialogo donde se nos indicara el puerto a capturar, pulsamos ok y se abrira Wireshark mostrandonos los paquetes request que envia nuestra maquina tiny linux y la respuesta de la VPC3, con doble clic podremos expandir la informacion de dichos paquetes y ver la informacion que tienen.


![dialog capture](Images/startcapt.PNG?raw=true "Title")


![dialog capture](Images/capturing.PNG?raw=true "Title")



