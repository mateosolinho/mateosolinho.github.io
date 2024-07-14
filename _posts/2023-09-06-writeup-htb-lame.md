---
title: Lame - HTB Writeup
date: 2023-09-06 15:00:00 +0800
categories: [Ciberseguridad, Writeups, HTB]
tags: [Writeup, HTB, Penetration Testing, Ethical Hacking, SMB, RMetasploit, FTP]
image:
  path: /assets/img/post/lame/luca-bravo-XJXWbfSo2f0-unsplash.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

**Lame** es la primera máquina publicada en la plataforma de **Hack The Box**, su nivel de dificultad es `Easy` y cuenta con gran participación entre la gente que acaba de comenzar en **HTB**

En esta máquina explotaremos una vulnerabilidad en el servicio `smb` (Server Message Block) por el puerto `445`

> SMB es un protocolo para compartir archivos, impresoras y puertos seriales entre nodos de una red

## Reconocimiento

Utilizaremos la herramienta `nmap`, para enumerar los **puertos abiertos** en la máquina victima.

`bash
nmap -p- -sS --min-rate 5000 -vvv -n -Pn 10.10.10.3 -oG allPorts
`

![Desktop View](/assets/img/post/lame/nmap.png)

Los puertos abiertos que nos reporta nmap son los siguientes: `21, 22, 139, 445, 3632`

Continuaremos con un análisis los **servicios** y **versiones** que se ejecutan en los puertos abiertos:

`bash
nmap -sCV -p21,22,139,445,3632 10.10.10.3 -oN targeted
`

![img](/assets/img/post/lame/f8e0e6a7-5272-4f67-8003-fd370c3fc23c.png)

## Explotación

Como podemos observar en la imagen anterior, `nmap` detecta que podemos realizar una conexión por `ftp` como usuario `anonymous` , vamos a ver que sucede:

`bash
ftp 10.10.10.3
`

> Podemos realizar la conexión sin indicandole como nombre `anonymous` y sin contraseña

![img](/assets/img/post/lame/fa13380a-a002-4c6f-9700-d53ef320d03c.png)

Y efectivamente, podemos hacer el **login** con el usuario `anonymous` dejando vacío el campo de la contraseña, pero, al realizar `dir` o `ls` para listar directorios y archivos, podremos observar que **no hay contenido**, por lo que obviamente, está vacío.

De nuevo, observando la última imagen del escaneo utilizando `nmap`, podemos ver, que tambien esta máquina esta ejecutando un servcio `smb` por el puerto `445` . Gracias a la **herramienta** `searchsploit` vamos a ver si existe alguna **vulnerabilidad** para la versión `3.0.20` de `samba`.

`bash
searchsploit samba 3.0.20
`

![img](/assets/img/post/lame/83366d04-9d5b-4182-8510-143a338decd1.png)

Como podemos ver, existe un **exploit** para `metasploit`. Podremos analizar dicho exploit para entender como funciona, y realizar el proceso nosotros mismos.

Utilizando la herramienta `smbmap` podremos listar los **recursos compartidos** por el protocolo `smb`

`bash
smbmap -H 10.10.10.3
`

![img](/assets/img/post/lame/d6bdceb4-245b-4ce6-9473-e46e5cad2e9a.png)

Vemos que hay un recurso compartido `tmp` en el que tenemos **permisos** de **lectura y escritura**

Utilizaremos la herramienta `smbclient` con la cual trataremos de conectarnos al servicio para poder ver el recurso compartido `tmp`

`bash
smbclient //10.10.10.3/tmp -N --options 'client min protocol = NT1'
`

![img](/assets/img/post/lame/07ed24e9-4362-4b59-825a-4edbbfcc4885.png)

Podemos ver que la conexión ha sido *exitosa*.

Ahora podremos usar la herramienta `logon` para hacer un login con un usuario y una contraseña que le indiquemos.

Observando el **exploit** mencionado anteriormente se puede observar que podemos hacer el login dando un usuario `/=`nohup + payload` , por lo tanto, trataremos de conseguir una consola en la máquina victima utilizando `nc`

`bash
nc -nlvp 443
`

![img](/assets/img/post/lame/43db9817-b863-4050-9abf-45af8a4d2bd6.png)

> Con la intrusión ya tendremos acceso como usuario privilegiado, por lo que no es necesario realizar una Escalada de Pivilegios

Y ya lo tendríamos, hemos conseguido **ganar acceso a la máquina víctima** como `root`

Por último realizaremos un *tratamiento de la tty* para una mayor comodidad a la hora de utilizar la consola.

Y finalmente ya tendremos acceso como usuario privilegiado en la máquina **Lame**, y ya podremos ver tanto la **user flag**, como la de **root flag**.

*Espero que os haya gustado y servido, cualquier comentario es de mucha ayuda. Adios!*
