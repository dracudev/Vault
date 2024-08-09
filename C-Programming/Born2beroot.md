## Configuracion inicial
- ```su```: iniciar root
- ```sudo apt```
	- ```update```: actualizar todos los packages
	- ```install nombrepackage```: instalar packages
- ```sudo adduser nombreuser```: anadir usuario
- ```sudo addgroup nombregrupo```: crear nuevo grupo
	- ```getent group nombregrupo```: ver si existe el grupo
	- ```cat /etc/group```: ver todos los grupos y usuarios dentro
	- ```sudo adduser nombreuser nombregroup```: anadir usuario al grupo
- ```sudo service ssh```
	- ```status```: ver si ssh esta activo
	- ```restart```: reiniciar ssh si haces cambios
- ```sudo ufw```
	- ```enable```: activar firewall
	- ```allow 4242```: permitir conexiones en el puerto
	- ```status```: mirar estado cortafuegos
- ```sudo chage``` 
	- ```-l nombreuser```: ver politica de contrasenas de ese usuario
	- ```-m <time> <nombreuser>```: cambiar el minimo de dias entre cambio de contrasena
	- ```-M <time> <nombreuser>```: cambiar el maximo de dias entre cambio de contrasena
- ```ip a```: ver ip
	- inet 10.11.250.163/16
	- VM -> network -> attached to -> bridge adapter
	-  ssh antandre@10.11.250.163 -p 4242

## Script 
- **Arquitectura**
	- ```uname -a```: ver arquitectura SO
- **Nucleos**
	- ```grep "physical id" /proc/cpuinfo | wc -l```: contar el numero de nucleos fisicos
	- ```grep processor /proc/cpuinfo | wc -l```: contar el numero de nucleos virtuales
- **Memoria RAM**
	- ```free --mega | awk '$1 == "Mem:" {print $3}'```: muestra la informacion de la memoria en megabytes y printa por pantalla la tercera palabra despues de la primera linea que empiece con "Mem:", que en este caso es la memoria usada
	- ```free --mega | awk '$1 == "Mem:" {print $2}'```: lo mismo pero printando la memoria total
	- ```free --mega | awk '$1 == "Mem:" {printf("(%.2f%%)\n", $3/$2*100)}'```: printa un float number hasta 2 numeros de precision convertido a porcentaje (multiplica la columna de memoria usada por 100)
- **Memoria disco**
	- ```df -m | grep "/dev/" | grep -v "/boot" | awk '{memory_use += $3} END {print memory_use}'``` :  "disk filesystem" muestra resumen completo del espacio en disco en megas (-m). Mostramos solo las lineas que contengn dev y quitamoos las que tengan boot. Sumamos el valor de la tercera palabra de cada linea para printar el total de memoria available
	- ```df -m | grep "/dev/" | grep -v "/boot" | awk '{memory_result += $2} END {printf ("%.0fGb\n"), memory_result/1024}'```: printar el espacio total en disco en Gb (dividiendo el resultado en Mb entre 1024)
	-  ```df -m | grep "/dev/" | grep -v "/boot" | awk '{use += $3} {total += $2} END {printf("(%d%%)\n"), use/total*100}' ```: printamos el porcentaje de memoria usada dividiendo la memoria en uso entre la total y multiplicando por 100
- **Uso CPU**
	- ```vmstat 1 4 | tail -1 | awk '{print $15}'```: vmstat muestra estadisticas del sistema . Ponemos un intervalo de segundos de 1 a 4, printamos solo la ultima linea con tail y solo la columna 15 co awk que es el uso de memoria disponible (!!!Mas tarde restaremos este valor a 100 y printaremos el resultado en decimal %)
- **Ultimo reinicio**
	- ```who -b | awk '$1 == "system" {print $3 " " $4}'```: mostramos ultimo arranque del sistemaa y printamos por pantalla la tercera y cuarta palabra (con un espacio en medio) de la primera linea que empiece por "system", es decir fecha y hora
- **Uso LVM**
	-  ```if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi```: lsblk muestra informacion de los dispositivos de bloque (discos duros, ssd, memorias). Contamos el numero de lineas que incluyen "lvm" si hay 0 printamos "no" si hay mas de 0 printamos "yes"
- **Conexiones TCP**
	- ```ss -ta | grep ESTAB | wc -l```: muestra el numero de lineas con conexiones TCP establecidas
- **Numero de usuarios**
	- ```users | wc -w```: cuenta el numero de palabras que salen con el comando users
- **Direccion IP y MAC**: 
	- ```hostname -I```: muestra direccion IP
	- ```ip link | grep "link/ether" | awk '{print $2}'```: muestra las interfaces de red y printamos solo la direccion mac
- **Numero de comandos ejecutados con sudo**
	- ```journalctl _COMM=sudo | grep COMMAND | wc -l```: muestra el registro de acciones en el sistema. Filtramos las entradas que incluyan el comando sudo y filtramos solo comandos. Printamos el numero de lineas

## MariaDB
- CREATE DATABASE nombrebase;
- SHOW DATABASE;
- CREATE USER 'antandre'@'localhost' IDENTIFIED BY '12345';
- GRANT ALL PRIVILEGES ON wp_database.* TO 'gemartin'@'localhost';
- FLUSH PRIVILEGES;