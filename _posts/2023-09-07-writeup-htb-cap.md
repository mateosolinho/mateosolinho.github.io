---
title: Cap - HTB Writeup
date: 2023-09-07 01:55:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Linux, Writeup]
image:
  path: /assets/img/post/cap/garrett-parker-aYHzEnSEH-w-unsplash.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

**Cap** es una máquina ```Easy``` en la plataforma **Hack The Box**

Esta máquina corre un servidor ```HTTP```, el cúal nos permitirá capturar el tráfico **no cifrado** y aprovecharnos de un ```IDOR``` *(referencia de objeto directo inseguro)*, gracias a esto conseguiremos las **credenciales** de un usuario y ganaermos acceso a la máquina.

## **Reconocimiento**

Para comenzar utilizaremos la herramienta ```nmap``` para ver que puertos están abiertos en la máquina víctima:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.95 -oG allPorts
```

![img](/assets/img/post/cap/27601d24-4bb1-4fe0-a372-65128d4ec88c.png)

Podemos oberservar que hay tres puertos abiertos ```21 - 22 - 80```

Vamos a realizar un escaneo más exhaustivo para ver las **versiones** y **servicios** que corren por dichos puertos:

```bash
nmap -sCV -p21,22,80 10.10.10.245 -oN targeted
```

![img](/assets/img/post/cap/6988a938-89c7-4f6b-a87f-a32295ebb5f9.png)

Vemos que en el puerto ```80``` hay un servicio ```http```, vamos a buscar la web a ver que encontramos:

![img](/assets/img/post/cap/02f91c54-e996-4b0b-a5a8-6bb4ce741d5f.png)

Podemos ver que hay un usuario ```nathan```, lo apuntamos ya que nos podrá hacer falta en un futuro

Si accedemos a ```/capture``` nos redigirá tras 5 segundos a ```/data/1``` donde prodemos ver información sobre ese reporte, realizando un poco de descubrimiento a mano, cambiando el id de la data, nos daremos cuenta de que en ```/data/0``` hay un reporte con información

![img](/assets/img/post/cap/23bf5c97-71c9-4722-86dd-bae6ae365ebe.png)

Procederemos a darle a ```Download``` y nos descarará un archivo ```.pcap```

### Archivos .pcap

> Los archivos ```.pcap``` son archivos relacionados con ```Wireshark```, estos son archivos de datos creados mediante el programa y que contiene el paquete de datos de una red. Estos archivos se utilizan principalmente en el **análisis de las características de la red de una fecha determinada**.

Gracias a esta información vamos a usar ```whireshark``` para ver si podemos obtener información sobre el archivo que acabamos de descargar:

![img](/assets/img/post/cap/9f059673-2d40-4575-a6dd-32e7407ea6a9.png)

Tras unos minutos de busqueda, podemos ver que hay un login bajo el usuario ```nathan``` y con una credencial ```B...............!```

## Explotación

Con estas credenciales podremos conectarnos por ```ftp``` a la máquina y podremos ver la **user flag**.

![img](/assets/img/post/cap/49772912-9c2d-4419-be46-54ffcceb37cb.png)

## Escalada de Privilegios

Para la realización de la escalada nos conectaremos con las mismas credenciales por ```SSH```

Una vez conectados seguiremos el procedimiento en busqueda de todo tipo de posibles escaldas sobre el usuario nathan

Vamos a ver si podemos escalar privilegios aprovechandonos de permisos ```SUID```, aunque no encontraremos nada positivo:

```bash
find / -perm -4000 2>/dev/null
```

![img](/assets/img/post/cap/70554d01-5ff4-4872-afca-ea373cf6c3b0.png)

Por lo tanto buscaremos binarios con ```capabilities``` que nos pueden servir:

```bash
getcap -r / 2>/dev/null
```

![img](/assets/img/post/cap/02a4ee85-e5ca-4f6f-8571-3470fdffaf70.png)

Y bingo! Como vemos en el binario ```pyhton3.8``` hay una capabilitie asignada de ```setuid```, con la que podremos escalar privilegos, para ello simplemente ejecutaremos un ```oneline```, para conseguir una consola como ```root```

```bash
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash");'
```

![img](/assets/img/post/cap/ff094ce5-1a72-4ea6-982c-05e55a5118b8.png)

Y despues de esto, ya tendriamos acceso privilegiado al sistema, pudiendo ver en ```/root``` la **root flag**

Espero que os haya gustado y servido de ayuda, cualquier comentario se agradece. Adios!