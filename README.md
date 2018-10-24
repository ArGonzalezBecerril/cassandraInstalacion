
[![N|Solid](https://image.ibb.co/c1rLGp/2_Instalar_Cassandra.png)](https://nodesource.com/products/nsolid)



A continuación se muestran los pasos para descargar e instalar apache cassandra en modo local sobre debian 7, para ello son necesarias las siguientes herramientas:

- Java JDK8
- Apache cassandra(ultima versión para este caso es 3.11.3)
- Sistema operativo de 64 bit basado en linux (**Para este caso se usa debian**)

##### 1- Ir a la siguiente URL y descargar la ultima versión de cassandra
.
> http://cassandra.apache.org/ ( Url de apache cassandra)

#### 2- Instalación de Java con apt-get debian

**Agregar el repositorio de jdk**
```sh
slackware@arturo$ sudo add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"
# Actualizar la lista de repo
slackware@arturo$ sudo apt-get update
```
**Instalar**
```sh
slackware@arturo$ sudo apt-get install oracle-java8-installer
--->Accept install :Y/N
```
**Comprobar que se instalo correctamente**
```sh
slackware@arturo$ javac -version
javac 1.8.0_181
```
#### 3 - Descarga e instalación de apache cassandra
Existen diferentes formas de instalar cassandra en debian, tal es el caso de instalarlo con **apt-get** o bien puede descargar el **tarball** con los binarios de cassandra e instalarlos a partir de este paquete

**Descargar el tarball con wget**
```sh
slackware@arturo$ cd /home/arturo/Descargas
slackware@arturo$ wget http://www-eu.apache.org/dist/cassandra/3.11.3/apache-cassandra-3.11.3-bin.tar.gz
0/30% ===========>                                                      100%
```
**Crear un directorio en /opt/ llamado cassandra y asignarle permisos de lectura y escritura a nuestro usuario, en mi caso el user es 'arturo'**
```sh
slackware@arturo$ cd /
slackware@arturo$ mkdir /opt/cassandra
slackware@arturo$ sudo chown -R arturo:arturo /opt/cassandra
slackware@arturo$ -ls -rlht
-rw-r--r-- 1 arturo arturo 0 oct  2 01:34 AM...
```
**Descomprimir el tar de cassandra y mover todos los ficheros a /opt/cassandra/**
```sh
slackware@arturo$ cd  /opt/cassandra/
# El punto al  final indica que todo se depositara en el directorio actual
slackware@arturo$ tar -xvf /home/arturo/Descargas/apache-cassandra-3.11.3-bin.tar.gz .
```
**Primer vistazo al fichero de configuración**
[![N|Solid](https://image.ibb.co/caQT6z/3_Apache_cassandrayaml.png)](https://nodesource.com/products/nsolid)

**Que contiene la instalación de cassandra?**
[![N|Solid](https://image.ibb.co/i9JzrL/4-Descripcion-Ficheros.png)](https://nodesource.com/products/nsolid)

**Paso 1.1 (Buscar y des-comentar la siguiente linea)**
[![N|Solid](https://image.ibb.co/iMrn0f/5-Buscar-Descomentarlinea.png)](https://nodesource.com/products/nsolid)

>La cadena a buscar en el fichero /opt/cassandra/conf/cassandra.yaml es: **data_file_directories**
##### Quedando asi 
```sh
112 data_file_directories
113     - /var/lib/cassandra/data
114 ...
```
**¿Cual es la definición de este parámetro?**
Es un directorio de metadato que necesita casandra para llevar un registro del estado y configuración del cluster.
Se necesitan 2 directorios uno para los meta datos y otro para los logs
>Por defecto si no se configura este parametro la configuración estara en:  **$CASSANDRA_HOME/data/data** recordando que $CASSANDRA_HOME = /opt/cassandra
##### Paso 1.2 - Crear los directorios y asignar permisos a mi usuario para poder leer y escribir en esta carpeta
```sh
slackware@arturo$ sudo mkdir /var/lib/cassandra && sudo chown -R arturo:arturo /var/lib/cassandra
slackware@arturo$ sudo mkdir /var/log/cassandra && sudo chown -R arturo:arturo /var/log/cassandra
```

#### Iniciar apache cassandra en modo local
```sh
slackware@arturo$ bash /opt/cassandra/bin/cassandra
INFO  [main] 2018-10-22 23:25:32,660 StorageService.java:1446 - JOINING: Finish joining ring
INFO  [main] 2018-10-22 23:25:33,546 StorageService.java:2289 - Node localhost/127.0.0.1 state jump to NORMAL
```

>**NodeTools**: 
>Es una utilidad de linea de comandos el cual nos va ayudar a consultar y gestionar el cluster en cassandra

##### Paso 1.3 - Revisando el estatus de los procesos que estan en ejecución 
```sh
# Usamos nodetools  para consultar el estatus
slackware@arturo$ bash /opt/cassandra/bin/nodetool status
|/ State=Normal/Leaving/Joining/Moving
--  Address    Load       Tokens       Owns (effective)  Host ID                               Rack
UN  127.0.0.1  160.83 KiB  256          100.0%            9ccfbf60-8548-4b2e-8f33-1ff99850a53f  rack1
```
Podemos acceder a información mas detallada del cluster, para ello lanzamos  el siguiente comando
```sh
slackware@arturo$ bash /opt/cassandra/bin/nodetool info
```
**Obtenemos algo como lo siguiente:**
[![N|Solid](https://image.ibb.co/c7F95q/7-info-cassandra.png)](https://nodesource.com/products/nsolid)

License
----
GNU
