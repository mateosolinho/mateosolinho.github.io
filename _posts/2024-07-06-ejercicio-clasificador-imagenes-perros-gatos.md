---
title: Clasificación de Imágenes con TensorFlow [ Comparación de Modelos Densos y Convolucionales ]
date: 2024-07-06 21:40:00 +0800
categories: [Programación, IA]
tags: [Machine Learning, Data Science, TensorFlow, ANN, CNN, Aprendizaje Automático, IA]
image:
  path: /assets/img/post/cats_vs_dogs/6.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

[Link Repositorio Github](https://github.com/mateosolinho/python/tree/master/projects/clasificador_perros_gatos)

En esta entrada de blog, exploraremos cómo construir y entrenar diferentes modelos de redes neuronales para clasificar imágenes de gatos y perros utilizando el dataset Cats vs. Dogs con TensorFlow.

A continuación, se explicará en detalle el tipo de red utilizada, su propósito y un desglose de cada parte del código.

## ¿Qué es el Dataset Cats vs Dogs?

El dataset Cats vs Dogs es un conjunto de datos de imágenes que contiene miles de imágenes de gatos y perros. Este dataset es ampliamente utilizado para tareas de clasificación binaria y ofrece un buen desafío para evaluar y comparar diferentes arquitecturas de redes neuronales.

## Estructura de la Red Neuronal Utilizada

### Red Neuronal Densa (ANN)

La `red neuronal densa (ANN)` es una red **totalmente conectada**, donde **cada neurona de una capa está conectada a cada neurona de la siguiente capa**. Esta rred es adecuada para tareas básicas de clasificación, aunque puede no ser tan efectiva para datos de imágenes más complejos como las `CNN`

#### Capas del Modelo Denso

* **Capa de Entrada (Flatten Layer):** Convierte cada imagen de `100x100 píxeles` en un `vector unidimensional de 10.000 elementos`.

* **Capas Ocultas (Dense Layers):** Dos capas `densas` `(fully connected)` con 150 neuronas cada una y `función de activación ReLU`. Estas capas capturan patrones y características complejas de las imágenes.

* **Capa de Salida (Dense Layer):** Una capa `densa` con 1 neurona y `función de activación sigmoide`, que produce una probabilidad para la clase (gato o perro)

### Red Neuronal Convolucional (CNN)

Las `redes neuronales convulocionales (CNN)` son especialmente eficaces para el **procesamiento de datos de imágenes**. Utilizan capas de `convolución y pooling` para extraer características espaciales y patrones relevantes de las imágenes

#### Capas del modelo CNN

* **Capas de Convolución y Pooling:**
  * ***Conv2D(32,(3,3), activation='relu'):*** Extrae características básicas de las imágenes.
  * ***MaxPooling2D(2, 2):*** Reduce la dimensionalidad de las características extraídas.
  * ***Conv2D(64, (3,3), activation='relu'):*** Extrae características más complejas.
  * ***MaxPooling2D(2, 2):*** Reduce aún más la dimensionalidad.
  * ***Conv2D(128, (3,3), activation='relu'):*** Extrae características aún más abstractas.
  * ***MaxPooling2D(2, 2):*** Reduce la dimensionalidad al nivel final.

* **Capa de Aplanado (Flatten):** Convierte la salida `tridimensional` de las capas convolucionales en un vector `unidimensional`.
* **Capa Densa (Dense Layer):** Una capa `densa con 100 neuronas` y `función de activación ReLU` para combinar las características extraídas.
* **Capa de Salida (Dense Layer):** Una capa densa con `una neurona` y `función de activación sigmoide` para la clasificación binaria.

#### Modelo CNN con Dropout

Para reducir el **sobreajuste**, añadimos una capa de `Dropout` con la **tasa del 50%**, que desactiva aleatoriamente neuronas durante el entrenamiento

El **sobreajuste** ocurre cuando un modelo tiene un rendimiento excelente en los datos de entrenamiento, pero falla al generalizar y obtener buenos resultados en datos no vistos *(de prueba o validación)*.

Esto generalmente sucede cuando el modelo es demasiado complejo y se ajusta demasiado a los detalles y ruidos específicos del conjunto de datos de entrenamiento.

## Implementación del Código

```python
import tensorflow as tf
import tensorflow_datasets as tfds

setattr(tfds.image_classification.cats_vs_dogs, '_URL',"https://download.microsoft.com/download/3/E/1/3E1C3F21-ECDB-4869-8368-6DEBA77B919F/kagglecatsanddogs_5340.zip")

datos, metadatos = tfds.load('cats_vs_dogs', as_supervised=True, with_info=True)

import matplotlib.pyplot as plt
import cv2

plt.figure(figsize=(20,20))

TAMANO_IMG=100

for i, (imagen, etiqueta) in enumerate(datos['train'].take(25)):
  imagen = cv2.resize(imagen.numpy(), (TAMANO_IMG, TAMANO_IMG))
  imagen = cv2.cvtColor(imagen, cv2.COLOR_BGR2GRAY)
  plt.subplot(5, 5, i+1)
  plt.xticks([])
  plt.yticks([])
  plt.imshow(imagen, cmap='gray')

datos_entrenamiento = []

for i, (imagen, etiqueta) in enumerate(datos['train']): 
  imagen = cv2.resize(imagen.numpy(), (TAMANO_IMG, TAMANO_IMG))
  imagen = cv2.cvtColor(imagen, cv2.COLOR_BGR2GRAY)
  imagen = imagen.reshape(TAMANO_IMG, TAMANO_IMG, 1) 
  datos_entrenamiento.append([imagen, etiqueta])

X = [] # imagenes de entrada (pixeles)
y = [] # etiquetas (perro o gato)

for imagen, etiqueta in datos_entrenamiento:
  X.append(imagen)
  y.append(etiqueta)

import numpy as np

X = np.array(X).astype(float) / 255

y = np.array(y)

modeloDenso = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(100, 100, 1)),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(100, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN2 = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Dropout(0.5),
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(250, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloDenso.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

modeloCNN.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

modeloCNN2.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

from tensorflow.keras.callbacks import TensorBoard

tensorboardDenso = TensorBoard(log_dir='logs/denso')
modeloDenso.fit(X, y, batch_size=32,
                validation_split=0.15,
                epochs=100,
                callbacks=[tensorboardDenso])

tensorboardCNN = TensorBoard(log_dir='logs/cnn')
modeloCNN.fit(X, y, batch_size=32,
                validation_split=0.15,
                epochs=100,
                callbacks=[tensorboardCNN])

tensorboardCNN2 = TensorBoard(log_dir='logs/cnn2')
modeloCNN2.fit(X, y, batch_size=32,
                validation_split=0.15,
                epochs=100,
                callbacks=[tensorboardCNN2])

plt.figure(figsize=(20, 8))
for i in range(10):
  plt.subplot(2, 5, i+1)
  plt.xticks([])
  plt.yticks([])
  plt.imshow(X[i].reshape(100, 100), cmap="gray")

from tensorflow.keras.preprocessing.image import ImageDataGenerator

datagen = ImageDataGenerator(
    rotation_range=30,
    width_shift_range=0.2,
    height_shift_range=0.2,
    shear_range=15,
    zoom_range=[0.7, 1.4],
    horizontal_flip=True,
    vertical_flip=True
)

datagen.fit(X)

plt.figure(figsize=(20,8))

for imagen, etiqueta in datagen.flow(X, y, batch_size=10, shuffle=False):
  for i in range(10):
    plt.subplot(2, 5, i+1)
    plt.xticks([])
    plt.yticks([])
    plt.imshow(imagen[i].reshape(100, 100), cmap="gray")
  break

modeloDenso_AD = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(100, 100, 1)),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN_AD = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(100, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN2_AD = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Dropout(0.5),
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(250, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloDenso_AD.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

modeloCNN_AD.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

modeloCNN2_AD.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

len(X) * .85 # 19700
len(X) - 19700 # 3562

X_entrenamiento = X[:19700]
X_validacion = X[19700:]

y_entrenamiento = y[:19700]
y_validacion = y[19700:]

data_gen_entrenamiento = datagen.flow(X_entrenamiento, y_entrenamiento, batch_size=32)

tensorboardDenso_AD = TensorBoard(log_dir='logs/denso_AD')

modeloDenso_AD.fit(
    data_gen_entrenamiento,
    epochs=100, batch_size=32,
    validation_data=(X_validacion, y_validacion),
    steps_per_epoch=int(np.ceil(len(X_entrenamiento) / float(32))),
    validation_steps=int(np.ceil(len(X_validacion) / float(32))),
    callbacks=[tensorboardDenso_AD]
)

tensorboardCNN_AD = TensorBoard(log_dir='logs-new/cnn_AD')

modeloCNN_AD.fit(
    data_gen_entrenamiento,
    epochs=150, batch_size=32,
    validation_data=(X_validacion, y_validacion),
    steps_per_epoch=int(np.ceil(len(X_entrenamiento) / float(32))),
    validation_steps=int(np.ceil(len(X_validacion) / float(32))),
    callbacks=[tensorboardCNN_AD]
)

tensorboardCNN2_AD = TensorBoard(log_dir='logs/cnn2_AD')

modeloCNN2_AD.fit(
    data_gen_entrenamiento,
    epochs=100, batch_size=32,
    validation_data=(X_validacion, y_validacion),
    steps_per_epoch=int(np.ceil(len(X_entrenamiento) / float(32))),
    validation_steps=int(np.ceil(len(X_validacion) / float(32))),
    callbacks=[tensorboardCNN2_AD]
)

modeloCNN_AD.save('perros-gatos-cnn-ad.h5')
```

## Explicación Detallada del Código

### Cargando y Preprocesando los Datos

```python
import tensorflow as tf
import tensorflow_datasets as tfds

setattr(tfds.image_classification.cats_vs_dogs, '_URL',"https://download.microsoft.com/download/3/E/1/3E1C3F21-ECDB-4869-8368-6DEBA77B919F/kagglecatsanddogs_5340.zip")

datos, metadatos = tfds.load('cats_vs_dogs', as_supervised=True, with_info=True)

import matplotlib.pyplot as plt
import cv2

plt.figure(figsize=(20,20))

TAMANO_IMG = 100

for i, (imagen, etiqueta) in enumerate(datos['train'].take(25)):
  imagen = cv2.resize(imagen.numpy(), (TAMANO_IMG, TAMANO_IMG))
  imagen = cv2.cvtColor(imagen, cv2.COLOR_BGR2GRAY)
  plt.subplot(5, 5, i+1)
  plt.xticks([])
  plt.yticks([])
  plt.imshow(imagen, cmap='gray')
```

Importamos las librerias `tensorflow`, `tensorflow_datasets`, `matplotlib` y `cv2`, cargamos el dataset y preprocesamos las imágenes para que tengan un tamaño uniforme y se conviertan a escala de grises

1. Utilizamos `tfds.load` paraq cargar el dataset `'cats_vs_dogs'`
2. Redimensionamos las imágenes a 100x100 píxeles y las convertimos a escala de grises para simplificar el procesamiento.
3. Mostramos algunas de imágenes del conjunto de datos para ver como se ven después del proprocesamiento.

Salida:

![img](/assets/img/post/cats_vs_dogs/1.png)

### Preparando los Datos de Entrenamiento

```python
datos_entrenamiento = []

for i, (imagen, etiqueta) in enumerate(datos['train']): 
  imagen = cv2.resize(imagen.numpy(), (TAMANO_IMG, TAMANO_IMG))
  imagen = cv2.cvtColor(imagen, cv2.COLOR_BGR2GRAY)
  imagen = imagen.reshape(TAMANO_IMG, TAMANO_IMG, 1) 
  datos_entrenamiento.append([imagen, etiqueta])

X = [] # imágenes de entrada (pixeles)
y = [] # etiquetas (perro o gato)

for imagen, etiqueta in datos_entrenamiento:
  X.append(imagen)
  y.append(etiqueta)

import numpy as np

X = np.array(X).astype(float) / 255
y = np.array(y)
```

Preparamos los datos de entrenamiento redimensionando y normalizando las imágenes.

1. Procesamos todas las imágenes del dataset y las convertimos a escala de grises.
2. Escalamos los valores de los píxeles a un rango de 0 a 1 para facilitar el entrenamiento del modelo.
3. Separamos las imágenes y las etiquetas en dos arrays, `X` para las imágenes `y` y para las etiquetas.

### Definiendo y Entrenando los Modelos

#### Modelo Denso (Fully Conected)

```python
modeloDenso = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(100, 100, 1)),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloDenso.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])
```

Primero, definimos un modelo denso, que es una red neuronal simple con capas totalmente conectadas.

1. **Capa de Entrada:** La capa `Flatten` convierte la imagen 2D en un vector 1D.
2. **Capas Ocultas:** Dos **capas densas con 150 neuronas cada una** y `función de activación ReLU`.
3. **Capa de Salida:** Una **capa densa con una neurona** y `función de activación sigmoide` para clasificación binaria (gato o perro).
4. **Compilación:** Utilizamos el optimizador `'adam'` y la `función de pérdida 'binary_crossentropy'`.

#### Modelo Convolucional (CNN)

```python
modeloCNN = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(100, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])
```

A continuación, definimos un modelo convolucional, que es más adecuado para el procesamiento de datos de imágenes.

1. **Capas Convolucionales:** Tres **capas convolucionales** con `funciones de activación ReLU` y **tamaños de filtro de 3x3**.
2. **Capas de Pooling:** Tres capas de max `pooling` para reducir la dimensionalidad.
3. **Capa de Flatten:** Convierte las **activaciones 2D en un vector 1D**.
4. **Capa Oculta y de Salida:** Una **capa densa con 100 neuronas** y la capa de **salida** con una neurona `sigmoide`.

#### Modelo Convolucional con Regularización

```python
modeloCNN2 = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Dropout(0.5),
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(250, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN2.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])
```

Por último, definimos un modelo convolucional similar al anterior, pero con técnicas de regularización para evitar el sobreajuste.

1. **Capas Convolucionales y de Pooling:** Igual que en el modelo `CNN` anterior.
2. **Capa de Dropout:** Se añade una capa de `Dropout` para **regularización**, que "apaga" aleatoriamente el **50% de las neuronas** durante el entrenamiento para evitar el sobreajuste.
3. **Capa de Flatten, Oculta y de Salida:** Igual que en el modelo `CNN` anterior, pero con más neuronas en la capa densa.

### Entrenando los modelos

```python
from tensorflow.keras.callbacks import TensorBoard

tensorboardDenso = TensorBoard(log_dir='logs/denso')
modeloDenso.fit(X, y, batch_size=32,
                validation_split=0.15,
                epochs=100,
                callbacks=[tensorboardDenso])

tensorboardCNN = TensorBoard(log_dir='logs/cnn')
modeloCNN.fit(X, y, batch_size=32,
                validation_split=0.15,
                epochs=100,
                callbacks=[tensorboardCNN])

tensorboardCNN2 = TensorBoard(log_dir='logs/cnn2')
modeloCNN2.fit(X, y, batch_size=32,
                validation_split=0.15,
                epochs=100,
                callbacks=[tensorboardCNN2])
```

Entrenamos cada modelo durante `100 épocas` con un `batch size de 32`, reservando el `15% de los datos para validación` y utilizamos `TensorBoard` para monitorear el proceso de entrenamiento. *(En las gráficas solo uso 25 épocas para observar la diferencia, en la ejecución normal uso 100)*.

### Visualizando los Datos de Entrenamiento

```python
plt.figure(figsize=(20, 8))
for i in range(10):
  plt.subplot(2, 5, i+1)
  plt.xticks([])
  plt.yticks([])
  plt.imshow(X[i].reshape(100, 100), cmap="gray")
```

Mostramos algunas imágenes del conjunto de datos después de la normalización y el preprocesamiento.

Salida:

![img](/assets/img/post/cats_vs_dogs/2.png)

### Aumentación de Datos

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

datagen = ImageDataGenerator(
    rotation_range=30,
    width_shift_range=0.2,
    height_shift_range=0.2,
    shear_range=15,
    zoom_range=[0.7, 1.4],
    horizontal_flip=True,
    vertical_flip=True
)

datagen.fit(X)

plt.figure(figsize=(20,8))

for imagen, etiqueta in datagen.flow(X, y, batch_size=10, shuffle=False):
  for i in range(10):
    plt.subplot(2, 5, i+1)
    plt.xticks([])
    plt.yticks([])
    plt.imshow(imagen[i].reshape(100, 100), cmap="gray")
  break
```

Utilizamos `ImageDataGenerator` para aplicar aumentación de datos, lo que ayuda a mejorar la generalización del modelo.

![img](/assets/img/post/cats_vs_dogs/3.png)

### Entrenando con Aumentación de Datos

```python
modeloDenso_AD = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(100, 100, 1)),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(150, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN_AD = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(100, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloCNN2_AD = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(100, 100, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2, 2),

  tf.keras.layers.Dropout(0.5),
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(250, activation='relu'),
  tf.keras.layers.Dense(1, activation='sigmoid')
])

modeloDenso_AD.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

modeloCNN_AD.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

modeloCNN2_AD.compile(optimizer='adam',
                    loss='binary_crossentropy',
                    metrics=['accuracy'])

X_entrenamiento = X[:19700]
X_validacion = X[19700:]

y_entrenamiento = y[:19700]
y_validacion = y[19700:]

data_gen_entrenamiento = datagen.flow(X_entrenamiento, y_entrenamiento, batch_size=32)

tensorboardDenso_AD = TensorBoard(log_dir='logs/denso_AD')

modeloDenso_AD.fit(
    data_gen_entrenamiento,
    epochs=100, batch_size=32,
    validation_data=(X_validacion, y_validacion),
    steps_per_epoch=int(np.ceil(len(X_entrenamiento) / float(32))),
    validation_steps=int(np.ceil(len(X_validacion) / float(32))),
    callbacks=[tensorboardDenso_AD]
)

tensorboardCNN_AD = TensorBoard(log_dir='logs-new/cnn_AD')

modeloCNN_AD.fit(
    data_gen_entrenamiento,
    epochs=150, batch_size=32,
    validation_data=(X_validacion, y_validacion),
    steps_per_epoch=int(np.ceil(len(X_entrenamiento) / float(32))),
    validation_steps=int(np.ceil(len(X_validacion) / float(32))),
    callbacks=[tensorboardCNN_AD]
)

tensorboardCNN2_AD = TensorBoard(log_dir='logs/cnn2_AD')

modeloCNN2_AD.fit(
    data_gen_entrenamiento,
    epochs=100, batch_size=32,
    validation_data=(X_validacion, y_validacion),
    steps_per_epoch=int(np.ceil(len(X_entrenamiento) / float(32))),
    validation_steps=int(np.ceil(len(X_validacion) / float(32))),
    callbacks=[tensorboardCNN2_AD]
)

```

Entrenamos los modelos nuevamente utilizando los datos aumentados.

1. Definimos nuevamente los modelos, esta vez para ser entrenados con **datos aumentados**.
2. Dividimos los datos en conjuntos de **entrenamiento y validación**.
3. Entrenamos los modelos utilizando el **generador de datos aumentados**.

### Guardando y Exportando el modelo

```python
modeloCNN_AD.save('perros-gatos-cnn-ad.h5')
!pip install tensorflowjs
!mkdir carpeta_salida
!tensorflowjs_converter --input_format keras perros-gatos-cnn-ad.h5 carpeta_salida
```

1. Guardamos el modelo para ser exportado a un navegador
2. Instalamos el paquete `TensorFlow.js`.
3. Creamos una **carpeta de salida** para el modelo convertido.
4. Convertimos el modelo de `Keras` a `TensorFlow.js` para su **despliegue en la web**.

## Despliegue del Modelo en la Web

Para desplegar el modelo en la web deberemos de seguir varios pasos:

* Descargamos todo el contenido de la carpeta `carpeta_salida`, deberían ser 3 archivos, `1 .json` y `2 .bin`
* Movemos todo el contenido descargado a una carpeta junto con un `index.html` con el siguiente código:
  
  ```html
    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
      <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
      <title>Perros y Gatos</title>

      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">

      <style>
        #resultado {
          font-weight: bold;
          font-size: 6rem;
          text-align: center;
        }
      </style>
    </head>
    <body>
      
      <main>
        <div class="px-4 py-2 my-2 text-center border-bottom">
          <img class="d-block mx-auto mb-2" src="LogotipoV2-Simple.png" alt="" width="80" height="80">
          <h1 class="display-5 fw-bold">Perros y gatos</h1>
          <div class="col-lg-6 mx-auto">
            <p class="lead mb-0">Clasificaci&oacute;n de im&aacute;genes (Perro o Gato) usando la c&aacute;mara web utilizando Tensorflow.js</p>
            <p class="lead mb-0">Hay que usarlo desde el telefono, servidor con pyhton y tunelar https con ngrok</p>
          </div>
        </div>

        <div class="b-example-divider"></div>

        <div class="container mt-5">
          <div class="row">
            <div class="col-12 col-md-4 offset-md-4 text-center">
              <video id="video" playsinline autoplay style="width: 1px;"></video>
              <button class="btn btn-primary mb-2" id="cambiar-camara" onclick="cambiarCamara();">Cambiar camara</button>
              <canvas id="canvas" width="400" height="400" style="max-width: 100%;"></canvas>
              <canvas id="otrocanvas" width="150" height="150" style="display: none"></canvas>
              <div id="resultado"></div>            
            </div>
          </div>
        </div>

        <div class="b-example-divider"></div>

        <div class="b-example-divider mb-0"></div>
      </main>

      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>

      <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@2.0.0/dist/tf.min.js"></script>

    <script type="text/javascript">

      var tamano = 400;
      var video = document.getElementById("video");
      var canvas = document.getElementById("canvas");
      var otrocanvas = document.getElementById("otrocanvas");
      var ctx = canvas.getContext("2d");
      var currentStream = null;
      var facingMode = "user";

      var modelo = null;

      (async() => {
        console.log("Cargando modelo...");
        modelo = await tf.loadLayersModel("model.json");
        console.log("Modelo cargado");
      })();

      window.onload = function() {
        mostrarCamara();
      }

      function mostrarCamara() {
        var opciones = {
          audio: false,
          video: {
            width: tamano, height: tamano
          }
        }

        if (navigator.mediaDevices.getUserMedia) {
          navigator.mediaDevices.getUserMedia(opciones)
              .then(function(stream) {
                currentStream = stream;
                video.srcObject = currentStream;
                procesarCamara();
                predecir();
              })
              .catch(function(err) {
                alert("No se pudo utilizar la camara :(");
                console.log(err);
                alert(err);
              })
        } else {
          alert("No existe la funcion getUserMedia");
        }
      }

      function cambiarCamara() {
            if (currentStream) {
                currentStream.getTracks().forEach(track => {
                    track.stop();
                });
            }

            facingMode = facingMode == "user" ? "environment" : "user";

            var opciones = {
                audio: false,
                video: {
                    facingMode: facingMode, width: tamano, height: tamano
                }
            };


            navigator.mediaDevices.getUserMedia(opciones)
                .then(function(stream) {
                    currentStream = stream;
                    video.srcObject = currentStream;
                })
                .catch(function(err) {
                    console.log("Oops, hubo un error", err);
                })
        }

      function procesarCamara() {
        ctx.drawImage(video, 0, 0, tamano, tamano, 0, 0, tamano, tamano);
        setTimeout(procesarCamara, 20);
      }

      function predecir() {
        if (modelo != null) {
          resample_single(canvas, 100, 100, otrocanvas);

          //Hacer la predicción
          var ctx2 = otrocanvas.getContext("2d");
          var imgData = ctx2.getImageData(0,0, 100, 100);

          var arr = [];
          var arr100 = [];

          for (var p=0; p < imgData.data.length; p+= 4) {
            var rojo = imgData.data[p] / 255;
            var verde = imgData.data[p+1] / 255;
            var azul = imgData.data[p+2] / 255;

            var gris = (rojo+verde+azul)/3;

            arr100.push([gris]);
            if (arr100.length == 100) {
              arr.push(arr100);
              arr100 = [];
            }
          }

          arr = [arr];

          var tensor = tf.tensor4d(arr);
          var resultado = modelo.predict(tensor).dataSync();

          var respuesta;
          if (resultado <= .5) {
            respuesta = "Gato";
          } else {
            respuesta = "Perro";
          }
          document.getElementById("resultado").innerHTML = respuesta;

        }

        setTimeout(predecir, 150);
      }

      /**
         * Hermite resize - fast image resize/resample using Hermite filter. 1 cpu version!
         * 
         * @param {HtmlElement} canvas
         * @param {int} width
         * @param {int} height
         * @param {boolean} resize_canvas if true, canvas will be resized. Optional.
         * Cambiado por RT, resize canvas ahora es donde se pone el chiqitillllllo
         */
        function resample_single(canvas, width, height, resize_canvas) {
            var width_source = canvas.width;
            var height_source = canvas.height;
            width = Math.round(width);
            height = Math.round(height);

            var ratio_w = width_source / width;
            var ratio_h = height_source / height;
            var ratio_w_half = Math.ceil(ratio_w / 2);
            var ratio_h_half = Math.ceil(ratio_h / 2);

            var ctx = canvas.getContext("2d");
            var ctx2 = resize_canvas.getContext("2d");
            var img = ctx.getImageData(0, 0, width_source, height_source);
            var img2 = ctx2.createImageData(width, height);
            var data = img.data;
            var data2 = img2.data;

            for (var j = 0; j < height; j++) {
                for (var i = 0; i < width; i++) {
                    var x2 = (i + j * width) * 4;
                    var weight = 0;
                    var weights = 0;
                    var weights_alpha = 0;
                    var gx_r = 0;
                    var gx_g = 0;
                    var gx_b = 0;
                    var gx_a = 0;
                    var center_y = (j + 0.5) * ratio_h;
                    var yy_start = Math.floor(j * ratio_h);
                    var yy_stop = Math.ceil((j + 1) * ratio_h);
                    for (var yy = yy_start; yy < yy_stop; yy++) {
                        var dy = Math.abs(center_y - (yy + 0.5)) / ratio_h_half;
                        var center_x = (i + 0.5) * ratio_w;
                        var w0 = dy * dy; //pre-calc part of w
                        var xx_start = Math.floor(i * ratio_w);
                        var xx_stop = Math.ceil((i + 1) * ratio_w);
                        for (var xx = xx_start; xx < xx_stop; xx++) {
                            var dx = Math.abs(center_x - (xx + 0.5)) / ratio_w_half;
                            var w = Math.sqrt(w0 + dx * dx);
                            if (w >= 1) {
                                //pixel too far
                                continue;
                            }
                            //hermite filter
                            weight = 2 * w * w * w - 3 * w * w + 1;
                            var pos_x = 4 * (xx + yy * width_source);
                            //alpha
                            gx_a += weight * data[pos_x + 3];
                            weights_alpha += weight;
                            //colors
                            if (data[pos_x + 3] < 255)
                                weight = weight * data[pos_x + 3] / 250;
                            gx_r += weight * data[pos_x];
                            gx_g += weight * data[pos_x + 1];
                            gx_b += weight * data[pos_x + 2];
                            weights += weight;
                        }
                    }
                    data2[x2] = gx_r / weights;
                    data2[x2 + 1] = gx_g / weights;
                    data2[x2 + 2] = gx_b / weights;
                    data2[x2 + 3] = gx_a / weights_alpha;
                }
            }


            ctx2.putImageData(img2, 0, 0);
        }
      </script>
      </body>
    </html>
  ```

* Dentro de la carpeta donde tengamos el `index.HTML`, lanzamos el siguiente comando para crear un servidor con python:

  ```cmd
  python -m http.server 8000
  ```

  Y nos podremos conectar al localhost creado mediante: [localhost:8000/index.html](localhost:8000/index.html)

* Por último, para poder utilzar cámara del móvil en la web deberemos usar un sitio con `HTTPS`, utilizaremos `NGROK` *[[Descarga NGROK]](https://ngrok.com/download)* para crear un túnel `HTTPS` y poder acceder a la cámara de la web desde el móvil

  Una vez instalado ejecutaremos lo siguiente en otra consola:

  ```cmd
  ngrok http 8000
  ```

  **¡Y ya tendremos la cámara junto con el modelo para comenzar a probarlo!**

## Ejemplo de Uso

![img](/assets/img/post/cats_vs_dogs/4.jpg)

![img](/assets/img/post/cats_vs_dogs/5.jpg)

## Conclusión

En este proyecto, he cubierto desde la carga y preparación de datos hasta la construcción, entrenamiento y exportación de modelos de clasificación de imágenes.

Este flujo de trabajo demuestra cómo aplicar técnicas de aumento de datos y evaluar diferentes arquitecturas de redes neuronales para mejorar la precisión en tareas de clasificación de imágenes.

***Créditos del proyecto al canal de Youtube [Ringa Tech](https://www.youtube.com/@RingaTech)***

*Espero que os haya gustado y servido, cualquier comentario es de mucha ayuda. Adios!*
