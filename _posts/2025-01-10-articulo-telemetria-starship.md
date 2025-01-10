---
title: Procesamiento de Telemetría de Lanzamientos de Cohetes utilizando OCR
date: 2024-12-08 12:30:00 +0800
categories: [Programación, Desarrollo de Software]
tags: [OCR, Tesseract, Python, OpenCV, Python, DataScience]
image:
  path: /assets/img/post/telemetria_starship/1.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## Introducción 

Seguro que alguna vez has visto el lanzamiento de un cohete, ya sea en directo o en video, pero, ¿Te has preguntado alguna vez que son todos esos números que aparecen en pantlla, como la velociad o la altitud.

En este post te enseáré como esos datos pueden ser muy interesantes y como con este proyecto he conseguido mediante un motor de OCR llamado Tesseract, procesar estos datos en tiempo real, analizarlos, sacar conclusiones e incluso llegar a otros puntos mucho más complejos que no vemos a simple vista en el lanzamiento.

Además te enseñaré como funciona el programa de extracción de telemetría, cuales son sus partes y por supuesto, como utilizarlo tu mismo.

## Conexto del Proyecto

El objetivo de este proyecto ha sido desde el primer momento la extracción de información relevante de la telemetría que se nos proporciona de manera pública en los directos de lanzamientos de cohetes como Starship y Falcon 9 de la empresa SpaceX.

Algunos importantes desafíos que me he encontrado en su desarrollo incluyen:

- Calidad de imagen y ruido visual

- Procesamiento de 6 fotogramas por segundo

- Variedad y conversión de diferentes formatos de vídeo

## Herramientas y Tecnologías Utilizadas

- **Python:** Lenguaje base del proyecto.

- **Tesseract:** Herramienta OCR utilziada para extraer texto de imágenes.

- **OpenCV:** Biblioteca para el procesamiento de imágenes y vídeos.

- **Expresiones Regulares:** Para filtrar y procesar los datos extraídos.

# UC
