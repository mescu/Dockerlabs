# TRUST DOCKER LAB
Descargamos el laboratorio y lo extraemos en una carpeta, en mi caso se llama trust.

![Trust lab pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/trustlabpic.png)

## Ejecución del lab
Para ejecutar el lab tenemos que poner en la consola "sudo bash autodeploy.sh trust.tar".
Una vez hecho esto nos dara la ip de la maquina que vamos a atacar. En mi caso es la 172.17.0.2

nmap -p- --open --min-rate 5000 -sS -vvv 172.17.0.2 -oG allPorts

## Comprobación de ping
Vamos a comprobar si hacemos ping con la máquina.
Para ello ponemos "ping -c1 172.17.0.2"
> -c1: envia solo un paquete a la ip indicada

## Escaneo de puertos
Ya que tenemos conecitividad con la maquina vamos a escanear los puertos abiertos de la maquina, primero tenemos que ser root "sudo su".
Para escanear los puertos abiertos podemos usar el comando "nmap -p- --open --min-rate 5000 -sS -vvv 172.17.0.2" aunque yo lo voy a hacer una tool creada por mi, "m0PortScanner", la podeis clonar desde mi GitHub con "git clone https://github.com/mescu/m0PortScanner".
![Nmap pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/nmap.png)
