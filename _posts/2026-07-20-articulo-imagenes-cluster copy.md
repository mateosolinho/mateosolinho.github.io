---
title: Model Context Protocol (MCP)
date: 2026-05-26 12:30:00 +0800
categories: [Programación, Desarrollo de Software]
tags: [OCR, Tesseract, Python, OpenCV, Python, DataScience]
image:
  path: /assets/img/post/telemetria_starship/1.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

# =================== UC ===================

---

## 1. Introducción

En la última década, el desarrollo de **modelos de AI**, en especial los **modelos de lenguaje grande** (LLMs, *large language models*), ha transformado el panorama del software moderno. Sin embargo, a medida que estos modelos se despliegan en aplicaciones del mundo real, ha surgido una necesidad crítica: **conectarlos de forma estructurada, segura y reutilizable a fuentes de datos, herramientas y sistemas externos**.

Hasta hace poco, esta integración se realizaba de forma ad hoc, por medio de *plugins* propietarios, *scripts* específicos o adaptadores cerrados, lo que limitaba la interoperabilidad, dificultaba el mantenimiento y presentaba desafíos de seguridad y escalabilidad.

Con el objetivo de resolver estas limitaciones, **Anthropic propuso en noviembre de 2024** el **Model Context Protocol (MCP)**, un estándar abierto que define cómo los modelos de IA pueden interactuar con su entorno contextual de forma **modular, portátil y auditable**. En palabras simples, MCP actúa como un **protocolo universal para describir y acceder al contexto que rodea a un modelo durante su ejecución**, incluyendo tanto recursos pasivos (archivos, textos, bases de datos) como activos (herramientas, APIs, *prompts* funcionales).

### 1.1 Una analogía necesaria: el USB-C de la IA

Tal como el estándar **USB-C** ha unificado la forma en que los dispositivos electrónicos se conectan a cargadores, pantallas y periféricos, **MCP busca convertirse en el conector estandarizado entre modelos de lenguaje y su ecosistema de ejecución**. Esta analogía no es meramente estética: al igual que en hardware, la interoperabilidad es crucial para la eficiencia, la seguridad y la innovación abierta.

### 1.2 Propósito de este artículo

Este artículo tiene como objetivo proporcionar una visión **técnica y exhaustiva** del *Model Context Protocol*, abordando no solo su arquitectura y casos de uso, sino también sus implicaciones prácticas y sus desafíos emergentes. A lo largo del contenido, analizaremos:

- Qué es un MCP desde el punto de vista técnico,
- Cómo está estructurado (componentes, mensajes, transporte),
- Qué problemas resuelve,
- Casos de uso reales y ejemplos prácticos,
- Cuáles son los riesgos de seguridad que introduce,
- Y por qué representa un avance clave en la era de los agentes IA autónomos y modulares.

## 2. Contexto y motivación

Uno de los mayores desafíos al trabajar con modelos de lenguaje en aplicaciones reales es la **incorporación de contexto externo**: información relevante, actualizada o específica del usuario que el modelo necesita para generar respuestas útiles. Aunque los LLMs son entrenados con grandes cantidades de datos, su conocimiento es estático y limitado por su fecha de corte. Por tanto, requieren acceso a recursos externos —como documentos, herramientas, bases de datos o APIs— para poder actuar de manera efectiva en entornos del mundo real.

### 2.1 El problema de la integración ad hoc

Antes de MCP, la conexión entre un modelo y su entorno contextual se resolvía caso por caso:

- Plugins cerrados (como los plugins de ChatGPT o extensiones específicas de Claude),
- Pipelines manuales en herramientas como LangChain o Semantic Kernel,
- Desarrollos personalizados para integrar funciones con datos internos.

Esto generaba múltiples problemas:

- **Falta de interoperabilidad**: cada integración tenía su propia interfaz y semántica.
- **Baja modularidad**: no se podía reutilizar el mismo conector entre distintas plataformas.
- **Difícil auditoría**: era complejo saber qué datos estaban siendo usados por el modelo y cómo.
- **Riesgos de seguridad**: no había forma estandarizada de controlar acceso a herramientas externas.

En entornos donde la trazabilidad, el aislamiento de contextos y la gobernanza de datos son críticos (por ejemplo, en sectores corporativos, médicos o financieros), estas limitaciones suponían un obstáculo significativo para la adopción de modelos LLM productivos.

### 2.2 La necesidad de un estándar universal

El surgimiento de agentes autónomos —sistemas capaces de planificar, razonar y ejecutar acciones— hizo aún más evidente la necesidad de un protocolo que:

1. Describa claramente el entorno en el que opera el modelo,
2. Permita al modelo descubrir, consultar y utilizar herramientas y datos de forma estructurada,
3. Sea auditable, portable y seguro por diseño.

Inspirados por estándares como el **Language Server Protocol (LSP)** —que transformó la integración de herramientas de desarrollo mediante una interfaz común—, **Anthropic diseñó MCP como una solución modular, extensible y abierta** para conectar modelos con su contexto. Al igual que LSP permitió que editores como VSCode o Neovim se comunicaran con múltiples lenguajes de programación de forma uniforme, MCP busca que cualquier modelo pueda interoperar con cualquier fuente o herramienta que exponga una interfaz estándar.

### 2.3 Una evolución inevitable

MCP no solo responde a una necesidad técnica, sino que refleja un cambio estructural en cómo concebimos los sistemas de IA: ya no como cajas negras monolíticas, sino como agentes componibles que interactúan con su entorno, guiados por protocolos explícitos, transparentes y reutilizables.

Esta transición hacia arquitecturas abiertas, interoperables y controlables es esencial para la próxima generación de aplicaciones basadas en IA, y coloca al Model Context Protocol como una de las piezas centrales del ecosistema de agentes inteligentes en 2025 y más allá.

## 3. Definición técnica del Model Context Protocol

El **Model Context Protocol (MCP)** es un protocolo de comunicación estructurado basado en **JSON-RPC 2.0**, diseñado para permitir que los modelos de lenguaje interactúen con su entorno contextual de forma estandarizada. Define tanto la **interfaz de comunicación** como la **estructura de los mensajes** que se intercambian entre los componentes del sistema.

El protocolo establece una arquitectura **cliente-servidor**, en la que uno o varios **clientes MCP** (normalmente parte del host donde se ejecuta el modelo) se comunican con uno o varios **servidores MCP** que exponen recursos y herramientas que el modelo puede utilizar durante la inferencia.

### 3.1 Arquitectura general

La arquitectura del MCP sigue un patrón cliente-servidor modificado para su uso dentro de entornos de modelos de lenguaje. Los componentes clave son:

- **Host**: Aplicación principal que ejecuta o interactúa con un modelo de lenguaje (por ejemplo, un IDE, asistente conversacional o entorno de escritorio como Claude Desktop).
- **Cliente MCP**: Módulo dentro del host encargado de gestionar las conexiones con servidores MCP.
- **Servidor MCP**: Servicio que expone recursos, herramientas y *prompts* accesibles para el modelo.
- **Recursos/entorno**: Archivos locales, bases de datos, APIs o funciones externas.

```text
    Host
     └── Cliente MCP
           ├── Servidor MCP (local o remoto)
           └── Servidor MCP (otro recurso externo)
```

Cada conexión cliente-servidor se rige por una sesión de mensajes intercambiados vía JSON-RPC.

### 3.2 Protocolo de mensajería: JSON-RPC 2.0

MCP utiliza JSON-RPC 2.0 como capa de transporte semántico. JSON-RPC es un protocolo ligero que permite invocar métodos de forma remota a través de objetos JSON. MCP aprovecha esta capacidad para permitir que los modelos de lenguaje consulten o invoquen herramientas externas.

Los tipos de mensajes permitidos incluyen:

- Request:
  ```json
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/list",
    "params": {}
  }
  ```

- Response:
  ```json
  {
    "jsonrpc": "2.0",
    "id": 1,
    "result": { "tools": ["summarize", "queryDatabase", "sendEmail"] }
  }
  ```

- Notification (no requiere respuesta):

  ```json
  {
    "jsonrpc": "2.0",
    "method": "context/updated",
    "params": { "documentId": "123", "status": "modified" }
  }
  ```

Cada servidor MCP puede implementar diferentes subconjuntos del protocolo, de acuerdo con las primitivas que soporte (ver punto 4).

### 3.3 Transporte de mensajes

MCP permite múltiples mecanismos de transporte físico para transmitir los mensajes JSON-RPC. Los principales definidos actualmente son:

    stdio: Estándar de entrada/salida del sistema operativo, útil para procesos locales.

    HTTP/SSE (Server-Sent Events): Para casos donde se requiere comunicación en red o streaming continuo de datos.

La especificación permite que el canal de transporte sea negociado entre cliente y servidor según el entorno de ejecución.

### 3.4 Extensibilidad y modularidad

Una de las características distintivas de MCP es su capacidad para describirse a sí mismo. A través de métodos como introspect, los servidores MCP pueden informar qué herramientas y recursos están disponibles, bajo qué esquemas, y cómo se espera que se utilicen.

Esta capacidad de auto-descripción permite construir sistemas IA dinámicos y adaptativos, donde un modelo puede descubrir su entorno y modificar su comportamiento de acuerdo con los recursos accesibles, sin requerir redefinición del código fuente.

### 3.5 Formalización del contexto

El “contexto” en MCP no se limita al prompt textual. Se define como una estructura compuesta de:

  - Recursos de datos (documentos, bases de datos, streams),

  - Funciones o herramientas invocables,

  - Plantillas o prompts preconfigurados,

  - Estado o metadatos persistentes.

Este diseño proporciona una base sólida para que los modelos operen como agentes cognitivos, capaces de razonar, actuar y adaptarse en tiempo real según su contexto operativo.
