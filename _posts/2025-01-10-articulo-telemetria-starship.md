---
title: Rocket Launch Telemetry Processing Using OCR
date: 2024-12-08 12:30:00 +0800
categories: [Programación, Desarrollo de Software]
tags: [OCR, Tesseract, Python, OpenCV, Python, DataScience]
image:
  path: /assets/img/post/telemetria_starship/1.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## Introduction 

Surely, you’ve watched a rocket launch at some point, either live or in a video, but have you ever wondered what all those numbers on the screen mean, like the speed or altitude?


In this post, I’ll show you how these data points can be incredibly interesting and how, with this project, I’ve managed to process them in real time using an OCR engine called Tesseract, this allows me to analyze the data, draw conclusions, and even explore much more complex aspects that aren’t immediately visible during the launch.

Además te enseñaré como funciona el programa de extracción de telemetría, cuales son sus partes y por supuesto, como utilizarlo tu mismo.

## Project Context

The goal of this project has always been to extract relevant information from the telemetry publicly provided during live broadcasts of rocket launches, such as SpaceX's Starship and Falcon 9.

Some significant challenges I encountered during development include:

- Image quality and visual noise

- Processing 6 frames per second

- Variety and conversion of different video formats

## Tools and Technologies Used

- **Python:** Base language of the project.

- **Tesseract:** OCR tool used to extract text from images.

- **OpenCV:** Library for image and video processing.

- **Expresiones Regulares:** To filter and process the extracted data.

# UC
