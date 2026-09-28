# Blog de Mateo Soliño

Código de <https://mateosolinho.github.io> (Jekyll + tema Chirpy 6.1, copiado y modificado en el repo).
Las plantillas del tema están modificadas (selector de idioma), así que no conviene migrar a chirpy-starter sin portarlas.
Este README no se publica (está en `exclude` de `_config.yml`).
Última revisión: 28-09-2026.

## Pendientes

- **Dominio propio** (p. ej. `mateosolino.dev`, ~10–15 €/año). GitHub Pages lo soporta gratis:
  fichero `CNAME` + registros DNS + actualizar `url` en `_config.yml`.

### Por decidir

- **Enlazar el portfolio de proyectos** desde el blog (ya existe un portfolio aparte). Falta concretar cómo
  (entrada en la barra lateral, enlace en el About…).
- **Google Analytics** (`G-MRKENC2RQ4`) sin aviso de cookies: en España hace falta consentimiento.
  Opciones: quitarlo o cambiarlo por analítica sin cookies (GoatCounter, Cloudflare Web Analytics).
- **About y CV desactualizados:** el About dice "junior developer, training in AI"; el CV es de dic-2024.
  Falta último curso GEI (UDC), FORVIA y ANUBE. El About podría ser bilingüe.
- **Comentarios con giscus** (GitHub Discussions, gratis): rellenar `comments.giscus` en `_config.yml`.
- **Google Search Console:** si se quiere, poner el token del método "Etiqueta HTML" en
  `google_site_verification` (antes tenía el nombre del fichero, que no valía).

## Cómo funciona (notas)

### Probar en local
```sh
npm install && npm run build   # JS del tema → assets/js/dist (está en .gitignore, el workflow lo genera)
PATH=/opt/homebrew/opt/ruby@3.3/bin:$PATH \
SDKROOT=/Library/Developer/CommandLineTools/SDKs/MacOSX26.5.sdk \
bundle exec jekyll serve      # http://127.0.0.1:4000
```
`SDKROOT` hace falta porque con el SDK de macOS 27 las gems nativas no compilan.

### Posts
- Fecha con zona horaria de España (`+0200` verano, `+0100` invierno). `future: true` publica también los
  posts con fecha futura.
- Si el título lleva `:`, ponerlo entre comillas (si no, el front matter se rompe).
- Fórmulas: añadir `math: true` al front matter (MathJax solo se carga en esos posts).
- Imágenes en WebP, máx. 1600 px de ancho (las capturas con texto a calidad 90).
- `lang: es` o `lang: en` según el idioma del post (se usa en `<html lang>` y en las metaetiquetas).

### Lecturas (papers, artículos…)
Posts que resumen o explican una fuente. Con `source:` en el front matter aparece la ficha arriba del post
y el post entra solo en la página **Lecturas** (`_tabs/readings.md`). Plantilla (`_posts/AAAA-MM-DD-slug.md`):

```yaml
---
title: "Attention Is All You Need, explicado"
lang: es                      # es | en, el idioma en que escribes el post
date: 2026-10-01 18:00:00 +0200
categories: [Readings]
tags: [Machine Learning, LLM]
math: true                    # solo si hay fórmulas
mermaid: true                 # solo si hay diagramas
source:
  type: paper                 # paper | article | post | talk | video | book
  title: Attention Is All You Need
  authors: Vaswani et al.
  year: 2017
  venue: NeurIPS              # opcional: congreso, revista, web...
  url: https://arxiv.org/abs/1706.03762
  pdf: https://arxiv.org/pdf/1706.03762       # opcional
  code: https://github.com/tensorflow/tensor2tensor   # opcional
  tldr: La idea principal en una o dos frases (admite **markdown**).
---
```

Estructura que funciona bien: Problema → Idea clave → Método → Resultados → Limitaciones → Mi opinión.

Recursos dentro del post:
- Idea clave destacada: `> texto` y en la línea siguiente `{: .prompt-key }`
  (también `.prompt-tip`, `.prompt-info`, `.prompt-warning`, `.prompt-danger`).
- Fórmulas: `$...$` en línea y `$$...$$` en bloque (con `math: true`).
- Diagramas: bloque ```` ```mermaid ```` (con `mermaid: true`).
- Referencias: notas al pie `texto[^1]` y al final `[^1]: Autor, año.`
- Figuras del paper: citar siempre la fuente en el pie de la imagen.

### Categorías y tags
Se escriben **en inglés** en el front matter y se traducen en la web. Usar las existentes:
- Categorías (un solo nivel): `Readings`, `Cybersecurity`, `Artificial Intelligence`, `Projects`, `Technology`.
- Tags: Hack The Box, Linux, Windows, Malware, Real-World Incidents, Machine Learning, Neural Networks,
  TensorFlow, Computer Vision, LLM, Python, Rust, SQL, Data Analysis, Aerospace, Embedded Systems,
  Computing History, Algorithms, Math.

Si se crea una nueva que no sea nombre propio, añadir su traducción en `term_names` de `_data/locales/es-ES.yml`.

### Idioma de la interfaz
El idioma de la interfaz (no de los posts) sigue al del navegador y se cambia con el botón ES/EN de la
barra lateral (se guarda en `localStorage`).
- Textos: `_data/locales/en.yml` y `es-ES.yml`.
- Plantillas: `_includes/t.html` (texto), `t-attr.html` (atributos), `term-name.html` (categorías/tags),
  `ui-lang.html` (detección en `<head>`), `ui-lang-apply.html` (atributos + botón).

### Favicon
Diseño original en `assets/img/favicons/favicon.svg` (M + órbita). Los PNG/ICO (16, 32, 180 iOS, 192/512
Android, 150 Windows) se generan a partir de él; los de iOS/Android usan una versión sin esquinas
redondeadas porque el sistema aplica su propia máscara.
