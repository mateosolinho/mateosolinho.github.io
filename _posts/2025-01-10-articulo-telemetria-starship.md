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

I will also show you how the telemetry extraction program works, what its parts are and of course, how to use it yourself.

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

- **Regular Expressions:** To filter and process the extracted data.

## Project Design

### **General Program Flow**

  1. Load launch video

  2. Detect the region of interest (ROI) where telemetry appears in the video

  3. Apply OCR to extract the text from the video

  4. Filter and clean the extracted data

  5. Save the result in a .csv and .xlsx file

### **Initial Configuration**

**How to install Tesseract OCR:**

- ***Windows***
  1. **Download the installer:**
      - Go to [Official Tesseract Project Site](https://github.com/tesseract-ocr/tesseract) and download .exe file from the links in 'Releases'

  2. **Install Tesseract:**
      - Execute the .exe file and follow the installer instructions
      - During the installation note or remember the directory where Tesseract will be installed (for example, `C:\Program Files\Tesseract-OCR`)

  3. **Add Tesseract to the system PATH**
      - Go to "System Configuration" > "Advanced System Configuration" > "Environment Variables"

      - Find the `Path` variable in the system variables, edit it and add the path to the Tesseract installation directory.

  4. **Verify the installation:**   
      - Open a terminal (cmd or PowerShell) and run:
      `tesseract --version`

- ***Linux***
  1. **Update the System**
  `sudo apt update && sudo apt upgrade -y`

  2. **Install Tesseract**
  `sudo apt install tesseract-ocr -y`

  3. **Verify the installation:**
  `tesseract --version`

- ***MacOS***
  1. **Install Homebrew (if you don't have it):**
  `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

  2. **Install Tesseract:**
  `brew install tesseract`

  3. **Verify the installation:**
  `tesseract --version`

- ***Python***
    Independent of the OS, you can install the module for python with:
    `pip install pytesseract` 








# UC
