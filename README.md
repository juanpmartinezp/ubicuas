Pasos para crear una cadena Blockchain e integrar una Web de administración usando Multichain.
Juan Pablo Martínez Pulido
Requisitos previos: 
•	Descargar la plataforma Multichain desde la página oficial (https://www.multichain.com/download-install/) e instalarlo en cada equipo que haga parte de la cadena de bloques.  Multichain está disponible para Windows, Mac y Linux, compilado por defecto para sistemas operativos de 64bits. En este caso se instalará el nodo administrador en una maquina Windows 10 y los demás nodos de la cadena en otros sistemas operativos).

1.	Creación de la cadena de Bloques
a.	Una vez instalado Multichain, hay que dirigirse a la carpeta donde quedo instalado y ejecutar una interfaz de comandos dentro de la ruta de la carpeta que contiene Multichain
b.	Se escribe el siguiente comando para crear la cadena:

multichain-util create chain1

donde “chain1” corresponde al nombre de la cadena
 
En este caso ya se contaba con una cadena de igual nombre.

c.	Para ver la configuración predeterminada de la cadena creada se va a la dirección proporcionada que se muestra una vez fue creada

C:\Users\Juan Pablo\AppData\Roaming\MultiChain\chain1\params.dat

En ella se puede editar la configuración base de toda la cadena, por ejemplo, el tamaño que tendrán los bloques, los permisos dentro de la cadena, etc. Sin embargo, por ahora los dejamos con su configuración por defecto

d.	Iniciar la cadena de bloques, incluida la minería del bloque génesis, teniendo en cuenta en cuál de las máquinas se inicializa el bloque génesis, que para este caso es la maquina Windows 10. Para inicializar la cadena se usa el siguiente comando:

multichaind chain1 -daemon

 
Ejecutado el comando anterior, muestra la dirección IP y el puerto con el cual podemos conectar otros nodos a la cadena. En este caso el nodo Windows tiene varias tarjetas de red, por lo tanto, nos indica cada una de sus direcciones IP.

2.	Conexión a la cadena de bloques creada desde otra maquina
Previamente se ha instalado una máquina virtual que utiliza sistema operativo Debian.
a.	Se instala Multichain siguiendo los comandos de la página oficial de descarga (https://www.multichain.com/download-community/)

su (enter root password)
cd /tmp
wget https://www.multichain.com/download/multichain-2.0.3.tar.gz
tar -xvzf multichain-2.0.3.tar.gz
cd multichain-2.0.3
mv multichaind multichain-cli multichain-util /usr/local/bin (to make easily accessible on the command line)
exit (to return to your regular user)

b.	Dentro de la carpeta en la que está ubicado Multichain se escribe el siguiente comando:  

multichaind chain1@[dirección IP y puerto de la maquina donde fue creada la red]
 

Se indica que el administrador del blockchain debe activar los permisos necesarios, los cuales pueden ser de conexión, envío o recepción.

c.	Se vuelve al equipo administrador (Windows) y se conceden los permisos para que reconozca al segundo nodo creado (Debian) a través de la dirección dada. se ejecuta el siguiente comando:

Multichain-cli chain1 grant [identificador segundo nodo] connect

d.	Ahora, de vuelta en el segundo nodo (Debian) se ejecuta el comando:
multichaind chain1 –daemon (no hay necesidad de especificar la dirección IP ni del puerto)

 
3.	Se crea un stream en el nodo Windows y en este caso lo llamamos stream1

Multichain-cli chain1 create stream stream1 false
El retorno “false” significa que en el stream solo pueden escribir aquellos con los permisos explicitos
 
En el otro nodo Debian nos suscribimos al stream1
 

Publicamos 13747265616d2064617461 en el stream1 (solo se reciben valores en hexadecimal)
 

Verificamos que el stream sea visible en el otro nodo
 

4.	Integrando una Web a Multichain
MultiChain Web Demo es una interfaz web escrita en PHP para cadenas blockchain MultiChain.
Este software utiliza PHP para proporcionar un front-end web para un nodo Blockchain MultiChain.
Actualmente es compatible con las siguientes características:
•	Visualización del estado general del nodo.
•	Crear direcciones y darles nombres reales (los nombres son visibles para todos los nodos).
•	Cambio de permisos globales para direcciones.
•	Emisión de activos, incluidos los campos personalizados y la carga de un archivo.
•	Actualización de activos, incluida la emisión de más unidades y la actualización de campos y archivos personalizados.
•	Visualización de activos emitidos, incluido el historial completo de campos y archivos.
•	Envío de activos de una dirección a otra.
•	Crear, decodificar y aceptar ofertas para intercambios de activos.
•	Creación de secuencias.
•	Publicación de elementos en secuencias, como JSON o texto o un archivo cargado.
•	Visualización de elementos de transmisión, incluida la lista por clave o editor y la descarga de archivos.
•	Escribir, probar y aprobar filtros inteligentes (tanto filtros de transacción como de transmisión).

Descargar el archivo que contiene todo lo necesario para desplegar la Web Demo (https://github.com/MultiChain/multichain-web-demo)

Se instala el servidor web de preferencia (por ejemplo, XAMPP, WAMP, LAMP), en nuestro caso se usará XAMPP desde la maquina Windows 10.
Ubicamos la ruta C:\xampp\htdocs\multichain-web-demo y copiamos la carpeta con los archivos de la multichain-web-demo.
Al interior de la carpeta multichain-web-demo encontramos el archivo config-example.txt, debemos hacer una copia del mismo y posteriormente le cambiamos el nombre a config.txt
 

Al interior de config.txt debemos incluir los detalles de la cadena que hemos creado, en nuestro caso queda de la siguiente manera:
default.name=chain1        
default.rpchost=localhost            # IP address of MultiChain node
default.rpcport=6718                  # puerto rcp tomado del archivo params.dat
default.rpcuser=multichainrpc          
multichain.conf
default.rpcpassword=8Dicy8mpk27R2qMP5UkGDGQ4Aae9N1uXfRFYCMSGwtm5 # password tomado del archivo multichain.conf
 
Iniciamos el servidor web desde la aplicación XAMPP 

Desde el navegador visitamos localhost/multichain-web-demo y nos aparecerán las cadenas creadas, en este caso, solo tenemos a cadena llamada chain1
 
Al ingresar a chain1 tendremos una interfaz web desde la cual podemos administrar la red blockchain. 
 

