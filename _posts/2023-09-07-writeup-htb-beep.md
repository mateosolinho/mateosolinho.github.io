---
title: Beep - HTB Writeup
date: 2023-09-07 14:13:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Linux, Writeup]
image:
  path: /assets/img/post/beep/2d974a53-bec1-4055-8f91-b1a8ddc58da6_alta-libre-aspect-ratio_default_0.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

**Beep** es un máquina de dificultad ```Easy``` en la plataforma **Hack The Box**

La máquina **Beep** tiene un gran número de **servicios en ejecucción**, lo que puede ser bueno o malo, ya puede ser dificil encontrar la manera correcta de ganar acceso a la máquina, pero lo bueno es que hay muchas maneras de realizar la intrusión

En este caso lo haremos de la manera que me parece más sencilla y fácil de entender, pero hay muchas maneras distintas de hacerlo. Vamos allá!

## **Reconocimiento**

Como de costumbre para comenzar con la fase de reconocimiento, empezaremos realizando un escaneo de puertos con la herramienta ```nmap``` , esta nos reportará los puertos abiertos en la dirección que le indiquemos:

![img](/assets/img/post/beep/c36e782c-17fb-4f76-83f4-ceacf9ab3b12.png)

Como comentabamos al principio, nos encontramos con un gran número de **servicios y puertos abiertos**

Vamos a hacer un escaneo más exhaustivo de los **servicios y versiones** que se ejecutan en cada puerto *(vamos a hacer un escaneo de pocos puertos para que no se demore mucho)*

![img](/assets/img/post/beep/1b19c82f-9dfb-4e29-ab8d-1e3533d44858.png)

Podemos ver que hay en ejecución un servicio ```HTTP``` por el puerto ```80```, veamos que encontramos:

![img](/assets/img/post/beep/ea12d889-e6a7-4a6d-8c5c-9e05e99c32f1.png)

Vemos que hay un panel de **inicio de sesión**, podemos buscar credenciales por defecto de este servicio, pero por desgracia estas han sido cambiadas, así que vamos a buscar si podemos acceder de alguna manera

Usando searchsploit vamos a ver si hay alguna forma de explotar algún fallo de ```Elastix 2.2.0```:

![img](/assets/img/post/beep/796b300a-81bd-4aa5-9e45-46939fff049e.png)

Vemos que hay una vulnerabilidad existente para la versión del servicio al que intentamos acceder

![img](/assets/img/post/beep/38acbf8a-ea6b-46ca-b2ec-a8cea437c502.png)

Si abrimos el archivo ```.txt```, podremos observar que el servicio es **vulnerable** a un ```Directory Path Traversal```, **con el que podremos apuntar a cualquier archivo del sistema** 

> En este [enlace](https://portswigger.net/web-security/file-path-traversal) dejo más información sobre esta vulnerabilidad

![img](/assets/img/post/beep/36210f96-940c-43f0-913f-bd7772993135.png)

Vemos que si apuntamos a ```/etc/amportal.conf``` accederemos a un archivo de configuración del servicio ```amportal```

En el podremos encontrar unas credenciales ```admin:jEhdIekWmdjE``` , las cuales podremos utilizar para loguearnos en el servicio ```vtigercrm```

Una vez logueados veremos la siguiente interfaz, haremos click en ```Settings```

![img](/assets/img/post/beep/8ed6b75f-d51e-4fe7-a78e-a86e79502aa1.png)

## **Explotación**

Una vez en ```Settings```, accederemos a ```Company Details```, donde accederemos a un panel donde podremos **cambiar la foto de la empresa**, este será vulnerable al ```LFI``` que hablabamos anteriormente, por lo tanto vamos a crear un **archivo malicioso**

![img](/assets/img/post/beep/d838263a-7cfd-4562-b529-f7197dfaf6c8.png)

![img](/assets/img/post/beep/9514ec5d-2f53-467d-b142-e4010bd4503e.png)

> La opción de subida de archivos no está bien sanitizada, por lo que debemos colocarle al archivo php la extensión ```.jpg``` para que nos permita subirlo

A su vez nos pondremos en escucha por el puerto ```443``` mediante ```nc```:

![img](/assets/img/post/beep/24c56a99-1502-4006-be91-05f81d135fd6.png)

Y ya tendremos acceso a la **user flag**, si lo deseamos podemos hacer un ```tratamiento de la tty``` para un mejor control de la consola

## **Escalada de Privilegios**

Como siempre, lo primero que haremos será ```sudo -l``` , para ver si hay algún ```binario``` que podamos ejecutar como ```root```:

![img](/assets/img/post/beep/9505d4a3-3e5b-4c9c-853d-7b29e65572c6.png)

Como vemos,** hay muchas opciones con las cuales probar**, pero nosotros lo intentaremos con la opción de ```nmap```. Lo que haremos para conseguir una ```consola``` como ```root``` aprovechandonos de poder ejecutar el ```binario``` de nmap como un usuario privilegiado es lo siguiente:

```bash 
sudo nmap --interactive
```

```bash 
!bash
```

> Al realizar el segundo paso no sucederá nada, pero ya deberiamos tener acceso como root

![img](/assets/img/post/beep/2fa43191-ce70-4ebc-b99f-d85cbdc9be1d.png)

Como podemos ver ya tendremos acceso como **usuario privilegiado**

![img](/assets/img/post/beep/75b8622e-7194-4b12-80f3-05f3b620d7d8.png)

Y finalmente ya podremos ver la **root flag**

Espero que os haya gustado y servido de ayuda, cualquier comentario se agradece. Adios!