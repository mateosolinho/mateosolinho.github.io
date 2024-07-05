---
title: Primera Red Neuronal
date: 2024-07-04 15:50:00 +0800
categories: [Programación, IA]
tags: [IA]
image:
  path: /assets/img/post/red_neuronal/brain.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

[Link Repositorio Github](https://github.com/mateosolinho/python/tree/master/projects/red_neuronal)

En esta entrada de blog, compartiré mi experiencia al crear mi primera red neuronal utilizando ```TensorFlow```.

La red neuronal que implementé es de prueba, la cual convierte temperaturas de Celsius a Fahrenheit.

A continuación, explicaré en detalle el tipo de red utilizada, su propósito y un desglose de cada parte del código.

## ¿Qué es una Red Neuronal?

Una red neuronal es un ```modelo``` de aprendizaje automático inspirado en el cerebro humano. Está compuesta por capas de ```nodos``` (o neuronas) que están conectados entre sí. Estas redes son capaces de aprender y hacer predicciones basadas en datos de entrenamiento.

## Tipo de Red Utilizada

Para este ejercicio, he utilizado una red neuronal ```conectada``` (o densamente conectada), también conocida como una red neuronal ```feedforward```.

Este tipo de red es uno de los más básicos y se caracteriza por tener capas donde cada neurona está conectada a todas las neuronas de la siguiente capa.

## Propósito de la Red

El propósito de esta red neuronal es aprender a convertir temperaturas de Celsius a Fahrenheit.
Utilizamos un conjunto de datos de ejemplo para entrenar el modelo y luego realizar predicciones basadas en lo que has aprendido.

### Implementación en TensorFlow

```python
import tensorflow as tf
import numpy as np

# Datos de entrenamiento: temperaturas en Celsius y sus equivalentes en Fahrenheit
celsius = np.array([-40, -10, 0, 8, 15, 22, 38], dtype=float)
fahrenheit = np.array([-40, 14, 32, 46, 59, 72, 100], dtype=float)

# Definición de las capas de la red neuronal
oculta1 = tf.keras.layers.Dense(units=3, input_shape=[1])
oculta2 = tf.keras.layers.Dense(units=3)
salida = tf.keras.layers.Dense(units=1)

# Construcción del modelo secuencial
modelo = tf.keras.Sequential([oculta1, oculta2, salida])

# Compilación del modelo con el optimizador y la función de pérdida
modelo.compile(
    optimizer=tf.keras.optimizers.Adam(0.1),
    loss='mean_squared_error'
)

# Entrenamiento del modelo
print("Comenzando entrenamiento del modelo...")
historial = modelo.fit(celsius, fahrenheit, epochs=1000, verbose=False)
print("Modelo entrenado")

# Visualización del proceso de entrenamiento
import matplotlib.pyplot as plt
plt.xlabel("# Epoca")
plt.ylabel("Magnitud de perdida")
plt.plot(historial.history["loss"])
plt.show()

# Realización de una predicción con el modelo entrenado
print("Hagamos una prediccion!")
resultado = modelo.predict([100])
print("El resultado es " + str(resultado) + " fahrenheit")

# Imprimir los pesos de las capas después del entrenamiento
print("Variables internas del modelo")
print(oculta1.get_weights())
print(oculta2.get_weights())
print(salida.get_weights())
```

## Explicación Detallada del Código

### **Importar Bibliotecas Necesarias**

```python
import tensorflow as tf
import numpy as np
```

Aquí, importamos ```TensorFlow``` para construir y entrenar nuestra red neuronal y ```NumPy``` para manejar los datos de manera eficiente.

### **Datos de Entrenamiento**

```python
celsius = np.array([-40, -10, 0, 8, 15, 22, 38], dtype=float)
fahrenheit = np.array([-40, 14, 32, 46, 59, 72, 100], dtype=float)
```

Definimos dos arrays de ```NumPy``` que contienen las temperaturas en Celsius y sus correspondientes en Fahrenheit.

### **Definición de las Capas del Modelo**

```python
oculta1 = tf.keras.layers.Dense(units=3, input_shape=[1])
oculta2 = tf.keras.layers.Dense(units=3)
salida = tf.keras.layers.Dense(units=1)
```

Creamos tres capas ```densas``` ```(fully connected layers)```. La primera capa oculta tiene ```3 neuronas``` y recibe ```una entrada``` (la temperatura en Celsius). La segunda capa oculta también tiene ```3 neuronas```. La ```capa de salida``` tiene una neurona que produce la temperatura en Fahrenheit.

### **Construcción del Modelo**

```python
modelo = tf.keras.Sequential([oculta1, oculta2, salida])
```

Utilizamos ```tf.keras.Sequential``` para definir el modelo como una pila lineal de capas.

### **Compilación del Modelo**

```python
modelo.compile(
    optimizer=tf.keras.optimizers.Adam(0.1),
    loss='mean_squared_error'
)
```

Compilamos el modelo especificando el optimizador ```Adam``` con una ```tasa de aprendizaje``` de 0.1 y una función de pérdida de error cuadrático medio.

### **Entrenamiento del Modelo**

```python
print("Comenzando entrenamiento del modelo...")
historial = modelo.fit(celsius, fahrenheit, epochs=1000, verbose=False)
print("Modelo entrenado")
```

Entrenamos el modelo con los datos de entrenamiento durante ```1000 épocas``` ("vueltas"). Almacenamos el historial del entrenamiento para visualizar la pérdida.

### **Visualización del Entrenamiento**

```python
import matplotlib.pyplot as plt
plt.xlabel("# Epoca")
plt.ylabel("Magnitud de perdida")
plt.plot(historial.history["loss"])
plt.show()
```

Salida:

![img](/assets/img/post/red_neuronal/grafica.png)

Usamos ```Matplotlib``` para graficar la magnitud de la pérdida a lo largo de las épocas, lo que ayuda a entender el modelo de mejora con el tiempo.

### **Realización de una Predicción**

```python
print("Hagamos una prediccion!")
resultado = modelo.predict([100])
print("El resultado es " + str(resultado) + " fahrenheit")
```

Salida:

```python
Hagamos una prediccion!
1/1 [==============================] - 0s 101ms/step
El resultado es [[211.74742]] fahrenheit
```

Utilizamos el modelo entrenado para predecir la temperatura en Fahrenheit correspondiente a 100 grados Celsius.

### **Visualización de los Pesos del Modelo**

```python
print("Variables internas del modelo")
print(oculta1.get_weights())
print(oculta2.get_weights())
print(salida.get_weights())
```

Salida:

```python
Variables internas del modelo
[array([[ 0.46807313,  0.10201637, -0.3496878 ]], dtype=float32), array([ 3.317963 , -2.85734  , -3.2782311], dtype=float32)]
[array([[-0.8704237 , -0.67110294,  0.6999068 ],
       [ 0.87758327,  0.09569932,  0.45393893],
       [ 1.3989487 ,  0.42570233,  0.68541443]], dtype=float32), array([-3.2544918, -3.00666  ,  3.1300454], dtype=float32)]
[array([[-1.5702448],
       [-1.0594199],
       [ 0.3759461]], dtype=float32), array([3.1386135], dtype=float32)]
```

Imprimimos los ```pesos de las capas``` desués del entrenamiento para ver cómo el modelo ha ajustado sus parámetros.

## Conclusión

En este ejercicio muestra cómo construir y entrenar una red neuronal básica para convertir temperaturas de Celsius a Fahrenheit. Aunque es un ejemplo simple, ilustra los conceptos fundamentales del aprendizaje automático y el uso de TensorFlow.

*Espero que os haya gustado y servido, cualquier comentario es de mucha ayuda. Adios!*
