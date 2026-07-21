---
title: Arquitecturas de la Invisibilidad: Viaje hacia la compresión de datos
date: 2026-07-19 12:30:00 +0800
categories: [Ciencias de la Computación, Desarrollo de Software]
tags: [Compresion, Rust, Web, Datos]
image:
  path: /assets/img/post/articulo_compresion/Gemini_Generated_Image_obhwucobhwucobhw.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

## 1. Introducción y contexto histórico

### Por qué comprimimos el mundo

Hay una capa de ingeniería que usamos miles de veces al día sin saberlo. Está ahí cuando abres una foto en el móvil, cuando cargas una web, cuando escuchas una canción en streaming o cuando envías un audio de WhatsApp desde una zona con mala cobertura. Nadie piensa en ella, nadie la ve, y sin embargo sostiene buena parte de cómo consumimos información en el mundo moderno. Esa capa es la **compresión de datos**, y este post es un intento de entenderla de verdad: no solo usarla, sino abrirla, mirar dentro.

Antes de tocar una sola línea de código, hace falta responder una pregunta más básica: ¿por qué existe esto? ¿Por qué alguien, en algún momento, decidió que merecía la pena gastar ciclos de procesador en hacer los datos más pequeños en vez de simplemente guardarlos tal cual?

### El problema original: el espacio siempre costó dinero

La respuesta corta es que la información nunca ha sido gratis. Ni el espacio para guardarla, ni el tiempo para transmitirla.

En los primeros tiempos de la computación, el almacenamiento era un recurso brutalmente escaso y caro. Las tarjetas perforadas, las cintas magnéticas, los primeros discos: todo tenía un coste altísimo por cada bit guardado. Un programa o un conjunto de datos que hoy consideraríamos ridículamente pequeño podía representar una inversión económica real en aquella época. En ese contexto, cualquier técnica que permitiera representar la misma información con menos espacio no era una curiosidad académica, era una necesidad de negocio.

Lo mismo pasaba, mucho antes, con la transmisión. Las primeras compañías de telégrafos cobraban por palabra transmitida. Esto, de forma casi accidental, generó uno de los primeros incentivos económicos reales para "comprimir" mensajes: nacieron libros de códigos comerciales donde frases enteras y habituales en el mundo de los negocios se sustituían por una única palabra en clave, reduciendo drásticamente el coste de un telegrama. No es compresión de datos en el sentido moderno, pero es exactamente la misma lógica: **sustituir algo largo y frecuente por algo corto**, y solo mantener la referencia al significado real en un lugar aparte (el libro de códigos).

Ese es el hilo que atraviesa toda la historia que viene ahora: espacio y tiempo cuestan, y donde hay coste, tarde o temprano aparece alguien dispuesto a optimizarlo.

### El precedente que nadie llama "compresión": el código Morse

Aquí hay un dato que sorprende la primera vez que lo lees: el código Morse, tal y como lo conocemos, ya aplicaba el principio central de la compresión moderna más de un siglo antes de que existiera la palabra "algoritmo" en el sentido en que la usamos hoy.

Y hay un matiz interesante en la propia historia: el trabajo de asignar los códigos no fue tanto de Samuel Morse como de su socio, Alfred Vail. El diseño original de Morse pensaba en transmitir números, que después había que buscar en un diccionario para traducirlos a palabras completas — un sistema lento y poco práctico. Fue Vail quien propuso pasar a representar directamente las letras del alfabeto con secuencias cortas de pulsos.

Pero quedaba una pregunta clave: ¿qué letras merecían los códigos más cortos? Para responderla, Vail hizo algo muy poco "teórico" y muy práctico: se acercó a la imprenta de un periódico local en Morristown, Nueva Jersey, y contó cuántas piezas de tipo móvil tenía el impresor de cada letra. Un impresor necesita muchas más piezas de las letras que usa constantemente que de las que apenas usa, así que ese cajón de tipos era, sin que nadie lo hubiera pensado así, un mapa físico de la frecuencia de las letras en inglés.

Con ese mapa, Vail hizo lo obvio en retrospectiva pero brillante en su momento: dio los códigos más cortos a las letras más frecuentes. La "E", la letra más común del inglés, se quedó con el código más corto posible: un único punto. La "T", la segunda más común, un único guión. Las letras raras, como la "Q" o la "Z", se quedaron con secuencias largas de cuatro símbolos.

Setenta años antes de que naciera la teoría de la información, alguien ya había entendido intuitivamente su principio más importante: **no todos los símbolos merecen el mismo coste de representación**. Los frecuentes deberían costar poco; los raros pueden permitirse costar más, precisamente porque aparecen poco.

Guarda esta idea, porque es literalmente el corazón de uno de los algoritmos que vamos a programar más adelante en este post.

### Cronología: de la intuición a la teoría formal

Lo que sigue a partir de aquí es una sucesión de personas resolviendo, cada vez con herramientas más sofisticadas, la misma pregunta que Vail respondió a ojo con un cajón de tipos de imprenta.

### 1948 — Claude Shannon pone la pregunta sobre papel

![FBW](/assets\img\post\articulo_compresion\shannon.png)

El salto más importante de toda esta historia no fue un algoritmo, fue un marco matemático. Claude Shannon, trabajando en los Bell Labs, publicó en 1948 un artículo que fundó la teoría de la información tal y como la conocemos. Su aportación no fue decir cómo comprimir algo, sino definir con precisión matemática qué es la información y, sobre todo, cuál es el límite teórico de cuánto se puede reducir un mensaje sin perder nada de su contenido: **la entropía**.

Esto es más importante de lo que parece a primera vista. Antes de Shannon, "comprimir mejor" era una cuestión de ingenio. Después de Shannon, existe una frontera matemática que ningún algoritmo sin pérdida puede cruzar, sin importar lo ingenioso que sea. Todo lo que viene después en esta cronología es, en el fondo, distintas formas de acercarse a esa frontera.

Le voy a dedicar el próximo capítulo entero a la entropía, porque es la pieza que explica por qué funciona todo lo demás.

#### 1951-1952 — Un estudiante le gana la partida a su profesor

![FBW](/assets\img\post\articulo_compresion\DavidHuffman.png)

Esta es, para mí, la mejor historia de toda la cronología. En 1951, David Huffman era estudiante de posgrado en el MIT, en un curso de teoría de la información impartido por Robert Fano — uno de los grandes nombres del campo, que además había trabajado codo con codo con el propio Shannon. Fano dio a sus alumnos una opción: examen final, o un trabajo de curso sobre cómo encontrar el código binario más eficiente posible para representar símbolos según su frecuencia.

Lo que Fano no les contó es que ese problema seguía abierto. Ni siquiera él, que llevaba tiempo trabajando en ello, tenía una solución óptima de hecho, el propio método que él había propuesto (hoy conocido como codificación Shannon-Fano) era una buena aproximación, pero no la mejor posible.

Huffman, sin saber que se estaba enfrentando a un problema sin resolver, pasó meses dándole vueltas sin éxito. Según su propio relato, estaba a pocos días del examen final, ya dispuesto a tirar la toalla y ponerse a estudiar, cuando tuvo el golpe de intuición que necesitaba: en vez de construir el código de arriba hacia abajo (como hacía el método de su profesor), había que construirlo de abajo hacia arriba, combinando primero los símbolos menos frecuentes en un árbol binario.

![FBW](/assets\img\post\articulo_compresion\hufman.png)

El resultado, publicado en 1952, no solo resolvía el problema demostrablemente daba el código óptimo, mejor que el método que su propio profesor llevaba tiempo desarrollando.

### 1977-1978 — Lempel y Ziv cambian la pregunta

![FBW](/assets\img\post\articulo_compresion\1.png)

Huffman resolvió cómo aprovechar la frecuencia de símbolos individuales. Pero un texto no es solo una bolsa de letras sueltas: tiene palabras repetidas, frases que vuelven a aparecer, estructuras que se repiten. Abraham Lempel y Jacob Ziv, en 1977 y 1978, publicaron dos algoritmos (conocidos hoy como `LZ77` y `LZ78`) que atacaban el problema desde un ángulo distinto: en vez de mirar símbolos individuales, buscar secuencias completas que ya hubieran aparecido antes en los datos, y sustituirlas por una referencia corta a dónde aparecieron y cuánto medían.

Es el nacimiento de la **compresión "por diccionario"**, y es un cambio de paradigma real: ya no se trata de cuántas veces aparece una letra, sino de cuánto se repite un patrón completo.

### 1984 — LZW y el algoritmo que llegó a todas partes

![FBW](/assets\img\post\articulo_compresion\2.png)

Terry Welch refinó esta familia de algoritmos en 1984 con `LZW`, una variante que construye un diccionario dinámico de patrones a medida que va leyendo los datos, sin necesidad de guardar ese diccionario en el archivo final — el propio proceso de descompresión es capaz de reconstruirlo de forma idéntica sobre la marcha. `LZW` se convirtió rápidamente en el motor detrás de la utilidad `compress` de Unix y, más adelante, del formato `GIF`. Esa segunda decisión, como veremos en un capítulo dedicado solo a esto, acabaría generando una de las disputas de patentes más influyentes en la historia del software libre.

### DEFLATE — cuando alguien decide combinar las dos familias

A principios de los 90, Phil Katz diseñó `DEFLATE`, un algoritmo que no inventa una técnica nueva, sino que combina inteligentemente las dos que ya existían: primero aplica una variante de `LZ77` para eliminar repeticiones, y después aplica codificación `Huffman` sobre el resultado para exprimir aún más los símbolos que quedan. `DEFLATE` se publicó como una especificación abierta y libre de royalties, y ese detalle —nada técnico, puramente legal— es buena parte de por qué acabó siendo el motor por defecto de `ZIP`, de `PNG` y de `gzip`. Hoy sigue siendo, décadas después, uno de los algoritmos de compresión sin pérdida más usados del planeta.

### Los compresores de hoy

La historia no se detuvo en los 90. Google publicó `Brotli` en 2013, pensado específicamente para el tráfico web: combina una variante de `LZ77` y `Huffman` con un diccionario estático precargado de fragmentos comunes en HTML, CSS y JavaScript, lo que le da una ventaja notable comprimiendo el tipo de contenido que viaja constantemente por internet. Meta, por su parte, publicó `Zstandard` en 2016, diseñado para ofrecer un equilibrio mucho más ajustable entre velocidad de compresión y ratio conseguido, algo crítico cuando se opera a la escala de miles de millones de peticiones diarias.

Lo llamativo es que ninguno de estos dos algoritmos modernos parte de cero: ambos siguen construyendo sobre las mismas dos ideas que ya estaban sobre la mesa en los años 70 y 80 — *patrones repetidos* y *símbolos ponderados por frecuencia* —, solo que ahora afinadas con décadas de investigación adicional en modelos estadísticos.

### El hilo que conecta todo esto

Si algo queda claro al mirar esta cronología de golpe es que casi nadie empezó de cero. Vail contó tipos de imprenta. Shannon convirtió esa intuición en matemáticas. Huffman automatizó la matemática de Shannon en un algoritmo. Lempel y Ziv añadieron una segunda dimensión que Huffman no cubría. Katz combinó ambas ideas en una sola herramienta. Google y Meta refinaron esa combinación para las necesidades de una internet que nadie en 1952 podría haber imaginado.

Setenta y tantos años después de que Shannon pusiera un límite matemático a cuánto se puede reducir un mensaje, seguimos, en el fondo, intentando acercarnos a esa frontera. Todo lo que viene en esta serie —la entropía, el porqué de mis experimentos con archivos de texto, y más adelante el código de cada uno de estos algoritmos implementado desde cero es un intento de entender esa frontera desde dentro, en vez de limitarme a usarla sin preguntarme por qué existe.

---

## 2. Teoría de la información: entropía (Shannon)

Antes de hablar de algoritmos concretos, conviene entender la pregunta que hay detrás de toda compresión: **¿cuánto se puede reducir un dato sin perder información?** La respuesta la dio Claude Shannon en 1948, y pasa por un concepto que suena más filosófico de lo que es: la entropía.

### La información como sorpresa

Para Shannon, la información no tiene que ver con el significado de los datos, sino con cuánta incertidumbre eliminan. Si un símbolo aparece siempre que lo esperas, no te está diciendo nada nuevo. Si aparece algo que no podías predecir, ahí sí hay información real.

Un ejemplo mental lo deja claro: imagina dos cadenas de 1000 caracteres.

1.  `"AAAA...A"` (la letra A repetida 1000 veces).
2.  `1000 caracteres elegidos al azar`, sin ningún patrón.

La primera no te sorprende nunca: en cuanto ves las primeras "A", ya sabes exactamente qué viene después. La segunda es pura sorpresa constante, carácter tras carácter. Esa diferencia de "sorpresa media" es, literalmente, la entropía.

### La fórmula

Shannon formalizó esta idea con una expresión sencilla. Para una fuente en la que cada símbolo tiene una probabilidad de aparición `pᵢ`:

`H = -Σ pᵢ · log₂(pᵢ)`

El resultado se mide en **bits por símbolo**, y se puede leer como el número medio de preguntas de tipo sí/no que hacen falta para adivinar el siguiente símbolo. Cuanto más predecible es la fuente, menos preguntas hacen falta, y menor es `H`.

Aplicado a los dos ejemplos anteriores:

- En `"AAAA...A"`, la probabilidad de que aparezca "A" es del 100%. La entropía es `0 bits por símbolo`: cero incertidumbre, cero información nueva.
- En una cadena verdaderamente aleatoria con 256 valores posibles por carácter (un byte) y distribución uniforme, la entropía alcanza `8 bits por símbolo`, el máximo posible para un byte.
- Entre ambos extremos está, por ejemplo, el texto en español o inglés escrito de forma natural, con una entropía estimada de `1 a 1.5 bits por carácter`. Muy por debajo de los 8 bits que ocupa cada carácter en memoria, precisamente porque el lenguaje es redundante: ciertas letras, combinaciones y palabras se repiten mucho más que otras.

### El límite matemático de la compresión

Aquí es donde la entropía deja de ser una curiosidad teórica y se convierte en la pieza clave para entender la compresión. El teorema de codificación sin ruido de Shannon establece que:

> La entropía de una fuente es el límite mínimo, en bits por símbolo, al que se puede reducir esa fuente sin perder información. Ningún algoritmo de compresión sin pérdidas puede bajar de ese límite, en promedio.

Dicho de otro modo: **la entropía no es una recomendación, es un techo matemático**. Algoritmos como `Huffman` o la `codificación aritmética` no son más que estrategias para acercarse a ese límite, asignando códigos cortos a los símbolos frecuentes y códigos largos a los raros.

### Por qué un JPG no se puede comprimir más

Esta idea explica un fenómeno que seguramente has comprobado alguna vez: intentar meter un archivo `.jpg`, `.mp3` o `.zip` dentro de otro archivo `.zip` apenas reduce su tamaño.

La razón es que un archivo ya comprimido tiene una entropía muy cercana al máximo posible para su tamaño en bytes: toda la redundancia estadística que tenía la fuente original ya fue explotada durante la compresión. Lo que queda se comporta, a efectos prácticos, casi como ruido aleatorio, igual que la cadena de 1000 caracteres al azar del ejemplo inicial.

Por eso comprimir un archivo comprimido no es "comprimir el doble de bien": es chocar contra el mismo muro matemático que describió Shannon hace más de 75 años.

---

## 3. Los dos grandes tipos de compresión

No toda compresión persigue el mismo objetivo. Existen dos familias completamente distintas, y entender la diferencia es clave para saber por qué usamos un formato u otro según el tipo de dato.

### Compresión sin pérdidas (lossless)

En la compresión `lossless`, el archivo descomprimido es **idéntico, bit a bit**, al original. No se descarta ni un solo dato: simplemente se representa la misma información de forma más eficiente, explotando la redundancia estadística que vimos en el apartado de entropía.

Formatos y herramientas habituales de este tipo: `.zip`, `.rar`, `.png` y `.flac`.

Esta garantía de exactitud tiene un límite claro: como vimos antes, no se puede bajar de la entropía de la fuente. Cuando ya no queda redundancia que explotar, el archivo deja de reducirse más.

### Compresión con pérdidas (lossy)

En la compresión `lossy`, en cambio, se descarta información de forma deliberada y permanente. El archivo resultante ya no puede reconstruir el original exacto, pero a cambio se consiguen reducciones de tamaño mucho mayores que las que permite cualquier método sin pérdidas.

Formatos habituales: `.jpg`, `.mp3`, `.mp4`.

La clave está en qué se descarta: no es información al azar, sino aquella que el ojo o el oído humano apenas perciben. Un `.jpg` elimina detalles de color y frecuencia a los que el ojo es poco sensible; un `.mp3` recorta frecuencias que el oído humano prácticamente no distingue. El resultado se percibe casi igual que el original, aunque matemáticamente sea un archivo distinto.

### Por qué elegimos perder datos en unos casos y no en otros

La elección entre `lossless` y `lossy` no es una cuestión técnica arbitraria, sino una decisión sobre qué tipo de error es tolerable:

- **Imagen, audio y vídeo:** el objetivo final es la percepción humana. Si una pérdida de datos es imperceptible para el ojo o el oído, no afecta al valor real del archivo, y a cambio se gana muchísimo espacio. Tiene sentido sacrificar exactitud por eficiencia.
- **Texto, código fuente, bases de datos, ejecutables:** aquí un solo bit alterado puede cambiar por completo el significado. Un carácter equivocado en un contrato, una línea de código corrupta o un registro de base de datos distinto puede romper un programa o cambiar un dato crítico. En estos casos no existe un "error imperceptible": cualquier pérdida es inaceptable, así que se necesita exactitud matemática absoluta, aunque eso limite cuánto se puede comprimir.

En resumen: la compresión `lossy` explota los límites de la percepción humana; la compresión `lossless` respeta los límites que marca la propia teoría de la información.

---

## 4. El experimento práctico inicial

Como primera aproximación, intenté comprimir un archivo de texto con la siguiente información: *"Este archivo es un archivo de prueba, quiero comprobar su peso antes y después de comprimir con un software como Winrar"*. 

![FBW](/assets\img\post\articulo_compresion\1.ImagenArchivoTexto.png)

Su peso en `.txt` era de `127 bytes`, y su comprimido en `.rar` es de `186 bytes`.

![FBW](/assets\img\post\articulo_compresion\2.ImagenArchivoTextoPeso.png)

![FBW](/assets\img\post\articulo_compresion\3.ImagenTextoComprimidoPesoMas.png)

Esto sucede porque al comprimir un archivo extremadamente pequeño (como un `.txt` con una sola frase), el archivo resultante (`.zip` o `.rar`) suele pesar más que el original. Esto no es un error del software, sino un fenómeno matemático predecible: un texto tan corto casi no tiene patrones repetidos, por lo que el algoritmo de compresión apenas tiene margen para "encoger" los datos reales.

El aumento de peso se debe a que el programa no solo envuelve el texto, sino que crea un contenedor que añade bytes extra por dos motivos: la **sobrecarga de metadatos** (cabeceras obligatorias que guardan el nombre del archivo, fechas, permisos y códigos de seguridad CRC) y el **diccionario de compresión** (las reglas matemáticas o "instrucciones" necesarias para que otro equipo pueda descomprimirlo). En resumen, los bytes que ocupa el "envase" superan con creces los pocos bytes que se lograron reducir del contenido original.

Para que podamos ver la compresión de forma efectiva, generé un `.txt` con más caracteres que pesa `1005 bytes`. 

![FBW](/assets\img\post\articulo_compresion\4.ArchivoTxt.png)

En este caso, al hacer la misma compresión, podemos ver que el archivo ha pasado de pesar `1005 bytes` a pesar `640 bytes`. 

![FBW](/assets\img\post\articulo_compresion\5.ArchivoPesoTxt.png)

![FBW](/assets\img\post\articulo_compresion\6.TxtComprimidoPesoMenos.png)

Esto ha sucedido porque hemos conseguido que el contenido del `.txt` tenga suficiente redundancia, de forma que el ahorro supera el peso de los metadatos que genera el `.rar`, permitiéndonos ver el beneficio real de la compresión.

Esto sucede de manera muy similar en imágenes. Si tenemos archivos de tipo `BMP`, `TIFF` sin compresión o `RAW` de la cámara, podemos comprimirlos y reducir sustancialmente su tamaño. En el caso de los `JPG`, `PNG`, `WEBP` o `GIF`, esa reducción sería mínima ya que son formatos que ya están comprimidos.

---

## 5. Aplicándolo a otros tipos de archivo

Con la teoría ya puesta sobre la mesa, vale la pena ver cómo se traduce todo esto en los formatos que usamos cada día. Cada familia de archivos —imagen, audio, vídeo— tiene su propia versión de la disyuntiva `lossless` vs `lossy`, y elegir el formato correcto depende de para qué vas a usar el archivo.

### Imágenes: BMP/TIFF/RAW vs. JPG/PNG/WEBP

- **BMP, TIFF, RAW:** almacenan la imagen prácticamente sin comprimir (`BMP`) o con compresión lossless (`TIFF`, `RAW`). Cada píxel se guarda con toda su información original, lo que se traduce en archivos enormes. Son el estándar en fotografía profesional y edición, donde cada retoque debe partir de los datos exactos de la captura, sin artefactos acumulados por compresiones sucesivas.
- **JPG:** compresión `lossy` pensada para fotografías. Descarta información de color y detalle que el ojo apenas percibe, logrando reducciones de tamaño enormes. No es adecuado para texto o líneas nítidas, donde genera artefactos visibles (el típico "efecto bloque" alrededor de bordes definidos).
- **PNG:** compresión `lossless`, ideal para capturas de pantalla, logotipos, iconos o cualquier imagen con áreas de color plano y bordes definidos, donde `JPG` se vería degradado.
- **WEBP:** formato más moderno que soporta ambos modos, `lossless` y `lossy`, normalmente con mejor ratio de compresión que `JPG` y `PNG` para un mismo nivel de calidad.

### Audio y vídeo: WAV vs. MP3

- **WAV:** almacena el audio sin comprimir, con la onda de sonido representada de forma directa. Es el formato de referencia en grabación y producción musical, donde no se puede permitir ninguna pérdida antes de la mezcla final.
- **MP3:** compresión `lossy` que descarta frecuencias que el oído humano apenas distingue, reduciendo el tamaño del archivo drásticamente (a menudo a una décima parte o menos) a cambio de una calidad prácticamente indistinguible para la escucha habitual.

La misma lógica se aplica a vídeo: formatos sin comprimir o con compresión `lossless` se usan en edición profesional, mientras que formatos `lossy` como `MP4` (con `H.264`/`H.265`) son los que realmente vemos al reproducir contenido, porque el ahorro de espacio es descomunal frente a una pérdida de calidad casi imperceptible.

### Tabla comparativa

| Tipo de archivo | Formato | Compresión | Uso típico |
|---|---|---|---|
| Imagen | `BMP` | Ninguna | Compatibilidad básica, poco uso hoy |
| Imagen | `TIFF` / `RAW` | Lossless | Fotografía profesional, edición |
| Imagen | `PNG` | Lossless | Capturas, logos, gráficos con bordes definidos |
| Imagen | `JPG` | Lossy | Fotografías para web y uso general |
| Imagen | `WEBP` | Lossless / Lossy | Web moderna, mejor ratio que JPG/PNG |
| Audio | `WAV` | Ninguna | Grabación y producción musical |
| Audio | `FLAC` | Lossless | Archivo de audio de alta fidelidad |
| Audio | `MP3` | Lossy | Escucha habitual, streaming, dispositivos |
| Vídeo | Sin comprimir | Lossless | Edición y postproducción profesional |
| Vídeo | `MP4` (H.264) | Lossy | Reproducción, streaming, distribución |

El patrón se repite en todos los casos: los formatos sin pérdidas se reservan para el trabajo "en bruto", donde cada bit importa; los formatos con pérdidas dominan la distribución final, donde lo que importa es la experiencia percibida y no la exactitud matemática del archivo.

---

## 6. Prácticas en código: implementando los algoritmos

> **Nota sobre el código:** Para las implementaciones en Rust de esta sección me he apoyado en herramientas de inteligencia artificial. Esto me ha permitido centrarme en validar y explicar la arquitectura y la lógica subyacente de cada algoritmo, ahorrándome la fricción inicial del lenguaje rust.

Toda la teoría anterior se entiende mucho mejor cuando se pasa por el teclado. Esta sección va a ir construyendo, algoritmo a algoritmo, implementaciones reales que cualquiera puede ejecutar y romper a propósito para ver dónde fallan.

### Nivel 1 — RLE (Run-Length Encoding)

El algoritmo más sencillo de todos, y el mejor punto de partida: **Run-Length Encoding** sustituye cada racha de caracteres repetidos por el propio carácter seguido del número de repeticiones. La idea es la traducción más literal posible de la entropía que vimos al principio del post: si un carácter se repite mucho, no hace falta escribirlo mil veces, basta con decir cuántas veces se repite.

`"AAAAABBBCC" → "5A3B2C"`

#### La implementación

Aquí está mi codificador y decodificador en Rust:

```rust
fn encoder_rle(text_to_encode: &str) -> Result<String, &'static str> {
    if text_to_encode.is_empty() {
        return Err("El string no puede estar vacío");
    }

    let mut res_encoded = String::new();
    let mut count = 0;
    let mut prev_char = text_to_encode.chars().next().unwrap();

    for c in text_to_encode.chars() {
        if c == prev_char {
            count += 1;
        } else {
            res_encoded.push(prev_char);
            res_encoded.push_str(&count.to_string());

            prev_char = c;
            count = 1;
        }
    }

    res_encoded.push(prev_char);
    res_encoded.push_str(&count.to_string());

    Ok(res_encoded)
}

fn decoder_rle(texto: &str) -> Result<String, &'static str> {
    if texto.is_empty() {
        return Err("El string no puede estar vacío");
    }

    let mut resultado = String::new();

    let mut letra_actual = '\0';
    let mut numero_str = String::new();

    for c in texto.chars() {
        if c.is_numeric() {
            numero_str.push(c);
        } else {
            if letra_actual != '\0' && !numero_str.is_empty() {
                let cantidad: usize = numero_str.parse().unwrap_or(1);
                resultado.push_str(&letra_actual.to_string().repeat(cantidad));
                numero_str.clear();
            }
            letra_actual = c;
        }
    }

    if letra_actual != '\0' && !numero_str.is_empty() {
        let cantidad: usize = numero_str.parse().unwrap_or(1);
        resultado.push_str(&letra_actual.to_string().repeat(cantidad));
    }

    Ok(resultado)
}
```

El codificador recorre la cadena carácter a carácter, acumulando un contador mientras el carácter no cambia y volcando el par `(carácter, cantidad)` en cuanto encuentra uno distinto. El decodificador hace el camino inverso: acumula dígitos hasta encontrar una letra, y en ese momento repite la letra anterior tantas veces como indique el número acumulado.

Con la entrada de ejemplo `"AAAAABBBBBCCCCCDDDDD"` (`20 bytes`), el resultado es `"A5B5C5D5"` (`8 bytes`): una reducción de más del 50%.

#### El caso límite que rompe RLE

Aquí está la parte realmente interesante, y la razón por la que `RLE` no se usa en la práctica para cualquier tipo de dato. Prueba a codificar una cadena sin repeticiones, como `"ABCDE"`:

`"ABCDE" → "1A1B1C1D1E"`

5 caracteres de entrada se convierten en 10 caracteres de salida. **El "comprimido" ocupa el doble que el original.** Esto no es un bug puntual: es la consecuencia directa de la entropía de la fuente. Como vimos en el apartado 2, una cadena sin ningún carácter repetido tiene entropía alta (cerca del máximo), y `RLE` es un algoritmo que solo gana cuando existen rachas largas de repetición. Si no hay redundancia que explotar, cualquier metadato añadido (aquí, el contador `1` delante de cada letra) es puro coste extra.

El código de arriba ya detecta esta situación: compara el tamaño del texto comprimido contra el original y, si el comprimido es igual o mayor, lanza un aviso (`WARNING: Compression is not efficient`). Ahora mismo esa detección solo informa por consola; el siguiente paso lógico —y que dejo apuntado para la iteración del Nivel 1— es que la función realmente **descarte el resultado comprimido y conserve el original sin tocar** cuando esto ocurra, en lugar de limitarse a avisar. Es exactamente el tipo de salvaguarda que un compresor real necesita: nunca se puede asumir que comprimir siempre reduce el tamaño.

#### Cuándo tiene sentido RLE en la práctica

`RLE` brilla en datos con rachas largas y predecibles: mapas de bits antiguos como `BMP` sin comprimir con grandes áreas de un solo color, faxes en blanco y negro, o máscaras de imagen binarias. En cuanto el dato se parece a texto normal, código fuente o cualquier contenido con alta variabilidad carácter a carácter, `RLE` deja de aportar nada y, como acabamos de ver, puede llegar a ser contraproducente. Por eso ningún formato moderno de propósito general (`ZIP`, `PNG`, `gzip`...) usa `RLE` puro: todos recurren a algoritmos como `LZ77` + `Huffman` (`DEFLATE`), capaces de explotar patrones mucho más complejos que una simple racha de repeticiones.

---

### Nivel 2 — Codificación Huffman

Con `RLE` aprendimos la idea más literal de la entropía: si algo se repite, no hace falta escribirlo entero. `Huffman` va un paso más allá y ataca el problema que planteaba Alfred Vail con su cajón de tipos de imprenta al principio del post: si unos símbolos aparecen mucho más que otros, ¿por qué darles a todos el mismo número de bits? La codificación `Huffman` es la versión formal, automatizable y demostrablemente óptima de esa intuición.

#### La propiedad de prefijo

Antes de construir nada, hace falta entender una regla que parece obvia pero es la que hace posible todo el algoritmo: en un código Huffman, **ningún código puede ser prefijo de otro**.

¿Por qué importa esto? Porque si el código de `"A"` fuera `01` y el código de `"B"` fuera `010`, un decodificador que va leyendo bit a bit no sabría, al llegar a `01`, si ya tiene una `"A"` completa o si tiene que seguir leyendo por si es el principio de una `"B"`. La propiedad de prefijo elimina esa ambigüedad: en cuanto el decodificador reconoce una secuencia de bits que coincide con un código, sabe con certeza que ese es el símbolo completo, sin necesidad de separadores ni de mirar hacia adelante.

Antes de tocar una línea de código, merece la pena hacerlo a mano con solo 4 letras. Imagina que tienes `A`, `B`, `C` y `D` con estas frecuencias: `A` aparece 5 veces, `B` aparece 3, `C` aparece 1 y `D` aparece 1. Intenta asignar códigos binarios de distinta longitud a cada letra, dando el código más corto a la más frecuente, y comprueba tú mismo que ningún código que elijas puede ser el principio de otro. Ese ejercicio en papel es exactamente el problema que resuelve el árbol binario que viene ahora.

#### Construyendo el árbol

La propiedad de prefijo se cumple automáticamente si los códigos salen de recorrer un árbol binario desde la raíz hasta cada hoja (`0` = izquierda, `1` = derecha). El reto real es construir ese árbol de forma que las letras frecuentes queden cerca de la raíz (códigos cortos) y las raras queden lejos (códigos largos).

El algoritmo, descubierto por David Huffman en 1951 dándole la vuelta al método de su profesor, es *ávido* (`greedy`) y sorprendentemente simple:

1. Cuenta la frecuencia de cada símbolo.
2. Coge los **dos** nodos con menor frecuencia.
3. Combínalos en un nuevo nodo interno cuyo peso es la suma de ambos.
4. Repite el proceso con el nodo combinado incluido, hasta que solo quede un nodo: la raíz.

Antes de programarlo, simúlalo a mano con `"MISSISSIPPI"` como caso de estudio. Las frecuencias son `I:4`, `S:4`, `P:2`, `M:1`. Los dos nodos menos frecuentes son `M` (1) y `P` (2), así que se combinan primero en un nodo de peso 3. A partir de ahí, ese nodo combinado (peso 3) compite con `S` (4) e `I` (4) para la siguiente combinación. Sigue el proceso en papel hasta llegar a una única raíz y comprueba que `I` y `S`, al ser las más frecuentes, terminan con los códigos más cortos.

#### La implementación

Aquí está la implementación en Rust, con el mismo estilo de funciones directas que usamos en `RLE`:

```rust
use std::collections::{BinaryHeap, HashMap};
use std::cmp::Ordering;

#[derive(Debug, Clone)]
enum Nodo {
    Hoja(char, usize),
    Interno(usize, Box<Nodo>, Box<Nodo>),
}

impl Nodo {
    fn peso(&self) -> usize {
        match self {
            Nodo::Hoja(_, peso) => *peso,
            Nodo::Interno(peso, _, _) => *peso,
        }
    }
}

impl PartialEq for Nodo {
    fn eq(&self, other: &Self) -> bool {
        self.peso() == other.peso()
    }
}
impl Eq for Nodo {}
impl PartialOrd for Nodo {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
        Some(self.cmp(other))
    }
}
impl Ord for Nodo {
    fn cmp(&self, other: &Self) -> Ordering {
        other.peso().cmp(&self.peso())
    }
}

fn contar_frecuencias(texto: &str) -> HashMap<char, usize> {
    let mut frecuencias = HashMap::new();
    for c in texto.chars() {
        *frecuencias.entry(c).or_insert(0) += 1;
    }
    frecuencias
}

fn construir_arbol(frecuencias: &HashMap<char, usize>) -> Nodo {
    let mut heap: BinaryHeap<Nodo> = frecuencias
        .iter()
        .map(|(&c, &f)| Nodo::Hoja(c, f))
        .collect();

    // el algoritmo ávido: saca los dos nodos menos frecuentes, combínalos, repite
    while heap.len() > 1 {
        let izquierda = heap.pop().unwrap();
        let derecha = heap.pop().unwrap();
        let nuevo_peso = izquierda.peso() + derecha.peso();
        heap.push(Nodo::Interno(nuevo_peso, Box::new(izquierda), Box::new(derecha)));
    }

    heap.pop().unwrap()
}

fn generar_codigos(nodo: &Nodo, prefijo: String, codigos: &mut HashMap<char, String>) {
    match nodo {
        Nodo::Hoja(c, _) => {
            let codigo = if prefijo.is_empty() { "0".to_string() } else { prefijo };
            codigos.insert(*c, codigo);
        }
        Nodo::Interno(_, izquierda, derecha) => {
            generar_codigos(izquierda, format!("{}0", prefijo), codigos);
            generar_codigos(derecha, format!("{}1", prefijo), codigos);
        }
    }
}

fn encoder_huffman(texto: &str) -> Result<(String, Nodo), &'static str> {
    if texto.is_empty() {
        return Err("El string no puede estar vacío");
    }

    let frecuencias = contar_frecuencias(texto);
    let arbol = construir_arbol(&frecuencias);

    let mut codigos = HashMap::new();
    generar_codigos(&arbol, String::new(), &mut codigos);

    let mut bits = String::new();
    for c in texto.chars() {
        bits.push_str(&codigos[&c]);
    }

    Ok((bits, arbol))
}
```

`contar_frecuencias` recorre el texto una vez y lleva la cuenta de cada carácter. `construir_arbol` mete todas las hojas en una cola de prioridad y va combinando los dos nodos más ligeros hasta que solo queda uno: la raíz. `generar_codigos` recorre ese árbol de forma recursiva, añadiendo un `"0"` cada vez que baja por la izquierda y un `"1"` cada vez que baja por la derecha, hasta llegar a una hoja. `encoder_huffman` junta las tres piezas y traduce el texto original a una cadena de `"0"`s y `"1"`s.

Con `"MISSISSIPPI"` (11 caracteres, `88 bits` si lo guardáramos como texto plano a 8 bits por carácter), el árbol que construimos a mano en el apartado anterior le da 2 bits a `I` y `S`, y algún código de 2-3 bits a `M` y `P`, el resultado son bastantes menos de los 88 bits originales, aunque el ahorro real depende de la forma exacta del árbol.

#### Manipulación de bits real

Aquí está el problema que solemos pasar por alto la primera vez: `encoder_huffman` te devuelve un `String` como `"010011"`, pero en memoria eso sigue ocupando 6 *bytes* (uno por cada carácter `'0'` o `'1'`), no los 6 bits que en teoría deberían bastar. Todo el ahorro que acabamos de calcular sobre el papel desaparece si no empaquetamos esos bits de verdad.

```rust
fn empaquetar_bits(bits: &str) -> Vec<u8> {
    let mut bytes = Vec::new();
    let mut byte_actual: u8 = 0;
    let mut posicion = 0;

    for bit in bits.chars() {
        if bit == '1' {
            byte_actual |= 1 << (7 - posicion);
        }
        posicion += 1;

        if posicion == 8 {
            bytes.push(byte_actual);
            byte_actual = 0;
            posicion = 0;
        }
    }

    if posicion > 0 {
        bytes.push(byte_actual);
    }

    bytes
}

fn desempaquetar_bits(bytes: &[u8], total_bits: usize) -> String {
    let mut bits = String::new();
    for byte in bytes {
        for i in 0..8 {
            let bit = (byte >> (7 - i)) & 1;
            bits.push(if bit == 1 { '1' } else { '0' });
        }
    }
    bits.truncate(total_bits);
    bits
}

fn decoder_huffman(bits: &str, arbol: &Nodo) -> String {
    let mut resultado = String::new();
    let mut nodo_actual = arbol;

    for bit in bits.chars() {
        nodo_actual = match nodo_actual {
            Nodo::Interno(_, izquierda, derecha) => {
                if bit == '0' { izquierda } else { derecha }
            }
            Nodo::Hoja(_, _) => arbol,
        };

        if let Nodo::Hoja(c, _) = nodo_actual {
            resultado.push(*c);
            nodo_actual = arbol;
        }
    }

    resultado
}
```

`empaquetar_bits` recorre la cadena de `"0"`s y `"1"`s y va encendiendo o dejando apagado el bit correspondiente de un byte real usando el operador `<<` para desplazarlo a su posición y `|` para fijarlo, hasta completar los 8 bits del byte. `desempaquetar_bits` hace el camino inverso con `>>` y `&` para extraer cada bit uno a uno. `decoder_huffman` recorre el árbol bit a bit: `0` baja a la izquierda, `1` baja a la derecha, y cada vez que llega a una hoja escribe el carácter y vuelve a empezar desde la raíz.

Un detalle que hay que guardar aparte, además del árbol (o de las frecuencias, que permiten reconstruirlo): el **número exacto de bits del mensaje original** (`total_bits`). Como el último byte puede quedar relleno con ceros de sobra, sin ese dato el decodificador no sabría dónde termina el mensaje real y dónde empieza el relleno — es el mismo tipo de metadato "invisible pero necesario" que ya vimos en la sobrecarga de las cabeceras `.rar` del experimento inicial.

#### Por qué Huffman por sí solo no basta

`Huffman` resuelve perfectamente el problema de "unos símbolos son más frecuentes que otros", y lo hace de forma demostrablemente óptima. Pero tiene un techo: solo mira símbolos individuales, uno a uno. No tiene forma de aprovechar que la palabra "que" o la secuencia "ing" aparezcan una y otra vez a lo largo de un texto eso ya no es una cuestión de frecuencia de un carácter suelto, sino de patrones completos que se repiten. Ese es exactamente el problema que Lempel y Ziv atacaron desde un ángulo distinto, y que veremos en el Nivel 3.

---

### Nivel 3 — LZ77 y la ventana deslizante

Con `Huffman` (Nivel 2) llegamos a la solución matemáticamente óptima para un problema muy concreto: qué hacer cuando unos símbolos aparecen más que otros. Pero, como vimos, Huffman tiene un punto ciego gigante. Es miope. Solo mira los caracteres de uno en uno. 

Si le damos el texto `"que que que que"`, Huffman dirá: "Vale, la 'q', la 'u' y la 'e' aparecen mucho, les daré códigos cortos". Pero jamás se dará cuenta de que las tres letras *siempre aparecen juntas* formando un bloque. En el mundo real, los datos (texto, código fuente, HTML) no son bolsas de letras aleatorias; son secuencias, estructuras y patrones repetidos.

Aquí es donde entran Abraham Lempel y Jacob Ziv en 1977. Su genialidad fue cambiar la pregunta. En lugar de "¿con qué frecuencia aparece esta letra?", se preguntaron **"¿dónde he visto esta secuencia antes?"**.

#### El concepto: la ventana deslizante

Para encontrar secuencias repetidas sin tener que guardar todo el texto en memoria, `LZ77` utiliza lo que se conoce como una **ventana deslizante**. 

Imagina el proceso como si estuvieras siguiendo la maniobra de un caza a través del visor de tu cámara fotográfica en un festival aéreo: no puedes ver todo el cielo a la vez, tu campo de visión es estrictamente limitado. Lo que el avión ya ha dejado atrás (el pasado) va quedando en un lado del encuadre, lo que estás enfocando ahora mismo es el presente, y lo que está por entrar en el visor es el futuro. A medida que sigues el movimiento, la "ventana" se desliza.

En `LZ77`, esa ventana virtual en la memoria se divide en dos partes:
1.  **Buffer de búsqueda (el pasado):** Los datos que ya hemos procesado.
2.  **Buffer de previsualización (el futuro):** Los datos que estamos a punto de procesar.

El algoritmo mira el *futuro* y busca si esa secuencia exacta de caracteres ya apareció en el *pasado*. Si la encuentra, en lugar de escribir los caracteres otra vez, emite una tupla con tres datos: `(distancia hacia atrás, longitud de la coincidencia, siguiente carácter que no coincide)`.

Si la ventana lee la palabra `"abracadabra"`, la segunda vez que aparece `"abra"`, el algoritmo simplemente emite las instrucciones: *"retrocede 7 espacios y copia 4 caracteres"*.

#### La implementación en Rust

Aquí tienes la implementación directa de `LZ77`. Fíjate en cómo recorremos el texto gestionando matemáticamente los límites de esa ventana virtual.

```rust
fn encoder_lz77(texto: &str, tamaño_ventana: usize, tamaño_buffer: usize) -> Vec<(u16, u16, char)> {
    let mut resultado = Vec::new();
    let chars: Vec<char> = texto.chars().collect();
    let mut pos = 0;

    while pos < chars.len() {
        let mut mejor_distancia = 0;
        let mut mejor_longitud = 0;

        // Determinamos dónde empieza nuestro "pasado" visible en la ventana
        let inicio_ventana = if pos > tamaño_ventana { pos - tamaño_ventana } else { 0 };

        // Buscamos la coincidencia más larga en el buffer de búsqueda
        for i in inicio_ventana..pos {
            let mut longitud = 0;
            
            // Comprobamos cuántos caracteres consecutivos coinciden
            while pos + longitud < chars.len()
                && longitud < tamaño_buffer
                && chars[i + longitud] == chars[pos + longitud]
            {
                longitud += 1;
            }

            if longitud > mejor_longitud {
                mejor_longitud = longitud;
                mejor_distancia = pos - i;
            }
        }

        // Calculamos cuál es el carácter literal que rompe la coincidencia
        let siguiente_char_pos = pos + mejor_longitud;
        let siguiente_char = if siguiente_char_pos < chars.len() {
            chars[siguiente_char_pos]
        } else {
            '\\0' // Usamos null para indicar el final del bloque
        };

        resultado.push((mejor_distancia as u16, mejor_longitud as u16, siguiente_char));
        
        // Deslizamos la ventana saltándonos lo que ya hemos referenciado
        pos += mejor_longitud + 1;
    }

    resultado
}
```

Y la descompresión es sorprendentemente sencilla. El decodificador no tiene que buscar ningún patrón, solo obedece a ciegas las instrucciones de copia:

```rust
fn decoder_lz77(comprimido: &[(u16, u16, char)]) -> String {
    let mut resultado = String::new();

    for &(distancia, longitud, siguiente_char) in comprimido {
        if longitud > 0 {
            let start = resultado.len() - distancia as usize;
            
            for i in 0..longitud as usize {
                let c = resultado.chars().nth(start + i).unwrap();
                resultado.push(c);
            }
        }
        if siguiente_char != '\\0' {
            resultado.push(siguiente_char);
        }
    }
    
    resultado
}
```

---

### Nivel 4 — LZW (diccionario dinámico)

Con `LZ77` vimos cómo aprovechar el "pasado" usando una ventana deslizante. Pero tenía un coste arquitectónico: el compresor tenía que hacer un trabajo de búsqueda bastante pesado, escaneando el buffer constantemente y escupiendo tuplas de tres valores `(distancia, longitud, carácter)`.

En 1984, Terry Welch refinó una idea de Lempel y Ziv para crear **LZW (Lempel-Ziv-Welch)**. Su enfoque elimina la necesidad de enviar distancias y longitudes. En su lugar, el algoritmo construye un **diccionario** sobre la marcha. Cada vez que ve un patrón nuevo, lo guarda en el diccionario y le asigna un número (un código entero). A partir de ese momento, cada vez que vuelva a ver ese patrón, solo emitirá ese número.

#### La codificación: construyendo el diccionario

El diccionario no arranca vacío. Empieza con todos los caracteres posibles. Si trabajamos con texto básico, lo inicializamos con los 256 caracteres de la tabla ASCII (códigos del `0` al `255`). A partir del código `256`, empezamos a registrar nuestras propias secuencias.

Aquí está la implementación del codificador en Rust, usando un `HashMap` para búsquedas rápidas en el diccionario:

```rust
use std::collections::HashMap;

fn encoder_lzw(texto: &str) -> Vec<u16> {
    // Inicializamos el diccionario con el alfabeto básico (0 a 255)
    let mut diccionario: HashMap<String, u16> = (0..=255)
        .map(|i| ((i as u8 as char).to_string(), i as u16))
        .collect();
        
    let mut actual = String::new();
    let mut resultado = Vec::new();
    let mut siguiente_codigo = 256;

    for c in texto.chars() {
        let combinado = format!("{}{}", actual, c);
        
        // Si el patrón ya está en el diccionario, seguimos acumulando
        if diccionario.contains_key(&combinado) {
            actual = combinado;
        } else {
            // Emitimos el código del patrón que sí conocemos
            resultado.push(*diccionario.get(&actual).unwrap());
            
            // Añadimos el nuevo patrón al diccionario
            diccionario.insert(combinado, siguiente_codigo);
            siguiente_codigo += 1;
            
            // Reiniciamos 'actual' con el carácter que rompió el patrón
            actual = c.to_string();
        }
    }
    
    // No olvidemos emitir el último patrón acumulado
    if !actual.is_empty() {
        resultado.push(*diccionario.get(&actual).unwrap());
    }
    
    resultado
}
```

Si le pasamos el texto `"ABABAB"`, emitirá códigos que empezarán siendo caracteres individuales (`A`, `B`) y rápidamente escalarán a números mayores que 255 para representar `"AB"` o `"ABA"`.

#### La magia real: La decodificación asíncrona

Aquí llega la que, para mí, es la parte más brillante de toda la compresión por diccionario. **El archivo comprimido no incluye el diccionario**. 

Piénsalo un segundo. Hemos codificado un texto entero usando referencias numéricas a palabras que nosotros mismos hemos inventado en tiempo de ejecución. ¿Cómo va a saber el decodificador qué significa el código `258` si no le mandamos la tabla de equivalencias?

Es ese momento en el que las piezas encajan con la misma inevitabilidad matemática que una demostración limpia, donde cada paso se deduce directamente del anterior sin necesitar información externa. El decodificador puede deducir exactamente qué hiciste porque sigue las mismas reglas lógicas: arranca con el mismo diccionario base de 256 caracteres y, con cada código que recibe, es capaz de reconstruir la palabra que el codificador tuvo que haber añadido en ese momento exacto.

```rust
fn decoder_lzw(comprimido: &[u16]) -> String {
    if comprimido.is_empty() {
        return String::new();
    }

    // El decodificador arranca con la misma base
    let mut diccionario: HashMap<u16, String> = (0..=255)
        .map(|i| (i as u16, (i as u8 as char).to_string()))
        .collect();
    let mut siguiente_codigo = 256;

    let mut codigo_anterior = comprimido[0];
    let mut cadena = diccionario.get(&codigo_anterior).unwrap().clone();
    let mut resultado = cadena.clone();
    let mut primer_caracter = cadena.chars().next().unwrap();

    for &codigo_actual in comprimido.iter().skip(1) {
        let entrada = if let Some(val) = diccionario.get(&codigo_actual) {
            val.clone()
        } else if codigo_actual == siguiente_codigo {
            // EL CASO LÍMITE: Cuando el código no está en el diccionario, 
            // la lógica de LZW nos dice que tiene que ser cadena + su propio primer carácter
            format!("{}{}", cadena, primer_caracter)
        } else {
            panic!("Código no válido en la descompresión");
        };

        resultado.push_str(&entrada);
        primer_caracter = entrada.chars().next().unwrap();
        
        // Reconstruimos el paso del codificador: añadimos al diccionario
        diccionario.insert(siguiente_codigo, format!("{}{}", cadena, primer_caracter));
        siguiente_codigo += 1;
        
        cadena = entrada;
    }

    resultado
}
```

Ese bloque `else if codigo_actual == siguiente_codigo` es vital. Trata la famosa excepción del patrón `"ABABAB"`. A veces, el codificador es tan rápido emitiendo un código nuevo que el decodificador lo recibe antes de haber tenido la oportunidad de registrarlo en su propia pasada. Pero las matemáticas del algoritmo dictan que, cuando esto ocurre, la palabra nueva siempre está formada por la palabra anterior más su propio primer carácter.

#### La memoria física: cuando el diccionario revienta

Todo este proceso tiene un problema físico: los recursos no son infinitos. Si estás comprimiendo un archivo de `1 GB`, el diccionario crecerá enormemente, consumiendo toda la RAM y generando códigos enteros larguísimos (de `32` o `64 bits`), perdiendo toda la ventaja de compresión. 

En los sistemas reales, como el formato `GIF` (que usa `LZW`), se pone un límite estricto de bits. Generalmente se fijan `12 bits` para los códigos, lo que permite un diccionario máximo de `4096` entradas. ¿Qué se hace al llegar a la entrada 4096? Tienes varias estrategias reales en la industria:
1. **Limpiar y reiniciar:** Borrar todo el diccionario y volver a empezar desde cero (estrategia `GIF`).
2. **Congelar:** Dejar de añadir nuevas entradas y usar el diccionario de 4096 palabras tal y como se quedó, útil si el archivo es muy homogéneo.
3. **Escalar dinámicamente:** Empezar con códigos de `9 bits` e ir subiendo el tamaño del entero bajo demanda a medida que se va necesitando, ahorrando espacio al principio.

---

### Nivel 5 — El Mini-DEFLATE (Combinando mundos)

A estas alturas ya tenemos implementados nuestros dos grandes pilares en código: `Huffman` (Nivel 2) y la familia `LZ` (Niveles 3 y 4). Pero en la vida real, los compresores rara vez eligen uno de ellos. Hacen lo que hizo Phil Katz al diseñar **DEFLATE**: conectarlos en serie.

La estructura mental es un *pipeline* clásico:

`Datos en bruto -> Compresión LZ77 -> (Distancias, Longitudes, Literales) -> Compresión Huffman -> Archivo binario final`

#### ¿Por qué apilarlos?

`LZ77` es espectacular encontrando patrones estructurales. Si repites la palabra "hipopótamo", `LZ77` la eliminará y la sustituirá por un puntero. Pero ¿qué forma tienen esos punteros? Generan muchísimos números pequeños. La distancia hacia atrás suele ser corta (las referencias locales ocurren más que las lejanas). Las longitudes de coincidencia suelen ser pequeñas (coincidencias de 3 o 4 letras ocurren mucho más que coincidencias de 100 letras).

Es decir, **el resultado de LZ77 tiene una distribución de frecuencias altamente desigual**. ¿Y qué algoritmo hemos programado que es literalmente la solución perfecta para exprimir datos con frecuencias desiguales? Exacto, `Huffman`.

#### Cómo funciona en el mundo real

No voy a implementar un `DEFLATE` completo desde cero en un bloque de código por una sencilla razón: el **RFC 1951** (la especificación oficial de DEFLATE) es brutalmente extenso. Un codificador completo tiene que gestionar bloques dinámicos, bloques estáticos y bloques no comprimidos.

Pero conceptualmente, la implementación en un sistema real (y la forma en que lo podrías estructurar en un proyecto personal) hace esto:
1. Pasa la ventana de `LZ77` por los datos y almacena los resultados en vez de emitirlos directamente.
2. Una vez evaluado un bloque, calcula las frecuencias de todos los literales, de todas las longitudes y de todas las distancias.
3. Construye **dos** árboles de `Huffman` diferentes: uno para unificar literales y longitudes, y otro exclusivo para las distancias de retroceso.
4. Serializa esos dos árboles en la cabecera del bloque.
5. Traduce toda la salida del `LZ77` usando los códigos binarios de los árboles recién generados.

El resultado es la base de todo. Tu navegador descomprimiendo el HTML de esta página web, el motor desempaquetando un `.zip` en tu disco duro o el visor de imágenes decodificando la estructura interna de un `.png`, todos están corriendo este exacto proceso a la inversa: leyendo el árbol, deduciendo las tuplas de distancia, y aplicando la ventana de copia en el buffer de salida.

Al haber implementado cada pieza atómica por separado, la magia desaparece y deja paso a la ingeniería pura. Y esa es, precisamente, la esencia de explorar las arquitecturas invisibles que usamos todos los días.

---

## 7. Historia, patentes y controversia: GIF vs PNG

Hay apartados de la historia de la tecnología que parecen anécdotas curiosas y en realidad esconden lecciones de arquitectura de software que seguimos aplicando hoy. La guerra entre `GIF` y `PNG` es una de ellas, y tiene todos los ingredientes: un algoritmo brillante, una patente olvidada, una empresa que decide cobrarla de repente y una comunidad que responde creando un formato libre desde cero.

### Un algoritmo patentado que nadie vio venir

En 1987, el equipo de CompuServe liderado por Steve Wilhite presentó el formato `GIF` (Graphics Interchange Format), pensado para compartir imágenes en color ocupando el mínimo espacio posible en una época de conexiones lentísimas y memoria cara. Para comprimir los píxeles usaron el algoritmo **LZW** (Lempel-Ziv-Welch), una variante muy eficiente de la familia de algoritmos `LZ` que construye un diccionario dinámico de patrones repetidos.

El problema es que `LZW` no era de dominio público: Sperry Corporation (que poco después se fusionaría con Burroughs para formar Unisys) tenía una patente sobre el algoritmo desde 1985. CompuServe lo integró en el `GIF` sin saber que estaba usando tecnología patentada, y durante años nadie —ni siquiera Unisys— se dio cuenta del choque. El formato se extendió por todo internet, se convirtió en el estándar de facto para imágenes en la web y prácticamente ningún desarrollador sabía que, técnicamente, debía pagar por usarlo.

### La "GIF Tax" y el estallido de la comunidad

Todo cambió a finales de 1994, cuando Unisys y CompuServe anunciaron conjuntamente que cualquier desarrollador de software capaz de crear o leer archivos `GIF` necesitaría licenciar el uso de `LZW`. El anuncio, además, llegó en un momento especialmente mal calculado: entre Navidad y Fin de Año, cuando gran parte de la comunidad estaba de vacaciones y se sintió emboscada. La reacción fue inmediata y furiosa: la prensa especializada de la época llegó a describirlo como un ataque por sorpresa a la comunidad de internet.

La tensión no hizo más que crecer con los años, y en 1999 Unisys volvió a cambiar las condiciones de licencia, subiendo las tarifas para páginas web que usaran `GIF`s. La respuesta de la comunidad open source fue tan simbólica como contundente: el 5 de noviembre de 1999, un grupo de desarrolladores organizó el **"Burn All GIFs Day"**, una protesta frente a la sede de Unisys en la que quemaron literalmente sus archivos `GIF` impresos, como forma de denunciar los peligros de patentar algoritmos que ya formaban parte de la infraestructura básica de internet.

### El nacimiento de PNG

La respuesta más duradera, sin embargo, no fue la quema simbólica de archivos, sino algo mucho más silencioso y efectivo: crear una alternativa técnica que hiciera irrelevante la patente. Ya en 1995, tras el primer anuncio de Unisys, un grupo de ingenieros —varios de ellos veteranos del propio desarrollo de `JPEG`— empezó a debatir en el grupo de noticias Usenet `comp.graphics` cómo sería un "sustituto del GIF" libre de ataduras legales.

De ahí nació **PNG** (Portable Network Graphics), presentado en 1996. El nombre original propuesto fue, de hecho, un guiño con humor de programador: **PING**, acrónimo recursivo de "Ping Is Not GIF". En lugar de `LZW`, `PNG` se apoya en **DEFLATE**, un algoritmo de compresión sin pérdidas libre de patentes (combinación de `LZ77` y codificación `Huffman`) que además del formato `ZIP` también acabaría siendo la base de `gzip`. Como beneficio añadido, `PNG` resultó tecnológicamente superior al `GIF` en varios aspectos: soporta millones de colores frente a los 256 del `GIF`, ofrece transparencia real mediante canal alfa, y su compresión sin pérdidas es más eficiente para gráficos con áreas de color plano.

La patente estadounidense de `LZW` acabó expirando el 20 de junio de 2003, y las patentes equivalentes en otros países expiraron poco después. Para entonces, sin embargo, la batalla ya estaba decidida: `PNG` se había consolidado como el estándar preferido para gráficos web sin pérdidas, y buena parte de la industria había aprendido una lección que no ha olvidado desde entonces.

### Por qué esta historia sigue importando

El episodio `GIF`-`PNG` no es solo folclore friki: es el origen de una práctica que hoy damos por sentada en la ingeniería de software. Desde entonces, es habitual que los equipos técnicos, antes de adoptar un algoritmo o una librería, revisen explícitamente su situación de patentes y licencias, algo que en los años 80 casi nadie hacía. También explica por qué formatos y algoritmos como `DEFLATE`, usado en `PNG`, `ZIP` y `gzip`, se diseñaron y difundieron deliberadamente como implementaciones abiertas y libres de royalties: no fue una casualidad técnica, sino una respuesta directa al riesgo de quedar atrapados otra vez en una dependencia patentada.

Dicho de otro modo: la próxima vez que guardes una captura de pantalla en `PNG` en lugar de `GIF`, estás usando, sin saberlo, el resultado directo de una disputa de patentes de los años 90 y de una comunidad que decidió que la infraestructura básica de internet no debía depender de la buena voluntad de una sola empresa.

---

## 8. Sistemas modernos: DEFLATE y compresión web

Después de ver la teoría (entropía), la historia (`GIF` vs `PNG`) y los tipos de compresión, toca aterrizar en el algoritmo que probablemente más veces ha pasado por tu ordenador sin que te dieras cuenta: **DEFLATE**.

### Qué es DEFLATE

`DEFLATE` no es un algoritmo nuevo de cero, sino la combinación inteligente de dos técnicas que ya hemos mencionado por separado:

1. **LZ77** (de la familia Lempel-Ziv): recorre los datos buscando secuencias repetidas y las sustituye por referencias del tipo "repite lo que viste hace X bytes, durante Y bytes". Esto elimina la redundancia a nivel de patrones.
2. **Codificación Huffman**: una vez eliminada esa redundancia, asigna códigos más cortos a los símbolos más frecuentes y códigos más largos a los menos frecuentes, acercándose al límite de entropía del que hablábamos al principio del post.

La combinación de ambas fases es lo que hace de `DEFLATE` un algoritmo tan eficaz: `LZ77` ataca la redundancia estructural (patrones repetidos) y `Huffman` ataca la redundancia estadística (símbolos desiguales). Y, como vimos en el apartado de patentes, su gran ventaja histórica es que nació libre de royalties, lo que explica por qué se convirtió en el motor de facto de gran parte de la infraestructura de compresión de internet.

**DEFLATE es, literalmente, el motor que hay detrás de:**

- El formato **ZIP** (y por extensión, de casi cualquier archivo `.zip` que hayas abierto en tu vida).
- El formato **PNG**, en cada uno de sus bloques de imagen comprimidos.
- **gzip**, que es lo que comprime buena parte del tráfico HTTP de la web moderna (cuando tu navegador recibe una respuesta con la cabecera `Content-Encoding: gzip`, está descomprimiendo `DEFLATE` en tiempo real).

### Explorar la estructura de un ZIP por dentro

Para entender que un `.zip` no es "magia", sino un contenedor con una estructura muy concreta, merece la pena abrir uno con `hexdump` (o cualquier visor hexadecimal) y buscar sus firmas características:

- Todo archivo `.zip` empieza con la firma `PK\x03\x04` (los bytes `50 4B 03 04`), en referencia a **Phil Katz**, creador del formato `PKZIP` en el que se basa el estándar actual.
- Cada entrada del `.zip` tiene su propia cabecera local con metadatos: nombre del archivo, tamaño comprimido, tamaño original y el método de compresión usado (normalmente, el código `8` indica `DEFLATE`).
- Al final del archivo hay un **directorio central** que indexa todas las entradas, lo que permite a un lector saltar directamente a un archivo concreto sin tener que descomprimir el `.zip` entero.

La documentación de **Info-ZIP** (el proyecto open source detrás de las utilidades `zip`/`unzip` que probablemente tengas instaladas) es un buen punto de partida si quieres profundizar en el formato `APPNOTE.TXT` que define oficialmente la especificación `ZIP`.

### Compresión aplicada al diseño web y al SEO

Aquí es donde toda la teoría se convierte en una decisión práctica del día a día para cualquiera que trabaje con una web:

- **PNG (lossless):** la elección correcta para logotipos, iconos, capturas de pantalla y cualquier gráfico con áreas de color plano y bordes nítidos. Al ser sin pérdidas, el texto y los bordes se mantienen perfectamente definidos, algo que `JPEG` no garantiza.
- **JPEG / WebP (lossy):** la elección correcta para fotografías. Al descartar información imperceptible para el ojo humano, permiten reducciones de tamaño mucho mayores, algo crítico para el rendimiento de una web: menos peso significa carga más rápida, y la velocidad de carga es, directamente, un factor de posicionamiento SEO (Google penaliza páginas lentas en sus rankings, especialmente en las métricas Core Web Vitals).

En la práctica, esto se traduce en una regla simple: si la imagen tiene pocos colores y bordes definidos, `PNG`; si es una fotografía con degradados y muchos colores, `JPEG` o, mejor aún, `WebP`.

### Probando la diferencia con TinyPNG

Una forma muy visual de comprobar todo esto es coger una imagen `PNG` con muchos colores y pasarla por una herramienta como **TinyPNG**. Su truco no es `DEFLATE` más agresivo, sino algo distinto: aplica **cuantización de color** (`quantization`), reduciendo el número de colores únicos de la imagen antes de comprimir, de forma similar a como `GIF` limitaba la paleta a 256 colores, pero de manera mucho más inteligente y selectiva sobre qué colores conservar.

El experimento interesante es comparar dos cosas a la vez:

- La **reducción en bytes**: normalmente entre un 50% y un 70% menos de tamaño de archivo.
- La **reducción visual percibida**: en la mayoría de los casos, prácticamente indistinguible a simple vista.

Ese contraste —tanto ahorro de espacio con tan poca pérdida perceptible— es la mejor demostración práctica de todo lo que hemos ido explicando en el post: la compresión no consiste en "recortar datos a lo bruto", sino en entender exactamente qué información es redundante o imperceptible, y eliminar justo esa, ni un bit más.

---

## 9. Conclusión

### Cuándo comprimir merece la pena y cuándo no

Después de recorrer la teoría, la historia y los algoritmos, la pregunta práctica se puede resumir en pocas líneas:

- **Merece la pena comprimir** cuando el dato tiene redundancia real que explotar (texto, código, imágenes con áreas planas, audio sin comprimir) o cuando se puede sacrificar información imperceptible a cambio de un ahorro enorme (fotografías, audio para streaming, vídeo). En ambos casos, el coste de comprimir/descomprimir es insignificante frente al beneficio en espacio y velocidad de transmisión.
- **No merece la pena, o incluso perjudica,** comprimir un archivo que ya está cerca de su límite de entropía: un `JPG`, un `MP3`, un `ZIP`. Ahí no queda redundancia real que aprovechar, y forzar una segunda compresión solo añade coste de CPU sin beneficio, y en algunos casos hasta puede aumentar ligeramente el tamaño por la sobrecarga de las cabeceras del nuevo formato.

La clave, en el fondo, es la misma en todo el post: comprimir bien no es "apretar los datos", es entender exactamente cuánta redundancia y cuánta información irrelevante contienen, y actuar solo sobre eso.

### La compresión como capa invisible

Hay algo que vale la pena remarcar al cerrar este tema: la compresión es una de esas piezas de infraestructura que solo se nota cuando falla o cuando no existe. Nadie piensa en `DEFLATE` al abrir una web, ni en el códec de vídeo al ver una serie en streaming, ni en la codificación de audio al escuchar música en el móvil con datos móviles limitados. Y sin embargo, sin esa capa invisible, buena parte de la internet actual sería sencillamente inviable: el streaming de vídeo en alta definición saturaría cualquier conexión doméstica, el almacenamiento en la nube sería una fracción de lo que es hoy por el mismo precio, y sincronizar un móvil con poca memoria sería una tarea constante de gestión de espacio.

Es exactamente el tipo de tecnología del que suelo hablar en "Arquitecturas de la Invisibilidad": sistemas tan bien diseñados y tan bien integrados que dejan de percibirse como tecnología y pasan a formar parte del paisaje, dando por sentado algo que en realidad es el resultado de décadas de teoría matemática, batallas de patentes y decisiones de ingeniería muy deliberadas. La próxima vez que subas una foto, veas una serie o mandes un archivo por WhatsApp, hay entropía, `LZ77` y `Huffman` trabajando en segundo plano, silenciosos, sosteniendo una experiencia que damos por hecha.