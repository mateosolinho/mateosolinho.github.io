title: Arquitecturas de la Invisibilidad: Viaje hacia la compresión de datos
date: 2026-07-18 12:30:00 +0800

1. Introducción / Historia

Por qué existe la compresión (limitaciones de almacenamiento y transmisión)
Código Morse como precedente (ya usaba códigos cortos para letras comunes)
Línea temporal: Shannon (teoría de la información) → Huffman (1952) → LZ77/LZ78 (1977-78) → LZW → DEFLATE → compresores modernos (Brotli, Zstandard)

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

