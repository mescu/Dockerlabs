# MÁQUINA UPLOAD DOCKER LAB

**Dificultad:** Muy fácil

**Creador:** El Pingüino de Mario

**IP:** 172.17.0.2

## Procedimiento Inicial
Descargamos el laboratorio y lo extraemos en una carpeta, en mi caso se llama upload.

![Upload lab pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/upload.png)

## Ejecución del lab
Para ejecutar el lab tenemos que poner en la consola 
> sudo bash autodeploy.sh upload.tar

Una vez hecho esto nos dará la ip de la máquina que vamos a atacar. En mi caso es la 172.17.0.2

## Comprobación de ping
Vamos a comprobar si hacemos ping con la máquina. Para ello ponemos 
> ping -c1 172.17.0.2

![Ping pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/ping.png)

## Escaneo de Puertos

Ya que tenemos conecitividad con la máquina vamos a escanear los puertos abiertos de la máquina.
Para ello podemos usar el comando 
> sudo nmap -p- --open --min-rate 5000 -sS -vvv 172.17.0.2

![Nmap pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/nmap.png)

Vemos que esta abierto el puerto 80 **HTTP**, vamos a comprobar si hay una web con la IP.

## Vistazo a la Web
Echamos un vistazo a la web y al entrar nos topamos con un portal para subir archivos.

![Web pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/web.png)

## Subida de Archivos

Vamos a crear un archivo y lo vamos a subir, voy a crear un archivo con extension .php

![Test pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/test.png)

Y lo voy a subir a ver si no me da problemas al subirlo.

![Archivo pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/archivo.png)

No nos ha dado problemas al subirlo.

## Fuzzing
Ahora con **Gobuster** vamos a ver que directorios oculta esta página, en algun sitio tendra que estar almacenado el archivo que hemos subido.
Usamos el comando
> sudo gobuster sudo gobuster dir -u "http://172.17.0.2" -w /home/mescu/Worldlists/directory_list_lowercase_2.3_medium.txt -x .php, .sh, .py, .txt

![Gobuster pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/gobuster.png)

Vemos que hay un directorio llamando **uploads**, vamos a entrar.

![Uploads pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/uploads.png)

Aquí esta el archivo que subimos anteriormente, de alguna manera podremos hacer una reverse shell con php.

## Reverse Shell
En https://www.revshells.com/ podemos crear una reverse shell de lo que queramos en segundos, en nuestro caso pondremos la ip de nuestra máquina, en mi caso **172.17.0.1** y el puerto que queremos usar para escuchar, en mi caso el **443** y seleccionamos PHP PentestMonkey que es de las más usadas.

![Reverse Shell pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/reverse.png)

Ahora copiamos el archivo que nos da el generador, le ponemos un nombre normal en mi caso **system** para que no sospechen y le ponemos la extension .php

![System pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/system.png)

## Ataque
Vamos a subir el archivo creado a la web.

![Subido pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/subido.png)

Ahora vamos a poner el puerto **443** en escucha con el comando
> nc -lvnp 443

![Escucha pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/escucha.png)

Y ahora vamos a abrir el direcotrio **uploads** y abrimos el archivo system.php

![Sys pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/sys.png)

Veremos que no reacciona la web y en nuestro terminal ya tendremos **acceso** a la máquina víctima con el comando
> whoami

![Acceso pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/acceso.png)

## Escalada de Privilegios
Ahora vamos a conseguir escalar nuestros privilegios hasta **root**, para ello vamos a ver que archivos puede ejecutar el usuario **www-data** con sudo, ponemos el comando
> sudo -l

![Env pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/env.png)

Vemos que puede ejecutar con sudo el binario **env**.
Ahora nosotros vamos a ir a https://gtfobins.github.io/ y buscamos **env**.

![Sudo pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/sudo.png)

Veremos que hay un comando con sudo para escalar privilegios, el comando es 
> sudo env /bin/sh
Lo ejecutamos en el terminal y ponemos **whoami** para ver si hemos logrado ser usuario **root**.

![Whoami pic](https://github.com/mescu/Dockerlabs/blob/main/upload/images/whoami.png)

Y efectivamente ya somos **root** y hemos resuelto el lab.

-----------

**Mario Escudero Blanco**
