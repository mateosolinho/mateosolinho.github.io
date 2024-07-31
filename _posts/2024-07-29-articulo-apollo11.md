---
title: Explorando el Código Fuente del Apollo 11 - Un Viaje al Corazón de la Computación Espacial
date: 2024-07-29 20:00:00 +0800
categories: [Tecnología, Articulos]
tags: [Apollo 11, AGC, AGS, Computación Espacial, RocketScience, Tecnología Espacial]
image:
  path: /assets/img/post/apollo11/2.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Imagina tener que guiar a un módulo espacial a través del vasto vacío del espacio y aterrizarlo con precisión en la superficie lunar, todo mientras se trabaja con un ordenador con menos potencia que un moderno teléfono móvil. Este fue el desafío enfrendato por el Apollo Guidance Computer (AGC) durante la histórica misión Apollo 11, la cual llevo al primer humano a la superficie lunar.

En este post, vamos a sumergirnos en el fascinante mundo del hardware y el software que hizo posible el alunizaje. Desde las intrincadas rutinas de auto-verificación hasta las complejas ecuaciones de guía lunar, descubrirás como el código detrás del AGC no solo llevó al hombre a la Luna, sino que tambíen marcó un hito en la historia de la computación espacial.

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

  <!-- ![Diagrama de Diagnóstico y Auto-Pruebas](//). -->

  - **Monitoreo Continuo:** Se realizaban auto-pruebas constantes para verificar su estado operativo.
  - **Alertas y Diagnósticos:** Proporcionaba alertas y diagnósticos simples para informar a los astronautas sobre el estado del sistema y posibles fallos.

El Abort Guidance System (AGS) fue un componente crucial del Módulo Lunar, diseñado para proporcionar un respaldo robusto y fiable en caso de fallo del sistema principal de guiado, el AGC.

Su diseño simplificado, junto con su capacidad para manejar situaciones de emergencia, lo convirtió en una herramienta vital para la seguridad de las misiones Apollo. Aunque menos potente y preciso que el AGC, el AGS desempeñó un papel esencial en la estrategia de redundancia y gestión de riesgos del programa Apollo.

## Codigo Fuente AGC

En las siguientes secciones, exploraremos varios archivos de código fuente, clave que ilustran cómo el AGC gestionaba tareas críticas como la navegación, el control de actitud, las maniobras automáticas y las rutinas de emergencia.

Analizaremos el propósito de cada archivo, su estructura y las innovaciones técnicas que hicieron posible la realización de estas funciones vitales en el entorno hostil del espacio.

- **AGC_BLOCK_TWO_SELF_CHECK.agc**

  - Descripción del Programa:

    - Fecha: 20 de diciembre de 1967
    - Nombre del Programa: SELF-CHECK
    - Sección: AGC BLOCK TWO SELF-CHECK
    - Subrutina de Ensamblaje: UTILITYM REV 25

  - Descripción Funcional:

    El programa tiene dos partes principales:

    - **SELF-CHECK**: Corre como un trabajo de prioridad cero sin núcleo establecido, como parte del bucle de respaldo inactivo.

    - **SHOW-BANKSUM**: Corre como un trabajo ejecutivo regular con su propio verbo de inicio.

  - Propósito:

    - **SELF-CHECK**: Verificar varias partes del ordenador según las opciones.

    - **SHOW-BANKSUM**: Mostrar la suma de cada banco, uno a la vez.

  - Opciones de SELF-CHECK:

    Controladas por números en el registro SMODE (NOUN 27):

    - 4: Memoria borrable.
    - 5: Memoria fija.
    - 1, 2, 3, 6, 7, 10: Todo lo anterior.
    - 0: Igual que +10 hasta que se detecta un error.
    - +0: Sin chequeo, pone la computadora en el bucle inactivo de respaldo.

  - Secuencia de Llamada:

    - Para llamar a SELF-CHECK: `V 21 N 27 E Número de opción E`
    - Para llamar a SHOW-BANKSUM:
    - `V 91 E`: Muestra el primer banco.
    - `V 33 E`: Procede y muestra el siguiente banco.

  - Modos de Salida

    - **SELF-CHECK**: Continúa indefinidamente a menos que se detecte un error.
    - **SHOW-BANKSUM**: Procede hasta que se termina (V 34 E).

  - Salida:

    - **SELF-CHECK**: Al detectar un error, carga la constante de alarma (01102) y enciende la luz de alarma.
      - Mostrar registros de fallas: `V 05 N 09 E`.
      - Información adicional: `V 05 N 08 E`.

    - **SHOW-BANKSUM**:

      - R1: Suma del banco.
      - R2: Número de banco.
      - R3: Palabra auxiliar.

  - Código de Ensamblador:

    - [AGC_BLOCK_TWO_SELF_CHECK.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/AGC_BLOCK_TWO_SELF_CHECK.agc)

### Archivos del Módulo Lunar (LM)

El software Luminary 099 es una pieza fundamental en la historia de la exploración espacial, ya que fue el software de vuelo utilizado en el Módulo Lunar (LM) durante las misiones Apollo

El término "Luminary" se refiere a algo que emite luz, como una estrella o una fuente de inspiración. Este nombre es particularmente apto para el software de guía y navegación del Modulo Lunar del AGC. De manera simbólica, "Luminary" también refleja la aspiración de la misión Apollo de iluminar el camino para la humanidad en la exploración espacial, convirtiéndose en una luz guía en la historia de la ciencia y la ingeniería.

El número "099" representa una versión específica del software. En el contexto del desarrollo de software, los números de versión son esenciales para llevar un registro de las distintas iteraciones del programa.

Luminary 099 es especialmente notable porque fue la versión utilizada durante la misión Apollo 11, la histórica misión que logró el primer alunizaje tripulado el 20 de julio de 1969. Este software fue responsable de las complejas tareas de navegación y control necesarias para guiar el módulo lunar "Eagle" desde la órbita lunar hasta su alunizaje en el Mar de la Tranquilidad y luego de vuelta a la órbita para reunirse con el Módulo de Comando y Servicio "Columbia".

- **ASCENT_GUIDANCE.agc**

    Este código es parte del Ordenador de Guía del Apollo (AGC) para la fase de ascenso del Módulo Lunar durante la misión Apollo 11. La fase de ascenso implica el lanzamiento del Módulo Lunar desde la superficie de la Luna de vuelta a la órbital lunar para encontrarse con el Módulo de comando.

  - Ensamblador:

    El ensamblador utilizado para este código es yaYUL.

  - Secciones:

    El código está estructurado en varias secciones con funcionalidad específica para la guía de ascenso, como cálculos para la velocidad, posición y señales de control para asegurar que el Módulo Lunar siga la trayectoria correcta.

  - Elementos y Conceptos Clave:
    - Cambio de Banco:

      El AGC usa cambio de banco para gestionar su memoria limitada. Comandos como BANK y SETLOC establecen el banco de memoria y la ubicación para las instrucciones subsiguientes.

    - Símbolos y Macros:

      - **TC:** Transferencia de control.
      - **OCT:** Constante octal.
      - **DXCH:** Intercambiar datos.
      - **DLOAD, DDV, DSU, DAD:** Carga de datos, división, resta, suma.
      - **STCALL:** Almacenar y llamar a una subrutina.

    - Cálculos de Guía:

      El código calcula varios parámetros para la fase de ascenso:

      - **ZDOT, YDOT, RDOT:** Componentes de velocidad.
      - **DZDOT, DYDOT, DRDOT:** Componentes delta de velocidad.
      - **VE, TGO:** Velocidad y tiempo restante.
      - **ATR, ATY:** Términos de aceleración en diferentes ejes.

    - Flags y Control:

      - **FLRCS:** Flag para el Sistema de Control de Reacción.
      - **MAINENG:** Operación del motor principal.
      - **IDLEFLAG:** Deshabilita la monitorización del Delta-V.
      - **T2TEST, FLPC, NORATES, RATES:** Varios flags para controlar la secuencia de ascenso.

    - Manejo y Corrección de Errores:

      - **ZATTEROR:** Errores de actitud cero.
      - **ENGOFF1:** Secuencia de apagado del motor.

    - Subrutinas y Operaciones:

      El código llama frecuentemente a varias subrutinas para realizar cálculos complejos y manejar tareas específicas:

      - **PHASCHNG:** Cambio de Fase.
      - **CHECKALT:** Comprobar altitud.
      - **CHECKYAW:** Comprobar alineación de guiñada.
  
  - Código de Ensamblador:

    - [ASCENT_GUIDANCE.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/ASCENT_GUIDANCE.agc)

- **BURN_BABY_BURN--MASTER_IGNITION_ROUTINE.agc**
  
  El nombre de esta rutina, "BURN BABY BURN", es una referencia a la frase famosa del locutor Magnificent Montague, reflejando un toque de humor histórico y cultural en la programación del AGC.

  La rutina es compleja, manejando diversos escenarios e integrándose con múltiples componentes y tareas. Utiliza una mezcla de comprobaciones inmediatas, gestión de cuenta atrás y manejo del estado del sistema para asegurar una ignición y operaciones posteriores sin problemas.

  - Propósito:
  
    La rutina BURN BABY BURN gestiona la ignición del sistema de propulsión del Módulo Lunar del Apolo. Se encarga de varias tareas relacionadas con la ignición, desde las comprobaciones de tiempo (si está dentro de los 45 segundos del tiempo de ignición) hasta los 26 segundos después de la ignición, cuando el sistema de propulsión comienza a aumentar su potencia.

  - Programas que usan esta Rutina:

    Se utiliza en los programas del LM: P12, P40, P42, P61, P63.

  - Secciones y Funciones Clave
    - Tablas para la Rutina de Ignición:

      Estas tablas (por ejemplo, P12TABLE, P40TABLE) contienen constantes e instrucciones TCF (Transfer Control Function) que guían el comportamiento específico de la rutina de ignición para diferentes programas.

    - Rutinas de Ignición de Propósito General:

      - **BURNBABY:** El punto de entrada de la rutina. Configura varios parámetros y llama a subrutinas como P40AUTO para manejar las tareas de ignición.
      - **TIG-35:** Maneja la cuenta atrás y gestiona situaciones donde la ignición podría ser retrasada.
      - **TIG-30, TIG-30A, TIG-5:** Manejan verificaciones específicas del tiempo y el estado del sistema en diferentes etapas de la cuenta atrás.
      - **P63IGN, P40IGN, P42IGN:** Inician procedimientos de ignición para programas específicos (P63, P40, P42).

    - Manejo de Errores y Fallos:

      - **COMFAILx:** Manejan fallos de comunicación y otros problemas. Cada tipo de fallo (por ejemplo, COMFAIL3, COMFAIL4) realiza diferentes acciones como reinciar monitores de visualización, reconectar pantallas o reinicializar componentes.

    - Manejo del Tiempo y Tareas:

      - **WAITABIT, TIGTASK:** Gestiona retrasos y aseguran que las tareas se completen en momentos específicos. Por ejemplo, WAITABIT asegua que ciertas tareas se retrasen adecuadamente.
      - **ULLAGOFF, COMMON:** Manejan tareas específicas como desactivar la detección de ullage o tareas comunes compartidas entre diferentes programas.

    - Código de Ensamblador:

      - [BURN_BABY_BURN--MASTER_IGNITION_ROUTINE.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/BURN_BABY_BURN--MASTER_IGNITION_ROUTINE.agc)

- **LANDING_ANALOG_DISPLAYS.agc**
  
    Controlar y actualizar las pantallas analógicas que muestran datos críticos a los astronautas durante el alunizaje. Estas pantallas proporcionan información sobre el estado del módulo lunar y ayudan a los astronautas a tomar decisiones durante el proceso de descenso

  - Organización del Código:

    - Configuración del Banco de Memoria y Localización

    ```nasm
    BANK    21
    SETLOC  R10
    BANK
    ```

    Aquí, se establece el banco de memoria 21 y se define la localización inicial del código en R10. Esto es esencial para asegurarse de que las instrucciones posteriores accedan a la memoria adecuada.

    - Actualización de TBASE2 y PIPCTR

    ```nasm
    LANDISP  LXCH  PIPCTR1
    CS      TIME1
    DXCH    TBASE2
    ```

    Este bloque de código actualiza TBASE2 y PIPCTR con valores de TIME1. PIPCTR es un contador para la visualización, mientras que TBASE2 puede estar relacionado con la base de tiempo de las mediciones.

    - Manejo de la Visualización de Altitud y Tasa de Altitud

    ```nasm
    CS      FLAGWRD7
    MASK    SWANDBIT
    CCS    A
    TCF    DISPRSET
    CA     IMODES33
    MASK   BIT7
    CCS    A
    TCF    ALTOUT
    ```

    Este bloque verifica si el indicador FLAGWRD7 está configurado. Si no está, se salta a DISPRSET, y si está, se verifica el modo de visualización en IMODES33. Si el bit 7 está activado, se dirige a ALTOUT, que maneja la visualización de la tasa de altitud.

    - Cálculo de la Tasa de Altitud

    ```nasm
    ALTROUT  TC   DISINDAT
    CS      IMODES33
    MASK    BIT7
    ADS    IMODES33
    CAF    BIT2
    EXTEND
    WOR    CHAN14
    ```

    En esta sección, se calcula la tasa de altitud basándose en el comando de tasa (BIT2). Se multiplica el componente VVECT por RUNIT para calcular la tasa de altitud.

    - Conversión de Datos

    ```nasm
    CA    ARCONV
    EXTEND
    MP    RUPTREG1
    DDOUBL
    DDOUBL
    XCH   RUPTREG1
    CA    DALTRATE
    EXTEND
    MP    DT
    AD    RUPTREG1
    TS    ALTRATE
    ```

    Este fragmento convierte la tasa de altitud a unidades de bit mediante un factor de conversión. Luego, se almacena en ALTRATE para su visualización.

    - Visualización de Datos

    ```nasm
    DATAOUT TS    ALTM
    CAF    BIT3
    EXTEND
    WOR    CHAN14
    TCF    TASKOVER
    ```

    Aquí, se envía el dato de tasa de altitud a la pantalla analógica (ALTM). El bit 3 controla la visualización de la tasa de altitud o altitud.

    - Manejo de Datos de Velocidad

    ```nasm
    SPEEDRUN  CS   PIPTIME +1
    AD    TIME1
    AD    HALF
    AD    HALF
    XCH   DT
    CA    1SEC
    TS    ITEMP5
    EXTEND
    ```

    Este bloque calcula la velocidad utilizando el tiempo y otros parámetros para actualizar la componente de velocidad en el vector VVECT.

    - Monitoreo de Velocidades

    ```nasm
    VMONITOR  TS   ITEMP5
    CCS   LATVEL
    TCF   +4
    TCF   LVLIMITS
    ```

    Este segmento realiza un monitoreo de las velocidades lateral y de avance. Verifica que los valores estén dentro de los límites aceptables y actualiza las pantallas en consecuencia.

    - Manejo de Errores y Salida

    ```nasm
    DISPRSET  CS   FLAGWRD0
    MASK  R10FLBIT
    EXTEND
    BZF   ABORTON
    ```

    Aquí se gestionan los errores y se realiza una limpieza de los indicadores. Si hay un error o se ha terminado el proceso, el código se dirige a TASKOVER.

  - Código de Ensamblador:

    - [LANDING_ANALOG_DISPLAYS.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/LANDING_ANALOG_DISPLAYS.agc)

- **THE_LUNAR_LANDING.agc**
  
    Se encarga de la fase de alunizaje, incluyendo la preparación, el cálculo de correcciones de guía, la verificación de componentes y la confirmación final del alunizaje. El código está dividido en bloques que manejan la configuración, el cálculo de guía, la verificación de componentes y la confirmación del alunizaje, asegurando así una transición precisa y controlada a la fase de aterrizaje en la Luna.

  - Guía e Integración:

    - La sección IGNALG es responsable de configurar las entradas para el algoritmo de guía y realizar los cálculos iniciales para el alunizaje.
    - Integra el estado hacia adelante hasta el tiempo de alunizaje y configura varias variables utilizadas en los cálculos de guía.

  - Correción y Ajuste de Errores:

    - DDUMCALC realiza cálculos para el algoritmo de guía, manejando términos de corrección y actualizando estimaciones para la guía del alunizaje.
    - Calcula y almacena parámetros necesarios para refinar el enfoque de alunizaje y corregir cualquier error.

  - Procedimientos Finales:

    - La sección P63SPOT2 inicializa el sistema para la actitud de alunizaje y alinea la nave espacial.
    - P63SPOT3 y P63SPOT4 manejan la inicialización del radar de alunizaje y verifican si los sistemas específicos están en la posición correcta.

  - Confirmación de Alunizaje:

    - La sección LANDJUNK maneja los pasos finales para la confirmación del alunizaje, estableciendo flags y realizando las últimas verificaciones antes del alunizaje.
    - Calcula la posición final del sitio de alunizaje y verifica si el alunizaje ha sido exitoso.

  - Explicación de las Secciones Clave:
    - **P63LM (Fase de Alunizaje del Módulo Lunar):**
      Este es el subprograma principal para la fase de alunizaje. Maneja diversas tareas, incluyendo la verificación de estado del IMU (Unidad de Medición Inercial), la inicialización de parámetros y la coordinación con los programas de guía.

    - **IGNALG (Algoritmo de Guía Inicial):**
      Este subprograma configura e integra los cálculos iniciales de guía para asegurar procedimientos de alunizaje precisos. Incluye varios cálculos y correcciones basados en el estado actual de la nave espacial.

    - **DDUMCALC (Cálculo de Dummy):**
      Este subprograma realiza cálculos específicos relacionados con la guía, implicando términos de corrección e integración de parámetros.

    - **P63SPOT2, P63SPOT3, y P63SPOT4:**
      Estas secciones son responsables de tareas específicas de configuración, incluyendo la alineación, la inicialización de sistemas y la verificación de la preparación de componentes críticos como antenas y radares.

    - **LANDJUNK (Confirmación de Alunizaje):**
      La fase final del proceso de alunizaje donde se realiza la confirmación del alunizaje. Incluye el cálculo de la posición final del sitio de alunizaje y tareas de verificación.

  - Explicación Detallada del Código:
    - Configuración Inicial:

      ```nasm
      BANK    32
      SETLOC  F2DPS*32
      BANK

      EBANK=  E2DPS
      ```

      - **BANK 32:** Selecciona el banco de memoria 32 para las siguientes operaciones.

      - **SETLOC F2DPS*32:** Establece la dirección de localización en la memoria.

      - **EBANK= E2DPS:** Define el banco de memoria extendida.

    - Fase de Alunizaje: Preparación y Configuración:

      ```nasm
      COUNT*  $$/P63
      ```

      - COUNT* $$/P63: Marca el comienzo de la sección del código P63, que se refiere a la fase de alunizaje.

      ```nasm
      P63LM   TC  PHASCHNG
          OCT 04024
      ```

      - P63LM TC PHASCHNG: Transición a la fase de alunizaje (P63), con el código de cambio de fase.

      ```nasm
      TC  BANKCALL  # DO IMU STATUS CHECK ROUTINE R02
      CADR R02BOTH
      ```

      - TC BANKCALL CADR R02BOTH: Llama a la rutina R02 para verificar el estado del IMU (Unidad de Medición Inercial).

      ```nasm
      CAF P63ADRES  # INITIALIZE WHICH FOR BURNBABY
      TS  WHICH
      CAF DPSTHRSH  # INITIALIZE DVMON
      TS  DVTHRUSH
      CAF FOUR
      TS  DVCNTR
      ```

      - CAF P63ADRES TS WHICH: Carga y almacena la dirección para la fase de alunizaje.
      - CAF DPSTHRSH TS DVTHRUSH: Carga y almacena el umbral para el monitoreo del DV.
      - CAF FOUR TS DVCNTR: Inicializa el contador del DV con el valor 4.

      ```nasm
      CS ONE    # INITIALIZE WCHPHASE AND FLPASS0
      TS  WCHPHASE
      CA  ZERO
      TS  FLPASS0
      ```

      - CS ONE TS WCHPHASE: Inicializa la fase de guía.
      - CA ZERO TS FLPASS0: Inicializa el flag de paso.

      ```nasm
      CS BIT14
      EXTEND
      WAND CHAN12    # REMOVE TRACK-ENABLE DISCRETE.
      ```

      - CS BIT14 EXTEND WAND CHAN12: Manipula el canal para deshabilitar el seguimiento.

    - Configuración de Flags y Parámetros:

      ```nasm
      FLAGORGY TC INTPRET
      CLEAR  CLEAR
          NOTHROTL
          REDFLAG
      CLEAR  SET
          LRBYPASS
          MUNFLAG
      CLEAR  CLEAR
          P25FLAG  # TERMINATE P25 IF IT IS RUNNING.
          RNDVZFLG  # TERMINATE P20 IF IT IS RUNNING
      ```

      - FLAGORGY TC INTPRET: Configura banderas para la fase de alunizaje.
      - CLEAR: Limpia los flags y realiza ajustes necesarios.

    - Algoritmo de Guía Inicial (IGNALG):

      ```nasm
      IGNALG SETPD VLOAD
      0   # AT 0D LANDING SITE IN MOON FIXED FRAME
      RLS # AT 6D ESTIMATED TIME OF LANDING
      PDDL    PUSH    # MPAC NON-ZERO TO INDICATE LUNAR CASE
              TLAND
      STCALL TPIP   # ALSO SET TPIP FOR FIRST GUIDANCE PASS
              RP-TO-R
      VSL4    MXV
              REFSMMAT
      STCALL LAND
              GUIDINIT # GUIDINIT INITIALIZES WM AND /LAND/
      DLOAD   DSU
              TLAND
              GUIDDURN
      STCALL TDEC1  # INTEGRATE STATE FORWARD TO THAT TIME
              LEMPREC
      ```

      - IGNALG SETPD VLOAD: Configura el algoritmo de guía inicial.
      - PDDL PUSH TLAND: Establece el tiempo estimado de alunizaje.
      - STCALL TPIP: Configura TPIP para el primer pase de guía.
      - STCALL LAND GUIDINIT: Inicializa el sistema de guía.
      - DLOAD DSU TLAND GUIDDURN: Carga datos para la duración de la guía y el tiempo.

      ```nasm
      SSP VLOAD
      NIGNLOOP
      40D
      UNITX
      STOVL CG
          UNITY
      STOVL CG +6
          UNITZ
      STODL CG +14
          99999CON
      STOVL DELTAH  # INITIALIZE DELTAH FOR V16N68 DISPLAY
          ZEROVECS
      STODL UNFC/2  # INITIALIZE TRIM VELOCITY CORRECTION TERM
          HI6ZEROS
      STORE  TTF/8
      ```

      - SSP VLOAD NIGNLOOP 40D UNITX: Inicializa los parámetros necesarios para el cálculo de la guía.
      - STOVL CG UNITY: Configura los valores de los vectores de guía.

    - Bucle de Cálculo y Corrección (IGNALOOP):

      ```nasm
      IGNALOOP DLOAD
              TAT
      STOVL PIPTIME1
              RATT1
      VSL4    MXV
              REFSMMAT
      STCALL R
              MUNGRAV
      STCALL GDT/2
              ?GUIDSUB  # WHICH DELIVERS N PASSES OF GUIDANCE
      ```

      - IGNALOOP DLOAD TAT STOVL PIPTIME1: Realiza cálculos continuos y ajustes de tiempo.
      - STCALL R MUNGRAV: Llama a la rutina para calcular la gravedad lunar.

    - Cálculo de Corrección Dummy (DDUMCALC):

      ```nasm
      DDUMCALC TS NIGNLOOP
        TC INTPRET
        DLOAD DMPR  # FORM DENOMINATOR FIRST
            VGU
            KIGNX/B4
      ```

      - DDUMCALC TS NIGNLOOP TC INTPRET: Configura el cálculo de corrección dummy.
      - DLOAD DMPR: Calcula el denominador para la corrección.

    - Procedimientos de Verificación y Finalización:

      ```nasm
      P63SPOT2 VLOAD UNIT
        R60VSAVE
      STOVL POINTVSM
              UNITX
      STORE  SCAXIS
      EXIT
      ```

      - P63SPOT2 VLOAD UNIT R60VSAVE: Inicializa el cálculo de actitud de alunizaje.

      ```nasm
      P63SPOT3 CA BIT6   # IS THE LR ANTENNA IN POSITION 1 YET
      EXTEND
      RAND CHAN33
      EXTEND
      BZF P63SPOT4  # BRANCH IF ANTENNA ALREADY IN POSITION 1
      ```

      - P63SPOT3 CA BIT6 EXTEND RAND CHAN33: Verifica si la antena está en la posición correcta.

      ```nasm
      P63SPOT4 TC BANKCALL  # ENTER  INITIALIZE LANDING RADAR
      CADR SETPOS1
      TC POSTJUMP  # OFF TO SEE THE WIZARD...
      CADR BURNBABY
      ```

      - P63SPOT4 TC BANKCALL CADR SETPOS1: Inicializa el radar de alunizaje y transita al siguiente paso.

    - Confirmación de Alunizaje (LANDJUNK):

      ```nasm
      LANDJUNK TC PHASCHNG
              OCT 04024
      ```

      - LANDJUNK TC PHASCHNG: Marca la fase de confirmación del alunizaje.

      ```nasm
      INHINT
      TC BANKCALL  # ZERO ATTITUDE ERROR
      CADR ZATTEROR

      TC BANKCALL  # SET 5 DEGREE DEADBAND
      CADR SETMAXDB
      ```

      - INHINT TC BANKCALL CADR ZATTEROR: Llama a la rutina para corregir el error de actitud y ajustar el rango muerto de 5 grados.

      ```nasm
      TC INTPRET
      SET CLEAR
          SURFFLAG
          LETABORT
      SET VLOAD
          APSFLAG
          RN
      STODL ALPHAV
          PIPTIME
      SET CALL
          LUNAFLAG
          LAT-LONG
      ```

      - SET CLEAR SURFFLAG LETABORT: Configura las banderas finales para la confirmación.
      - SET VLOAD APSFLAG RN STODL ALPHAV PIPTIME: Configura y almacena los parámetros finales del alunizaje.

      ```nasm
      CAF V06N43*   # ASTRONAUT: NOW LOOK WHERE YOU ENDED UP
      TC BANKCALL
      CADR GOFLASH
      TCF GOTOPOOH  # TERMINATE
      TCF +2        # PROCEED
      TCF -5        # RECYCLE
      ```

      - CAF V06N43 TC BANKCALL CADR GOFLASH*: Muestra la posición final y permite proceder o reciclar según sea necesario.

    - Constantes y Parámetros:

      ```nasm
      P63ADRES GENADR P63TABLE

      ASTNDEX = MD1   # OCT 25: INDEX FOR CLOKTASK

      CODE500  OCT 00500

      99999CON 2DEC 30479.7 B-24

      GUIDDURN 2DEC +66440  # GUIDDURN +6.64400314 E+2
      DDUMCRIT 2DEC +8 B-28  # CRITERION FOR IGNALG CONVERGENCE
      ```

      - P63ADRES GENADR P63TABLE: Define la dirección para el proceso de alunizaje.
      - CODE500 OCT 00500: Define constantes necesarias para el proceso.
      - 99999CON 2DEC 30479.7 B-24: Establece una constante para el cálculo.

      ```nasm
      LANDJUNK TC PHASCHNG
              OCT 04024
      ```

      - LANDJUNK TC PHASCHNG: Marca la fase de confirmación del alunizaje, indicando que el proceso de alunizaje ha llegado a su fin.

  - Código de Ensamblador:

    - [THE_LUNAR_LANDING.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/THE_LUNAR_LANDING.agc)

### Archivos del Módulo de Comando y Servicio (CSM)

El software Comanche 055 es otra pieza crucial en la historia de la exploración espacial, ya que fue el software de vuelo utilizado en el Módulo de Comando (CM) durante las misiones Apollo.

El término "Comanche" hace referencia a una tribu nativa americana conocida por su habilidad en la navegación y estrategia en el terreno, cualidades que son paralelas a las necesidades del Módulo de Comando en las misiones Apollo. Esta denominación simboliza la misión del software de guiar y proteger a los astronautas durante sus viajes a la Luna y su regreso a la Tierra, de la misma manera que los Comanches eran maestros en guiar a su gente a través de vastas regiones.

Al igual que con el número del Luminary 099, el 055 representa una versión específica del software del Módulo de Comando.

Para entender cómo el módulo de comando y servicio (CSM) del Apollo 11 manejó sus complejas tareas, exploraremos algunos de los archivos más relevantes del software Comanche055. Estos archivos son fundamentales para comprender el funcionamiento del CSM durante la misión. A continuación, presentamos una selección de los archivos más importantes e interesantes:

- **CM_BODY_ATTITUDE.agc**

    El propósito de este código es actualizar y manejar los ángulos de orientación del módulo de comando en respuesta a las lecturas de los acelerómetros y giroscopios. El código guarda las posiciones actuales y calcula los cambios en la orientación basándose en estas lecturas, manteniendo la estabilidad y control necesarios para las maniobras durante la misión.

  - Sección y Subrutina

    - **Sección Principal:** Control de la Actitud del Cuerpo (CM_BODY_ATTITUDE)

    - **Subrutina Principal:** CM/ATUP (Actualización de los Ángulos de Actitud)

  - Descripción Funcional

    - **Guardado de Ángulos y Valores de Gimbal:**

      Los ángulos de gimbal y del cuerpo válidos en el momento PIP son guardados durante la lectura de los acelerómetros (READACCS)

    - **Configuración Inicial y Cálculo de Vectores:**
      - El intérprete establece el banco correcto.
      - Se realizan cálculos vectoriales y se almacenan los resultados para su uso en la guía de entrada y coordenadas de referencia.

    - **Cálculos de Trayectoria y Ángulos:**
      - Se calculan los vectores relativos y se alamacenan los resultados.
      - Se obtienen triadas de trayectoria y se calculan los componentes del movimiento del módulo.

    - **Actualización de Ángulos:**
      - La subrutina CM/ATUP se encarga de actualizar los ángulos de actitud (ROLL, BETA, ALFA) basándose en los datos obtenidos desde la última actualización.
      - Se manejan las interrupciones y se aseguran los cálculos precisos corrigiendo los posibles desbordamientos.

    - **Correción y Ajuste de Ángulos:**
      - La subrutina CM/ATUP se encarga de actualizar los ángulos de actitud (ROLL, BETA, ALFA) basándose en los datos obtenidos desde la última actualización.
      - Se manejan las interrupciones y se aseguran los cálculos precisos corrigiendo los posibles desbordamientos.
  
  - Detalles Técnicos
    - **KVSCALE:** Constante utilizada para la escala de velocidad.
    - **TCDU:** Tiempo de ciclo de unidad (0.1 segundos).
    - **Subrutinas Específicas:**
      - CM/POSE, CM/POSE2, CM/POSE3: Manejan diferentes etapas del cálculo de orientación.
      - DOGAMDOT, NOGAMDOT: Deciden si calcular o no la velocidad angular en base a ciertas condiciones.
      - REDOPOSE, CORANGOV: Ajustan los ángulos y manejan los posibles desbordamientos.
  
  - Código de ensamblador:

    - [CM_BODY_ATTITUDE.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/CM_BODY_ATTITUDE.agc)

- **IMU_CALIBRATION_AND_ALIGNMENT.agc**

    Esta porción de código del AGC se usa para la Calibración y Alineación de la IMU, diseñada específicamente para soportar varias pruebas de rendimiento y rutinas relacionadas con la Unidad de Medición Inercial (IMU) para el Módulo de Comando del Apollo. A continuación presento una explicación de las secciones claves y sus propósitos:

  - Pruebas de Rendimiento de la IMU:
  
    - **Inicialización:** El código inicializa las pruebas de la IMU, configurando varios contadores y flags a cero. Esto incluye el NDXCTR (contador de índice) y el TORQNDX (índice de torque).

    - **Rutinas de Posicionamiento:** Se utilizan varias rutinas para posicionar y alinear la IMU. El programa configura y verifica varios componentes gométricos y realiza cálculos como seno y coseno de la latitud, azimut y otros parámetros de alineación.

    - **Pruebas de Deriva del Giroscopio:** El código realiza pruebas del giroscopio para asegurar que os giroscopios funcionen correctamente. Esto implica configurar temporizadores (LENGTHOT), capturar y procesar pulsos PIPA (Acelerómetro Péndulo Integrador Pulsado).

    - **Compensación de la Velocidad de la Tierra:** El código compensa la velocidad de rotación de la Tierra calculando y aplicando correcciones a las lecturas de la IMU. Eso asegura un posicionamiento y alineación precisos a pesar del movimiento de la Tierra.

  - Manejo de Datos y Prueba de Reinicio:

    - **Prueba de Reinicio:** Para manejar posibles reinicios, el código incluye rutinas para almacenar y recargar datos críticos. Esto asegura que el sistema pueda reanudar pruebas o procedimientos de alineación sin comenzar desde cero.

    - **Almacenamiento y Carga:** Los datos relacionados con la alineación y calibración de la IMU se almacenan periódicamente y se pueden recargar para continuar las operaciones sin problemas después de un reinicio.
  
  - Procedimientos de Alineación y Calibración:

    - **Alineación Gruesa (COAALIGN):** Esta rutina maneja la alineación inicial de la IMU, configurando la orientación y asegurando que la plataforma esté correctamente alineada.

    - **Alineación Fina (IMUFINE):** Después de la alineación gruesa, la rutina de alineación fina ajusta la orientación de la IMU con mayor precisión.

    - **Pruebas PIPA (PIPACHK):** Esta rutina prueba el PIPA para medir tasas verticales y asegurar la precisión de la IMU.
  
  - Desglose Detallado:

    - **Inicialización (IMUTEST):** Limpia y configura valores iniciales para tiempos de deriva, componentes geográficos y otros parámetros de prueba.

    - **Pruebas de Deriva del Giroscopio (VERTDRFT, TORQUE):** Realiza pruebas para medir y compensar la deriva en los giroscopios.

    - **Compensación de la Velocidad de la Tierra (ERTHRVSE, EARTHR):** Calcula la compensación necesaria para la rotación de la Tierra para mantener lecturas precisas de la IMU.

    - **Procedimientos de Alineación (COAALIGN, IMUFINE):** Alinea inicialmente la IMU y ajusta la alineación para operaciones precisas.

    - **Captura de Pulsos PIPA (CHECKG, PIPACHK):** Captura y procesa pulsos PIPA para asegurar que la IMU mida con precisión la aceleración y orientación.

    - **Prueba de Reinicio (STOREDTA, LOADSTDT):** Asegura que en caso de un reinicio, los datos críticos de alineación y calibración se guarden y puedan recargarse para reanudar operaciones rápidamente.
  
  - Código de ensamblador:

    - [IMU_CALIBRATION_AND_ALIGNMENT.agc]([assets\img\post\apollo11\code\IMU_CALIBRATION_AND_ALIGNMENT.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/IMU_CALIBRATION_AND_ALIGNMENT.agc))
  
- **ORBITAL_INTEGRATION.agc**
  
  Este código se divide en distintas "páginas" que realizan diferentes funciones, a continuación explicaré de manera detallada cada página o conjunto de ellas:

  - **Páginas 1334-1336: ORBITAL INTEGRATION y KEPPREP:**

    Estas páginas configuran las rutinas para la integración orbital, que es un proceso clave en la navegación espacial. La rutina KEPPREP parece preparar variables y realizar cálculos preliminares necesarias para la integración de las órbitas.

    - **Bancos de Memoria:** El código usa diferentes bancos de memoria para almacenar datos específicos.

    - **Cargas y Almacenamientos:** Se cargan y alamcenan valores en diferentes ubicaciones de memoria, que incluyen cálculos de varos físicos como la raíz cuadrada de la constante gravitacional.

  - **Páginas 1337: ACCOMP:**

    La rutina ACCOMP calcula los componentes de aceleración. Utiliza vectores de posición y velocidad, y realiza operaciones vectoriales como multiplicaciones y sumas.

    - **Carga de Vectores:** Carga y almacena vectores de aceleración.

    - **Normalización:** Normaliza los vectores para los cálculos posteriores.

  - **Páginas 1338-1340: GAMCOMP:**

    Esta subrutina calcula componentes específicos de la aceleración gravitacional (GAMCOMP).

    - **Cálculo de la aceleración:** Realiza cálculos complejos para determinar la aceleración debido a la gravedad y otras fuerzas.

    - **Normalización:** Asegura que los valores calculados estén en un rango normalizado.

  - **Página 1341: OBLATE:**

    La rutina OBLATE calcula la aceleración debido a la oblación terrestre (achatamiento de la Tierra en los polos).

    - **Cálculo de componentes de Oblateness:** Usa el vector de posición del vehículo y su distancia al centro de la Tierra para calcular la aceleración adicional debido a la forma oblata de la Tierra.

    - **Ajustes y Sumas:** Ajusta los valores de aceleración calculados y los suma a las aceleraciones existentes.

  - **Página 1343: COMTERM y COSPHIE:**

    Estas secciones manejan cálculos adicionales y ajustes específicos relacionados con los efectos del oblateness.

    - **Cálculos de términos adicionales:** Calcula y ajusta términos adicionales necesarios para la precisión en la integración orbital.

  - **Páginas 1344-1345: TIMESTEP y RECTEST:**

    Estas rutinas gestionan los pasos de tiempo en la integración y verifican si se necesita una rectificación en la órbita calculada.

    - **Verificaciones y ajustes:** Verifica condiciones específicas que podrían requerir ajustes en la órbita calculada.

    - **Integración:** Realiza la integración de la órbita utilizando pasos de tiempo específicos.

  - **Página 1346: ORIGCHNG:**

    La rutina ORIGCHNG maneja los cambios de origen, es decir, cuando el sistema de referencia necesita ser ajustado debido a cambios significativos en la posición o velocidad.

    - **Ajustes de Origen:** Ajusta el sistema de referencia y recalcula los vectores de posición y velocidad en relación con el nuevo origen.

  - **Página 1347: RECTIFY:**

    La subrutina RECTIFY establece una nueva órbita cónica cuando se detectan cambios significativos.

    - **Reestablecimiento de Órbita:** Establece una nueva órbita cónica basada en los cálculos actuales de posición y velocidad.

    - **Reinicialización:** Reinicializa las variables y prepara el sistema para la siguiente etapa de integración.
  
  - Código de ensamblador:

    - [ORBITAL_INTEGRATION.agc]([assets\img\post\apollo11\code\ORBITAL_INTEGRATION.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/ORBITAL_INTEGRATION.agc))

- **TVCEXECUTIVE.arg**

  TVCEXECUTIVE es uan rutina crítica del AGC que se encarga de gestionar y actualizar el sistema de control de actitud y navegación (TVC) del Módulo de Comando. Esta rutina se ejecuta a intervalos regulares de 0.5 segundos y realiza varias tareas esenciales:

  - Preparación de Datos de Control:

    - **Actualización de Roll DAP:** Se prepara el DAP (Data Access Processor) para el control del roll del módulo con los datos actuales.

    - **Actualización del FDAI Needle:** Se actualiza la aguja del Indicador de Actitud de Vuelo (FDAI) con el error de control de actitud del roll.

    - **Actualización del Phase Plane:** Se prepara el plano de fase del roll para el error OGA (Error de Guía de Actitud).

  - Actualización de Necesidades y Datos:

    - **Actualización de Needles:** Llama a una subrutina que actualiza los indicadores de actitud.

    - **Actualización de Masa:** Actualiza la masa del vehículo y, por ende, los datos de inercia.

    - **Actualización de Ganancias:** Actualiza las ganancias de control de actitud en pitch (elevación), yaw (guía) y roll, basándose en los datos de masa actualizados.

  - Corrección y Reajustes:

    - **Corrección de Un Solo Disparos:** Realiza ajustes únicos en el bucle TMC (Throttle Maneuver Control) poco después de la ignición.

    - **Corrección Repetitiva:** Ejecuta correcciones repetitivas para el bucle TMC después de la corrección de un solo disparo.
  
  - Llamadas y Secuencia de Ejecución:

    - TVCEXEC es llamado como una tarea en la lista de espera por TVCINIT4 y por sí mismo a intervalos de 0.5 segundos.

      - **Verificación de Terminación:** El código primero verifica si debe terminarse, usando un chequeo de bits en la palabra de bandera (FLAGWRD6).

      - **Preparación de Datos:** Luego prepara diversos datos y actualiza los indicadores y sistemas necesarios.

    - Actualización y Manejo de Errores:

      - **Preparación del DAP:** Actualiza el DAP y otros componentes con los errores actuales y los datos de control.

      - **Actualización de Needles:** Llama a una subrutina para actualizar los indicadores de actitud.

    - Manejo de Ganancias y Correcciones:

      - **Chequeo del Estado del Motor:** Verifica si el motor está encendido para decidir si se deben actualizar las ganancias y la masa.

      - **Actualización de Ganancias y Masa:** Realiza ajustes basados en el estado actual del motor y actualiza las ganancias y datos de masa.

    - Correcciones de Un Solo Disparo y Repetitivas:

      - **Chequeo de Tiempo:** Verifica el tiempo para decidir cuándo realizar una corrección de un solo disparo o una corrección repetitiva.

      - **Ajustes y Correcciones:** Realiza correcciones según sea necesario, basándose en el estado de las correcciones anteriores.

    - Subrutinas Importantes:

      - **NEEDLER:** Actualiza los indicadores de actitud (needles).
      - **MASSPROP:** Actualiza la masa y las propiedades relacionadas.
      - **IBNKCALL:** Llama a una subrutina de banco de instrucciones para realizar operaciones específicas.

    - Salidas y Variables:

      - **ROLL DAP, FDAI Needle, y Phase Plane:** Se actualizan con los valores más recientes para el control del roll.
      - **Ganancias Variables:** Se ajustan para el pitch, yaw y roll basados en los datos más recientes.
      - **Correcciones de Ángulo de Motor:** Se ajustan los ángulos de trim del motor y otros parámetros relevantes.
  
  - Código de ensamblador:

    - [TVCEXECUTIVE.agc]([assets\img\post\apollo11\code\TVCEXECUTIVE.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/TVCEXECUTIVE.agc))

[https://ntrs.nasa.gov/](https://ntrs.nasa.gov/)
[The Apollo Guidance Computer: Architecture and Operation](https://nss.org/book-review-the-apollo-guidance-computer-architecture-and-operation/)
[https://github.com/chrislgarry/Apollo-11](https://github.com/chrislgarry/Apollo-11)
[http://www.ibiblio.org/apollo/#gsc.tab=0](http://www.ibiblio.org/apollo/#gsc.tab=0)
[https://www.nasa.gov/history/alsj/main.html](https://www.nasa.gov/history/alsj/main.html)
[https://www.nasa.gov/history/alsj/alsj-LMdocs.html](https://www.nasa.gov/history/alsj/alsj-LMdocs.html)

## UC
