---
title: Shocker - HTB Writeup
date: 2023-09-06 16:00:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Windows, Writeup]
image:
  path: /assets/img/post/jerry/catherine-heath-i4W8OINLI_I-unsplash.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Shocker es un máquina de dificultad ```Easy``` en la plataforma **Hack The Box**

Esta máquina me resulta muy interesante ya que es la primera vez que voy a tocar el exploit ```Shellshock```

## **Reconocimiento**

Comenzamos con un escaneo de puertos utilizando ```nmap```:

![img](/assets/img/post/shocker/626f4269-71bd-4a1b-a55e-c2b827453f41.png)

Encontramos los puertos ```80``` y ```2222``` abiertos, vamos a ver que **servicios** se ejecutan en dichos puertos:

![img](/assets/img/post/shocker/7e3b09ac-648b-4229-b80d-2c105154baed.png)

Vemos que en el puerto ```80``` hay un servicio ```Apache``` y en el ```2222``` hay un servicio ```OpenSSH```

Vamos a ver el puerto ```80```, a ver que encontramos:

![img](/assets/img/post/shocker/1f0f0165-25ee-41ae-8baf-7b3b786a9068.png)

No encontramos nada, por lo que vamos a seguir con la **fase de reconocimiento**, a ver si podemos listar algun **directorio o archivo interesante**:

![img](/assets/img/post/shocker/c24da344-a70e-43f5-a53a-404137e1c77a.png)

Encontramos un directorio ```cgi-bin```, el cual suele tener cosas interesantes

Si volvemos a enumerar directorios desde esta ruta, nos encontraremos un archivo ```user.sh```, el cual al acceder a la ruta junto al archivo, nos hará la descarga automaticamente, lo descargaremos para ver que contiene:

![img](/assets/img/post/shocker/eb179a12-d186-4a41-b0f0-acd32adf742e.png)

Vemos que es un archivo que al ejecutarlo, guarda la hora en la que se ha ejecutado

Si buscamos información podemos encontrar que los archivos ```CGI``` suelen ser usados para explotar ```Shellshock```, así que vamos a realizar la **explotación**

## **Explotación**

Podemos ver en este [repositorio](https://github.com/opsxcq/exploit-CVE-2014-6271) como poder explotar ```Shellshock``` desde consola, nosotros lo haremos desde **Burpsuite**, aunque al final el resultado es el mismo

Primero vamos a ver si podemos ejecutar el ```Shellshock```

![img](/assets/img/post/shocker/a287c41c-c780-42bb-b2f0-12015947b21b.png)

Como podemos ver, podemos inyectar comandos en el ```User-Agent```, vamos a conseguir una ```reverse shell```, con el oneline que usamos siempre para ello:

![img](/assets/img/post/shocker/3f8fb66f-9696-4160-8938-e07203953664.png)

Si nos ponemos en escucha con ```nc``` por el puerto indicado, conseguiremos **acceso al sistema** y a la **user flag**

![img](/assets/img/post/shocker/fb2428ff-0267-4e82-a421-4e9e9fdae12b.png)

## **Escalada de Privilegios**

Vamos a hacer un pequeño **reconocimiento**, para ver de que manera podemos escalar privilegios

Ejecutando ```sudo -l``` podemos ver que binarios podem os ejecutar como ```root```:

![img](/assets/img/post/shocker/06d78087-6e38-4455-b2dd-91e65d6dbb54.png)

Como vemos podemos ejecutar como ```root``` el binario ```perl```

Por lo tanto vamos a consultar [GTFOBins](https://gtfobins.github.io/), un recurso web que nos ayudará muchísimo

Siguiendo los pasos de la web ejecutaremos:

```bash
sudo perl -e 'exec "/bin/sh";'
```

![img](/assets/img/post/shocker/3e8fa848-ab6e-4e04-b9d1-cd40fe6b7c32.png)

Y Bingo! Conseguiremos **acceso** a la máquina como el usuario ```root```, y por tanto tambien a la **root flag**

![img](/assets/img/post/shocker/9b6268d5-3382-4ca6-b470-c96c5f9acfc6.png)

Espero que os haya gustado y servido de ayuda, cualquier comentario se agradece. Adios!