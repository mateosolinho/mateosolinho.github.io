---
title:
date:
categories:
tags:
image:
  path:
  lqip:
---

# =================== UC ===================

---

## 1. Introducción al agrupamiento no supervisado en visión por computador: contexto histórico y funcional

### 1.1. Breve historia del análisis de imágenes sin etiquetas

Durante las décadas de los 80 y 90, la visión por ordenador se basaba sobre todo en descriptores diseñados a mano y en heurísticas bastante específicas, modelos como SIFT (Scale-Invariant Feature Transform) y HOG (Histogram of Oriented Gradients) eran clave para capturar detalles locales o patrones generales en las imágenes.

Estos descriptores dan la posibilidad del tratamiento de tareas tales como el reconocimiento de objetos o clasificación, si bien con algunas limitaciones, eran débiles contra cambios de iluminación, posición del objeto o escala, lo que los hacía muy sensibles a fallos.

En cuanto al agrupamiento, se basaban anteriormente en métricas simples tales como la distancia euclidiana o la correlación directa de estos vectores de características, K-means o clustering jerárquico (agglomerative clustering) eran algoritmos más populares en ese momento.

Esto cambió a partir de 2012, con la aparición del deep learning y las Redes Neuronales Convolucionales (CNNs), que permitieron extraer representaciones mucho más ricas, jerárquicas y con un significado más profundo a nivel semántico.

A medida que los datos no etiquetados comenzaron a superar a los conjuntos anotados manualmente, aparecieron enfoques de aprendizaje no supervisado y auto-supervisado, estas técnicas abrieron nuevas posibilidades para aprovechar al máximo esos enormes volúmenes de datos sin necesidad de intervención humana directa.

### 1.2. Funcionalidad y retos actuales

El crecimiento exponencial de datos visuales en redes sociales, sistemas médicos, satélites, etc., hace inviable la anotación manual.

El agrupamiento no supervisado permite descubrir patrones emergentes, estructurar datos para análisis posteriores y acelerar tareas de clasificación o recuperación de imágenes.

**Retos técnicos** incluyen: alta dimensionalidad, escalabilidad, ruido y heterogeneidad en los datos, y evaluación sin etiquetas.

---

## 2. Embeddings visuales: definición, modelos y propiedades matemáticas

### 2.1. ¿Para qué y por qué se crean los embeddings visuales?

Los **embeddings visuales** se diseñan para transformar imágenes en vectores que capturen su **información semántica** de forma compacta y numéricamente manipulable. Este paso es esencial porque los algoritmos tradicionales de aprendizaje automático y análisis no pueden operar directamente sobre datos de alta dimensión sin estructura explícita, como píxeles crudos.

#### Principales objetivos:

- **Reducción de dimensionalidad**: pasar de una imagen con miles o millones de píxeles a una representación en ℝᵈ (por ejemplo, d = 128, 512 o 1024) que concentre la información discriminativa.
- **Comparación eficiente**: al representar imágenes como vectores en un espacio métrico, se facilita la medición de similitud mediante distancias estándar (euclidiana, coseno, etc.).
- **Generalización semántica**: embeddings bien entrenados agrupan imágenes con significado similar aunque varíen en color, pose, escala, fondo o ruido.
- **Transferencia de conocimiento**: los embeddings pueden ser reutilizados en múltiples tareas (clasificación, búsqueda, detección, clustering) sin necesidad de reentrenar el modelo base, gracias a su capacidad de abstracción.
- **Facilitar tareas no supervisadas**: en clustering, recuperación de imágenes o detección de anomalías, disponer de una representación numérica coherente y estructurada es clave para obtener buenos resultados sin etiquetas.

#### Motivación técnica:

Los espacios de embedding actúan como **espacios latentes** (representación matemática comprimida donde los datos complejos (como imágenes) se transforman en vectores que capturan sus características más relevantes [](https://www.ibm.com/think/topics/latent-space)) donde se pueden aplicar técnicas matemáticas estándar (álgebra lineal, estadística, geometría) para modelar relaciones complejas entre imágenes. En vez de tratar con millones de dimensiones (píxeles), se opera en espacios reducidos pero informativamente ricos.

Así, los embeddings permiten **eficiencia computacional**, además de que **habilitan inteligencia visual**, al capturar conceptos abstractos como "perro", "paisaje urbano" o "estructura anatómica" en vectores que se pueden comparar, agrupar o clasificar.

### 2.2. Qué es un embedding visual

Un embedding es una función **f: I → ℝᵈ** que transforma una imagen **I ∈ 𝓘** en un vector numérico en un espacio d-dimensional.

Idealmente, esta función debe preservar la similitud semántica, es decir, imágenes con contenido similar deben tener embeddings cercanos según alguna métrica (coseno, euclidiana).

El espacio ℝᵈ suele ser continuo, denso y estructurado, donde la distancia refleja relaciones semánticas.

### 2.3. Arquitecturas para extracción de embeddings

- **CNN clásicas**: ResNet, DenseNet, EfficientNet, donde las capas finales actúan como extractores de características globales.
- **Transformers para visión (ViT)**: procesan la imagen como un conjunto de parches, usando atención para modelar relaciones a larga distancia.
- **Modelos multimodales (CLIP)**: entrenados con aprendizaje contrastivo entre imágenes y texto, generando espacios semánticos alineados entre ambos dominios.
- **Auto-supervisados**: SimCLR, BYOL, DINO que aprenden invariancias sin etiquetas, usando estrategias como la comparación entre vistas aumentadas.

### 2.4. Propiedades matemáticas relevantes

- **Normalización**: Embeddings suelen ser normalizados (L2 norm) para que la distancia coseno equivalga a la similitud angular.
- **Linealidad y estructuras geométricas**: operaciones vectoriales pueden tener significado semántico (e.g., interpolaciones).
- **Separabilidad y margen**: modelos buscan maximizar márgenes entre clases.
- **Distribución**: la alta dimensionalidad provoca problemas de concentración de medida; técnicas de reducción dimensional buscan mitigarlo.

---

## 3. Proyección a espacios de menor dimensión para análisis y visualización

### 3.1. Motivaciones

- Facilitar la visualización 2D/3D para inspección humana.
- Mejorar la eficiencia y calidad del agrupamiento.
- Comprender mejor la estructura latente en el conjunto de datos.

### 3.2. Métodos clásicos y avanzados

- **PCA**: proyección lineal basada en autovalores; rápido y determinista.
- **t-SNE**: mantiene estructura local, complejo para datasets grandes.
- **UMAP**: mantiene estructura local y global, escalable, permite inferencia en nuevos datos.
- **Autoencoders y VAE**: codificaciones comprimidas; VAE añade modelo probabilístico generativo.
- **Isomap, LLE**: conservan distancias geodésicas y relaciones locales.

### 3.3. Consideraciones prácticas

- Elección de número de dimensiones (ej. 50 para clustering, 2-3 para visualización).
- Balance entre estructura y eficiencia.
- Manejo de outliers y datos atípicos.

---

## 4. Algoritmos de agrupamiento no supervisado: fundamentos y variantes

### 4.1. Clustering basado en particiones: K-Means y variantes

- Minimiza suma de distancias al centroide.
- Rápido pero requiere definir **k**, asume clusters convexos.
- Variantes: K-Medoids, MiniBatch K-Means.

### 4.2. Clustering jerárquico

- Basado en distancias entre puntos o grupos (dendrogramas).
- Agregativo (bottom-up) o divisivo (top-down).
- Escasa escalabilidad, sensible a ruido.

### 4.3. Clustering basado en densidad: DBSCAN y HDBSCAN

- **DBSCAN**: define regiones densas como clusters, detecta ruido.
- **HDBSCAN**: generaliza DBSCAN, maneja densidades variables y jerarquía.

### 4.4. Métodos basados en grafos y espectrales

- Construcción de grafo de similitud (k-NN).
- Usa autovectores de la matriz laplaciana.
- Costoso computacionalmente.

### 4.5. Métodos modernos y emergentes

- **Deep Clustering**: aprendizaje conjunto de representación y clusters (DEC, DeepCluster).
- Métodos con auto-supervisión y atención.
- Adaptativos a datos en flujo (online/incremental).

---

## 5. Preparación y organización de los resultados para interpretación y análisis

### 5.1. Etiquetado y asignación de grupos

- Asignación de etiquetas numéricas o categorías.
- Revisión de clusters pequeños o dispersos.

### 5.2. Visualización de agrupamientos

- Grids visuales para inspección.
- Visualización interactiva (Plotly, Bokeh, Streamlit).
- Mapas de calor y diagramas de distribución.

### 5.3. Análisis estadístico y métricas

- Tamaño, distribución de clusters.
- Métricas: silhouette score, Davies-Bouldin.
- Evaluación sin etiquetas: métricas internas.

### 5.4. Uso de resultados para tareas posteriores

- Etiquetado proxy.
- Filtrado y curación de datasets.
- Detección de anomalías.

---

## 6. Aspectos prácticos y consideraciones técnicas para sistemas a escala

### 6.1. Extracción eficiente de embeddings

- Uso de GPUs, batch processing.
- Preprocesamiento de imágenes.
- Paralelización y distribución.

### 6.2. Almacenamiento y gestión de vectores

- Formatos compactos: float16, int8.
- Bases vectoriales: FAISS, Annoy, ScaNN.
- Indexación y búsqueda de vecinos cercanos.

### 6.3. Escalabilidad en clustering

- Clustering incremental o jerárquico distribuido.
- Submuestreo, clustering aproximado.
- Actualización continua.

### 6.4. Evaluación y validación

- Datasets benchmark.
- Validación humana.
- Comparación con modelos supervisados.

---

## 7. Aplicaciones reales y casos de uso detallados

### 7.1. Organización y búsqueda en fototecas personales y empresariales

- Google Photos, Apple Photos.
- Identificación automática de personas, eventos, lugares.

### 7.2. Curación y limpieza de datasets para machine learning

- Eliminación de duplicados.
- Balanceo de clases, filtrado de ruido.

### 7.3. Análisis en medicina y biología

- Agrupamiento en histología.
- Pre-clasificación en radiología sin etiquetas.

### 7.4. Vigilancia satelital y monitoreo ambiental

- Detección de cambios en imágenes terrestres.
- Agrupamiento según cobertura o actividad.

### 7.5. Industria y comercio electrónico

- Organización de catálogos visuales.
- Recomendaciones por similitud.

---

## 8. Conclusiones y perspectivas futuras

- El aprendizaje no supervisado es clave para escalar el análisis visual.
- La combinación embeddings + reducción dimensional + clustering es una sinergia potente.
- Tendencias futuras: auto-supervisión, clustering end-to-end, adaptación continua.
- Desafíos: balance entre precisión, interpretabilidad y escalabilidad.
