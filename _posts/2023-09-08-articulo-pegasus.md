---
title: Spyware Pegasus
date: 2023-09-08 15:30:00 +0800
categories: [Ciberseguridad, Articulos]
tags: [Spyware, Pegasus, NSO Group, Ciberseguridad, Zero-day, CVE-2016-4655, CVE-2016-4656, CVE-2016-4657, CVE-2023-41064, CVE-2023-41061, Citizen Lab]
image:
  path: /assets/img/post/articulo-pegasus/1.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Vivimos en una época en la que la **tecnología** está omnipresente. Todo lo que usamos diariamente está conectado a la red y, por lo tanto, es vulnerable a **ataques cibernéticos** por parte de criminales.

En lo que respecta a los **datos**, se estima que en 2023, hasta la fecha (08-09-2023), se han reportado un promedio de **1252 ataques por semana** solo en España.

Por ello, está claro que la **ciberseguridad** desempeña un papel cada vez más importante en nuestra vida diaria.

Es por esto que hoy vamos a hablar de un caso muy mediático y conocido en los últimos años, que ha afectado a todo tipo de personajes públicos, personas poderosas y reconocidas en todo el mundo.

Este caso es el del **Spyware Pegasus**, un programa que infecta dispositivos móviles **iOS** y **Android**, desarrollado por la empresa israelí **NSO Group**.

![img](/assets/img/post/articulo-pegasus/7.jpg)

## **¿Que es NSO Group?**

**NSO Technologies** es una empresa de tecnología conocida por ser los autores de este spyware, que permite la **vigilancia remota** de teléfonos móviles.

![img](/assets/img/post/articulo-pegasus/2.jpg)

Los fundadores de **NSO** son exmiembros de la **Unidad 8200**, el Cuerpo de Inteligencia de Israel responsable de recopilar inteligencia de señales.

Esta misma empresa, a través del **Ministerio de Defensa israelí**, es la encargada de otorgar licencias para la **exportación de Pegasus** a gobiernos extranjeros.

Este spyware se usa principalmente para el **espionaje** por parte de gobiernos a **periodistas**, **activistas**, **políticos**, **empresarios**, etc.

## **¿Como fue descubierto el uso del malware?**

![img](/assets/img/post/articulo-pegasus/3.jpg)

En **2016**, el defensor árabe de los derechos humanos **Ahmed Mansoor** recibió un mensaje de texto que prometía revelar secretos sobre la tortura en las prisiones de los Emiratos Árabes Unidos, junto con un enlace. Muy precavido, envió este enlace a **Citizen Lab** (una organización muy importante en el descubrimiento de este spyware) para su investigación.

Tras una larga investigación, se determinó que, si hubiera seguido el enlace, se habría producido un **jailbreak** en su dispositivo para eliminar las restricciones impuestas por el sistema operativo y se habría implementado el spyware.

**Citizen Lab** vinculó el software malicioso a la compañía privada de software **NSO**, que, como mencionamos anteriormente, vende licencias de **Pegasus** a los gobiernos para la supuesta investigación sobre crimen organizado y terrorismo. Sin embargo, ya en 2016 se sospechaba que este malware tenía otros propósitos, lo cual nos lleva a **2018**.

En **2018**, la activista saudí **Loujain al-Hathloul** fue detenida durante más de **1000 días** por ser una de las principales defensoras de los derechos de las mujeres en el país. Su liberación ocurrió en **2021**, fecha en la cual **Google** le informó que **hackers** contratados por las autoridades saudíes habían intentado ingresar a su cuenta de **Gmail**.

Debido a esto, Loujain al-Hathloul pidió ayuda de expertos por miedo de que su **iPhone** también estuviese comprometido. En este punto de la historia, entra de nuevo **Citizen Lab**, clave para el descubrimiento a nivel global de este spyware.

### Falla del Spyware

![img](/assets/img/post/articulo-pegasus/4.png)

Los expertos de **Citizen Lab** llevaron a cabo un arduo trabajo durante varios meses, intentando encontrar indicios de la presencia de algún spyware en el dispositivo de **Loujain al-Hathloul**. Finalmente, lograron identificar la amenaza. El software había sufrido una **falla significativa**; esta dejó almacenada en el teléfono de al-Hathloul una copia del archivo de imagen malicioso que se había utilizado en el dispositivo para robar los mensajes almacenados.

Esta falla fue particularmente destacable porque, por lo general, este tipo de **malware** está diseñado para la **recolección de información** y para su propia **autoeliminación** para no dejar rastro. La capacidad de autoeliminación es una característica clave de este spyware, ya que permite a los atacantes mantener sus actividades encubiertas y dificultar la detección del software malicioso.

El descubrimiento de esta falla permitió a los investigadores confirmar con certeza que se trataba del **software desarrollado por NSO Group**. Este hallazgo fue crucial para entender la magnitud del **espionaje** y el alcance del impacto del spyware en las víctimas.

## **¿Como funciona Pegasus?**

![img](/assets/img/post/articulo-pegasus/5.jpg)

Para entender cómo funciona, primero debemos saber qué es un **spyware**.

Un **spyware** es un software malicioso diseñado para recopilar información de un dispositivo y transmitir esa información a una entidad externa sin el consentimiento del propietario del dispositivo. Su principal objetivo es **espiar** y recolectar datos sensibles sin que la víctima se dé cuenta.

**Pegasus** es un tipo de **malware** que explota **vulnerabilidades zero-day**, las cuales son fallos en el software que son desconocidos tanto para los usuarios como para el fabricante del producto. Estas vulnerabilidades representan un riesgo significativo porque no hay parches disponibles para protegerse contra ellas en el momento en que se explotan.

Después de una exhaustiva investigación sobre Pegasus, se revelaron tres vulnerabilidades específicas:

* **[CVE-2016-4655](https://www.incibe.es/incibe-cert/alerta-temprana/vulnerabilidades/cve-2016-4655)**: **Fuga de información en el kernel**. Esta vulnerabilidad permite al atacante acceder a información sensible del kernel y calcular su ubicación en la memoria, lo que facilita la explotación del sistema.

* **[CVE-2016-4656](https://nvd.nist.gov/vuln/detail/CVE-2016-4656)**: **Corrupción de la memoria del núcleo lleva a Jailbreak**. Esta vulnerabilidad en el kernel de iOS, tanto en versiones de 32 como de 64 bits, permite al atacante realizar un jailbreak en secreto, liberando el dispositivo y permitiendo la instalación de software de vigilancia sin el conocimiento del usuario.

* **[CVE-2016-4657](https://nvd.nist.gov/vuln/detail/CVE-2016-4657)**: **Corrupción de la memoria en WebKit**. Esta vulnerabilidad en Safari WebKit permite al atacante comprometer el dispositivo cuando el usuario hace clic en un enlace malicioso, exponiéndolo a la infección del malware.

Lo primero que hace este programa es **infectar el teléfono móvil** aprovechándose de las vulnerabilidades zero-day. Aunque la forma exacta en la que opera este malware no está completamente confirmada, se cree que puede infiltrarse a través de **enlaces enviados por mensajería instantánea**. Además, también se ha sugerido que el malware podría distribuirse mediante **llamadas de WhatsApp**, **mensajes de iMessage**, así como **ataques de phishing** y **suplantación de identidad**.

Una vez que el dispositivo está infectado, **Pegasus** concede al atacante **control total** sobre el mismo. Esto incluye acceso a **mensajes**, **correos electrónicos**, el **micrófono**, la **cámara**, las **llamadas** y los **contactos**. La capacidad de monitorear y controlar estos aspectos del dispositivo convierte a Pegasus en una herramienta extremadamente poderosa y peligrosa para la vigilancia y el espionaje.

## **Casos en España**

![img](/assets/img/post/articulo-pegasus/6.jpg)

En nuestro propio país, ha habido varios casos muy mediáticos sobre el uso de este software por parte del gobierno, pero sin duda el más polémico y conocido ha sido el **espionaje mediante Pegasus** de **65 abogados**, **académicos**, **periodistas** y **políticos** de los entornos independentistas vascos y catalanes.

Este caso se considera la **mayor operación de espionaje contra un solo grupo de víctimas** documentada por **Citizen Lab**, quienes, como ya mencionamos, son los encargados de investigar y rastrear las actividades de este software espía.

Según **Citizen Lab**, el espionaje contra los independentistas vascos y catalanes fue llevado a cabo con un gran nivel de **personalización** para cada uno de ellos. En este caso, se utilizaron **SMS fraudulentos** y **mensajes desde redes sociales**, además de la explotación de **vulnerabilidades zero-day** en la aplicación **iMessage** para infiltrar los dispositivos.

Según varias fuentes, la **Agencia Española de Inteligencia (CNI)** adquirió **Pegasus** por unos **6 millones de euros** con el supuesto objetivo de espiar en el extranjero.

## **Pegasus en el presente**

A día de hoy, se podría pensar que este software está muy controlado y que los casos de espionaje utilizando esta herramienta son casi inexistentes. Sin embargo, esto no es así. Justamente ayer, **07/09/2023**, **Citizen Lab** reportó a través de sus redes sociales una **vulnerabilidad** utilizada por el software **Pegasus** de **NSO Group**, e instó a todos los usuarios de **Apple** a actualizar a la versión **16.6.1**.

La vulnerabilidad recientemente descubierta, denominada **BLASTPASS Exploit Chain**, está diseñada para ejecutarse en versiones de **iOS 16.6** sin ningún tipo de interacción por parte de la víctima.

Al parecer, el exploit involucra **archivos PassKit** que contienen imágenes maliciosas enviadas desde una cuenta de **iMessage** del atacante a la víctima.

**Citizen Lab** ya reportó estos fallos a **Apple**, y se añadieron dos **CVE** relacionados con este exploit:

* **[CVE-2023-41064](https://nvd.nist.gov/vuln/detail/CVE-2023-41064)**
* **[CVE-2023-41061](https://nvd.nist.gov/vuln/detail/cve-2023-41061)**

Esta noticia es la razón por la que hemos decidido publicar este mini artículo sobre el tema.

Es fundamental **mantenernos al día** con los temas de **ciberseguridad**, enseñar y fomentar buenas prácticas en esta área para que todos nosotros, además de nuestros datos, estemos seguros en la red y podamos utilizarla sin ningún riesgo.

*Espero que os haya gustado leer el artículo tanto como a mi hacerlo. ¡Gracias por leer!*
