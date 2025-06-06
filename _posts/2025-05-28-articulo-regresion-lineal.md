---
title: Regresión Lineal - Teoría, implementación y análisis
date: 2025-06-05 12:30:00 +0800
categories: [Inteligencia Artificial, Machine Learning]
tags: [aprendizaje, inteligencia, machine, regresion-lineal, numpy, python]
image:
  path: /assets/img/post/regresion-lineal/regresion_lineal_curva.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

La `regresión lineal` es uno de los **modelos matemáticos más fundamentales** en el `aprendizaje automático` de modelos de IA, a pesar de su simplicidad, es una técnica **muy importante** para comprender las **relaciones entre las variables** y sirve como base para entender y hacer funcionar modelos más complejos.

En este artículo veremos diferentes **modelos y técnicas** relacionadas con el tema central del post:

### Aplicaciones prácticas

- **Predicción de precios** (viviendas, acciones, etc.)
- **Estimación de tendencias**
- **Relación entre variables** en estudios científicos

---

## ¿Por qué es esencial saber lo que es la regresión lineal para empezar con la IA?

Aprender lo que es y la esencia de la `regresión lineal` es uno de los **primeros pasos recomendados** para iniciarse en el mundo de la `inteligencia artificial` y `machine learning`, veamos por qué desde cuatro perspectivas clave:

### ✅ 1. Establece las bases del aprendizaje supervisado:

- La `regresión lineal` es el modelo **más simple de aprendizaje supervisado**.
- Permite entender cómo un modelo **aprende a reconocer patrones** a partir de datos **etiquetados**.
- Introduce el concepto de `función de pérdida`, que mide qué tan bien el modelo está aprendiendo.
- Enseña la idea de `generalización` = **aplicar el aprendizaje a datos nuevos**.

### ✅ 2. Introduce conceptos matemáticos claves:

- Define **funciones matemáticas** para **medir errores** (como el `Error Cuadrático Medio`, `MSE`).
- Permite estudiar **cálculo básico como derivadas** y `gradiente descendente` para **optimizar modelos**.
- Facilita el uso de **álgebra lineal** `(vectores, matrices)`, clave en modelos complejos.

### ✅ 3. Conexión directa con redes neuronales:

- Imagina una **red neuronal** como varias `“capas”` que procesan datos, si solo tienes una capa que hace una suma y multiplicación simple (sin cosas complicadas), eso es justo una `regresión lineal`.
- La `regresión logística` es casi igual, pero después de esa suma, le aplicamos una función que convierte el resultado en una **probabilidad** (`sigmoide`).
- Algunas ideas para evitar que el modelo se confunda con los datos (como `Ridge` y `Lasso`) vienen de la `regresión lineal` y también se usan en **redes neuronales** más grandes.

> Todo esto lo veremos en futuros artículos :)

### ✅ 4. Aplicable a la práctica desde el primer día:

- Puedes aplicar `regresión lineal` a datasets reales rápidamente.
- Ejemplos típicos: predicción de precios de vivienda, estimación salarial o análisis de ventas.
- Sirve para **validar ideas y generar prototipos ágiles** en proyectos de IA.

---

## Planteamiento del problema

Dado un conjunto de datos con una **variable explicativa** $X$ y una **variable objetivo** $Y$, queremos encontrar una `relación lineal` que nos permita predecir el valor de $Y$ a partir de $X$ mediante una fórmula sencilla:

$\begin{equation} \hat{y} = wx + b \end{equation}$

Donde:

- **$w$ (pendiente o coeficiente):** representa cuánto **cambia la variable objetivo** $Y$ cuando la **variable explicativa** $X$ **aumenta** en una unidad.  
  - Por ejemplo, si $w = 2$, significa que por cada **aumento de 1** en $X$, $Y$ aumentará aproximadamente 2.

- **$b$ (término independiente o bias):** es el valor que toma $Y$ cuando $X = 0$.  
  - Es el punto donde la `línea de regresión` cruza el **eje vertical** (eje $Y$) y ajusta la línea para que se acerque **mejor a los datos**.

- **$\hat{y}$ (predicción):** es el valor estimado o calculado de $Y$ para un valor dado de $X$, es **la salida** que el modelo nos da para hacer **predicciones**.

### Comprensión práctica

Imagina que quieres predecir el **precio de una casa según su tamaño** (en metros cuadrados). En este caso:

- **$w$** es el coeficiente que indica **cuánto aumenta el precio por cada metro cuadrado** adicional.  
  - Por ejemplo, si $w = 1500$, significa que cada metro cuadrado extra aumenta el precio en 1500 €.

- **$b$** es el **precio base**, es decir, el **precio estimado de una casa con tamaño 0** (conceptualmente el punto de partida).  
  - Por ejemplo, si $b = 50000$, ese sería el costo mínimo o base de la casa sin importar el tamaño.

- **$\hat{y}$** es la predicción del precio para un tamaño específico.

Por ejemplo, si tienes una casa de **100 metros cuadrados**:

$\begin{equation}
\hat{y} = 1500 \cdot 100 + 50000 = 150000 + 50000 = 200000
\end{equation}$

El modelo predice que el **precio aproximado será de 200000 €**.

### Relación con redes neuronales

Este modelo de regresión lineal puede verse como una **neurona muy simple** en una red neuronal, donde:

- La `entrada` es el valor $x$ (tamaño de la casa).
- El `peso` $w$ multiplica esa entrada para ajustar su importancia.
- El `sesgo` $b$ se suma para desplazar la salida y mejorar el ajuste.
- No hay `función de activación`, por lo que la salida es **simplemente una combinación lineal**.

Es decir, esta neurona calcula directamente **el valor predicho sin transformarlo**, que es exactamente lo que hace la `regresión lineal`.

Esta comprensión ayuda a ver que **modelos más complejos**, como redes neuronales profundas, están construidos a partir de **muchas capas y neuronas similares**, pero con `funciones de activación` que les permiten aprender relaciones no lineales.

---

## Función de pérdida: Error Cuadrático Medio (MSE)

La `función de pérdida` nos dice qué tan bien o mal está funcionando nuestro modelo, comparando las predicciones con los valores reales.

Es simple, necesitamos saber si nuestras predicciones se **acercan a la realidad** o son un **completo disparate** y con esa información poder mejorar la **accuracy** del modelo.

Para `regresión lineal` usamos una función llamada `Error Cuadrático Medio (MSE)`, que se calcula así:

$\begin{equation}
MSE = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2
\end{equation}$

### Explicación

- Comparamos cada valor real $y_i$ con su predicción $\hat{y}_i$.
- Restamos para ver **la diferencia** (error).
- **Elevamos esa diferencia al cuadrado** para que los errores **más grandes pesen más**.
- Sacamos **el promedio** de todos esos errores cuadrados.

Así, si nuestras predicciones están muy lejos de los valores reales, el `MSE` será grande; si están cerca, será pequeño.

### ¿Por qué es útil?

El objetivo del modelo es hacer el `MSE` lo más **pequeño posible**, es decir, que las predicciones sean lo más **cercanas a los valores reales**.

### Aplicación práctica:

Volviendo con el ejemplo de la casa, imagina que queremos de nuevo predecir el precio de una casa según su tamaño.

- Supongamos que el tamaño de la casa es de **100 metros cuadrados**.
- Nuestro modelo predice que el **precio será 150000 €**.
- Pero el **precio real es 160000 €**.

El **error** es la **diferencia entre el precio real y el predicho**:

$\begin{equation}
160{,}000 - 150{,}000 = 10{,}000
\end{equation}$

Elevamos este error al cuadrado para que **errores grandes tengan más peso**:

$\begin{equation}
10{,}000^2 = 100{,}000{,}000
\end{equation}$

Si hacemos esto para muchas casas y luego sacamos el promedio, obtenemos el `MSE`.

Cuanto más **pequeño sea ese número**, mejor es nuestro modelo porque significa que nuestras predicciones están **cerca de los precios reales**.

Por eso, en la práctica, al entrenar un modelo, intentamos **minimizar** el `MSE` para que nuestras predicciones sean **lo más exactas posible**.

### Relación con redes neuronales

En las **redes neuronales**, esta `función de pérdida` también se usa para saber qué tan bien están funcionando y para **ajustar los números** que controlan el modelo (los pesos y sesgos).  

Minimizar el `MSE` es como decir *“quiero que mi modelo cometa el menor error posible”*.

---

## Solución analítica (closed-form)

Cuando tenemos nuestros datos, podemos calcular **directamente la pendiente** $w$ y el **punto de intersección** $b$ de la mejor línea que se ajusta a esos datos sin necesidad de hacer muchas pruebas.

Las fórmulas son:

$\begin{equation}
w = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2}
\end{equation}$

$\begin{equation}
b = \bar{y} - w \bar{x}
\end{equation}$


Donde $\bar{x}$ y $\bar{y}$ son los promedios de todos los tamaños de casas y precios.

### Intuición práctica

Este método encuentra **en un solo paso** la línea que **mejor se ajusta a todos los datos**, minimizando la suma de los errores al cuadrado.

### Aplicación práctica con el ejemplo de la casa

Imagina que tienes datos de varias casas con sus tamaños y precios:

| Tamaño (m²) | Precio (€) |
|-------------|------------|
| 80          | 120000    |
| 100         | 150000    |
| 120         | 180000    |

El método calcula cuánto **cambia el precio promedio** cuando cambia el tamaño promedio (eso es $w$), y cuál es el **precio base cuando el tamaño es cero** (eso es $b$).

Con esos valores, puedes predecir el precio de una casa nueva cxon solo saber su tamaño**.

### Implementación en Python

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

Esto significa que por cada metro cuadrado adicional en el tamaño de la casa, el precio **sube en 1500 €**.

Por ejemplo, si una casa es 10 m² más grande, el precio aumentará aproximadamente 15000 € (10 × 1500).

**Intercepto (b) = 0**

Este valor es el **precio base cuando el tamaño de la casa es 0 m²**. En este caso es 0 euros, lo cual tiene sentido porque una casa sin tamaño no tendría precio.

En otros casos, el `intercepto` puede ser diferente y ajusta la `línea de predicción`.

Entonces, la fórmula de predicción sería:

$\begin{equation}
\hat{y} = 1500 \cdot x + 0
\end{equation}$

Entonces, si quieres predecir el precio de una casa de 90 m², solo tienes que multiplicar:

$\begin{equation}
\hat{y} = 1500 \cdot 90 = 135{,}000 \, \text{euros}
\end{equation}$

## 5. Descenso del gradiente

Cuando tenemos **muchos datos o varias variables**, usar la fórmula directa puede ser complicado o muy lento, entonces usamos el `descenso del gradiente`, que **ajusta poco a poco los parámetros** para mejorar la predicción.

Este método nos sirve para ajustar poco a poco los parámetros `(pendiente y sesgo)` de la línea hasta que el error sea el mínimo posible.

### Derivadas parciales

Estas fórmulas nos dicen cómo cambiar `w` y `b` para mejorar el modelo, **calculando la pendiente del error**:

$\begin{equation}
\frac{\partial \text{MSE}}{\partial w} = -\frac{2}{n} \sum x_i (y_i - \hat{y}_i)
\end{equation}$

$\begin{equation}
\frac{\partial \text{MSE}}{\partial b} = -\frac{2}{n} \sum (y_i - \hat{y}_i)
\end{equation}$


### Algoritmo paso a paso

1. Empezamos con `w` y `b` iguales a cero (o cualquier valor).
2. **Calculamos las prediccione**s con esos valores.
3. Medimos el **error entre las predicciones y los valores reales**.
4. Calculamos cuánto hay que cambiar `w` y `b` para reducir ese error `(gradientes)`.
5. Actualizamos `w` y `b` un poco hacia la **dirección que reduce el error**.
6. Repetimos muchas veces hasta que el error **sea mínimo o cambie poco**.

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

- $w$ = 2.18, significa que por **cada metro cuadrado adicional que tenga la casa**, el precio sube **aproximadamente 2.18 miles de €**

- $b$ = 10.24 miles de € es el valor **de precio base cuando la casa tiene 0 metros** cuadrados, aunque en la vida real no tiene sentido una casa con 0 m², este valor sirve como **punto de partida para la `línea de regresión`**.

La fórmula para predecir el precio es:

$\begin{equation}
\text{precio} = 2.18 \times m^2 + 10.24
\end{equation}
$

Entonces, si una casa tiene 50 m², su precio estimado sería:

$\begin{equation}
2.18 \times 50 + 10.24 = 119.24 \text{ miles de €}
\end{equation}$

### Aplicación práctica (ejemplo del precio de la casa)

Si empezamos con $w = 0$ y $b = 0$, el modelo **predice siempre 0 euros sin importar el tamaño**, usando `descenso del gradiente`, cada paso **ajusta** el precio base $b$ y cuánto aumenta el precio por metro cuadrado $w$ para acercarse al precio real de las casas.

### Relación con redes neuronales

Este proceso de **ajustar los parámetros paso a paso** es exactamente lo que hacen las redes neuronales cuando se entrenan con `retropropagación` 

> También lo explicaremos en futuros posts. 

La `regresión lineal` es la forma **más simple** de aplicar esta idea.

### 6. Visualización

Ver los datos y la línea de regresión en un gráfico nos ayuda a **entender cómo el modelo funciona**.

Al comparar la línea con los puntos, podemos ver si el modelo está haciendo **buenas predicciones** o si se **aleja mucho de los datos reales**, esta visualización es una herramienta práctica para interpretar y validar el modelo.

```python
x = np.array([45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100, 110, 120, 130, 140, 150, 160, 170, 180])
y = np.array([90, 95, 100, 115, 120, 125, 135, 140, 150, 160, 165, 175, 190, 210, 220, 240, 255, 265, 280, 300])

N = len(x)
sum_x = np.sum(x)
sum_y = np.sum(y)
sum_xy = np.sum(x * y)
sum_x2 = np.sum(x ** 2)

w = (N * sum_xy - sum_x * sum_y) / (N * sum_x2 - sum_x**2)
b = (sum_y / N) - w * (sum_x / N)

def predict(x):
    return w * x + b

plt.scatter(x, y, label="Datos")
plt.plot(x, predict(x), color="red", label=f"Regresión (w={w:.2f}, b={b:.2f})")
plt.xlabel("m2")
plt.ylabel("Precio (miles $)")
plt.legend()
plt.show()

```

### Explicación del código y gráfica

![plot](/assets/img/post/regresion-lineal/1.png)

- **Puntos azules (scatter)**: representan los datos reales, casas con su tamaño y precio.

- **Línea roja (plot)**: la recta que mejor ajusta esos datos según la `regresión lineal`.

- **La pendiente $w$** nos dice cuánto sube el precio por cada m² extra.

- **La intersección $b$** es el precio estimado para una casa con tamaño 0.

- La gráfica muestra cómo el modelo **predice el precio** según el tamaño de la casa.

## 7. Relación con modelos complejos

- Una red neuronal con una sola **capa lineal** es básicamente una `regresión lineal`.

- La **regresión logística** es similar, pero aplica una función `sigmoide` al resultado para resolver problemas de clasificación (decidir entre categorías).

- Técnicas como `Ridge (L2)` y `Lasso (L1)` son formas de **regularización que ayudan a evitar que el modelo se ajuste demasiado** a los datos de **entrenamiento** (`sobreajuste`), estas técnicas se usan tanto en `regresión lineal` como en redes neuronales más complejas.

> Veremos lo que es una función sigmoide en un futuro

## 8. Conclusión

En este artículo hemos cubierto:

- La **teoría y matemáticas** que fundamentan la `regresión lineal`.
- Dos formas de resolverla: `solución analítica` y `descenso del gradiente`.
- **Implementaciones prácticas** en `Python`.
- **Visualización** para entender el modelo.
- Conexiones clave con **redes neuronales** y su **importancia en IA**.

_Espero que os haya gustado leer el artículo tanto como a mi hacerlo. ¡Gracias por leer!_
