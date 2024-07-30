---
title: Explorando el Código Fuente del Apollo 11 - Un Viaje al Corazón de la Computación Espacial
date: 2024-07-19 18:30:00 +0800
categories: [Tecnología, Articulos]
tags: [Ciberseguridad, CrowdStrike, Windows, BlueScreenOfDeath, Microsoft, Tecnología, IncidentesIT]
image:
  path: /assets/img/post/apollo11/1.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Imagina tener que guiar a un módulo espacial a través del vasto vacío del espacio y aterrizarlo con precisión en la superficie lunar, todo mientras se trabaja con un ordenador con menos potencia que un moderno teléfono móvil. Este fue el desafío enfrendato por el Apollo Guidance Computer (AGC) durante la histórica misión Apollo 11, la cual llevo al primer humano a la superficie lunar.

En este post, vamos a sumergirnos en el fascinante mundo del software que hizo posible el alunizaje. Desde las intrincadas rutinas de auto-verificación hasta las complejas ecuaciones de guía lunar, descubrirás como el código detrás del AGC no solo llevó al hombre a la Luna, sino que tambíen marcó un hito en la historia de la computación espacial.

<!-- ![Foto de la misión Apollo](//) -->

## Historia del AGC

Para entender el impacto del Apollo Guidance Computer (AGC) en la misión Apollo 11, primero debemos explorar su historia y su contexto.

<!-- ![Foto del Apollo Guidance Computer (AGC)](//) -->

El AGC, desarrollado en el MIT Instrumentation Laboratory bajo la dirección de Charles Stark Draper, fue un avance revolucionario en la computacion espacial. Su diseño compacto y robusto estaba destinado a soportar las rigurosas condiciones del espacio y a facilitar las complejas maniobras necesarias para una misión lunar exitosa.

Uno de los nombres más destacados en el desarrollo del AGC es Margaret Hamilton, quien lideró el equipo de ingenieros de software. Su enfoque meticuloso en la ingeniería de Software y su visión pionera no solo garantizaron el éxito de la misión Apollo 11, sino que también establecieron muchos de los principio de programación que hoy consideramos fundamentales.

<!-- ![Margaret Hamilton trabajando en el AGC](//) -->

## Hardware del AGC

El Apollo Guidance Computer (AGC) fue una pieza fundamental del éxito de las misiones Apollo, destacando por sus innovaciones en la ingeniería de hardware y software en la época.

### Características técnicas del AGC

- **Procesador:** 2.048 MHz

  <!-- ![Diagrama del procesador del AGC](//) -->

  - **Ciclo de Instrucción:** El AGC operaba a una frecuencia de 2.048 MHz, lo que permitía un ciclo de instrucción de aproximadamente 11.72 microsegundos. Cada instrucción del conjunto de instrucciones del AGC se ejecutaba en uno o más de estos ciclos.
  - **Arquitectura:** El procesador del AGC tenía una arquitectura de 16 bits, utilizando una aquitectura word de 15 bits más un bit de paridad. El conjunto de instrucciones incluía operaciones básicas aritmétricas, lógicas, de control de flujo y de manipulación de datos.
  - **Pipelining:** Aunque no contaba con pipelining en el sentido moderno, el diseño eficiente del AGC permitía un flujo constante de datos e instrucciones a través del sistema.

- **Memoria Ram (Random Access Memory):** 2 KB
  
  <!-- ![Imagen de núcleos magnéticos de RAM](//) -->

  - **Tecnología:** La RAM del AGC estaba basada en núcleos magnéticos (magnetic core memory), una tecnología robusta y no volátil que permitía mantener el estado incluso en caso de pérdida de energía.
  - **Función:** La RAM se utilizaba principalmente para almacenar variables temporales y datos intermedios necesarios para los cálculos en tiempo real. También se usaba para almacenar el estado del programa y los datos de navegación.

- **ROM (Read-Only Memory):** 36 KB

  <!-- ![Diagrama del Core Rope Memory](//) -->

  - **Tecnología:** La ROM utilizaba la tecnología Core Rope Memory, donde los bits de datos se almacenaban en núcleos magnéticos mediante cables trenzados. Esta tecnología ofrecía alta densidad de almacenamiento y era extremadamente resistente a fallos.
  - **Función:** La ROM contenía el código de programa esencial para todas las fases de la misión, incluyendo rutinas de navegación, control y manejo de datos. La estrcutura del software era modular, permitiendo actualizaciones y modificaciones sin alterar el código base.

### Componentes Clave

- **Teclado y Pantalla (DSKY - Display and Keyboard):**

  <!-- ![Imagen del DSKY](//) -->

  - **Interfaz de Usuario:** El DSKY consistía en una pantalla de 7 segmentos para mostrar datos numéricos y una serie de luces de estado para indicar diferentes modos y estados del sistema. El teclado permitía la entrada de comandos mediante un conjunto de teclas numéricas y de función.
  - **Comunicaciones:** El DSKY se comunicaba con el AGC mediante un bus de datos dedicado, permitiendo la entrada y salida rápida y eficiente de información.

- **Unidad de Navegación Inercial (IMU - Inertial Measurement Unit):**
  
  <!-- ![Diagrama de la Unidad de Navegación Inercial (IMU)](//) -->

  - **Componentes:** El IMU contenía giroscopios y acelerómetros que proporcionaban datos sobre la orientación y aceleración de la nave. Estos datos eran críticos para la navegación y el control de la trayectoria.
  - **Integración:** El AGC integraba los datos del IMU con sus cálculos de navegación para determinar la posición y orientación precisas del módulo lunar y el módulo de comando.
  
- **Core Rope Memory:**

  - **Contrucción:** La Core Rope Memory es un tipo de memoria ROM, que estaba construida mediante el entrelazado de hilos a través de núcleos magnéticos. Un hilo que pasaba a través de un núcleo representaba un bit "1", mientras que uno que pasaba alrededor del núcleo respresentaba un bit "0".
  - **Ventajas:** Esta tecnología proporcionaba una alta densidad de almacenamiento y era muy resistente a las perturbaciones electromagnéticas, crucial para el entorno espacial.

### Innovaciones y Desafíos Técnicos

- **Código:**

  - **Optimización de Código:** Debido a las limitaciones de memoria y procesamiento, los ingenieros tuvieron que optimizar cada línea de código. Esto incluyó el uso eficiente de ciclos de máquina y la minimización de operaciones innecesarias.
  - **Subrutinas y Modularidad:** El software estaba altamente modularizado, permitiendo la reutilización de código y facilitando las pruebas y la verificación.

- **Robustez y Fiabilidad:**

  - **Redundancia:** El AGC estaba diseñado con una redundancia en mente. Tenía múltiples niveles de verificación y recuperación de fallos para garantizar que los errores pudieran ser detectados y corregidos sin poner en peligro la misión.
  - **Pruebas Exhaustivas:** Se realizaron pruebas extensivas de cada componente y subrutina del AGC para asegurar su correcto funcionamiento bajo todas las condiciones previstas.

- **Interacción Humana:**
  
  - **Simplicidad y Eficiencia:** El diseño del DSKY tenía que ser intuitivo para los astronautas, permitiéndoles ingresar comandos y recibir información crítica rápidamente y con mínima posibilida de error.
  - **Entrenamiento:** Los astronautas recibieron un entrenamiento extensivo en el uso del DSKY y la interpretación de los datos mostrados, asegurando que pudieran tomar decisiones informadas durante la misión.

El Apollo Guidance Computer no solo fue un logro técnico impresionante en su época, sino que también estableció muchos de los principios y prácticas que siguen siendo relevantes en la ingeniería de sistemas críticos y la computación en tiempo real.

## Abort Guidance System (AGS)

El Abort Guidance System (AGS) del Módulo Lunar del programa Apollo fue diseñado específicamente para proporcionar capacidades de navegación y control en caso de un fallo del sistema principal de guiado, el AGC. A continuación, algunos aspectos importantes a tener en cuenta sobre el AGS

<!-- ![Foto del AGS](//) -->

### Arquitectura y Diseño del AGS

- **Computadora de Guiado (Abort Electronics Assembly, AEA)**

  <!-- ![Abort Electronics Assembly (AEA)](//) -->

  - **Procesador:** Basado en tecnología de lógica discreta, menos avanzada que el procesador del AGC.
  - **RAM:** Aproximadamente 2 KB de memoria volátil, utilzada para operaciones temorales.
  - **ROM:** 4 KB de memoria de núcleo de ferrita para almacenar el software de emergencia.
  - **Instrucciones:** Capaz de ejecutar un conjunto más limitado de instrucciones comparado con el AGC, adeacuado para su propósito específico de manejo de abortos y emergencias.

- **Unidad de Medición Inercial (IMU)**
  
  <!-- ![Unidad de Medición Inercial (IMU)](//) -->

  - **Descripción:** La IMU del AGS era una versión simplificada y menos precisa que la utilzada por el AGC.
  - **Giroscopio:** Menos precisos pero suficientes para las operaciones de emergencia.
  - **Acelerómetro:** Proporcionaba mediciones básicas de aceleración para la navegación de respaldo.
  
- **Interfaces de Entrada/Salida**
  
  <!-- ![DSKY Simplificado del AGS](//) -->

  - **DSKY Simplificado:** La pantalla proporcionaba información básica de estado y navegación, el teclado permitía la entrada de comandos esenciales para la operación del AGS.
  
- **Sensores y Actuadores**
  
  <!-- ![Sensores y Actuadores](//) -->

  - **Sensores de Presión y Temperatura:** Utilzados para monitorear condiciones críticas del Modulo Lunar.
  - **Actuadores de Control:** Permitían el ajuste de la orientación y la ejecución de maniobras de emergencia.
  
- **Algoritmos de Navegación**

  <!-- ![Diagrama de Algoritmos de Navegación](//) -->

  - **Ascent Guidance Algorithm:** Diseñado para calcular y ejecutar una trayectoria de ascenso segura desde la superficie lunar en caso de aborto.
  - **Abort Modes:** Incluía modos específicos para diferentes fases del descenso y ascenso, cada uno con rutas de navegación predefinidad para maximizar la probabilidad de éxito.

- **Componentes de Redundancia:**

  - **Baterías Independientes:** El AGS tenía su propio sistema de energía para asegurar que funcionara incluso si el sistema principal fallaba.
  - **Circuitos Redundantes:** Diseñados para tolerancia a fallos, permitiendo que el sistema continúe opernado en condiciones adversas.

### Innovaciones y Capacidades Específicas

- **Navegación de Respaldo**

  <!-- ![Diagrama de Precisión del AGS](//) -->

  - **Precisión:** Aunque menos preciso que el AGC, el AGS podía proporcionar suficiente precisión para una operación de emergencia.
  - **Fiabilidad:** Diseñado para ser extremadamente fiable y fácil de usar en situaciones de alta tensión.

- **Manejo de Emergencias**

  <!-- ![Procedimientos de Emergencia](//) -->

  - **Abort Modes:** Capacidad para iniciar y gestionar diferentes modos de aborto basados en la fase de la misión y las condiciones presentes.
  - **Ascent Capability:** Proporcionaba una ruta de ascenso segura en caso de un aborto durante el descenso o en la superficie lunar.

- **Diagnóstico y Auto-Pruebas**

  <!-- ![Diagrama de Diagnóstico y Auto-Pruebas](//) -->

  - **Monitoreo Continuo:** Se realizaban auto-pruebas constantes para verificar su estado operativo.
  - **Alertas y Diagnósticos:** Proporcionaba alertas y diagnósticos simples para informar a los astronautas sobre el estado del sistema y posibles fallos.

El Abort Guidance System (AGS) fue un componente crucial del Módulo Lunar, diseñado para proporcionar un respaldo robusto y fiable en caso de fallo del sistema principal de guiado, el AGC.

Su diseño simplificado, junto con su capacidad para manejar situaciones de emergencia, lo convirtió en una herramienta vital para la seguridad de las misiones Apollo. Aunque menos potente y preciso que el AGC, el AGS desempeñó un papel esencial en la estrategia de redundancia y gestión de riesgos del programa Apollo.

[https://ntrs.nasa.gov/](https://ntrs.nasa.gov/)
[The Apollo Guidance Computer: Architecture and Operation](https://nss.org/book-review-the-apollo-guidance-computer-architecture-and-operation/)
[https://github.com/chrislgarry/Apollo-11](https://github.com/chrislgarry/Apollo-11)
[http://www.ibiblio.org/apollo/#gsc.tab=0](http://www.ibiblio.org/apollo/#gsc.tab=0)

## UC
