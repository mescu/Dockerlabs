# MÁQUINA TRUST DOCKERLABS

**Web:** https://dockerlabs.es

**Dificultad:** Muy fácil

**Creador:** El Pingüino de Mario

**IP:** 172.17.0.2

## Procedimiento Inicial

Descargamos el laboratorio y lo extraemos en una carpeta, en mi caso se llama trust.

![Trust lab pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/trustlabpic.png)

## Ejecución del lab
Para ejecutar el lab tenemos que poner en la consola 
> sudo bash autodeploy.sh trust.tar

Una vez hecho esto nos dará la ip de la máquina que vamos a atacar. En mi caso es la 172.17.0.2

## Comprobación de ping
Vamos a comprobar si hacemos ping con la máquina.
Para ello ponemos "ping -c1 172.17.0.2"
> -c1: envía solo un paquete a la ip indicada.

![Ping pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/ping.png)

## Escaneo de Puertos
Ya que tenemos conecitividad con la máquina vamos a escanear los puertos abiertos de la máquina, primero tenemos que ser root "sudo su".
Para escanear los puertos abiertos podemos usar el comando 
> nmap -p- --open --min-rate 5000 -sS -vvv 172.17.0.2

Yo lo voy a hacer una tool creada por mi, "m0PortScanner".
Voy a mostrar ambos procesos.

**Con Nmap:**

![Nmap pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/nmap.png)

**Con m0PortScanner:**

![m0PortScanner pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/m0PortScanner.png)

Vemos que estan abiertos los puertos 22 **SSH** y 80 **HTTP**

## Vistazo a la Web
Echamos un vistazo a la web que hay montada con la ip 172.17.0.2 y vemos que es la pagina por defecto de apache.

![Web pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/web.png)

## Enumeración web con Gobuster

Vamos a usar la herramienta **Gobuster** para hacer un reconocimiento de la web y asi poder ver los directorios de la misma.
Para ello vamos a necesitar una **wordlist**, la que usaré yo va a ser 
https://github.com/kkrypt0nn/wordlists/blob/main/wordlists/discovery/directory_list_lowercase_2.3_medium.txt

Para hacer el reconocimiento usaremos el comando 
> gobuster dir -u http://172.17.0.2 -w /home/mescu/Worldlists/directory_list_lowercase_2.3_medium.txt -x .php, .html, .py, .txt

Vemos que cuando acaba el reconocimiento descubre un directorio llamado secret.php,.

![Gobuster pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/gobuster.png)

Si accedemos a ese directorio vemos que aparece un posible usuario llamado "Mario"

![Mario pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/mario.png)

## Fuerza Bruta con Hydra

Vamos a hacer un ataque de fuerza bruta al protocolo ssh usando Hydra y la famosa wordlist "rockyou".
Ponemos el comando
> hydra -l mario -P /home/mescu/wordlists/rockyou.txt ssh://172.17.0.2

![Hydra pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/hydra.png)

Vemos que existe el usuario "mario" y que su contraseña es "chocolate".

## Conexión con SSH

Vamos a intentar conectarnos por ssh con el usr y la pass que hemos obtenido.
> ssh mario@172.17.0.2

![SSH pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/ssh.png)

Y nos conectamos exitosamente.

## Escalada de Privilegios
Vamos a ver si mario tiene permisos para ejecutar algun archivo. 
Para ello usamos el comando
> sudo -l

![sudo -l  pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/sudo-l.png)

Y vemos que tiene permisos de ejecucion como sudo en el binario "vim".
Por lo que vamos a entrar en https://gtfobins.github.io/ y buscamos "vim".

Vemos que podemos escalar privilegios con el comando
> sudo vim -c ':!/bin/sh'

Así que lo introducimos y hacemos un whoami.

![root pic](https://github.com/mescu/Dockerlabs/blob/main/trust/images/root.png)

Y ya somos **root**

-----------

**Mario Escudero Blanco**
