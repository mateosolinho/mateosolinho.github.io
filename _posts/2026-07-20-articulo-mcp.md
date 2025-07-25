---
title: Model Context Protocol (MCP)
date: 2025-07-23 12:30:00 +0800
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
 
## 4. Componentes clave del protocolo
 
El Model Context Protocol (MCP) está diseñado para ser **modular, extensible y bidireccional**. Su arquitectura no solo especifica cómo se transportan mensajes, sino también qué entidades funcionales deben existir para que la interacción entre modelos de lenguaje y su entorno se produzca de forma coherente y segura.
 
Los componentes se dividen en dos grandes categorías: **componentes estructurales** (host, cliente, servidor) y **componentes funcionales** (recursos, herramientas, prompts). Esta sección describe ambos.
 
### 4.1 Host
 
El **host** es la aplicación principal que contiene o ejecuta al modelo de lenguaje. Puede tratarse de un asistente de escritorio, un IDE, un sistema conversacional embebido o cualquier otro entorno donde el modelo funcione como componente central.
 
El host:
 
- Gestiona la sesión de usuario o interacción,
- Contiene o lanza al modelo (local o vía API),
- Incorpora uno o más **clientes MCP**,
- Orquesta la comunicación entre modelo y contexto.
 
Ejemplos comunes de hosts: Claude Desktop, ChatGPT con herramientas, VSCode con asistencia IA, asistentes conversacionales integrados en navegadores.
 
### 4.2 Cliente MCP
 
El **cliente MCP** es una entidad dentro del host que mantiene una conexión activa 1:1 con un **servidor MCP**. Su función es actuar como intermediario de bajo nivel:
 
- Establece el canal de transporte (stdio, HTTP/SSE),
- Envía y recibe mensajes JSON-RPC,
- Interpreta y adapta la semántica de los recursos según el modelo que los consume.
 
Un host puede mantener múltiples clientes MCP simultáneamente, conectados a distintos servidores para diferentes propósitos (por ejemplo, un cliente para herramientas matemáticas y otro para acceso a archivos).
 
### 4.3 Servidor MCP
 
Un **servidor MCP** es cualquier servicio que exponga **recursos estructurados, prompts o herramientas** a través del protocolo MCP. Estos servidores pueden ejecutarse localmente o en red, y deben cumplir con la especificación del protocolo.
 
Funciones principales del servidor:
 
- Implementar los métodos definidos por el protocolo (`tools/list`, `resources/get`, etc.),
- Exponer de forma declarativa qué recursos y funcionalidades ofrece,
- Gestionar autorizaciones, versiones, descripciones y metadatos.
 
Ejemplos de servidores MCP ya existentes:
 
- Conectores a Google Drive, GitHub o Slack,
- Servidores locales que exponen carpetas y archivos,
- Interfaces hacia APIs REST o bases de datos.
 
### 4.4 Recursos
 
Un **recurso** es cualquier entrada de datos estática o dinámica que el modelo puede consumir como parte de su contexto. Los recursos se representan como objetos serializados con metadatos y se identifican mediante rutas únicas dentro del servidor MCP.
 
Ejemplos de recursos:
 
- Documentos (`docx`, `pdf`, `md`, etc.),
- Archivos de código fuente,
- Tablas de bases de datos,
- Resúmenes de navegación web o resultados de búsqueda.
 
Los recursos pueden incluir también su contenido embebido, referencias cruzadas y anotaciones semánticas, facilitando que el modelo entienda su estructura y relevancia contextual.
 
### 4.5 Herramientas
 
Las **herramientas** son funciones invocables por el modelo de lenguaje durante su ejecución. A diferencia de los recursos, que son pasivos, las herramientas son activas y pueden producir efectos secundarios o resultados dinámicos.
 
Una herramienta expuesta por un servidor MCP debe declarar:
 
- Su identificador y propósito (`"tool_id": "calculate_vat"`),
- Su tipo de entrada y salida (`"input_schema"`, `"output_schema"`),
- Sus restricciones y parámetros (`"timeout"`, `"rate_limit"`).
 
Ejemplos de herramientas:
 
- Consultas SQL sobre bases de datos,
- Ejecutores de código Python,
- Llamadas a APIs externas (por ejemplo, `GET /weather?city=Barcelona`),
- Funciones de cálculo o transformación.
 
El modelo puede decidir invocar una herramienta si el cliente MCP le ofrece la información adecuada sobre su disponibilidad y utilidad contextual.
 
### 4.6 Prompts
 
Los **prompts** en MCP son plantillas reutilizables de interacción textual que un modelo puede utilizar como punto de partida para una tarea. No deben confundirse con el prompt de usuario libre: estos son prompts predefinidos, especializados y diseñados para tareas recurrentes.
 
Cada prompt puede incluir:
 
- Texto base con variables parametrizables,
- Ejemplos de uso (*few-shot learning*),
- Instrucciones de formato o estilo de respuesta.
 
Un ejemplo de prompt podría ser:
 
```json
{
  "id": "summarize_customer_feedback",
  "template": "Resume en 5 puntos las opiniones del cliente: {{feedback_text}}",
  "defaults": {
    "tone": "neutral",
    "max_points": 5
  }
}
```
 
El uso estructurado de prompts dentro del protocolo permite que el modelo se adapte a tareas comunes sin requerir que el usuario proporcione una instrucción detallada cada vez.
 
## 5. Casos de uso prácticos
 
El verdadero valor del Model Context Protocol (MCP) se manifiesta cuando se aplica en escenarios del mundo real, donde los modelos de lenguaje necesitan acceder a datos externos, ejecutar funciones especializadas o integrarse con flujos de trabajo complejos. Esta sección presenta varios **casos de uso representativos**, que ilustran la versatilidad y aplicabilidad del protocolo en contextos modernos de inteligencia artificial.
 
### 5.1 Acceso estructurado a documentos y archivos
 
Un entorno MCP puede exponer como recursos una colección jerárquica de archivos y documentos, de forma que el modelo los pueda consultar, resumir, comparar o editar.
 
**Ejemplo**: un servidor MCP local expone un directorio de trabajo:
 
```json
{
  "resource_id": "docs/contrato2025.pdf",
  "type": "document/pdf",
  "content": "...",
  "metadata": {
    "title": "Contrato Marco 2025",
    "modified": "2025-07-14T12:01:00Z"
  }
}
```
 
El modelo puede acceder a este recurso, extraer cláusulas relevantes y generar un resumen legal sin requerir una carga manual por parte del usuario.
 
### 5.2 Asistencia en entornos de desarrollo (IDE)
 
Integrado en un IDE como VSCode o Zed, un cliente MCP puede exponer archivos de código fuente, historial de cambios y herramientas como linters o ejecutores de pruebas, lo que permite al modelo realizar:
 
- Diagnóstico de errores
 
- Refactorización guiada
 
- Generación de pruebas unitarias
 
- Revisión de código orientada por contexto
 
Ventaja clave: el modelo no actúa sobre texto plano aislado, sino sobre un contexto estructurado y activo, sincronizado con el proyecto.
 
### 5.3 Automatización de flujos de trabajo
 
MCP permite al modelo invocar herramientas externas para ejecutar tareas automatizadas dentro de una sesión controlada.
 
Ejemplo: el modelo actúa como asistente para redactar y enviar correos de seguimiento a través de una herramienta MCP conectada a una API de email.
 
```json
  {
    "tool_id": "send_email",
    "params": {
      "to": "cliente@empresa.com",
      "subject": "Resumen de reunión",
      "body": "Estimado cliente, adjunto el resumen de nuestra reunión..."
    }
  }
```
 
La interacción puede incluir validación, confirmación y seguimiento, todo orquestado mediante el cliente MCP.
 
###   5.4 Modelos como agentes cognitivos
 
Una aplicación avanzada consiste en permitir que el modelo descubra y seleccione herramientas dinámicamente de acuerdo con su entorno MCP. Esto sienta las bases para un enfoque de agentes autónomos.
 
Flujo típico:
 
1. El modelo consulta tools/list para ver qué puede hacer.
 
2. Analiza la tarea actual y decide cuál herramienta usar.
 
3. Genera la invocación apropiada.
 
4. Procesa el resultado y lo integra a su flujo de razonamiento.
 
Este patrón se acerca al paradigma de toolformer o function-calling agents, pero usando una capa de abstracción estandarizada.
 
### 5.5 Integración con sistemas de información empresarial
 
MCP puede actuar como intermediario entre un modelo de lenguaje y los sistemas internos de una organización: CRM, ERP, sistemas de soporte o inteligencia de negocio.
 
Ejemplo: un analista interactúa con el modelo para generar informes semanales:
 
- El servidor MCP expone consultas preconfiguradas a la base de datos de ventas.
 
- El modelo selecciona una, la ejecuta y resume los resultados en lenguaje natural.
 
- El usuario revisa y exporta el informe en PDF.
 
Este caso de uso reduce la fricción entre el conocimiento técnico requerido para acceder a los datos y la capacidad de analizarlos y comunicarlos de forma efectiva.
 
### 5.6 Interacción multimodal (textual + visual + código)
En entornos que soportan recursos multimodales, MCP permite que el modelo acceda a imágenes, gráficos, resultados numéricos o fragmentos de código estructurados como recursos.
 
Ejemplo:
 
- Un recurso visual: imagen médica, gráfico estadístico.
 
- Un recurso estructurado: tabla CSV.
 
- Un recurso de código: notebook Jupyter con salidas parciales.
 
El modelo puede consultar, razonar, comparar o transformar estos recursos de forma más natural y precisa, en lugar de operar solo sobre texto plano.
 
## 6. Beneficios y limitaciones
 
El Model Context Protocol (MCP) introduce una abstracción fundamental en la interacción entre modelos de lenguaje y entornos externos. Si bien su diseño representa una mejora significativa en términos de flexibilidad y estandarización, también plantea ciertos desafíos técnicos, operativos y de adopción que es necesario considerar. Este apartado evalúa ambos lados del protocolo: sus beneficios clave y sus limitaciones actuales.
 
### 6.1 Beneficios
 
#### 6.1.1 Estandarización de interfaces contextuales
 
Uno de los principales logros de MCP es proporcionar un **marco de referencia común** para que distintos sistemas expongan recursos, herramientas y prompts de forma coherente ante los modelos. Esto elimina la necesidad de integrar cada aplicación de manera ad hoc, permitiendo interoperabilidad entre proveedores y plataformas.
 
#### 6.1.2 Mejora de la capacidad contextual del modelo
 
Gracias al protocolo, el modelo puede operar sobre **datos estructurados y actuales**, no solo sobre texto libre. Esto permite interacciones más ricas, tareas más complejas y una comprensión más precisa del entorno.
 
Ejemplo: comparar dos contratos legales, ejecutar pruebas unitarias en código o revisar las métricas de un pipeline de datos.
 
#### 6.1.3 Extensibilidad y modularidad
 
MCP está diseñado de forma **modular**, permitiendo a los desarrolladores crear sus propios servidores, herramientas o recursos personalizados sin necesidad de modificar el núcleo del host o del modelo. Esto acelera la innovación y permite adaptaciones específicas por dominio.
 
#### 6.1.4 Alineación con paradigmas emergentes
 
El protocolo es compatible con tendencias actuales como:
 
- **Agentes autónomos** basados en modelos LLM,
- **LLMs con herramientas y funciones externas** (*tool use*),
- **Sistemas cognitivos asistidos por contexto en tiempo real**.
 
Su arquitectura lo posiciona como una infraestructura clave para el futuro de la IA aplicada.
 
#### 6.1.5 Separación de responsabilidades y seguridad
 
Al delegar el acceso a datos sensibles o funciones críticas al servidor MCP, se promueve una **separación de responsabilidades** clara. Esto facilita el control de permisos, auditoría de uso y cumplimiento normativo.
 
### 6.2 Limitaciones
 
#### 6.2.1 Curva de adopción inicial
 
Implementar un cliente y un servidor MCP desde cero requiere comprender las especificaciones del protocolo, gestionar transporte, autenticación, estructuras de recursos y herramientas. Esto puede suponer una **barrera técnica** para equipos pequeños o con poca experiencia en protocolos.
 
#### 6.2.2 Dependencia de la compatibilidad del modelo
 
El protocolo MCP es solo una capa de transporte. Para que funcione, el modelo LLM debe ser capaz de:
 
- Interpretar correctamente los recursos estructurados,
- Decidir cuándo y cómo invocar herramientas,
- Mantener coherencia entre contexto externo y generación.
 
Esto limita su aplicación a modelos suficientemente avanzados y configurados para *tool use* o agentes.
 
#### 6.2.3 Latencia y rendimiento
 
Cada interacción con un servidor MCP (por ejemplo, una consulta de recurso o invocación de herramienta) añade latencia a la sesión del modelo. En aplicaciones donde la inmediatez es clave, esto puede afectar la experiencia del usuario si no se gestiona correctamente mediante cachés, anticipación contextual o colas de ejecución.
 
#### 6.2.4 Seguridad y control de acceso
 
Aunque el protocolo favorece una separación clara entre modelo y datos, la exposición de recursos o herramientas debe ser controlada cuidadosamente:
 
- ¿Qué herramientas puede invocar el modelo sin supervisión?
- ¿Qué datos sensibles están disponibles como recursos?
- ¿Qué mecanismos de validación o trazabilidad existen?
 
El diseño de servidores MCP seguros y auditables es esencial para su uso en entornos corporativos o regulados.
 
#### 6.2.5 Ecosistema aún incipiente
 
Al tratarse de un protocolo reciente, el ecosistema de **clientes, servidores, bibliotecas y plantillas reutilizables** todavía está en construcción. Esto limita su adopción inmediata a proyectos pioneros o experimentales, si bien se espera un crecimiento sostenido en los próximos años.
 
## 7. Alternativas y protocolos relacionados
 
Aunque el Model Context Protocol (MCP) representa un enfoque novedoso y estructurado para conectar modelos de lenguaje con su entorno, no es la única solución que ha surgido con este objetivo. Existen diversas tecnologías, marcos y patrones arquitectónicos que buscan abordar el problema de la contextualización, la ejecución de herramientas externas y la integración funcional de los LLMs. Este apartado presenta un análisis comparativo entre MCP y las principales alternativas disponibles actualmente.
 
### 7.1 LangChain
 
LangChain es una de las bibliotecas más conocidas para construir aplicaciones que integran LLMs con herramientas, bases de datos, documentos y APIs. A diferencia de MCP, **LangChain es una librería orientada a flujos de ejecución dentro de entornos Python**, y no un protocolo interoperable.
 
**Comparación**:
 
| Característica         | MCP                               | LangChain                           |
|------------------------|------------------------------------|--------------------------------------|
| Tipo                   | Protocolo RPC                      | Biblioteca Python                    |
| Interoperabilidad      | Alta (modelo-agnóstico)            | Baja (acoplado al entorno de ejecución) |
| Modularidad            | Alta (cliente, servidor, herramientas separadas) | Moderada (todo en el mismo proceso) |
| Observabilidad         | Estandarizada por diseño           | Depende de implementación           |
| Agentes y reasoning    | Soportado si el modelo lo permite  | Altamente integrado                 |
 
LangChain proporciona herramientas útiles para la rápida construcción de agentes, pero no define una interfaz estándar para exponer contexto a modelos de terceros.
 
### 7.2 Semantic Kernel
 
Semantic Kernel, impulsado por Microsoft, permite construir agentes que combinan prompts, funciones y memoria contextual. Está pensado para integrarse con servicios empresariales (Microsoft Graph, Azure, etc.).
 
**Comparación clave**:
 
- **Semantic Kernel** promueve la composición de flujos de ejecución LLM + funciones, pero no define un protocolo estandarizado para interoperar con herramientas externas.
- MCP puede integrarse con Semantic Kernel como capa de transporte o descubrimiento de funciones.
 
### 7.3 OpenAI Function Calling / Tool Use API
 
OpenAI introdujo `function_call` como mecanismo para que los modelos GPT pudieran invocar funciones declaradas por el usuario. Más recientemente, su **Tool Use API** amplía este modelo hacia entornos más complejos con múltiples herramientas.
 
**Comparación con MCP**:
 
- Ambos permiten que el modelo invoque funciones externas con argumentos estructurados.
- Sin embargo, **OpenAI define un protocolo cerrado, específico de sus modelos**, mientras que MCP propone un estándar abierto, independiente del proveedor.
- La semántica de herramientas y recursos en MCP es más explícita y auditable.
 
### 7.4 ReAct y Toolformer
 
Aunque no son protocolos como tal, estos métodos definen patrones para el uso de herramientas por parte de LLMs:
 
- **ReAct** (Reasoning and Acting) promueve el uso intercalado de razonamiento y acción.
- **Toolformer** introduce una estrategia de entrenamiento supervisado para que los modelos aprendan a usar herramientas en el momento adecuado.
 
**MCP puede considerarse una capa de infraestructura** que permite implementar estrategias ReAct o Toolformer de forma controlada y estructurada, dentro de un entorno de ejecución trazable.
 
### 7.5 LSP (Language Server Protocol)
 
Inspiración directa para MCP, el **Language Server Protocol** revolucionó la interacción entre editores de código y servidores de análisis estático, mediante un protocolo RPC universal.
 
La analogía:
 
- **LSP** conecta editores con herramientas de análisis semántico del código.
- **MCP** conecta modelos con herramientas y contextos externos.
 
Ambos separan cliente y servidor, definen métodos estándar y promueven un ecosistema modular y reusable.
 
## 8. Ecosistema actual y herramientas disponibles
 
Aunque el Model Context Protocol (MCP) es un estándar emergente, su adopción ha comenzado a materializarse a través de implementaciones de referencia, herramientas open-source y contribuciones de la comunidad. En esta sección se detallan los principales recursos técnicos disponibles, su nivel de madurez y cómo se pueden utilizar para comenzar a trabajar con MCP en aplicaciones reales.
 
### 8.1 Repositorios oficiales
 
La especificación de MCP y sus implementaciones iniciales están alojadas en GitHub bajo la organización [modelcontext](https://github.com/modelcontext).
 
Repositorios clave:
 
- [`modelcontext/spec`](https://github.com/modelcontext/spec): Define formalmente el protocolo MCP, incluyendo los mensajes JSON-RPC, los tipos de recursos y herramientas, y las convenciones de uso.
- [`modelcontext/server-py`](https://github.com/modelcontext/server-py): Implementación de referencia de un **servidor MCP en Python**, útil para exponer herramientas y recursos a modelos compatibles.
- [`modelcontext/client-js`](https://github.com/modelcontext/client-js): Cliente MCP en JavaScript para integraciones en aplicaciones web o entornos Node.js.
 
Estos repositorios proporcionan documentación, ejemplos y tests que permiten implementar y desplegar un entorno MCP funcional.
 
### 8.2 Clientes y servidores de terceros
 
Además de las implementaciones oficiales, algunos desarrolladores han comenzado a construir clientes, servidores y adaptadores MCP específicos para distintos entornos:
 
- **Bindings para otras plataformas**: se están explorando adaptaciones a Rust, Go y Java, aunque en etapas tempranas.
- **Integraciones experimentales**: algunos usuarios han integrado MCP con LangChain y Semantic Kernel como backend, usando MCP como capa de descubrimiento de herramientas.
 
### 8.3 Compatibilidad con modelos
 
A día de hoy (2025), el soporte de MCP por parte de proveedores de LLMs aún es limitado y experimental, aunque está creciendo:
 
- **Claude (Anthropic)**: Soporta MCP de forma nativa en algunos entornos empresariales. Es el modelo más alineado con el protocolo.
- **GPT-4**: No soporta MCP directamente, pero puede ser adaptado mediante tool use si el cliente implementa el protocolo.
- **Modelos open-source (Mistral, LLaMA, etc.)**: Pueden trabajar con MCP siempre que se les proporcione el contexto estructurado necesario vía `prompt engineering` o herramientas tipo ReAct.
 
### 8.4 Herramientas complementarias
 
MCP puede integrarse con otras piezas del ecosistema de IA para ofrecer soluciones más completas:
 
- **LLM orchestration frameworks**: como LangGraph, Flowise o CrewAI pueden incorporar MCP como backend de herramientas o nodos de recursos.
- **Observabilidad y auditoría**: se pueden conectar herramientas de logging y trazabilidad (OpenTelemetry, Prometheus, etc.) al servidor MCP.
- **Gestores de contexto**: se están desarrollando middlewares para enriquecer dinámicamente los recursos servidos a los modelos (ej. transformar documentos, aplicar filtros o validaciones).
 
### 8.5 Recursos comunitarios
 
El desarrollo de MCP está acompañado por recursos de la comunidad técnica:
 
- Documentación oficial: https://modelcontextprotocol.io
- Canal de Discord: [Model Context Protocol](https://discord.gg/xhquc5RSpT)
- Ejemplos prácticos y plantillas en notebooks
- Vídeos técnicos explicando el diseño del protocolo y casos de uso
 
### 8.6 Estado de madurez
 
| Aspecto              | Estado actual (2025)                   |
|----------------------|----------------------------------------|
| Especificación       | Estable, versión 1.0 publicada         |
| Cliente oficial      | JS disponible, otros en desarrollo     |
| Servidor oficial     | Python funcional y extensible          |
| Ecosistema de terceros | En crecimiento, aún incipiente       |
| Adopción por modelos | Limitada, liderada por Anthropic       |
 
## 9. Casos de uso y escenarios de aplicación
 
El Model Context Protocol (MCP) habilita un amplio espectro de aplicaciones prácticas al permitir que los modelos de lenguaje interactúen directamente con información estructurada, herramientas programáticas y entornos operativos reales. Esta sección analiza casos de uso concretos en los que MCP aporta un valor diferencial respecto a otras arquitecturas, organizados por dominio y tipo de tarea.
 
### 9.1 Asistentes empresariales con contexto operativo
 
#### 9.1.1 Análisis de sistemas de datos
 
Un asistente puede utilizar MCP para:
 
- Consultar directamente el estado de pipelines de datos expuestos como recursos JSON,
- Invocar herramientas para relanzar procesos o validar integridad,
- Obtener trazas y logs en tiempo real desde herramientas internas.
 
Esto evita que el modelo "imagine" la respuesta basándose únicamente en texto, y lo fuerza a trabajar sobre información viva y verificada.
 
#### 9.1.2 Informes de rendimiento y alertas
 
Un analista puede pedir: "¿Qué pasó con las métricas de conversión tras el último despliegue?". A través de recursos conectados al dashboard de analítica, el modelo puede:
 
- Leer las métricas antes y después del despliegue,
- Detectar anomalías usando una herramienta estadística,
- Generar un informe explicativo o sugerencias de mitigación.
 
### 9.2 Asistentes técnicos y de programación
 
#### 9.2.1 Linter, test y ejecución de código
 
Al conectarse con herramientas como linters, ejecutores de tests o compiladores vía MCP, un asistente puede:
 
- Leer el código desde un recurso,
- Invocar la herramienta `run_tests` con argumentos,
- Analizar resultados y sugerir correcciones directamente.
 
Ejemplo: ejecutar `pytest` sobre una carpeta específica y resumir los errores en lenguaje natural.
 
#### 9.2.2 Refactorización guiada por contexto
 
MCP permite al modelo consultar un recurso de dependencias o grafo de clases, identificar cuellos de botella y proponer un plan de refactorización basado en datos reales del sistema, no solo heurísticas aprendidas.
 
### 9.3 Aplicaciones legales y financieras
 
#### 9.3.1 Análisis de contratos y cláusulas
 
Mediante recursos estructurados que contienen versiones de contratos, cláusulas específicas o registros de negociación, un modelo puede:
 
- Comparar cláusulas entre documentos,
- Detectar inconsistencias o cláusulas de riesgo,
- Proponer modificaciones fundamentadas.
 
#### 9.3.2 Automatización de tareas regulatorias
 
Un asistente puede consultar normativas (como el GDPR o directivas financieras) expuestas como recursos y ejecutar herramientas que validen el cumplimiento sobre documentos o bases de datos reales.
 
### 9.4 Soporte técnico y troubleshooting
 
MCP permite que un modelo actúe como asistente de primer nivel técnico, con acceso a logs, métricas de sistema, herramientas de diagnóstico y bases de conocimiento.
 
Ejemplo de flujo:
 
1. Usuario reporta un error,
2. El modelo accede al log de la instancia afectada,
3. Invoca una herramienta de análisis automático,
4. Informa al usuario del fallo y lo corrige si es posible.
 
### 9.5 Composición de agentes multi-modelo
 
Al estandarizar la forma en que se describen herramientas y recursos, MCP permite coordinar múltiples agentes LLM, cada uno con distintos roles y capacidades, colaborando sobre un mismo entorno.
 
Ejemplo:
 
- Un agente extrae datos de una API (herramienta),
- Otro los analiza y detecta outliers (herramienta),
- Otro genera el reporte final (sin herramienta).
 
Todo esto puede orquestarse de forma estructurada, auditable y repetible mediante MCP.
 
### 9.6 Integración con procesos humanos
 
MCP no está limitado a tareas automáticas: puede coordinar flujos humano-en-bucle, permitiendo que el modelo:
 
- Solicite validaciones a humanos en ciertos pasos,
- Registre decisiones tomadas manualmente,
- Proponga planes de acción progresivos.
 
Esto lo hace útil en sectores como medicina, consultoría, investigación o derecho, donde el control humano es esencial.
 
## 10. Futuro del protocolo MCP
 
El Model Context Protocol (MCP) es una propuesta joven pero con una arquitectura sólida y fundamentos que lo posicionan como un posible estándar de facto para la integración de modelos de lenguaje con el mundo operativo. Este apartado examina las posibles direcciones de evolución del protocolo, así como los retos técnicos, comunitarios y éticos que condicionarán su adopción.
 
### 10.1 Estandarización y gobernanza
 
MCP se encuentra actualmente en fase abierta de desarrollo, impulsado por una comunidad técnica liderada por Anthropic y otros colaboradores independientes. En el futuro cercano se contemplan:
 
- **Propuestas de estandarización formal** a través de organismos como W3C, OpenAI Alliance o incluso consorcios tipo IEEE o IETF.
- **Versionado estable y contratos de compatibilidad** que garanticen interoperabilidad entre distintas versiones de clientes y servidores.
- **Modelos de gobernanza abiertos** que permitan propuestas de cambio, discusión comunitaria y adopción progresiva.
 
Un protocolo robusto y duradero requiere una comunidad activa y un proceso transparente de evolución técnica.
 
### 10.2 Extensiones y especializaciones del protocolo
 
La especificación actual cubre los fundamentos: descubrimiento de herramientas, ejecución, recursos de contexto y metadatos. Sin embargo, ya se identifican áreas para futuras extensiones:
 
#### 10.2.1 Tipado semántico avanzado
 
- Soporte para **esquemas de validación más ricos** (JSON Schema, Protobuf, Avro).
- Inclusión de **ontologías** o **tipos semánticos compartidos** que permitan interoperabilidad entre dominios (por ejemplo, tipos financieros, clínicos, legales).
 
#### 10.2.2 Acceso a datos en streaming
 
Actualmente MCP funciona en modo pull síncrono. Futuros cambios podrían permitir:
 
- Subscribirse a cambios en recursos,
- Recibir flujos continuos de datos (streaming) como logs, sensores o eventos.
 
#### 10.2.3 Composición dinámica de herramientas
 
Podría añadirse un mecanismo para que el propio modelo, o un sistema externo, **componga herramientas dinámicamente** a partir de primitivas básicas, generando funciones ad hoc según la tarea.
 
### 10.3 Integración nativa en modelos
 
Uno de los desafíos más importantes es lograr que los **propios modelos LLM interpreten y actúen de forma nativa sobre el protocolo MCP**, sin necesidad de ingeniería de prompts o capas de traducción.
 
- Anthropic está avanzando en este sentido con sus modelos Claude.
- Otras casas como OpenAI o Mistral podrían seguir este enfoque si la comunidad demuestra suficiente tracción.
- La inclusión del "protocolo de herramientas" como parte del preentrenamiento o fine-tuning se perfila como una estrategia viable.
 
### 10.4 Ecosistemas colaborativos
 
MCP puede convertirse en la columna vertebral para entornos de ejecución multi-agente, distribuidos y colaborativos:
 
- Plataformas como AutoGen, LangGraph o CrewAI podrían adoptar MCP como protocolo subyacente.
- Modelos especializados (ej. extracción de datos, planificación, razonamiento) pueden interoperar a través de MCP compartiendo herramientas.
- Entornos híbridos (humano + LLM + agente simbólico) pueden utilizar MCP como capa de orquestación.
 
### 10.5 Riesgos y retos
 
#### 10.5.1 Seguridad y control
 
Permitir que un modelo ejecute herramientas externas implica:
 
- Riesgo de ejecución de código malicioso,
- Pérdida de control si las herramientas no están bien auditadas,
- Exposición a datos sensibles.
 
Será necesario establecer **mecanismos de validación, sandboxing, autenticación y trazabilidad** estrictos.
 
#### 10.5.2 Sobrecarga cognitiva del modelo
 
No todos los modelos tienen la capacidad de gestionar múltiples herramientas con múltiples argumentos. Un protocolo demasiado rico puede volverse inoperable si el modelo no está preparado para navegarlo.
 
#### 10.5.3 Fragmentación
 
Si emergen múltiples dialectos, extensiones incompatibles o implementaciones cerradas del protocolo, se corre el riesgo de perder interoperabilidad. Esto debe mitigarse con test suites de conformidad y contratos formales.
 
### 10.6 Visión a largo plazo: infraestructura para LLMs autónomos
 
MCP podría desempeñar un papel clave como infraestructura base en:
 
- Sistemas autónomos dirigidos por lenguaje natural,
- Modelos AGI que interactúan con su entorno de forma robusta,
- Plataformas donde la IA es capaz de observar, actuar, planificar y ejecutar tareas complejas utilizando herramientas programáticas verificadas.
 
Tal como el **Language Server Protocol (LSP)** revolucionó los entornos de desarrollo, **MCP puede ser el equivalente para los entornos de inferencia interactiva**.
 
## 11. Conclusiones y recomendaciones finales
 
El Model Context Protocol (MCP) representa un avance significativo en la forma en que los modelos de lenguaje interactúan con su entorno. A lo largo de este artículo se ha mostrado cómo MCP introduce una abstracción clara, auditable y extensible para conectar modelos con herramientas, datos y procesos reales, resolviendo una de las limitaciones más importantes de los LLMs contemporáneos: su aislamiento contextual.
 
### 11.1 MCP como capa de infraestructura
 
MCP debe entenderse como una **pieza de infraestructura fundamental** para construir agentes inteligentes, interoperables y seguros. Su adopción promueve buenas prácticas como:
 
- Separación entre capacidades del modelo y lógica de negocio,
- Auditabilidad de entradas y salidas,
- Modularidad y reutilización de componentes,
- Estandarización de interfaces entre IA y sistemas.
 
Del mismo modo que el HTTP permitió el crecimiento de la web o el SQL permitió manipular datos de forma declarativa, MCP puede ser el catalizador que permita el crecimiento sostenible de sistemas basados en IA generativa.
 
### 11.2 MCP no es una solución mágica
 
A pesar de su potencial, MCP no resuelve todos los problemas relacionados con los LLMs:
 
- No garantiza razonamiento correcto: simplemente estructura el entorno.
- No asegura resultados precisos: depende de la calidad de los datos y herramientas conectadas.
- No elimina la necesidad de supervisión: en entornos sensibles, el control humano sigue siendo imprescindible.
 
Por tanto, su implementación debe acompañarse de **evaluaciones rigurosas**, **pruebas de seguridad** y **monitorización continua**.
 
### 11.3 Recomendaciones para adopción
 
#### 11.3.1 Para ingenieros y desarrolladores
 
- Explorar las especificaciones oficiales en [modelcontextprotocol.io](https://modelcontextprotocol.io),
- Empezar creando una implementación mínima de servidor MCP con recursos y herramientas controladas,
- Usar clientes tipo Claude o wrappers experimentales para validar flujos simples,
- Mantener los contratos JSON-RPC bien documentados y testeados.
 
#### 11.3.2 Para arquitectos y responsables técnicos
 
- Evaluar MCP como capa intermedia entre sistemas internos y modelos de lenguaje,
- Diseñar arquitecturas donde los recursos sean versionables, seguros y desacoplados del modelo,
- Estudiar cómo MCP puede integrarse con pipelines de datos existentes, APIs corporativas y sistemas legacy.
 
#### 11.3.3 Para comunidades open source
 
- Crear librerías cliente y servidor en múltiples lenguajes,
- Desarrollar conjuntos de herramientas MCP reutilizables en diferentes dominios,
- Publicar ejemplos, tutoriales y entornos de prueba para facilitar la adopción.
 
### 11.4 Futuro emergente
 
A medida que los modelos de lenguaje se convierten en **agentes autónomos capaces de ejecutar tareas complejas**, la necesidad de entornos estructurados, auditables y controlables se vuelve crítica. MCP proporciona un marco sólido para ello, y su adopción —aunque aún incipiente— podría ser determinante para la próxima generación de sistemas de inteligencia artificial interactiva, confiable y modular.
 
## 12. Webgrafía y fuentes
 
#### 12.1 Documentación oficial
- **Anthropic – Model Context Protocol (MCP) Documentation**  
  MCP es un protocolo abierto que unifica cómo las aplicaciones proporcionan contexto a LLMs: [docs.anthropic.com/en/docs/mcp](https://docs.anthropic.com/en/docs/mcp)
- **ModelContextProtocol.io – Introducción y guías**  
  Sitio oficial con recursos, SDKs y especificación detallada: [modelcontextprotocol.io](https://modelcontextprotocol.io/)
- **GitHub – Organización ModelContextProtocol**  
  Repositorio principal con SDKs (Python, TypeScript, Java, C#, Kotlin, Rust): [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)
 
#### 12.2 Artículos técnicos y papers
- **“Model Context Protocol (MCP) at First Glance” (Jun 2025)**  
  Estudio a gran escala de vulnerabilidades y métricas de salud en servidores MCP: [arXiv:2506.13538](https://arxiv.org/abs/2506.13538)
- **“Unveiling Attack Vectors in the Model Context Protocol Ecosystem” (Jun 2025)**  
  Identificación de vectores de ataque (tool poisoning, puppet, rug pull): [arXiv:2506.02040](https://arxiv.org/abs/2506.02040)
- **“Enterprise‑Grade Security for the Model Context Protocol” (Apr 2025)**  
  Amenazas avanzadas y estrategias de mitigación a nivel empresarial: [arXiv:2504.08623](https://arxiv.org/abs/2504.08623)
- **“ETDI: Mitigating Tool Squatting and Rug Pull Attacks in MCP” (Jun 2025)**  
  Propuesta de extensión de seguridad (OAuth, control de versiones, políticas): [arXiv:2506.01333](https://arxiv.org/abs/2506.01333)
- **“Model Context Protocol: Landscape, Security Threats, and Future Research Directions” (Mar 2025)**  
  Revisión exhaustiva de estado, amenazas y líneas futuras de investigación: [arXiv:2503.23278](https://arxiv.org/abs/2503.23278)
 
 
### 12.3 Wikipedia y otros recursos generales
- **Wikipedia – Protocolo de Contexto de Modelo (MCP)**  
  Resumen descriptivo, cronología y adopción de MCP: [es.wikipedia.org/wiki/Protocolo_de_Contexto_de_Modelo](https://es.wikipedia.org/wiki/Protocolo_de_Contexto_de_Modelo)
