---
title: Spyware Pegasus
date: 2023-09-08 15:30:00 +0800
categories: [Ciberseguridad, Articulos]
tags: [Articulos]
image:
  path: /assets/img/post/articulo-pegasus/what-is-pegasus-spyware.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Vivimos en una época donde la tecnología está a la orden del día, todo lo que usamos diariamente está conectado a la red, y por lo tanto, es **vulnerable** a ataques cibernéticos por parte de cibercriminales. En cuanto a los datos, se resume que en este 2023, en lo que llevamos de año (09/08), se están reportando un **promedio de 1252 ataques** por semana solamente en España. Por ello la ciberseguridad cada vez desempeña un papel más importante en nuestro día a día.

Es por ello que hoy vamos a hablar de un caso muy mediático y conocido en los últimos años, que ha afectado a todo tipo de personajes públicos, personas poderosas y reconocidas en todo el mundo.

Este caso es el del **Spyware Pegasus**, se trata de un programa que infecta a dispositivos móviles **IOS y Android**, desarrollado por la empresa israeli **NSO Group**

![img](/assets/img/post/articulo-pegasus/Analisis-Pegasus-programa-ciberespionaje-adquirido_1675943173_156545770_667x375.jpg)

## **¿Que es NSO Group?**

**NSO Technologies** es una empresa de tecnología conocida por ser los autores de este Spyware, que permite la vigilancia remota de teléfonos móviles.

![img](/assets/img/post/articulo-pegasus/cf44a2fd-6c83-4b5a-8174-46819c43cb17_alta-libre-aspect-ratio_default_0.jpg)

Los fundadores de NSO son ex-miembros de la *Unidad 8200*, el *Cuerpo de Inteligencia de Israel* responsable de recopilar Inteligencia de Señales. Esta misma empresa por medio del Ministerio de Defensa Israelí es la encargada de **otorgar licencias** para la exportación de Pegasus a **gobiernos extranjeros**, ya que si, este spyware se usa en su mayoría para el espionaje por parte de gobiernos a periodistas, activistas, políticos, empresarios, etc.

## **¿Como se descubre el uso del malware?**

![img](/assets/img/post/articulo-pegasus/277711-1024x433.jpg)

En **2016** el defensor arabe de los derechos humanos *Ahmed Mansoor* había recibido un mensaje de texto, que prometía **secretos sobre la tortura que ocurría en prisiones de los Emiratos Arabes Unidos**, junto con un enlace. Este muy precavido, envio este enlace a **Citizen Lab** *(empresa muy importante en el descubrimiento de este spyware)* para su investigación. Tras una larga investigación se determino que si hubiera seguido el enlace, se habría producido *jailbreak* en su dispositivo para eliminar las restricciones impuestas por el sistema operativo y se habría implementado el **spyware**. Citizen Lab vinculo el software malicioso a la compañia privada de software **NSO**, que como comentamos anteriormente vende licecias de Pegasus a los gobiernos para supuesta investigación sobre **crimen organizado y terrorismo**, pero ya en 2016 se sospechaba que este malware tenía **otros propositos**, lo cual nos lleva a 2018.

En **2018** una activista Saudi *Loujain al-Hathloul* fue detenida por más de 1000 días por ser una de las principales defensoras de los derechos de las mujeres en el país. Su liberación ocurrio en 2021, fecha en la cual **Google** le informó que hackers contratados por las autoridades saudies habían intentado ingresar a su cuenta de Gmail.

Debido a esto, Loujain al-Hathloul pidió ayuda de expertos por miedo de que su iPhone tambien estuviese comprometido, en este punto de la historia entra de nuevo **Citizen Lab**, clave para el descubrimiento a nivel global de este spyware.

### Falla del Spyware

![img](/assets/img/post/articulo-pegasus/citizen-lab.png)

Los expertos de **Citizen Lab** hicieron un duro trabajo **durante meses** para intentar buscar indicidos de la presencia de algún spyware, resulta que finalmente las encontraron. El software había sufrido una falla, este, dejo almacenada en el teléfono de al-Hathloul una **copia del archivo de imagen malicioso** que se uso en el dispositivo para robar los mensajes almacenados.

Esta falla **no tuvo precedentes**, ya que este tipo de malware esta pensado para la recolección de información y su propia **autoeliminación para no dejar rastro**.

Este descubrimiento dio pie a los investigadores a afirmar con certeza que se trataba del software de **NSO Group**

## **¿Como funciona Pegasus?**

![img](/assets/img/post/articulo-pegasus/spyware_1200_900.jpg)

Para entender como funciona, primero deberemos de saber que es un Spyware.

Un **Spyware** es un **software malicoso** que **recopila información** de un dispositivo y **transmite** esa información recopilada a una **entidad externa** sin el consetimiento del propietario del dispositivo.

Pegasus es un malware que aprovecha vulnerabilidades **Zero-day**, las cuales son desconocidas para los usuarios y el fabricante del producto. Después de la investigación de Pegasus se revelaron 3 de vulnerabilidades:

* **CVE-2016-4655**: Fuga de información en el kernel: Una vulnerabilidad de mapeo de la base del kernel que filtra información al atacante y le permite    calcular la ubicación del kernel en la memoria.
* **CVE-2016-4656**: La corrupción de la memoria del núcleo lleva a Jailbreak: Vulnerabilidades a nivel de kernel de iOS de 32 y 64 bits que permiten al atacante liberar en secreto el dispositivo e instalar un software de vigilancia.
* **CVE-2016-4657**: Corrupción de la memoria en Webkit: Una vulnerabilidad en Safari WebKit que permite al atacante poner en peligro el dispositivo cuando el usuario hace clic en un enlace.

Lo primero que hace este programa es infectar el telefono movil aprovechandose de estas vulnerabilidades **Zero-day**. No se conoce la forma exacta en la que opera este malware, pero se piensa que puede ser a través de **enlaces enviados por mensajería instantanea**, aunque tambien se ha comentado que tambien puede ser a través de llamadas de WhatsApp o utlilizando iMessage, además de ataques de phishing y de suplantación de identidad.

Una vez el dispositivo este infectado, permite al atacante un **control total** sobre el, además de acceso a mensajes, correos electrónicos, micrófono, cámara, llamadas y contactos.

## **Casos en España**

![img](/assets/img/post/articulo-pegasus/COVER-PEGASUSESP-BLOG.jpg)

En nuestro propio pais, ha habido varios casos muy mediaticos sobre el uso de este software por parte del **propio gobierno**, pero sin duda el más polémico y conocido ha sido el espionaje mediante Pegasus de **65 abogados, académicos, periodistas y políticos de los entornos independentistas vascos y catalanes**.

Se trata de la **mayor operación de espionaje** contra un solo grupo de víctimas documentada por **Citizen Lab**, los cuales como ya comentamos son los encargados de la investigación y rastreo de las actividades de este software espía.

Según **Citizen Lab**, el espionaje contra independentistas vascos y catalanes fue diseñado con un **gran nivel de personalización** para cada uno de ellos.

En este caso se utilizaron SMS fraudulentos y mensajes desde redes sociales, además de la explotación Zero-day de la app iMessage.

Segun varias fuentes, la Agencia Española de Inteligencia *CNI* adquirio Pegasus por unos **6 millones de euros** para espiar en el extranjero (supuestamente).

## **Pegasus en el presente**

A día de hoy se puede pensar que este software esta muy controlado y que ya no hay casi casos de espionaje utilizando esta herramienta, pero esto no es así justamente ayer día **07/09/2023** Citizen Lab reporta mediante sus [redes sociales](https://twitter.com/citizenlab/status/1699873620070191520?s=20) una vulnerabilidad utilizada por el software Pegasus de **NSO Group**, y se insta a todos los usuarios de apple a actualizar a la *versión 16.6.1*.

Esta vulnerabilidad recientemente descubierta denominada *BLASTPASS Exploit Chain* esta diseñada para correr en versiones de **iOS 16.6** sin **ningún tipo de interacción** por parte de la víctima.

Al parecer el exploit involucra archivos *PassKit* que contienen imágenes maliciosas enviadas desde una cuenta de iMessage del atacante a la víctima.

Citizen Lab ya reportó estos fallos a Apple y se añadieron dos *CVEs* relacionados con este exploit (**CVE-2023-41064** y **CVE-2023-41061**)

Esta noticia es la motivadora de sacar este mini articulo hablando sobre el tema.

Es por eso que debemos de estar al día sobre temas de *ciberseguridad*, enseñar y fomentar **buenas prácticas** sobre ella para que todos nosotros además de nuestros datos estén seguros en la red y podamos hacer uso de ella **sin ningún riesgo**.

*Espero que os haya gustado leer el artículo tanto como a mi hacerlo. ¡Gracias por leer!*
