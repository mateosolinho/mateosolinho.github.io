title: Arquitecturas de la Invisibilidad: Viaje hacia la compresión de datos
date: 2026-07-18 12:30:00 +0800

1. Introducción / Historia

### Capítulo 1 — Por qué comprimimos el mundo

Hay una capa de ingeniería que usamos miles de veces al día sin saberlo. Está ahí cuando abres una foto en el móvil, cuando cargas una web, cuando escuchas una canción en streaming o cuando envías un audio de WhatsApp desde una zona con mala cobertura. Nadie piensa en ella, nadie la ve, y sin embargo sostiene buena parte de cómo consumimos información en el mundo moderno. Esa capa es la compresión de datos, y esta post es un intento de entenderla de verdad: no solo usarla, sino abrirla, mirar dentro.

Antes de tocar una sola línea de código, hace falta responder una pregunta más básica: ¿por qué existe esto? ¿Por qué alguien, en algún momento, decidió que merecía la pena gastar ciclos de procesador en hacer los datos más pequeños en vez de simplemente guardarlos tal cual?

### El problema original: el espacio siempre costó dinero

La respuesta corta es que la información nunca ha sido gratis. Ni el espacio para guardarla, ni el tiempo para transmitirla.

En los primeros tiempos de la computación, el almacenamiento era un recurso brutalmente escaso y caro. Las tarjetas perforadas, las cintas magnéticas, los primeros discos: todo tenía un coste altísimo por cada bit guardado. Un programa o un conjunto de datos que hoy consideraríamos ridículamente pequeño podía representar una inversión económica real en aquella época. En ese contexto, cualquier técnica que permitiera representar la misma información con menos espacio no era una curiosidad académica, era una necesidad de negocio.

Lo mismo pasaba, mucho antes, con la transmisión. Las primeras compañías de telégrafos cobraban por palabra transmitida. Esto, de forma casi accidental, generó uno de los primeros incentivos económicos reales para "comprimir" mensajes: nacieron libros de códigos comerciales donde frases enteras y habituales en el mundo de los negocios se sustituían por una única palabra en clave, reduciendo drásticamente el coste de un telegrama. No es compresión de datos en el sentido moderno, pero es exactamente la misma lógica: sustituir algo largo y frecuente por algo corto, y solo mantener la referencia al significado real en un lugar aparte (el libro de códigos).

Ese es el hilo que atraviesa toda la historia que viene ahora: espacio y tiempo cuestan, y donde hay coste, tarde o temprano aparece alguien dispuesto a optimizarlo.

### El precedente que nadie llama "compresión": el código Morse

Aquí hay un dato que sorprende la primera vez que lo lees: el código Morse, tal y como lo conocemos, ya aplicaba el principio central de la compresión moderna más de un siglo antes de que existiera la palabra "algoritmo" en el sentido en que la usamos hoy.

Y hay un matiz interesante en la propia historia: el trabajo de asignar los códigos no fue tanto de Samuel Morse como de su socio, Alfred Vail. El diseño original de Morse pensaba en transmitir números, que después había que buscar en un diccionario para traducirlos a palabras completas — un sistema lento y poco práctico. Fue Vail quien propuso pasar a representar directamente las letras del alfabeto con secuencias cortas de pulsos.

Pero quedaba una pregunta clave: ¿qué letras merecían los códigos más cortos? Para responderla, Vail hizo algo muy poco "teórico" y muy práctico: se acercó a la imprenta de un periódico local en Morristown, Nueva Jersey, y contó cuántas piezas de tipo móvil tenía el impresor de cada letra. Un impresor necesita muchas más piezas de las letras que usa constantemente que de las que apenas usa, así que ese cajón de tipos era, sin que nadie lo hubiera pensado así, un mapa físico de la frecuencia de las letras en inglés.

Con ese mapa, Vail hizo lo obvio en retrospectiva pero brillante en su momento: dio los códigos más cortos a las letras más frecuentes. La "E", la letra más común del inglés, se quedó con el código más corto posible: un único punto. La "T", la segunda más común, un único guión. Las letras raras, como la "Q" o la "Z", se quedaron con secuencias largas de cuatro símbolos.

Setenta años antes de que naciera la teoría de la información, alguien ya había entendido intuitivamente su principio más importante: no todos los símbolos merecen el mismo coste de representación. Los frecuentes deberían costar poco; los raros pueden permitirse costar más, precisamente porque aparecen poco.

Guarda esta idea, porque es literalmente el corazón de uno de los algoritmos que vamos a programar más adelante en esta serie.

Cronología: de la intuición a la teoría formal

Lo que sigue a partir de aquí es una sucesión de personas resolviendo, cada vez con herramientas más sofisticadas, la misma pregunta que Vail respondió a ojo con un cajón de tipos de imprenta.

### 1948 — Claude Shannon pone la pregunta sobre papel

El salto más importante de toda esta historia no fue un algoritmo, fue un marco matemático. Claude Shannon, trabajando en los Bell Labs, publicó en 1948 un artículo que fundó la teoría de la información tal y como la conocemos. Su aportación no fue decir cómo comprimir algo, sino definir con precisión matemática qué es la información y, sobre todo, cuál es el límite teórico de cuánto se puede reducir un mensaje sin perder nada de su contenido: la entropía.

Esto es más importante de lo que parece a primera vista. Antes de Shannon, "comprimir mejor" era una cuestión de ingenio. Después de Shannon, existe una frontera matemática que ningún algoritmo sin pérdida puede cruzar, sin importar lo ingenioso que sea. Todo lo que viene después en esta cronología es, en el fondo, distintas formas de acercarse a esa frontera.

Le voy a dedicar el próximo capítulo entero a la entropía, porque es la pieza que explica por qué funciona todo lo demás.

### 1951-1952 — Un estudiante le gana la partida a su profesor

Esta es, para mí, la mejor historia de toda la cronología. En 1951, David Huffman era estudiante de posgrado en el MIT, en un curso de teoría de la información impartido por Robert Fano — uno de los grandes nombres del campo, que además había trabajado codo con codo con el propio Shannon. Fano dio a sus alumnos una opción: examen final, o un trabajo de curso sobre cómo encontrar el código binario más eficiente posible para representar símbolos según su frecuencia.

Lo que Fano no les contó es que ese problema seguía abierto. Ni siquiera él, que llevaba tiempo trabajando en ello, tenía una solución óptima — de hecho, el propio método que él había propuesto (hoy conocido como codificación Shannon-Fano) era una buena aproximación, pero no la mejor posible.

Huffman, sin saber que se estaba enfrentando a un problema sin resolver, pasó meses dándole vueltas sin éxito. Según su propio relato, estaba a pocos días del examen final, ya dispuesto a tirar la toalla y ponerse a estudiar, cuando tuvo el golpe de intuición que necesitaba: en vez de construir el código de arriba hacia abajo (como hacía el método de su profesor), había que construirlo de abajo hacia arriba, combinando primero los símbolos menos frecuentes en un árbol binario.

El resultado, publicado en 1952, no solo resolvía el problema — demostrablemente daba el código óptimo, mejor que el método que su propio profesor llevaba tiempo desarrollando. La codificación Huffman, en esencia, es una versión matemáticamente rigurosa y automatizable de lo que Alfred Vail hizo a ojo con un cajón de tipos de imprenta un siglo antes.

### 1977-1978 — Lempel y Ziv cambian la pregunta

Huffman resolvió cómo aprovechar la frecuencia de símbolos individuales. Pero un texto no es solo una bolsa de letras sueltas: tiene palabras repetidas, frases que vuelven a aparecer, estructuras que se repiten. Abraham Lempel y Jacob Ziv, en 1977 y 1978, publicaron dos algoritmos (conocidos hoy como LZ77 y LZ78) que atacaban el problema desde un ángulo distinto: en vez de mirar símbolos individuales, buscar secuencias completas que ya hubieran aparecido antes en los datos, y sustituirlas por una referencia corta a dónde aparecieron y cuánto medían.

Es el nacimiento de la compresión "por diccionario", y es un cambio de paradigma real: ya no se trata de cuántas veces aparece una letra, sino de cuánto se repite un patrón completo.

### 1984 — LZW y el algoritmo que llegó a todas partes

Terry Welch refinó esta familia de algoritmos en 1984 con LZW, una variante que construye un diccionario dinámico de patrones a medida que va leyendo los datos, sin necesidad de guardar ese diccionario en el archivo final — el propio proceso de descompresión es capaz de reconstruirlo de forma idéntica sobre la marcha. LZW se convirtió rápidamente en el motor detrás de la utilidad compress de Unix y, más adelante, del formato GIF. Esa segunda decisión, como veremos en un capítulo dedicado solo a esto, acabaría generando una de las disputas de patentes más influyentes en la historia del software libre.

### DEFLATE — cuando alguien decide combinar las dos familias

A principios de los 90, Phil Katz diseñó DEFLATE, un algoritmo que no inventa una técnica nueva, sino que combina inteligentemente las dos que ya existían: primero aplica una variante de LZ77 para eliminar repeticiones, y después aplica codificación Huffman sobre el resultado para exprimir aún más los símbolos que quedan. DEFLATE se publicó como una especificación abierta y libre de royalties, y ese detalle —nada técnico, puramente legal— es buena parte de por qué acabó siendo el motor por defecto de ZIP, de PNG y de gzip. Hoy sigue siendo, décadas después, uno de los algoritmos de compresión sin pérdida más usados del planeta.

### Los compresores de hoy

La historia no se detuvo en los 90. Google publicó Brotli en 2013, pensado específicamente para el tráfico web: combina una variante de LZ77 y Huffman con un diccionario estático precargado de fragmentos comunes en HTML, CSS y JavaScript, lo que le da una ventaja notable comprimiendo el tipo de contenido que viaja constantemente por internet. Meta, por su parte, publicó Zstandard en 2016, diseñado para ofrecer un equilibrio mucho más ajustable entre velocidad de compresión y ratio conseguido, algo crítico cuando se opera a la escala de miles de millones de peticiones diarias.

Lo llamativo es que ninguno de estos dos algoritmos modernos parte de cero: ambos siguen construyendo sobre las mismas dos ideas que ya estaban sobre la mesa en los años 70 y 80 — patrones repetidos y símbolos ponderados por frecuencia —, solo que ahora afinadas con décadas de investigación adicional en modelos estadísticos.

### El hilo que conecta todo esto

Si algo queda claro al mirar esta cronología de golpe es que casi nadie empezó de cero. Vail contó tipos de imprenta. Shannon convirtió esa intuición en matemáticas. Huffman automatizó la matemática de Shannon en un algoritmo. Lempel y Ziv añadieron una segunda dimensión que Huffman no cubría. Katz combinó ambas ideas en una sola herramienta. Google y Meta refinaron esa combinación para las necesidades de una internet que nadie en 1952 podría haber imaginado.

Setenta y tantos años después de que Shannon pusiera un límite matemático a cuánto se puede reducir un mensaje, seguimos, en el fondo, intentando acercarnos a esa frontera. Todo lo que viene en esta serie —la entropía, el porqué de mis experimentos con archivos de texto, y más adelante el código de cada uno de estos algoritmos implementado desde cero— es un intento de entender esa frontera desde dentro, en vez de limitarme a usarla sin preguntarme por qué existe.

2. Teoría de la información: entropía (Shannon)

Qué es la entropía: la información mide "sorpresa" — si un carácter se repite siempre, no aporta información nueva
Ejemplo mental: ¿qué tiene más entropía, "AAAA...1000 veces" o 1000 caracteres aleatorios? El aleatorio, y por tanto es menos compresible
La entropía marca el límite matemático de cuánto se puede comprimir algo — conecta directamente con por qué un JPG no se reduce más

3. Los dos grandes tipos de compresión

Lossless: ZIP, RAR, PNG, FLAC — recuperas el archivo exacto
Lossy: JPG, MP3, MP4 — se descarta info imperceptible a cambio de mucho tamaño
Por qué a veces preferimos perder datos (imagen) y otras necesitamos exactitud matemática (texto, código)

4. El experimento práctico inicial /////

Primera impresión, estaba intentando comprimir un archivo de texto con esta info "Este archivo es un archivo de prueba, quiero comprobar su peso antes y después de comprimir con un software como Winrar", su peso en txt era de 127 bytes, y su comprimido en rar es de 186 bytes

Esto sucede porque al comprimir un archivo extremadamente pequeño (como un .txt con una sola frase), el archivo resultante (.zip o .rar) suele pesar más que el original. Esto no es un error del software, sino un fenómeno matemático predecible: un texto tan corto casi no tiene patrones repetidos, por lo que el algoritmo de compresión apenas tiene margen para "encoger" los datos reales.

El aumento de peso se debe a que el programa no solo envuelve el texto, sino que crea un contenedor que añade bytes extra por dos motivos: la sobrecarga de metadatos (cabeceras obligatorias que guardan el nombre del archivo, fechas, permisos y códigos de seguridad CRC) y el diccionario de compresión (las reglas matemáticas o "instrucciones" necesarias para que otro equipo pueda descomprimirlo). En resumen, los bytes que ocupa el "envase" superan con creces los pocos bytes que se lograron reducir del contenido original.

Para que podamos ver la compresión he generado un txt con más caracteres que pesa 1005 bytes, en este caso al hacer la misma compresión podemos ver que el archivo a pasado de pesar 1005 a pesar 640 bytes, esto ha sucedido ya que hemos conseguido que el contenido del txt pese más que los metadatos que genera el rar y podamos ver el beneficio de la compresion

Esto sucede de manera muy similar en imágenes, si tenemos archivos de tipo BMP, TIFF sin compresion o Raw de la camara, podemos comprimirlos y reducir sustancialmente su tamaño, en el caso de los JPG, PNG, WEBP o GIF esa ereduccion seria minima ya que son formatos que ya estan comprimidos

5. Aplicándolo a otros tipos de archivo

Imágenes: BMP/TIFF/RAW vs JPG/PNG/WEBP
Audio/vídeo: WAV vs MP3
Tabla comparativa

6. Prácticas en código: implementando los algoritmos tú mismo

- Nivel 1 — RLE (Run-Length Encoding)

Codificador + decodificador: "AAAAABBBCC" → "5A3B2C"
Caso límite importante (Día 4 de tu path): probar con "ABCDE" → el RLE ingenuo da "1A1B1C1D1E", más grande que el original. Implementar lógica para detectarlo y evitarlo
Bueno para imágenes simples tipo BMP antiguo, terrible para texto normal

- Nivel 2 — Codificación Huffman

La propiedad de prefijo (Día 6): por qué ningún código puede ser prefijo de otro — ejercicio en papel con 4 letras antes de programar
Construcción del árbol (Día 7): algoritmo ávido — contar frecuencias, combinar los dos nodos menos frecuentes, repetir. Simulación a mano con "MISSISSIPPI" como caso de estudio antes de tocar código
Implementación (Días 8-9): función de frecuencias → cola de prioridad/min-heap → ciclo de ensamblado del árbol → recorrido recursivo para generar los códigos
Manipulación de bits real (Día 10): el reto de que escribir "010" como texto gasta 3 bytes, no 3 bits — empaquetar bits sueltos en bytes reales con operadores bitwise (<<, |, &) y escribir un archivo binario de verdad

- Nivel 3 — LZ77 (ventana deslizante)

Busca coincidencias recientes en una ventana y las sustituye por referencias (distancia, longitud)
Base conceptual de ZIP/GZIP/DEFLATE

- Nivel 4 — LZW (diccionario dinámico)

Codificación (Día 12): empieza con diccionario de 256 caracteres ASCII, añade combinaciones nuevas automáticamente. Simular a mano "ABABAB" y ver cómo aparecen códigos >256
Decodificación (Día 13): la parte "milagrosa" — no hace falta guardar el diccionario en el archivo, el decodificador lo reconstruye deduciéndolo sobre la marcha. Simular a mano usando solo los números emitidos
Implementación (Día 14): estructura eficiente (hash map o trie) que asocie cadenas a códigos enteros
Casos límite (Día 19): qué pasa cuando el diccionario se llena (ej. límite de 12 bits = 4096 entradas) — estrategias reales: limpiar, congelar, o aumentar bits dinámicamente

- Nivel 5 — Combinar LZ + Huffman (mini-DEFLATE)

Aplicar Huffman sobre la salida de tu LZ77 — es literalmente lo que hace DEFLATE
Comparar tu versión casera contra zlib/gzip con los mismos archivos

7. Historia, patentes y controversia: GIF vs PNG
(nueva sección, viene del Día 15 — contenido narrativo muy fuerte para un blog)

Unisys patentó LZW, CompuServe lo usó en GIF sin saberlo
El "Burn All GIFs Day" y la reacción de la comunidad open source
Cómo nace PNG como alternativa libre de patentes, usando DEFLATE
Por qué esta historia explica decisiones de arquitectura de software que siguen vigentes hoy

8. Sistemas modernos: DEFLATE y compresión web
(nueva sección, Días 16-17)

DEFLATE = LZ + Huffman combinados, motor real de ZIP y PNG
Explorar la estructura de un ZIP con hexdump o documentación de Info-ZIP
Compresión en diseño web/SEO: PNG (lossless, logos) vs JPEG/WebP (lossy, fotos)
Probar una imagen con TinyPNG (quantization) y comparar reducción visual vs bytes

9. Práctica final: tu propio mini-compresor de imágenes

BMP sin comprimir + filtro de predicción tipo PNG (cada píxel como diferencia del anterior) + tu mini-DEFLATE del Nivel 5
Comparar contra: BMP original, PNG real, tu mini-DEFLATE sin el filtro de predicción
Por qué PNG no es solo "DEFLATE aplicado a una imagen", sino DEFLATE + un paso previo específico para píxeles

10. Métricas y benchmarking

Añadir "Compression Ratio" (tamaño original / comprimido) a cada uno de tus programas
Probar con archivos de 1MB y 10MB, medir tiempos de ejecución (introducir Big O de forma práctica, no solo teórica)
Buen punto para una tabla o gráfico comparando tus 5 niveles + zlib real

11. Conclusión

Cuándo comprimir merece la pena y cuándo no
Reflexión: la compresión como capa invisible que sostiene streaming, nube y móviles — conecta con "Arquitecturas de la Invisibilidad"

