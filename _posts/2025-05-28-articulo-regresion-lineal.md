---
title: Regresión Lineal: Teoría, implementación y análisis
date: 2024-12-08 12:30:00 +0800
categories: [Inteligencia Artificial, Machine Learning]
tags: [aprendizaje, inteligencia, machine, regresion-lineal, numpy, python]
image:
  path: /assets/img/post/telemetria_starship/1.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## 1. Introducción

La regresión lineal es uno de los modelos más fundamentales en el aprendizaje automátic, a pesar de su simplicidad, es una herramienta importante para comprender las relaciones entre las variables y sirve como base para modelos más complejos.

Este artículo te guiará paso a paso desde la teoría hasta una implementación en Python:

### Aplicaciones prácticas

- Predicción de precios (viviendas, acciones, etc.)
- Estimación de tendencias
- Relación entre variables en estudios científicos

---

## ¿Por qué es esencial aprender regresión lineal para empezar con la IA?

Aprender regresión lineal es uno de los primeros pasos recomendados para iniciarse en inteligencia artificial y machine learning, veamos por qué, desde cuatro perspectivas clave:

### ✅ 1. Establece las bases del aprendizaje supervisado:

- La regresión lineal es el modelo más simple de aprendizaje supervisado.
- Permite entender cómo un modelo aprende a reconocer patrones a partir de datos etiquetados.
- Introduce el concepto de función de pérdida, que mide qué tan bien el modelo está aprendiendo.
- Enseña la idea de generalización -> aplicar el aprendizaje a datos nuevos.

### ✅ 2. Introduce conceptos matemáticos claves:

- Define funciones matemáticas para medir errores (como el Error Cuadrático Medio, MSE).
- Permite estudiar cálculo básico como derivadas y gradiente descendente para optimizar modelos.
- Facilita el uso de álgebra lineal (vectores, matrices), clave en modelos complejos.

### ✅ 3. Conexión directa con redes neuronales:

- Imagina una red neuronal como varias “capas” que procesan datos, si solo tienes una capa que hace una suma y multiplicación simple (sin cosas complicadas), eso es justo una regresión lineal.
- La regresión logística es casi igual, pero después de esa suma, le aplicamos una función que convierte el resultado en una probabilidad (llamada sigmoide).
- Algunas ideas para evitar que el modelo se confunda con los datos (como Ridge y Lasso) vienen de la regresión lineal y también se usan en redes neuronales más grandes.

> Todo esto lo veremos en futuros artículos :)

### ✅ 4. Aplicable a la práctica desde el primer día:

- Puedes aplicar regresión lineal a datasets reales rápidamente.
- Ejemplos típicos: predicción de precios de vivienda, estimación salarial o análisis de ventas.
- Sirve para validar ideas y generar prototipos ágiles en proyectos de IA.

---

## 2. Planteamiento del problema

Dado un conjunto de datos con una variable explicativa (X) y una variable objetivo (Y), queremos encontrar una relación lineal que nos permita predecir el valor de Y a partir de X mediante una fórmula sencilla:

$\begin{equation} \hat{y} = wx + b \end{equation}$

Donde:

- **$w$ (pendiente o coeficiente):** representa cuánto cambia la variable objetivo $Y$ cuando la variable explicativa $X$ aumenta en una unidad.  
  Por ejemplo, si $w = 2$, significa que por cada aumento de 1 en $X$, $Y$ aumentará aproximadamente 2.

- **$b$ (término independiente o bias):** es el valor que toma $Y$ cuando $X = 0$.  
  Es el punto donde la línea de regresión cruza el eje vertical (eje $Y$) y ajusta la línea para que se acerque mejor a los datos.

- **$\hat{y}$ (predicción):** es el valor estimado o calculado de $Y$ para un valor dado de $X$, es la salida que el modelo nos da para hacer predicciones.

### Comprensión práctica

Imagina que quieres predecir el precio de una casa según su tamaño (en metros cuadrados). En este caso:

- **$w$** es el coeficiente que indica cuánto aumenta el precio por cada metro cuadrado adicional.  
  Por ejemplo, si $w = 1500$, significa que cada metro cuadrado extra aumenta el precio en 1500 unidades monetarias.

- **$b$** es el precio base, es decir, el precio estimado de una casa con tamaño 0 (conceptualmente el punto de partida).  
  Por ejemplo, si $b = 50000$, ese sería el costo mínimo o base del inmueble sin importar el tamaño.

- **$\hat{y}$** es la predicción del precio para un tamaño específico.

Por ejemplo, si tienes una casa de 100 metros cuadrados:

\[
\hat{y} = 1500 \times 100 + 50000 = 150000 + 50000 = 200000
\]

El modelo predice que el precio aproximado será de 200,000 unidades monetarias.

### Relación con redes neuronales

Este modelo de regresión lineal puede verse como una neurona muy simple en una red neuronal, donde:

- La entrada es el valor `x` (tamaño de la casa).
- El peso `w` multiplica esa entrada para ajustar su importancia.
- El sesgo `b` (bias) se suma para desplazar la salida y mejorar el ajuste.
- No hay función de activación, por lo que la salida es simplemente una combinación lineal.

Es decir, esta neurona calcula directamente el valor predicho sin transformarlo, que es exactamente lo que hace la regresión lineal.

Esta comprensión ayuda a ver que modelos más complejos, como redes neuronales profundas, están construidos a partir de muchas capas y neuronas similares, pero con funciones de activación que les permiten aprender relaciones no lineales.

---

## 3. Función de pérdida: Error Cuadrático Medio (MSE)

La función de pérdida nos dice qué tan bien o mal está funcionando nuestro modelo, comparando las predicciones con los valores reales.

Para regresión lineal usamos una función llamada **Error Cuadrático Medio (MSE)**, que se calcula así:

\[
MSE = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2
\]

### Explicación sencilla

- Comparamos cada valor real $y_i$ con su predicción $\hat{y}_i$.
- Restamos para ver la diferencia (error).
- Elevamos esa diferencia al cuadrado para que los errores más grandes pesen más.
- Sacamos el promedio de todos esos errores cuadrados.

Así, si nuestras predicciones están muy lejos de los valores reales, el MSE será grande; si están cerca, será pequeño.

### ¿Por qué es útil?

El objetivo del modelo es hacer el MSE lo más pequeño posible, es decir, que las predicciones sean lo más cercanas a los valores reales.

### Aplicación práctica:

Volviendo con el ejemplo de la casa, imagina que queremos de nuevo predecir el precio de una casa según su tamaño.

- Supongamos que el tamaño de la casa es de 100 metros cuadrados.
- Nuestro modelo predice que el precio será 150000 €.
- Pero el precio real es 160000 €.

El error es la diferencia entre el precio real y el predicho:

\[
160,000 - 150,000 = 10,000
\]

Elevamos este error al cuadrado para que errores grandes tengan más peso:

\[
10,000^2 = 100,000,000
\]

Si hacemos esto para muchas casas y luego sacamos el promedio, obtenemos el MSE.

Cuanto más pequeño sea ese número, mejor es nuestro modelo porque significa que nuestras predicciones están cerca de los precios reales.

Por eso, en la práctica, al entrenar un modelo, intentamos minimizar el MSE para que nuestras predicciones sean lo más exactas posible.

### Relación con redes neuronales

En las redes neuronales, esta función de pérdida también se usa para saber qué tan bien están funcionando y para ajustar los números que controlan el modelo (los pesos y sesgos).  

Minimizar el MSE es como decir “quiero que mi modelo cometa el menor error posible”.

---

## 4. Solución analítica (closed-form)

Cuando tenemos nuestros datos, podemos calcular directamente la pendiente $w$ y el punto de intersección $b$ de la mejor línea que se ajusta a esos datos sin necesidad de hacer muchas pruebas o iteraciones.

Las fórmulas son:

\[
w = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2}
\]

\[
b = \bar{y} - w \bar{x}
\]

Donde $\bar{x}$ y $\bar{y}$ son los promedios de todos los tamaños de casas y precios.

### Intuición práctica

Este método encuentra en un solo paso la línea que mejor se ajusta a todos los datos, minimizando la suma de los errores al cuadrado.

### Aplicación práctica con el ejemplo de la casa

Imagina que tienes datos de varias casas con sus tamaños y precios:

| Tamaño (m²) | Precio (€) |
|-------------|------------|
| 80          | 120000    |
| 100         | 150000    |
| 120         | 180000    |

El método calcula cuánto cambia el precio promedio cuando cambia el tamaño promedio (eso es $w$), y cuál es el precio base cuando el tamaño es cero (eso es $b$).

Con esos valores, puedes predecir el precio de una casa nueva con solo saber su tamaño.

### Implementación en Python (analítica)

```python
import numpy as np

def linear_regression_analytic(x, y):
    x_mean, y_mean = np.mean(x), np.mean(y)
    w = np.sum((x - x_mean) * (y - y_mean)) / np.sum((x - x_mean)**2)
    b = y_mean - w * x_mean
    return w, b

x = np.array([80, 100, 120])          # Tamaños
y = np.array([120000, 150000, 180000])  # Precios

w, b = linear_regression_analytic(x, y)
print(f"Pendiente (w): {w}")
print(f"Intercepto (b): {b}")

```

Resultado:

```pyhton

Pendiente (w): 1500.0 
Intercepto (b): 0.0

```

**Pendiente (w) = 1500.0**  

Esto significa que por cada metro cuadrado adicional en el tamaño de la casa, el precio sube en 1500 euros.

Por ejemplo, si una casa es 10 m² más grande, el precio aumentará aproximadamente 15000 euros (10 × 1500).

**Intercepto (b) = 0** 
Este valor es el precio base cuando el tamaño de la casa es 0 m². En este caso es 0 euros, lo cual tiene sentido porque una casa sin tamaño no tendría precio.

En otros casos, el intercepto puede ser diferente y ajusta la línea de predicción.

Entonces, la fórmula de predicción sería:

\[
\hat{y} = 1500 \times x + 0
\]

Entonces, si quieres predecir el precio de una casa de 90 m², solo tienes que multiplicar:

\[
\hat{y} = 1500 \times 90 = 135,000 \, \text{euros}
\]


## 5. Descenso del gradiente (método numérico)

Cuando tenemos muchos datos o varias variables, usar la fórmula directa puede ser complicado o muy lento. Entonces usamos el **descenso del gradiente**, que ajusta poco a poco los parámetros para mejorar la predicción.

### Derivadas parciales

Estas fórmulas nos dicen cómo cambiar `w` y `b` para mejorar el modelo, calculando la pendiente del error:

\[
\frac{\partial \text{MSE}}{\partial w} = -\frac{2}{n} \sum x_i (y_i - \hat{y}_i)
\]

\[
\frac{\partial \text{MSE}}{\partial b} = -\frac{2}{n} \sum (y_i - \hat{y}_i)
\]

### Algoritmo paso a paso

1. Empezamos con `w` y `b` iguales a cero (o cualquier valor).
2. Calculamos las predicciones con esos valores.
3. Medimos el error entre las predicciones y los valores reales.
4. Calculamos cuánto hay que cambiar `w` y `b` para reducir ese error (gradientes).
5. Actualizamos `w` y `b` un poco hacia la dirección que reduce el error.
6. Repetimos muchas veces hasta que el error sea mínimo o cambie poco.

### Implementación en Python

```python
def gradient_descent(x, y, lr=0.01, epochs=1000):
    n = len(x)
    w, b = 0.0, 0.0
    for _ in range(epochs):
        y_pred = w * x + b
        error = y - y_pred
        dw = -2 * np.dot(x, error) / n
        db = -2 * np.sum(error) / n
        w -= lr * dw
        b -= lr * db
    return w, b
```

### Aplicación práctica (ejemplo del precio de la casa)

Si empiezas con `w = 0` y `b = 0`, el modelo predice siempre 0 euros sin importar el tamaño, usando descenso del gradiente, cada paso ajusta el precio base (`b`) y cuánto aumenta el precio por metro cuadrado (`w`) para acercarse al precio real de las casas.

### Relación con redes neuronales

Este proceso de ajustar los parámetros paso a paso es exactamente lo que hacen las redes neuronales cuando se entrenan con retropropagación (También lo explicaremos en futuros posts). 

La regresión lineal es la forma más simple de aplicar esta idea.

### 6. Visualización

Ver los datos y la línea de regresión en un gráfico nos ayuda a entender cómo el modelo funciona.

- Los puntos azules representan los datos reales (por ejemplo, casas con sus tamaños y precios).
- La línea roja es la predicción del modelo, que intenta ajustarse lo mejor posible a esos puntos.

Al comparar la línea con los puntos, podemos ver si el modelo está haciendo buenas predicciones o si se aleja mucho de los datos reales. Esta visualización es una herramienta práctica para interpretar y validar el modelo.

```python
import matplotlib.pyplot as plt

def plot_regression(x, y, w, b):
    plt.scatter(x, y, label="Datos")
    plt.plot(x, w * x + b, color="red", label="Regresión")
    plt.legend()
    plt.show()

```

## 7. Relación con modelos complejos

- Una red neuronal con una sola capa lineal (sin activación) es básicamente una regresión lineal.  
- La regresión logística es similar, pero aplica una función sigmoide al resultado para resolver problemas de clasificación (decidir entre categorías).  
- Técnicas como Ridge (L2) y Lasso (L1) son formas de regularización que ayudan a evitar que el modelo se ajuste demasiado a los datos de entrenamiento (sobreajuste). Estas técnicas se usan tanto en regresión lineal como en redes neuronales más complejas.


## 8. Conclusión

En este artículo hemos cubierto:

- La teoría y matemáticas que fundamentan la regresión lineal.
- Dos formas de resolverla: solución analítica y descenso del gradiente.
- Implementaciones prácticas en Python.
- Visualización para entender el modelo.
- Conexiones clave con redes neuronales y su importancia en IA.

La regresión lineal es un conocimiento esencial y un excelente punto de partida para avanzar hacia modelos más complejos en inteligencia artificial.

