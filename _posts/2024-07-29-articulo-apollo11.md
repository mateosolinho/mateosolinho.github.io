---
title: Explorando el Código Fuente del Apollo 11 - Un Viaje al Corazón de la Computación Espacial
date: 2024-07-31 20:00:00 +0800
categories: [Tecnología, Articulos]
tags: [Apollo 11, AGC, AGS, Computación Espacial, RocketScience, Tecnología Espacial]
image:
  path: /assets/img/post/apollo11/2.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

Imagina tener que guiar a un **módulo espacial** a través del vasto vacío del espacio y aterrizarlo con precisión en la **superficie lunar**, todo mientras se trabaja con un ordenador con menos potencia que un moderno **teléfono móvil**. Este fue el desafío enfrentado por el **Apollo Guidance Computer (AGC)** durante la histórica misión **Apollo 11**, la cual llevó al primer humano a la superficie lunar.

En este post, vamos a sumergirnos en el fascinante mundo del **hardware** y el **software** que hizo posible el alunizaje. Desde las _intrincadas rutinas de auto-verificación_ hasta las _complejas ecuaciones de guía lunar_, descubrirás cómo el código detrás del **AGC** no solo llevó al hombre a la Luna, sino que también marcó un hito en la historia de la _computación espacial_.

![Foto de la misión Apollo](/assets/img/post/apollo11/1.jpg)

## Historia del AGC

Para entender el impacto del **Apollo Guidance Computer (AGC)** en la misión **Apollo 11**, primero debemos explorar su _historia_ y su _contexto_.

![Foto del Apollo Guidance Computer (AGC)](/assets/img/post/apollo11/4.jpg)

El **AGC**, desarrollado en el **MIT Instrumentation Laboratory** bajo la dirección de **Charles Stark Draper**, fue un avance revolucionario en la _computación espacial_. Su diseño _compacto_ y _robusto_ estaba destinado a soportar las rigurosas condiciones del espacio y a facilitar las complejas maniobras necesarias para una misión lunar exitosa.

Uno de los nombres más destacados en el desarrollo del **AGC** es **Margaret Hamilton**, quien lideró el equipo de _ingenieros de software_. Su enfoque meticuloso en la ingeniería de software y su visión pionera no solo garantizaron el éxito de la misión **Apollo 11**, sino que también establecieron muchos de los **principios** de programación que hoy consideramos fundamentales.

![Margaret Hamilton trabajando en el AGC](/assets/img/post/apollo11/5.jpg)

## Hardware del AGC

El **Apollo Guidance Computer (AGC)** fue una pieza fundamental del éxito de las misiones **Apollo**, destacando por sus _innovaciones_ en la ingeniería de _hardware_ y _software_ en la época.

![AGC](/assets/img/post/apollo11/25.jpeg)

### Características técnicas del AGC

- **Procesador:** 2.048 MHz

  ![Diagrama del procesador del AGC](/assets/img/post/apollo11/8.jpg)

  - **Ciclo de Instrucción:** El **AGC** operaba a una frecuencia de `_2.048 MHz_`, lo que permitía un _ciclo de instrucción_ de aproximadamente `_11.72 microsegundos_`. Cada instrucción del conjunto de instrucciones del **AGC** se ejecutaba en uno o más de estos ciclos.
  - **Arquitectura:** El procesador del **AGC** tenía una `arquitectura de 16 bits`, utilizando una _arquitectura word_ de `_15 bits_` más un bit de paridad. El conjunto de instrucciones incluía operaciones básicas como _aritméticas_, _lógicas_, de _control de flujo_, y de _manipulación de datos_.
  - **Pipelining:** Aunque no contaba con _pipelining_ en el sentido moderno, el diseño eficiente del **AGC** permitía un _flujo constante_ de datos e instrucciones a través del sistema.

- **Memoria Ram (Random Access Memory):** 2 KB
  
  ![Imagen de núcleos magnéticos de RAM](/assets/img/post/apollo11/6.jpg)

  - **Tecnología:** La **RAM** del **AGC** estaba basada en _núcleos magnéticos_ (**magnetic core memory**), una tecnología _robusta_ y _no volátil_ que permitía mantener el estado incluso en caso de pérdida de energía.
  - **Función:** La **RAM** se utilizaba principalmente para almacenar _variables temporales_ y _datos intermedios_ necesarios para los _cálculos en tiempo real_. También se usaba para almacenar el _estado del programa_ y los _datos de navegación_.

- **ROM (Read-Only Memory):** 36 KB

  ![Diagrama del Core Rope Memory](/assets/img/post/apollo11/7.jpg)

  - **Tecnología:** La **ROM** utilizaba la tecnología **Core Rope Memory**, donde los _bits de datos_ se almacenaban en _núcleos magnéticos_ mediante _cables trenzados_. Esta tecnología ofrecía _alta densidad de almacenamiento_ y era _extremadamente resistente_ a fallos.
  - **Función:** La **ROM** contenía el _código de programa esencial_ para todas las fases de la misión, incluyendo _rutinas de navegación_, _control_ y _manejo de datos_. La estructura del software era _modular_, permitiendo _actualizaciones y modificaciones_ sin alterar el código base.

### Componentes Clave

- **Teclado y Pantalla (DSKY - Display and Keyboard):**

  ![Imagen del DSKY](/assets/img/post/apollo11/9.png)

  - **Interfaz de Usuario:** El **DSKY** consistía en una _pantalla de 7 segmentos_ para mostrar datos numéricos y una serie de _luces de estado_ para indicar diferentes modos y estados del sistema. El teclado permitía la entrada de comandos mediante un conjunto de _teclas numéricas_ y de _función_.
  - **Comunicaciones:** El **DSKY** se comunicaba con el **AGC** mediante un _bus de datos dedicado_, permitiendo la entrada y salida rápida y eficiente de información.

- **Unidad de Navegación Inercial (INU - Inertial Navigation Unit):**
  
  ![Diagrama de la Unidad de Navegación Inercial (INU)](/assets/img/post/apollo11/14.jpg)

  - **Componentes:** El **IMU** contenía _giroscopios_ y _acelerómetros_ que proporcionaban datos sobre la _orientación_ y _aceleración_ de la nave. Estos datos eran críticos para la _navegación_ y el _control de la trayectoria_.
  - **Integración:** El **AGC** integraba los datos del **IMU** con sus cálculos de navegación para determinar la _posición_ y _orientación_ precisas del **módulo lunar** y el **módulo de comando**.
  
- **Core Rope Memory:**

  - **Construcción:** La **Core Rope Memory** es un tipo de memoria **ROM**, que estaba construida mediante el _entrelazado_ de hilos a través de núcleos magnéticos. Un hilo que pasaba a través de un núcleo representaba un bit **"1"**, mientras que uno que pasaba alrededor del núcleo representaba un bit **"0"**.
  - **Ventajas:** Esta tecnología proporcionaba una _alta densidad_ de almacenamiento y era muy _resistente_ a las perturbaciones electromagnéticas, crucial para el entorno espacial.

### Innovaciones y Desafíos Técnicos

- **Código:**

  - **Optimización de Código:** Debido a las _limitaciones_ de memoria y procesamiento, los ingenieros tuvieron que **optimizar** cada línea de código. Esto incluyó el uso eficiente de _ciclos de máquina_ y la minimización de operaciones innecesarias.
  - **Subrutinas y Modularidad:** El software estaba _altamente modularizado_, permitiendo la reutilización de código y facilitando las _pruebas_ y la _verificación_.

- **Robustez y Fiabilidad:**

  - **Redundancia:** El **AGC** estaba diseñado con una _redundancia_ en mente. Tenía múltiples niveles de _verificación_ y _recuperación de fallos_ para garantizar que los errores pudieran ser detectados y corregidos sin poner en peligro la misión.
  - **Pruebas Exhaustivas:** Se realizaron pruebas _extensivas_ de cada componente y subrutina del **AGC** para asegurar su correcto funcionamiento bajo todas las condiciones previstas.

- **Interacción Humana:**
  
  - **Simplicidad y Eficiencia:** El diseño del **DSKY** tenía que ser _intuitivo_ para los astronautas, permitiéndoles ingresar comandos y recibir información crítica rápidamente y con mínima posibilidad de error.
  - **Entrenamiento:** Los astronautas recibieron un _entrenamiento extensivo_ en el uso del **DSKY** y la interpretación de los datos mostrados, asegurando que pudieran tomar decisiones informadas durante la misión.

El **Apollo Guidance Computer** no solo fue un logro técnico _impresionante_ en su época, sino que también estableció muchos de los **principios** y **prácticas** que siguen siendo relevantes en la ingeniería de _sistemas críticos_ y la computación en _tiempo real_.

## Abort Guidance System (AGS)

El **Abort Guidance System (AGS)** del **Módulo Lunar** del programa **Apollo** fue diseñado específicamente para proporcionar capacidades de _navegación_ y _control_ en caso de un fallo del sistema principal de guiado, el **AGC**. A continuación, algunos aspectos importantes a tener en cuenta sobre el **AGS**:

![Foto del AGS](/assets/img/post/apollo11/11.jpg)

![Foto del AGS](/assets/img/post/apollo11/12.jpg)

### Arquitectura y Diseño del AGS

- **Computadora de Guiado (Abort Electronics Assembly, AEA)**

  ![Abort Electronics Assembly (AEA)](/assets/img/post/apollo11/13.jpg)

  - **Procesador:** Basado en tecnología de _lógica discreta_, menos avanzada que el procesador del **AGC**.
  - **RAM:** Aproximadamente _2 KB_ de memoria volátil, utilizada para operaciones _temporales_.
  - **ROM:** _4 KB_ de memoria de núcleo de ferrita para almacenar el software de emergencia.
  - **Instrucciones:** Capaz de ejecutar un conjunto más limitado de instrucciones comparado con el **AGC**, _adecuado_ para su propósito específico de manejo de abortos y emergencias.

- **Unidad de Medición Inercial (IMU)**
  
  ![Unidad de Medición Inercial (IMU)](/assets/img/post/apollo11/10.jpg)

  - **Descripción:** La **IMU** del **AGS** era una versión _simplificada_ y menos precisa que la utilizada por el **AGC**.
  - **Giroscopio:** Menos preciso pero _suficiente_ para las operaciones de emergencia.
  - **Acelerómetro:** Proporcionaba mediciones básicas de _aceleración_ para la navegación de respaldo.
  
- **Interfaces de Entrada/Salida**
  
  ![DSKY Simplificado del AGS](/assets/img/post/apollo11/15.jpg)

  - **DSKY Simplificado:** La pantalla proporcionaba _información básica_ de estado y navegación, y el teclado permitía la entrada de _comandos esenciales_ para la operación del **AGS**.
  
- **Sensores y Actuadores**

  - **Sensores de Presión y Temperatura:** Utilizados para monitorear condiciones críticas del **Módulo Lunar**.
  - **Actuadores de Control:** Permitían el ajuste de la _orientación_ y la ejecución de _maniobras de emergencia_.
  
- **Algoritmos de Navegación**

  ![Diagrama de Algoritmos de Navegación](/assets/img/post/apollo11/16.jpg)

  - **Ascent Guidance Algorithm:** Diseñado para calcular y ejecutar una trayectoria de ascenso segura desde la _superficie lunar_ en caso de aborto.
  - **Abort Modes:** Incluía modos específicos para diferentes fases del _descenso_ y _ascenso_, cada uno con rutas de navegación predefinidas para maximizar la probabilidad de éxito.

- **Componentes de Redundancia:**

  - **Baterías Independientes:** El **AGS** tenía su propio sistema de energía para asegurar que funcionara incluso si el sistema principal fallaba.
  - **Circuitos Redundantes:** Diseñados para _tolerancia a fallos_, permitiendo que el sistema continúe operando en condiciones adversas.

### Innovaciones y Capacidades Específicas

- **Navegación de Respaldo**

  - **Precisión:** Aunque menos preciso que el **AGC**, el **AGS** podía proporcionar suficiente _precisión_ para una operación de emergencia.
  - **Fiabilidad:** Diseñado para ser _extremadamente fiable_ y _fácil de usar_ en situaciones de alta tensión.

- **Manejo de Emergencias**

  ![Procedimientos de Emergencia](/assets/img/post/apollo11/18.jpg)

  - **Abort Modes:** Capacidad para iniciar y gestionar diferentes modos de aborto basados en la _fase de la misión_ y las _condiciones presentes_.
  - **Ascent Capability:** Proporcionaba una _ruta de ascenso segura_ en caso de un aborto durante el _descenso_ o en la _superficie lunar_.

- **Diagnóstico y Auto-Pruebas**

  ![Diagrama de Diagnóstico y Auto-Pruebas](/assets/img/post/apollo11/19.png)

  - **Monitoreo Continuo:** Se realizaban _auto-pruebas_ constantes para verificar su estado operativo.
  - **Alertas y Diagnósticos:** Proporcionaba alertas y _diagnósticos simples_ para informar a los astronautas sobre el estado del sistema y posibles fallos.

El **Abort Guidance System (AGS)** fue un componente **crucial** del **Módulo Lunar**, diseñado para proporcionar un respaldo _robusto_ y _fiable_ en caso de fallo del sistema principal de guiado, el **AGC**.

Su diseño _simplificado_, junto con su capacidad para manejar situaciones de _emergencia_, lo convirtió en una herramienta **vital** para la seguridad de las misiones **Apollo**. Aunque menos potente y preciso que el **AGC**, el **AGS** desempeñó un papel **esencial** en la estrategia de _redundancia_ y _gestión de riesgos_ del programa **Apollo**.

## Codigo Fuente AGC

En las siguientes secciones, exploraremos varios archivos de código fuente **clave** que ilustran cómo el **AGC** gestionaba tareas críticas como la _navegación_, el _control de actitud_, las _maniobras automáticas_ y las _rutinas de emergencia_.

Analizaremos el **propósito** de cada archivo, su **estructura** y las **innovaciones técnicas** que hicieron posible la realización de estas funciones vitales en el entorno _hostil_ del espacio.

- **AGC_BLOCK_TWO_SELF_CHECK.agc**

  - **Descripción del Programa**:

  - **Fecha**: 20 de diciembre de 1967
  - **Nombre del Programa**: `SELF-CHECK`
  - **Sección**: AGC BLOCK TWO SELF-CHECK
  - **Subrutina de Ensamblaje**: `UTILITYM REV 25`

  - **Descripción Funcional:**

    El programa tiene dos partes principales:

    - **`SELF-CHECK`**: Corre como un trabajo de prioridad cero sin núcleo establecido, como parte del bucle de respaldo inactivo.

    - **`SHOW-BANKSUM`**: Corre como un trabajo ejecutivo regular con su propio verbo de inicio.

  - **Propósito:**

    - **`SELF-CHECK`**: Verificar varias partes del ordenador según las opciones.

    - **`SHOW-BANKSUM`**: Mostrar la suma de cada banco, uno a la vez.

  - **Opciones de SELF-CHECK:**

    Controladas por números en el registro `SMODE` (`NOUN 27`):

    - `4`: Memoria borrable.
    - `5`: Memoria fija.
    - `1`, `2`, `3`, `6`, `7`, `10`: Todo lo anterior.
    - `0`: Igual que `+10` hasta que se detecta un error.
    - `+0`: Sin chequeo, pone la computadora en el bucle inactivo de respaldo.

  - **Secuencia de Llamada:**

    - Para llamar a **SELF-CHECK**: `V 21 N 27 E Número de opción E`
    - Para llamar a **SHOW-BANKSUM**:
    - `V 91 E`: Muestra el primer banco.
    - `V 33 E`: Procede y muestra el siguiente banco.

  - **Modos de Salida**

    - **`SELF-CHECK`**: Continúa indefinidamente a menos que se detecte un error.
    - **`SHOW-BANKSUM`**: Procede hasta que se termina (`V 34 E`).

  - **Salida:**

    - **SELF-CHECK**: Al detectar un error, carga la constante de alarma (`01102`) y enciende la luz de alarma.
      - Mostrar registros de fallas: `V 05 N 09 E`.
      - Información adicional: `V 05 N 08 E`.

    - **SHOW-BANKSUM**:

      - `R1`: Suma del banco.
      - `R2`: Número de banco.
      - `R3`: Palabra auxiliar.

  - **Código de Ensamblador:**

    - [AGC_BLOCK_TWO_SELF_CHECK.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/AGC_BLOCK_TWO_SELF_CHECK.agc)

### Archivos del Módulo Lunar (LM)

El software **Luminary 099** es una pieza fundamental en la historia de la exploración espacial, ya que fue el software de vuelo utilizado en el **Módulo Lunar (LM)** durante las misiones **Apollo**.

![Lunar Module Diagram](/assets/img/post/apollo11/20.jpg)

El término **"Luminary"** se refiere a algo que emite luz, como una estrella o una fuente de inspiración. Este nombre es particularmente apto para el software de guía y navegación del **Módulo Lunar** del **AGC**. De manera simbólica, **"Luminary"** también refleja la aspiración de la misión **Apollo** de iluminar el camino para la humanidad en la exploración espacial, convirtiéndose en una luz guía en la historia de la ciencia y la ingeniería.

El número **"099"** representa una versión específica del software. En el contexto del desarrollo de software, los números de versión son esenciales para llevar un registro de las distintas iteraciones del programa.

**Luminary 099** es especialmente notable porque fue la versión utilizada durante la misión **Apollo 11**, la histórica misión que logró el primer alunizaje tripulado el **20 de julio de 1969**. Este software fue responsable de las complejas tareas de navegación y control necesarias para guiar el módulo lunar _"Eagle"_ desde la órbita lunar hasta su alunizaje en el _Mar de la Tranquilidad_ y luego de vuelta a la órbita para reunirse con el Módulo de Comando y Servicio _"Columbia"_.

- **ASCENT_GUIDANCE.agc**

    Este código es parte del **Ordenador de Guía del Apollo (AGC)** para la **fase de ascenso** del Módulo Lunar durante la misión **Apollo 11**. La fase de ascenso implica el **lanzamiento del Módulo Lunar** desde la superficie de la Luna de vuelta a la **órbita lunar** para encontrarse con el **Módulo de Comando**.

  - **Ensamblador:**

    El ensamblador utilizado para este código es **`yaYUL`**.

  - **Secciones:**

    El código está estructurado en varias **secciones** con **funcionalidad específica** para la **guía de ascenso**, como cálculos para la **velocidad**, **posición** y **señales de control** para asegurar que el Módulo Lunar siga la **trayectoria correcta**.

  - **Elementos y Conceptos Clave:**
    - **Cambio de Banco:**

      El AGC usa **cambio de banco** para gestionar su **memoria limitada**. Comandos como `BANK` y `SETLOC` establecen el **banco de memoria** y la **ubicación** para las instrucciones subsiguientes.

    - **Símbolos y Macros:**

      - **`TC`**: **Transferencia de control**.
      - **`OCT`**: **Constante octal**.
      - **`DXCH`**: **Intercambiar datos**.
      - **`DLOAD`, `DDV`, `DSU`, `DAD`**: **Carga de datos**, **división**, **resta**, **suma**.
      - **`STCALL`**: **Almacenar** y **llamar a una subrutina**.

    - **Cálculos de Guía:**

      El código calcula varios parámetros para la fase de ascenso:

      - **`ZDOT`, `YDOT`, `RDOT`**: **Componentes de velocidad**.
      - **`DZDOT`, `DYDOT`, `DRDOT`**: **Componentes delta de velocidad**.
      - **`VE`, `TGO`**: **Velocidad** y **tiempo restante**.
      - **`ATR`, `ATY`**: **Términos de aceleración** en diferentes ejes.

    - **Flags y Control:**

      - **`FLRCS`**: Flag para el **Sistema de Control de Reacción**.
      - **`MAINENG`**: Operación del **motor principal**.
      - **`IDLEFLAG`**: Deshabilita la **monitorización del Delta-V**.
      - **`T2TEST`, `FLPC`, `NORATES`, `RATES`**: Varios flags para controlar la **secuencia de ascenso**.

    - **Manejo y Corrección de Errores:**

      - **`ZATTEROR`**: Errores de **actitud cero**.
      - **`ENGOFF1`**: Secuencia de **apagado del motor**.

    - **Subrutinas y Operaciones:**

      El código llama frecuentemente a varias **subrutinas** para realizar cálculos complejos y manejar tareas específicas:

      - **`PHASCHNG`**: Cambio de **Fase**.
      - **`CHECKALT`**: Comprobar **altitud**.
      - **`CHECKYAW`**: Comprobar **alineación de guiñada**.
  
  - **Código de Ensamblador:**

    - [ASCENT_GUIDANCE.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/ASCENT_GUIDANCE.agc)

- **BURN_BABY_BURN--MASTER_IGNITION_ROUTINE.agc**
  
  El nombre de esta rutina, **"BURN BABY BURN"**, es una referencia a la frase famosa del locutor **Magnificent Montague**, reflejando un toque de **humor histórico y cultural** en la programación del AGC.

  La rutina es **compleja**, manejando diversos escenarios e integrándose con múltiples componentes y tareas. Utiliza una mezcla de **comprobaciones inmediatas**, **gestión de cuenta atrás** y **manejo del estado del sistema** para asegurar una ignición y operaciones posteriores sin problemas.

  - **Propósito:**
  
    La rutina **BURN BABY BURN** gestiona la **ignición del sistema de propulsión** del Módulo Lunar del Apolo. Se encarga de varias tareas relacionadas con la ignición, desde las **comprobaciones de tiempo** (si está dentro de los 45 segundos del tiempo de ignición) hasta los **26 segundos después de la ignición**, cuando el sistema de propulsión comienza a **aumentar su potencia**.

  - **Programas que usan esta Rutina:**

    Se utiliza en los programas del LM: `P12`, `P40`, `P42`, `P61`, `P63`.

  - **Secciones y Funciones Clave**
    - **Tablas para la Rutina de Ignición:**

      Estas tablas (por ejemplo, `P12TABLE`, `P40TABLE`) contienen **constantes** e **instrucciones TCF** (Transfer Control Function) que guían el **comportamiento específico** de la rutina de ignición para diferentes programas.

    - **Rutinas de Ignición de Propósito General:**

      - **`BURNBABY`**: El **punto de entrada** de la rutina. Configura varios parámetros y llama a subrutinas como `P40AUTO` para manejar las tareas de **ignición**.
      - **`TIG-35`**: Maneja la **cuenta atrás** y gestiona situaciones donde la ignición podría ser retrasada.
      - **`TIG-30`, `TIG-30A`, `TIG-5`**: Manejan **verificaciones específicas** del tiempo y el estado del sistema en diferentes etapas de la cuenta atrás.
      - **`P63IGN`, `P40IGN`, `P42IGN`**: Inician procedimientos de **ignición** para programas específicos (`P63`, `P40`, `P42`).

    - **Manejo de Errores y Fallos:**

      - **`COMFAILx`**: Manejan **fallos de comunicación** y otros problemas. Cada tipo de fallo (por ejemplo, `COMFAIL3`, `COMFAIL4`) realiza diferentes acciones como **reiniciar monitores de visualización**, **reconectar pantallas** o **reinicializar componentes**.

    - **Manejo del Tiempo y Tareas:**

      - **`WAITABIT`, `TIGTASK`**: Gestiona **retrasos** y asegura que las tareas se completen en momentos específicos. Por ejemplo, `WAITABIT` asegura que ciertas tareas se retrasen adecuadamente.
      - **`ULLAGOFF`, `COMMON`**: Manejan tareas específicas como **desactivar la detección de ullage** o **tareas comunes** compartidas entre diferentes programas.

    - **Código de Ensamblador:**

      - [BURN_BABY_BURN--MASTER_IGNITION_ROUTINE.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/BURN_BABY_BURN--MASTER_IGNITION_ROUTINE.agc)

- **LANDING_ANALOG_DISPLAYS.agc**
  
    Controlar y actualizar las **pantallas analógicas** que muestran datos críticos a los astronautas durante el **alunizaje**. Estas pantallas proporcionan **información sobre el estado del módulo lunar** y ayudan a los astronautas a **tomar decisiones** durante el proceso de descenso.

  - **Organización del Código:**

    - **Configuración del Banco de Memoria y Localización**

    ```nasm
    BANK    21
    SETLOC  R10
    BANK
    ```

    Aquí, se establece el **banco de memoria 21** y se define la **localización inicial del código en `R10`**. Esto es esencial para asegurarse de que las instrucciones posteriores accedan a la memoria adecuada.

    - **Actualización de TBASE2 y PIPCTR**

    ```nasm
    LANDISP  LXCH  PIPCTR1
    CS      TIME1
    DXCH    TBASE2
    ```

    Este bloque de código actualiza `TBASE2` y `PIPCTR` con valores de `TIME1`. `PIPCTR` es un **contador** para la visualización, mientras que `TBASE2` puede estar relacionado con la **base de tiempo** de las mediciones.

    - **Manejo de la Visualización de Altitud y Tasa de Altitud**

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

    Este bloque verifica si el indicador `FLAGWRD7` está configurado. Si **no está**, se salta a `DISPRSET`. Si **está**, se verifica el modo de visualización en `IMODES33`. Si el **bit 7** está activado, se dirige a `ALTOUT`, que maneja la **visualización de la tasa de altitud**.

    - **Cálculo de la Tasa de Altitud**

    ```nasm
    ALTROUT  TC   DISINDAT
    CS      IMODES33
    MASK    BIT7
    ADS    IMODES33
    CAF    BIT2
    EXTEND
    WOR    CHAN14
    ```

    En esta sección, se calcula la **tasa de altitud** basándose en el comando de tasa (`BIT2`). Se multiplica el componente `VVECT` por `RUNIT` para calcular la tasa de altitud.

    - **Conversión de Datos**

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

    Este fragmento convierte la **tasa de altitud** a unidades de bit mediante un **factor de conversión**. Luego, se almacena en `ALTRATE` para su **visualización**.

    - **Visualización de Datos**

    ```nasm
    DATAOUT TS    ALTM
    CAF    BIT3
    EXTEND
    WOR    CHAN14
    TCF    TASKOVER
    ```

    Aquí, se envía el dato de **tasa de altitud** a la pantalla analógica (`ALTM`). El **bit 3** controla la visualización de la tasa de altitud o altitud.

    - **Manejo de Datos de Velocidad**

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

    Este bloque **calcula la velocidad** utilizando el tiempo y otros parámetros para actualizar la componente de velocidad en el vector `VVECT`.

    - **Monitoreo de Velocidades**

    ```nasm
    VMONITOR  TS   ITEMP5
    CCS   LATVEL
    TCF   +4
    TCF   LVLIMITS
    ```

    Este segmento realiza un **monitoreo** de las velocidades lateral y de avance. **Verifica** que los valores estén dentro de los límites aceptables y **actualiza** las pantallas en consecuencia.

    - **Manejo de Errores y Salida**

    ```nasm
    DISPRSET  CS   FLAGWRD0
    MASK  R10FLBIT
    EXTEND
    BZF   ABORTON
    ```

    Aquí se **gestionan los errores** y se realiza una **limpieza de los indicadores**. Si hay un error o se ha terminado el proceso, el código se dirige a `TASKOVER`.

  - **Código de Ensamblador:**

    - [LANDING_ANALOG_DISPLAYS.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/LANDING_ANALOG_DISPLAYS.agc)

- **THE_LUNAR_LANDING.agc**
  
    El archivo **`THE_LUNAR_LANDING.agc`** se encarga de la **fase de alunizaje** del Módulo Lunar durante las misiones Apollo. Incluye la **preparación**, el **cálculo de correcciones de guía**, la **verificación de componentes** y la **confirmación final del alunizaje**. El código está dividido en bloques que manejan la **configuración**, el **cálculo de guía**, la **verificación de componentes** y la **confirmación del alunizaje**, asegurando así una **transición precisa y controlada** a la fase de aterrizaje en la Luna.

  - **Guía e Integración:**
    - La sección **`IGNALG`** es responsable de **configurar las entradas** para el algoritmo de guía y realizar los **cálculos iniciales** para el alunizaje.
    - Integra el **estado hacia adelante** hasta el tiempo de alunizaje y **configura varias variables** utilizadas en los cálculos de guía.

  - **Corrección y Ajuste de Errores:**
    - **`DDUMCALC`** realiza **cálculos para el algoritmo de guía**, manejando términos de corrección y actualizando estimaciones para la guía del alunizaje.
    - Calcula y almacena **parámetros necesarios** para **refinar el enfoque de alunizaje** y corregir cualquier error.

  - **Procedimientos Finales:**
    - La sección **`P63SPOT2`** inicializa el sistema para la **actitud de alunizaje** y alinea la nave espacial.
    - **`P63SPOT3`** y **`P63SPOT4`** manejan la **inicialización del radar de alunizaje** y verifican si los **sistemas específicos están en la posición correcta**.

  - **Confirmación de Alunizaje:**
    - La sección **`LANDJUNK`** maneja los **pasos finales para la confirmación del alunizaje**, estableciendo **flags** y realizando las **últimas verificaciones** antes del alunizaje.
    - Calcula la **posición final del sitio de alunizaje** y verifica si el **alunizaje ha sido exitoso**.

  - **Explicación de las Secciones Clave:**
    - **`P63LM` (Fase de Alunizaje del Módulo Lunar):**
      - Este es el **subprograma principal** para la fase de alunizaje. Se encarga de:
        - **Verificar el estado del IMU** (Unidad de Medición Inercial).
        - **Inicializar parámetros necesarios** para la fase de alunizaje.
        - **Coordinarse con los programas de guía** para asegurar un alunizaje preciso.

    - **`IGNALG` (Algoritmo de Guía Inicial):**
      - Este subprograma **configura e integra los cálculos iniciales de guía**, necesarios para la fase de alunizaje. Sus tareas incluyen:
        - **Realizar cálculos y correcciones** basados en el estado actual de la nave espacial.
        - Asegurar que los procedimientos de alunizaje sean **precisos** mediante la configuración del algoritmo de guía.

    - **`DDUMCALC` (Cálculo de Dummy):**
      - Este subprograma realiza **cálculos específicos relacionados con la guía**, que incluyen:
        - **Manejo de términos de corrección** e integración de parámetros.
        - Ajustes necesarios para el **algoritmo de guía** durante el alunizaje.

    - **`P63SPOT2`, `P63SPOT3`, y `P63SPOT4`:**
      - Estas secciones se encargan de **tareas específicas de configuración**, tales como:
        - **`P63SPOT2`**: Inicializa el sistema para la **actitud de alunizaje** y alinea la nave espacial.
        - **`P63SPOT3`**: Verifica la posición de la **antena LR** y realiza ajustes según sea necesario.
        - **`P63SPOT4`**: Maneja la **inicialización del radar de alunizaje** y confirma que todos los sistemas están listos para el alunizaje.

    - **`LANDJUNK` (Confirmación de Alunizaje):**
      - La **fase final** del proceso de alunizaje donde se realiza la **confirmación del alunizaje**. Incluye:
        - El **cálculo de la posición final** del sitio de alunizaje.
        - **Verificación de que el alunizaje ha sido exitoso**.
        - Establecimiento de **flags** y realización de las **últimas comprobaciones** antes del alunizaje final.

  - **Explicación Detallada del Código:**
    - **Configuración Inicial:**

      ```nasm
      BANK    32
      SETLOC  F2DPS*32
      BANK

      EBANK=  E2DPS
      ```

      - `BANK 32`: Selecciona el **banco de memoria 32** para las siguientes operaciones.
      - `SETLOC F2DPS*32`: Establece la **dirección de localización** en la memoria.
      - `EBANK= E2DPS`: Define el **banco de memoria extendida**.

    - **Fase de Alunizaje, Preparación y Configuración:**

      ```nasm
      COUNT*  $$/P63
      ```

      - `COUNT* $$/P63`: Marca el comienzo de la **sección del código P63**, que se refiere a la **fase de alunizaje**.

      ```nasm
      P63LM   TC  PHASCHNG
          OCT 04024
      ```

      - `P63LM TC PHASCHNG`: Transición a la **fase de alunizaje (P63)**, con el **código de cambio de fase**.

      ```nasm
      TC  BANKCALL  # DO IMU STATUS CHECK ROUTINE R02
      CADR R02BOTH
      ```

      - `TC BANKCALL CADR R02BOTH`: Llama a la rutina **R02** para verificar el estado del **IMU (Unidad de Medición Inercial)**.

      ```nasm
      CAF P63ADRES  # INITIALIZE WHICH FOR BURNBABY
      TS  WHICH
      CAF DPSTHRSH  # INITIALIZE DVMON
      TS  DVTHRUSH
      CAF FOUR
      TS  DVCNTR
      ```

      - `CAF P63ADRES TS WHICH`: Carga y almacena la **dirección** para la fase de alunizaje.
      - `CAF DPSTHRSH TS DVTHRUSH`: Carga y almacena el **umbral** para el monitoreo del DV.
      - `CAF FOUR TS DVCNTR`: Inicializa el **contador del DV** con el valor 4.

      ```nasm
      CS ONE    # INITIALIZE WCHPHASE AND FLPASS0
      TS  WCHPHASE
      CA  ZERO
      TS  FLPASS0
      ```

      - `CS ONE TS WCHPHASE`: Inicializa la **fase de guía**.
      - `CA ZERO TS FLPASS0`: Inicializa el **flag de paso**.

      ```nasm
      CS BIT14
      EXTEND
      WAND CHAN12    # REMOVE TRACK-ENABLE DISCRETE.
      ```

      - `CS BIT14 EXTEND WAND CHAN12`: Manipula el **canal** para **deshabilitar el seguimiento**.

    - **Configuración de Flags y Parámetros:**

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

      - `FLAGORGY TC INTPRET`: Configura **banderas para la fase de alunizaje**.
      - `CLEAR`: Limpia los **flags** y realiza **ajustes necesarios**.

    - **Algoritmo de Guía Inicial (`IGNALG`):**

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

      - `IGNALG SETPD VLOAD`: Configura el **algoritmo de guía inicial**.
      - `PDDL PUSH TLAND`: Establece el **tiempo estimado de alunizaje**.
      - `STCALL TPIP`: Configura `TPIP` para el **primer pase de guía**.
      - `STCALL LAND GUIDINIT`: Inicializa el **sistema de guía**.
      - `DLOAD DSU TLAND GUIDDURN`: Carga datos para la **duración de la guía** y el **tiempo**.

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

      - `SSP VLOAD NIGNLOOP 40D UNITX`: Inicializa los **parámetros necesarios para el cálculo de la guía**.
      - `STOVL CG UNITY`: Configura los **valores de los vectores de guía**.

    - **Bucle de Cálculo y Corrección (`IGNALOOP`):**

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

      - `IGNALOOP DLOAD TAT STOVL PIPTIME1`: Realiza **cálculos continuos y ajustes de tiempo**.
      - `STCALL R MUNGRAV`: Llama a la **rutina para calcular la gravedad lunar**.

    - **Cálculo de Corrección Dummy (`DDUMCALC`):**

      ```nasm
      DDUMCALC TS NIGNLOOP
        TC INTPRET
        DLOAD DMPR  # FORM DENOMINATOR FIRST
            VGU
            KIGNX/B4
      ```

      - `DDUMCALC TS NIGNLOOP TC INTPRET`: Configura el **cálculo de corrección dummy**.
      - `DLOAD DMPR`: Calcula el **denominador** para la corrección.

    - **Procedimientos de Verificación y Finalización:**

      ```nasm
      P63SPOT2 VLOAD UNIT
        R60VSAVE
      STOVL POINTVSM
              UNITX
      STORE  SCAXIS
      EXIT
      ```

      - `P63SPOT2 VLOAD UNIT R60VSAVE`: Inicializa el **cálculo de actitud de alunizaje**.

      ```nasm
      P63SPOT3 CA BIT6   # IS THE LR ANTENNA IN POSITION 1 YET
      EXTEND
      RAND CHAN33
      EXTEND
      BZF P63SPOT4  # BRANCH IF ANTENNA ALREADY IN POSITION 1
      ```

      - `P63SPOT3 CA BIT6 EXTEND RAND CHAN33`: Verifica si la **antena** está en la **posición correcta**.

      ```nasm
      P63SPOT4 TC BANKCALL  # ENTER  INITIALIZE LANDING RADAR
      CADR SETPOS1
      TC POSTJUMP  # OFF TO SEE THE WIZARD...
      CADR BURNBABY
      ```

      - `P63SPOT4 TC BANKCALL CADR SETPOS1`: Inicializa el **radar de alunizaje** y transita al **siguiente paso**.

    - **Confirmación de Alunizaje (`LANDJUNK`):**

      ```nasm
      LANDJUNK TC PHASCHNG
              OCT 04024
      ```

      - `LANDJUNK TC PHASCHNG`: Marca la **fase de confirmación del alunizaje**.

      ```nasm
      INHINT
      TC BANKCALL  # ZERO ATTITUDE ERROR
      CADR ZATTEROR

      TC BANKCALL  # SET 5 DEGREE DEADBAND
      CADR SETMAXDB
      ```

      - `INHINT TC BANKCALL CADR ZATTEROR`: Llama a la **rutina para corregir el error de actitud** y ajustar el **rango muerto de 5 grados**.

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

      - `SET CLEAR SURFFLAG LETABORT`: Configura las **banderas finales** para la confirmación.
      - `SET VLOAD APSFLAG RN STODL ALPHAV PIPTIME`: Configura y almacena los **parámetros finales del alunizaje**.

      ```nasm
      CAF V06N43*   # ASTRONAUT: NOW LOOK WHERE YOU ENDED UP
      TC BANKCALL
      CADR GOFLASH
      TCF GOTOPOOH  # TERMINATE
      TCF +2        # PROCEED
      TCF -5        # RECYCLE
      ```

      - `CAF V06N43 TC BANKCALL CADR GOFLASH*`: Muestra la **posición final** y permite **proceder o reciclar** según sea necesario.

    - **Constantes y Parámetros:**

      ```nasm
      P63ADRES GENADR P63TABLE

      ASTNDEX = MD1   # OCT 25: INDEX FOR CLOKTASK

      CODE500  OCT 00500

      99999CON 2DEC 30479.7 B-24

      GUIDDURN 2DEC +66440  # GUIDDURN +6.64400314 E+2
      DDUMCRIT 2DEC +8 B-28  # CRITERION FOR IGNALG CONVERGENCE
      ```

      - `P63ADRES GENADR P63TABLE`: Define la **dirección para el proceso de alunizaje**.
      - `CODE500 OCT 00500`: Define **constantes necesarias para el proceso**.
      - `99999CON 2DEC 30479.7 B-24`: Establece una **constante para el cálculo**.

      ```nasm
      LANDJUNK TC PHASCHNG
              OCT 04024
      ```

      - `LANDJUNK TC PHASCHNG`: Marca la **fase de confirmación del alunizaje**, indicando que el proceso de alunizaje ha llegado a su fin.

  - **Código de Ensamblador:**

    - [THE_LUNAR_LANDING.agc](https://github.com/chrislgarry/Apollo-11/blob/master/Luminary099/THE_LUNAR_LANDING.agc)

### Archivos del Módulo de Comando y Servicio (CSM)

El **software Comanche 055** es otra pieza crucial en la historia de la exploración espacial, ya que fue el software de vuelo utilizado en el Módulo de Comando (CM) durante las misiones Apollo.

![CM](/assets/img/post/apollo11/21.jpg)

El término **"Comanche"** hace referencia a una tribu nativa americana conocida por su habilidad en la **navegación** y **estrategia** en el terreno, cualidades que son paralelas a las necesidades del Módulo de Comando en las misiones Apollo. Esta denominación simboliza la misión del software de **guiar y proteger** a los astronautas durante sus viajes a la Luna y su regreso a la Tierra, de la misma manera que los Comanches eran maestros en guiar a su gente a través de vastas regiones.

Al igual que con el número del **Luminary 099**, el **055** representa una versión específica del software del Módulo de Comando.

Para entender cómo el **Módulo de Comando y Servicio (CSM)** del Apollo 11 manejó sus complejas tareas, exploraremos algunos de los archivos más relevantes del software **Comanche 055**. Estos archivos son fundamentales para comprender el funcionamiento del CSM durante la misión. A continuación, presentamos una **selección de los archivos más importantes e interesantes**:

- **CM_BODY_ATTITUDE.agc**

  El propósito de este código es **actualizar y manejar los ángulos de orientación** del módulo de comando en respuesta a las lecturas de los **acelerómetros** y **giroscopios**. El código guarda las **posiciones actuales** y calcula los **cambios en la orientación** basándose en estas lecturas, manteniendo la **estabilidad y control** necesarios para las maniobras durante la misión.

  - **Sección y Subrutina**

    - **Sección Principal:** Control de la Actitud del Cuerpo (`CM_BODY_ATTITUDE`)

    - **Subrutina Principal:** `CM/ATUP` (Actualización de los Ángulos de Actitud)

  - **Descripción Funcional**

    - **Guardado de Ángulos y Valores de Gimbal:**

      Los ángulos de gimbal y del cuerpo válidos en el momento `PIP` son guardados durante la lectura de los acelerómetros (`READACCS`).

    - **Configuración Inicial y Cálculo de Vectores:**
      - El intérprete establece el banco correcto.
      - Se realizan cálculos vectoriales y se almacenan los resultados para su uso en la guía de entrada y coordenadas de referencia.

    - **Cálculos de Trayectoria y Ángulos:**
      - Se calculan los vectores relativos y se almacenan los resultados.
      - Se obtienen triadas de trayectoria y se calculan los componentes del movimiento del módulo.

    - **Actualización de Ángulos:**
      - La subrutina `CM/ATUP` se encarga de actualizar los ángulos de actitud (`ROLL`, `BETA`, `ALFA`) basándose en los datos obtenidos desde la última actualización.
      - Se manejan las interrupciones y se aseguran los cálculos precisos corrigiendo los posibles desbordamientos.

    - **Corrección y Ajuste de Ángulos:**
      - La subrutina `CM/ATUP` se encarga de actualizar los ángulos de actitud (`ROLL`, `BETA`, `ALFA`) basándose en los datos obtenidos desde la última actualización.
      - Se manejan las interrupciones y se aseguran los cálculos precisos corrigiendo los posibles desbordamientos.

  - **Detalles Técnicos**
    - **`KVSCALE`:** Constante utilizada para la escala de velocidad.
    - **`TCDU`:** Tiempo de ciclo de unidad (0.1 segundos).
    - **Subrutinas Específicas:**
      - `CM/POSE`, `CM/POSE2`, `CM/POSE3`: Manejan diferentes etapas del cálculo de orientación.
      - `DOGAMDOT`, `NOGAMDOT`: Deciden si calcular o no la velocidad angular en base a ciertas condiciones.
      - `REDOPOSE`, `CORANGOV`: Ajustan los ángulos y manejan los posibles desbordamientos.

  - **Código de ensamblador:**

    - [`CM_BODY_ATTITUDE.agc`](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/CM_BODY_ATTITUDE.agc)

- **IMU_CALIBRATION_AND_ALIGNMENT.agc**

  Esta porción de código del AGC se usa para la _Calibración y Alineación de la IMU_, diseñada específicamente para soportar varias pruebas de rendimiento y rutinas relacionadas con la Unidad de Medición Inercial (IMU) para el Módulo de Comando del Apollo. A continuación, presento una explicación de las secciones clave y sus propósitos:

  - **Pruebas de Rendimiento de la IMU:**
  
    - **Inicialización:** El código inicializa las pruebas de la IMU, configurando varios contadores y flags a cero. Esto incluye el `NDXCTR` (contador de índice) y el `TORQNDX` (índice de torque).

    - **Rutinas de Posicionamiento:** Se utilizan varias rutinas para posicionar y alinear la IMU. El programa configura y verifica varios componentes geométricos y realiza cálculos como seno y coseno de la latitud, azimut y otros parámetros de alineación.

    - **Pruebas de Deriva del Giroscopio:** El código realiza pruebas del giroscopio para asegurar que los giroscopios funcionen correctamente. Esto implica configurar temporizadores (`LENGTHOT`), capturar y procesar pulsos `PIPA` (Acelerómetro Péndulo Integrador Pulsado).

    - **Compensación de la Velocidad de la Tierra:** El código compensa la velocidad de rotación de la Tierra calculando y aplicando correcciones a las lecturas de la IMU. Esto asegura un posicionamiento y alineación precisos a pesar del movimiento de la Tierra.

  - **Manejo de Datos y Prueba de Reinicio:**

    - **Prueba de Reinicio:** Para manejar posibles reinicios, el código incluye rutinas para almacenar y recargar datos críticos. Esto asegura que el sistema pueda reanudar pruebas o procedimientos de alineación sin comenzar desde cero.

    - **Almacenamiento y Carga:** Los datos relacionados con la alineación y calibración de la IMU se almacenan periódicamente y se pueden recargar para continuar las operaciones sin problemas después de un reinicio.
  
  - **Procedimientos de Alineación y Calibración:**

    - **Alineación Gruesa (`COAALIGN`):** Esta rutina maneja la alineación inicial de la IMU, configurando la orientación y asegurando que la plataforma esté correctamente alineada.

    - **Alineación Fina (`IMUFINE`):** Después de la alineación gruesa, la rutina de alineación fina ajusta la orientación de la IMU con mayor precisión.

    - **Pruebas `PIPA` (`PIPACHK`):** Esta rutina prueba el `PIPA` para medir tasas verticales y asegurar la precisión de la IMU.
  
  - **Desglose Detallado:**

    - **Inicialización (`IMUTEST`):** Limpia y configura valores iniciales para tiempos de deriva, componentes geográficos y otros parámetros de prueba.

    - **Pruebas de Deriva del Giroscopio (`VERTDRFT`, `TORQUE`):** Realiza pruebas para medir y compensar la deriva en los giroscopios.

    - **Compensación de la Velocidad de la Tierra (`ERTHRVSE`, `EARTHR`):** Calcula la compensación necesaria para la rotación de la Tierra para mantener lecturas precisas de la IMU.

    - **Procedimientos de Alineación (`COAALIGN`, `IMUFINE`):** Alinea inicialmente la IMU y ajusta la alineación para operaciones precisas.

    - **Captura de Pulsos `PIPA` (`CHECKG`, `PIPACHK`):** Captura y procesa pulsos `PIPA` para asegurar que la IMU mida con precisión la aceleración y orientación.

    - **Prueba de Reinicio (`STOREDTA`, `LOADSTDT`):** Asegura que, en caso de un reinicio, los datos críticos de alineación y calibración se guarden y puedan recargarse para reanudar operaciones rápidamente.
  
  - **Código de ensamblador:**

    - [`IMU_CALIBRATION_AND_ALIGNMENT.agc`](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/IMU_CALIBRATION_AND_ALIGNMENT.agc)

- **ORBITAL_INTEGRATION.agc**

  Este código se divide en distintas **"páginas"** que realizan diferentes funciones. A continuación, se explica de manera detallada cada página o conjunto de ellas:

  - **Páginas 1334-1336: `ORBITAL INTEGRATION` y `KEPPREP`:**

    Estas páginas configuran las rutinas para la _integración orbital_, un proceso clave en la navegación espacial. La rutina `KEPPREP` parece preparar variables y realizar cálculos preliminares necesarios para la integración de las órbitas.

    - **Bancos de Memoria:** El código usa diferentes bancos de memoria para almacenar datos específicos.

    - **Cargas y Almacenamientos:** Se cargan y almacenan valores en diferentes ubicaciones de memoria, que incluyen cálculos de varios físicos, como la raíz cuadrada de la constante gravitacional.

  - **Páginas 1337: `ACCOMP`:**

    La rutina `ACCOMP` calcula los componentes de aceleración. Utiliza vectores de posición y velocidad, y realiza operaciones vectoriales como multiplicaciones y sumas.

    - **Carga de Vectores:** Carga y almacena vectores de aceleración.

    - **Normalización:** Normaliza los vectores para los cálculos posteriores.

  - **Páginas 1338-1340: `GAMCOMP`:**

    Esta subrutina calcula componentes específicos de la aceleración gravitacional (`GAMCOMP`).

    - **Cálculo de la Aceleración:** Realiza cálculos complejos para determinar la aceleración debido a la gravedad y otras fuerzas.

    - **Normalización:** Asegura que los valores calculados estén en un rango normalizado.

  - **Página 1341: `OBLATE`:**

    La rutina `OBLATE` calcula la aceleración debido a la _oblación terrestre_ (achatamiento de la Tierra en los polos).

    - **Cálculo de Componentes de Oblateness:** Usa el vector de posición del vehículo y su distancia al centro de la Tierra para calcular la aceleración adicional debido a la forma oblata de la Tierra.

    - **Ajustes y Sumas:** Ajusta los valores de aceleración calculados y los suma a las aceleraciones existentes.

  - **Página 1343: `COMTERM` y `COSPHIE`:**

    Estas secciones manejan cálculos adicionales y ajustes específicos relacionados con los efectos del oblateness.

    - **Cálculos de Términos Adicionales:** Calcula y ajusta términos adicionales necesarios para la precisión en la integración orbital.

  - **Páginas 1344-1345: `TIMESTEP` y `RECTEST`:**

    Estas rutinas gestionan los pasos de tiempo en la integración y verifican si se necesita una rectificación en la órbita calculada.

    - **Verificaciones y Ajustes:** Verifica condiciones específicas que podrían requerir ajustes en la órbita calculada.

    - **Integración:** Realiza la integración de la órbita utilizando pasos de tiempo específicos.

  - **Página 1346: `ORIGCHNG`:**

    La rutina `ORIGCHNG` maneja los cambios de origen, es decir, cuando el sistema de referencia necesita ser ajustado debido a cambios significativos en la posición o velocidad.

    - **Ajustes de Origen:** Ajusta el sistema de referencia y recalcula los vectores de posición y velocidad en relación con el nuevo origen.

  - **Página 1347: `RECTIFY`:**

    La subrutina `RECTIFY` establece una nueva órbita cónica cuando se detectan cambios significativos.

    - **Reestablecimiento de Órbita:** Establece una nueva órbita cónica basada en los cálculos actuales de posición y velocidad.

    - **Reinicialización:** Reinicializa las variables y prepara el sistema para la siguiente etapa de integración.
  
  - **Código de ensamblador:**

    - [`ORBITAL_INTEGRATION.agc`](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/ORBITAL_INTEGRATION.agc)

- **TVCEXECUTIVE.arg**

  `TVCEXECUTIVE` es una rutina crítica del AGC que se encarga de gestionar y actualizar el sistema de control de actitud y navegación (TVC) del Módulo de Comando. Esta rutina se ejecuta a intervalos regulares de _0.5 segundos_ y realiza varias tareas esenciales:

  - **Preparación de Datos de Control:**

    - **Actualización de `Roll DAP`:** Se prepara el `DAP` (Data Access Processor) para el control del roll del módulo con los datos actuales.

    - **Actualización del `FDAI Needle`:** Se actualiza la aguja del Indicador de Actitud de Vuelo (`FDAI`) con el error de control de actitud del roll.

    - **Actualización del `Phase Plane`:** Se prepara el plano de fase del roll para el error `OGA` (Error de Guía de Actitud).

  - **Actualización de Necesidades y Datos:**

    - **Actualización de `Needles`:** Llama a una subrutina que actualiza los indicadores de actitud.

    - **Actualización de `Masa`:** Actualiza la masa del vehículo y, por ende, los datos de inercia.

    - **Actualización de `Ganancias`:** Actualiza las ganancias de control de actitud en `pitch` (elevación), `yaw` (guía) y `roll`, basándose en los datos de masa actualizados.

  - **Corrección y Reajustes:**

    - **Corrección de Un Solo Disparo:** Realiza ajustes únicos en el bucle `TMC` (Throttle Maneuver Control) poco después de la ignición.

    - **Corrección Repetitiva:** Ejecuta correcciones repetitivas para el bucle `TMC` después de la corrección de un solo disparo.
  
  - **Llamadas y Secuencia de Ejecución:**

    - `TVCEXEC` es llamado como una tarea en la lista de espera por `TVCINIT4` y por sí mismo a intervalos de _0.5 segundos_.

      - **Verificación de Terminación:** El código primero verifica si debe terminarse, usando un chequeo de bits en la palabra de bandera (`FLAGWRD6`).

      - **Preparación de Datos:** Luego prepara diversos datos y actualiza los indicadores y sistemas necesarios.

    - **Actualización y Manejo de Errores:**

      - **Preparación del `DAP`:** Actualiza el `DAP` y otros componentes con los errores actuales y los datos de control.

      - **Actualización de `Needles`:** Llama a una subrutina para actualizar los indicadores de actitud.

    - **Manejo de Ganancias y Correcciones:**

      - **Chequeo del Estado del Motor:** Verifica si el motor está encendido para decidir si se deben actualizar las ganancias y la masa.

      - **Actualización de Ganancias y Masa:** Realiza ajustes basados en el estado actual del motor y actualiza las ganancias y datos de masa.

    - **Correcciones de Un Solo Disparo y Repetitivas:**

      - **Chequeo de Tiempo:** Verifica el tiempo para decidir cuándo realizar una corrección de un solo disparo o una corrección repetitiva.

      - **Ajustes y Correcciones:** Realiza correcciones según sea necesario, basándose en el estado de las correcciones anteriores.

    - **Subrutinas Importantes:**

      - `NEEDLER`: Actualiza los indicadores de actitud (`needles`).
      - `MASSPROP`: Actualiza la masa y las propiedades relacionadas.
      - `IBNKCALL`: Llama a una subrutina de banco de instrucciones para realizar operaciones específicas.

    - **Salidas y Variables:**

      - **`ROLL DAP`, `FDAI Needle`, y `Phase Plane`:** Se actualizan con los valores más recientes para el control del roll.
      - **Ganancias Variables:** Se ajustan para el `pitch`, `yaw` y `roll` basados en los datos más recientes.
      - **Correcciones de Ángulo de Motor:** Se ajustan los ángulos de trim del motor y otros parámetros relevantes.
  
  - **Código de ensamblador:**

    - [`TVCEXECUTIVE.agc`](https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/TVCEXECUTIVE.agc)

## Influencia en Futuras Misiones Espaciales

![CM](/assets/img/post/apollo11/22.jpg)

1. **Innovaciones en Navegación y Control Espacial**

   El **AGC** revolucionó la forma en que se realizaban las maniobras espaciales. Su capacidad para realizar cálculos complejos y proporcionar **orientación precisa** permitió a las misiones Apollo realizar maniobras críticas con un grado de **_exactitud sin precedentes_**. Esta tecnología de control y navegación se convirtió en un estándar para la planificación de misiones espaciales posteriores, incluyendo la **_navegación interplanetaria_**. Las técnicas desarrolladas para el **AGC** se adaptaron y mejoraron para misiones espaciales posteriores, como las **_sondas Voyager_** y las **_misiones a Marte_**.

2. **Desarrollo de Sistemas de Control de Actitud**

    El AGC también sentó las bases para el desarrollo de _sistemas avanzados de control de actitud_ en vehículos espaciales. Su sistema de **Control de Actitud y Navegación (CAN)** se utilizó para mantener la orientación y trayectoria de la nave espacial, un principio que sigue siendo esencial en las naves espaciales modernas. Los _algoritmos de control adaptativos_ y las _técnicas de compensación de errores_ desarrolladas para el AGC se emplearon en las misiones de la **Estación Espacial Internacional (EEI)** y en los _satélites actuales_.

3. **Influencia en la Exploración Robótica**

    Las tecnologías y metodologías desarrolladas para el **AGC** también influyeron en los sistemas de control de las **_sondas espaciales_** y los **_robots_**. Las primeras misiones espaciales robóticas, como las **_sondas Mariner_** y **_Pioneer_**, adoptaron técnicas de control basadas en las experiencias del **AGC**. La **_precisión en la navegación_** y el **_procesamiento de datos_** inspiraron avances en la robótica espacial, que ahora permite a los **_rovers_** explorar Marte y otras superficies planetarias con alta autonomía.

4. **Implementación en el Diseño de Satélites**

    Los **_principios de diseño_** del **AGC** se incorporaron en los sistemas de control de **_satélites_**. La capacidad del **AGC** para realizar cálculos de manera eficiente y controlar sistemas críticos a bordo influyó en el diseño de **_satélites de comunicaciones_** y **_observación de la Tierra_**. Los satélites modernos utilizan técnicas de procesamiento similares para manejar sus operaciones, mantener la estabilidad y transmitir datos con precisión.

## Impacto y Legado del programa Apollo

  ![CM](/assets/img/post/apollo11/23.jpg)

1. **Pionero en la Informática Embebida**

    El **AGC** es reconocido como uno de los primeros ejemplos de _informática embebida_. Diseñado para realizar tareas específicas dentro de un sistema mayor, el **AGC** demostró la viabilidad de utilizar computadoras especializadas para realizar funciones críticas en _tiempo real_. Este concepto se ha expandido enormemente, y la _informática embebida_ ahora se utiliza en una amplia gama de dispositivos, desde _electrodomésticos_ hasta _automóviles_ y _sistemas de control industrial_.

2. **Avances en Arquitectura de Computadoras**

    La **arquitectura** del **AGC** fue revolucionaria en su tiempo. Utilizaba una _arquitectura de palabra_ de `15 bits` con una memoria de solo `64 KB`, lo que fue extremadamente compacto en comparación con las computadoras de la época. El diseño del **AGC**, con su enfoque en la _simplicidad_ y la _eficiencia_, influyó en la evolución de las arquitecturas de computadoras de sistemas embebidos y en la optimización de recursos en entornos de hardware limitados.

3. **Desarrollo de Software y Algoritmos**

    El desarrollo del software para el **AGC**, incluyendo su sistema operativo y sus algoritmos de control, fue un avance significativo en la programación de sistemas críticos. El **AGC** empleó el uso de _programación en lenguaje ensamblador_ y técnicas avanzadas de _gestión de interrupciones_ y _prioridades_. Estas prácticas sentaron las bases para el desarrollo de software en _sistemas embebidos_ y en _tiempo real_.

4. **Inspiración para Innovaciones Tecnológicas**

    El éxito del **AGC** inspiró a generaciones de _ingenieros_ y _científicos_. La forma en que el **AGC** superó los desafíos técnicos de su época impulsó la investigación y el desarrollo en campos relacionados con la _informática_, la _ingeniería de control_ y la _electrónica_. La narrativa de cómo un equipo pequeño y dedicado pudo lograr avances significativos en tecnología con recursos limitados sigue siendo una fuente de **inspiración** para la **innovación** y el **ingenio** en la ingeniería y la tecnología.

5. **Legado en la Cultura Popular y la Educación**

    El **AGC** también ha tenido un _impacto cultural_ significativo. Su papel en las misiones **Apollo** ha sido celebrado en numerosos _documentales_, _películas_ y _libros_. Además, el estudio del **AGC** y su _código fuente_ se ha convertido en una _herramienta educativa_ valiosa para enseñar sobre la historia de la informática, el diseño de sistemas embebidos y la ingeniería espacial.

6. **Contribuciones a la Arquitectura de Redes y Comunicaciones**

    Aunque no directamente, el **AGC** influyó en el desarrollo de las redes de comunicaciones. La necesidad de transmitir datos entre la nave espacial y el **control de misión** en la Tierra inspiró avances en las técnicas de _comunicación espacial_ y en la forma en que se gestionan las _redes de datos_ en sistemas críticos.

## Conclusión

![CM](/assets/img/post/apollo11/24.jpeg)

**La misión Apollo 11** fue un logro monumental en la historia de la humanidad, siendo el primer aterrizaje del hombre en la Luna el **20 de julio de 1969**.

Un componente esencial para este éxito fue el _Apollo Guidance Computer (AGC)_, uno de los primeros ordenadores digitales embarcados. Este dispositivo permitió manejar las complejidades del vuelo espacial con una eficiencia y fiabilidad extraordinarias para su época. Su **diseño compacto** y su capacidad para ejecutar múltiples tareas a través de una _interfaz intuitiva_ demostraron la importancia de una buena interacción entre el hombre y la máquina.

El desarrollo del AGC y la misión Apollo 11 fueron **ejemplos sobresalientes de colaboración interdisciplinaria**, donde ingenieros, científicos, programadores y astronautas trabajaron juntos bajo la coordinación de la NASA.

Los avances tecnológicos y los conocimientos adquiridos durante esta misión han dejado un _legado duradero_ en la exploración espacial y en la tecnología informática. Las lecciones aprendidas siguen influyendo en las misiones espaciales actuales.

El éxito de Apollo 11 y el papel crucial del AGC continúan _inspirando a nuevas generaciones_ de científicos y entusiastas del espacio, recordándonos la capacidad humana para superar límites y alcanzar nuevas fronteras mediante la _innovación y el trabajo en equipo_.

_Espero que os haya gustado leer el artículo tanto como a mi hacerlo. ¡Gracias por leer!_

## Webgrafía

[The Apollo Guidance Computer: Architecture and Operation](https://nss.org/book-review-the-apollo-guidance-computer-architecture-and-operation/)

[https://github.com/chrislgarry/Apollo-11](https://github.com/chrislgarry/Apollo-11)

[http://www.ibiblio.org/apollo/#gsc.tab=0](http://www.ibiblio.org/apollo/#gsc.tab=0)

[https://www.nasa.gov/history/alsj/main.html](https://www.nasa.gov/history/alsj/main.html)

[https://ntrs.nasa.gov/citations/19880069935](https://ntrs.nasa.gov/citations/19880069935)

[https://www.nasa.gov/history/alsj/alsj-LMdocs.html](https://www.nasa.gov/history/alsj/alsj-LMdocs.html)

[https://www.nasa.gov/the-apollo-program/](https://www.nasa.gov/the-apollo-program/)

[https://hackaday.com/tag/apollo-guidance-computer/](https://hackaday.com/tag/apollo-guidance-computer/)

[https://en.wikipedia.org/wiki/Apollo_Guidance_Computer](https://en.wikipedia.org/wiki/Apollo_Guidance_Computer)

[http://www.righto.com/2019/09/a-computer-built-from-nor-gates-inside.html](http://www.righto.com/2019/09/a-computer-built-from-nor-gates-inside.html)
