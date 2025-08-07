---
title: Model Context Protocol (MCP)
date: 2025-07-23 12:30:00 +0800
categories: [Programación, Desarrollo de Software]
tags: [OCR, Tesseract, Python, OpenCV, Python, DataScience]
image:
  path: /assets/img/post/mcp/mcp.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---
 
# =================== UC ===================
 
---
 
## 1. Introduction
 
In the past decade, the development of **AI models**, especially **large language models** (LLMs), has transformed the landscape of modern software. However, as these models are deployed in real-world applications, a critical need has emerged: **to connect them in a structured, secure, and reusable way to data sources, tools, and external systems**.
 
Until recently, this integration was done in an ad hoc manner, through proprietary *plugins*, specific *scripts*, or closed adapters, which limited interoperability, made maintenance difficult, and posed challenges in terms of security and scalability.

To address these limitations, **Anthropic proposed in November 2024** the **Model Context Protocol (MCP)**, an open standard that defines how AI models can interact with their contextual environment in a **modular, portable, and auditable** way. Simply put, MCP acts as a **universal protocol for describing and accessing the context surrounding a model during its execution**, including both passive resources (files, texts, databases) and active ones (tools, APIs, functional *prompts*).
 
### 1.1 A Necessary Analogy: The USB-C of AI

Just as the **USB-C** standard has unified the way electronic devices connect to chargers, displays, and peripherals, **MCP aims to become the standardized connector between language models and their execution ecosystem**. This analogy is not merely aesthetic: as in hardware, interoperability is crucial for efficiency, security, and open innovation.

### 1.2 Purpose of This Article

This article aims to provide a **technical and comprehensive** overview of the *Model Context Protocol*, covering not only its architecture and use cases but also its practical implications and emerging challenges. Throughout the content, we will analyze:

- What an MCP is from a technical standpoint,  
- How it is structured (components, messages, transport),  
- What problems it solves,  
- Real-world use cases and practical examples,  
- What security risks it introduces,  
- And why it represents a key advancement in the era of autonomous and modular AI agents.
 
## 2. Context and Motivation

One of the main challenges when working with language models in real-world applications is the **incorporation of external context**: relevant, up-to-date, or user-specific information that the model needs to generate useful responses. Although LLMs are trained on vast amounts of data, their knowledge is static and limited by their training cutoff date. Therefore, they require access to external resources—such as documents, tools, databases, or APIs—to operate effectively in real-world environments.

### 2.1 The Problem of Ad Hoc Integration

Before MCP, the connection between a model and its contextual environment was handled on a case-by-case basis:

- Closed plugins (such as ChatGPT plugins or Claude-specific extensions),  
- Manual pipelines using tools like LangChain or Semantic Kernel,  
- Custom-built solutions to integrate features with internal data.

This led to multiple issues:

- **Lack of interoperability**: each integration had its own interface and semantics.  
- **Low modularity**: the same connector couldn't be reused across different platforms.  
- **Difficult auditing**: it was hard to track what data was being used by the model and how.  
- **Security risks**: there was no standardized way to control access to external tools.

In environments where traceability, context isolation, and data governance are critical (such as corporate, medical, or financial sectors), these limitations posed a significant obstacle to the adoption of productive LLMs.
 
### 2.2 The Need for a Universal Standard

The rise of autonomous agents—systems capable of planning, reasoning, and executing actions—made the need for a protocol even more evident, one that:

1. Clearly describes the environment in which the model operates,  
2. Allows the model to discover, query, and use tools and data in a structured way,  
3. Is auditable, portable, and secure by design.

Inspired by standards such as the **Language Server Protocol (LSP)**—which transformed the integration of development tools through a common interface—**Anthropic designed MCP as a modular, extensible, and open solution** to connect models with their context. Just as LSP enabled editors like VSCode or Neovim to communicate with multiple programming languages uniformly, MCP aims to allow any model to interoperate with any source or tool that exposes a standard interface.

### 2.3 An Inevitable Evolution

MCP not only addresses a technical need, but also reflects a structural shift in how we conceive AI systems: no longer as monolithic black boxes, but as composable agents that interact with their environment, guided by explicit, transparent, and reusable protocols.

This transition toward open, interoperable, and controllable architectures is essential for the next generation of AI-based applications and positions the Model Context Protocol as one of the central components of the intelligent agent ecosystem in 2025 and beyond.
 
## 3. Technical Definition of the Model Context Protocol

The **Model Context Protocol (MCP)** is a structured communication protocol based on **JSON-RPC 2.0**, designed to enable language models to interact with their contextual environment in a standardized way. It defines both the **communication interface** and the **structure of the messages** exchanged between system components.

The protocol establishes a **client-server architecture**, in which one or more **MCP clients** (typically part of the host where the model is running) communicate with one or more **MCP servers** that expose resources and tools the model can use during inference.

### 3.1 General Architecture

The MCP architecture follows a modified client-server pattern tailored for use within language model environments. The key components are:

- **Host**: The main application that runs or interacts with a language model (e.g., an IDE, conversational assistant, or desktop environment such as Claude Desktop).  
- **MCP Client**: A module within the host responsible for managing connections to MCP servers.  
- **MCP Server**: A service that exposes resources, tools, and accessible *prompts* to the model.  
- **Resources/environment**: Local files, databases, APIs, or external functions.
 
```text
    Host
     └── MCP Client
           ├── MCP Server (local or remote)
           └── MCP Server (another external resource)
```
 
Each client-server connection is governed by a session of messages exchanged via JSON-RPC.

### 3.2 Messaging Protocol: JSON-RPC 2.0

MCP uses JSON-RPC 2.0 as its semantic transport layer. JSON-RPC is a lightweight protocol that enables remote method invocation using JSON objects. MCP leverages this capability to allow language models to query or invoke external tools.

The allowed message types include:
 
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
 
Each MCP server can implement different subsets of the protocol, depending on the primitives it supports (see Section 4).

### 3.3 Message Transport

MCP supports multiple physical transport mechanisms for transmitting JSON-RPC messages. The main ones currently defined are:

    stdio: Standard input/output of the operating system, useful for local processes.

    HTTP/SSE (Server-Sent Events): For cases where network communication or continuous data streaming is required.

The specification allows the transport channel to be negotiated between client and server based on the execution environment.

### 3.4 Extensibility and Modularity

One of MCP’s defining features is its ability to describe itself. Through methods like `introspect`, MCP servers can report which tools and resources are available, under what schemas, and how they are expected to be used.

This self-describing capability enables the construction of dynamic and adaptive AI systems, where a model can discover its environment and modify its behavior according to accessible resources, without requiring changes to the source code.
 
### 3.5 Formalization of Context

In MCP, “context” is not limited to the textual prompt. It is defined as a structure composed of:

- Data resources (documents, databases, streams),  
- Invokable functions or tools,  
- Preconfigured templates or prompts,  
- Persistent state or metadata.

This design provides a solid foundation for models to operate as cognitive agents, capable of reasoning, acting, and adapting in real time according to their operational context.

## 4. Key Components of the Protocol

The Model Context Protocol (MCP) is designed to be **modular, extensible, and bidirectional**. Its architecture not only specifies how messages are transported, but also what functional entities must exist for coherent and secure interaction between language models and their environment.

The components are divided into two main categories: **structural components** (host, client, server) and **functional components** (resources, tools, prompts). This section describes both.

### 4.1 Host

The **host** is the main application that contains or runs the language model. It can be a desktop assistant, an IDE, an embedded conversational system, or any other environment where the model functions as a central component.

The host:

- Manages the user session or interaction,  
- Contains or launches the model (locally or via API),  
- Incorporates one or more **MCP clients**,  
- Orchestrates communication between the model and context.

Common examples of hosts: Claude Desktop, ChatGPT with tools, VSCode with AI assistance, conversational assistants integrated in browsers.
 
### 4.2 MCP Client

The **MCP client** is an entity within the host that maintains an active 1:1 connection with an **MCP server**. Its role is to act as a low-level intermediary:

- Establishes the transport channel (stdio, HTTP/SSE),  
- Sends and receives JSON-RPC messages,  
- Interprets and adapts the semantics of resources according to the model consuming them.

A host can maintain multiple MCP clients simultaneously, connected to different servers for different purposes (for example, one client for mathematical tools and another for file access).

### 4.3 MCP Server

An **MCP server** is any service that exposes **structured resources, prompts, or tools** through the MCP protocol. These servers can run locally or over a network and must comply with the protocol specification.

Main functions of the server:

- Implement the methods defined by the protocol (`tools/list`, `resources/get`, etc.),  
- Declaratively expose which resources and functionalities it offers,  
- Manage authorizations, versions, descriptions, and metadata.

Examples of existing MCP servers:

- Connectors to Google Drive, GitHub, or Slack,  
- Local servers exposing folders and files,  
- Interfaces to REST APIs or databases.

### 4.4 Resources

A **resource** is any static or dynamic data input that the model can consume as part of its context. Resources are represented as serialized objects with metadata and are identified by unique paths within the MCP server.

Examples of resources:

- Documents (`docx`, `pdf`, `md`, etc.),  
- Source code files,  
- Database tables,  
- Web browsing summaries or search results.

Resources may also include embedded content, cross-references, and semantic annotations, enabling the model to understand their structure and contextual relevance.
 
### 4.5 Tools

**Tools** are functions invokable by the language model during its execution. Unlike resources, which are passive, tools are active and can produce side effects or dynamic results.

A tool exposed by an MCP server must declare:

- Its identifier and purpose (`"tool_id": "calculate_vat"`),  
- Its input and output types (`"input_schema"`, `"output_schema"`),  
- Its constraints and parameters (`"timeout"`, `"rate_limit"`).

Examples of tools:

- SQL queries on databases,  
- Python code executors,  
- Calls to external APIs (e.g., `GET /weather?city=Barcelona`),  
- Calculation or transformation functions.

The model may decide to invoke a tool if the MCP client provides adequate information about its availability and contextual usefulness.

### 4.6 Prompts

**Prompts** in MCP are reusable textual interaction templates that a model can use as a starting point for a task. They should not be confused with free-form user prompts: these are predefined, specialized prompts designed for recurring tasks.

Each prompt can include:

- Base text with parameterizable variables,  
- Usage examples (*few-shot learning*),  
- Formatting or response style instructions.

An example of a prompt could be:
 
```json
{
  "id": "summarize_customer_feedback",
  "template": "Summarize in 5 points the client feedback: {{feedback_text}}",
  "defaults": {
    "tone": "neutral",
    "max_points": 5
  }
}
```

The structured use of prompts within the protocol allows the model to adapt to common tasks without requiring the user to provide a detailed instruction each time.

## 5. Practical Use Cases

The true value of the Model Context Protocol (MCP) manifests when applied in real-world scenarios, where language models need to access external data, execute specialized functions, or integrate with complex workflows. This section presents several **representative use cases**, illustrating the protocol’s versatility and applicability in modern AI contexts.

### 5.1 Structured Access to Documents and Files

An MCP environment can expose a hierarchical collection of files and documents as resources, enabling the model to query, summarize, compare, or edit them.

**Example**: a local MCP server exposes a working directory:
 
```json
{
  "resource_id": "docs/contrato2025.pdf",
  "type": "document/pdf",
  "content": "...",
  "metadata": {
    "title": "Marco Contract 2025",
    "modified": "2025-07-14T12:01:00Z"
  }
}
```
 
The model can access this resource, extract relevant clauses, and generate a legal summary without requiring manual upload by the user.

### 5.2 Assistance in Development Environments (IDE)

Integrated into an IDE like VSCode or Zed, an MCP client can expose source code files, change history, and tools such as linters or test runners, enabling the model to perform:

- Error diagnosis  
- Guided refactoring  
- Unit test generation  
- Context-aware code review  

Key advantage: the model does not operate on isolated plain text, but on a structured and active context synchronized with the project.

### 5.3 Workflow Automation

MCP allows the model to invoke external tools to execute automated tasks within a controlled session.

Example: the model acts as an assistant to draft and send follow-up emails through an MCP tool connected to an email API.
 
```json
  {
    "tool_id": "send_email",
    "params": {
      "to": "client@enterprise.com",
      "subject": "Summary of the meeting",
      "body": "Dear client, please find attached the summary of our meeting..."
    }
  }
```
 
The interaction can include validation, confirmation, and follow-up, all orchestrated through the MCP client.

### 5.4 Models as Cognitive Agents

An advanced application consists of allowing the model to dynamically discover and select tools according to its MCP environment. This lays the foundation for an autonomous agents approach.

Typical flow:

1. The model queries `tools/list` to see what it can do.
2. It analyzes the current task and decides which tool to use.
3. It generates the appropriate invocation.
4. It processes the result and integrates it into its reasoning flow.

This pattern approaches the paradigm of toolformer or function-calling agents, but using a standardized abstraction layer.

### 5.5 Integration with Enterprise Information Systems

MCP can act as an intermediary between a language model and an organization's internal systems: CRM, ERP, support systems, or business intelligence.

Example: an analyst interacts with the model to generate weekly reports:

- The MCP server exposes preconfigured queries to the sales database.
- The model selects one, executes it, and summarizes the results in natural language.
- The user reviews and exports the report in PDF.

This use case reduces friction between the technical knowledge required to access data and the ability to analyze and communicate it effectively.

### 5.6 Multimodal Interaction (Textual + Visual + Code)

In environments that support multimodal resources, MCP allows the model to access images, charts, numerical results, or structured code snippets as resources.

Example:

- A visual resource: medical image, statistical chart.
- A structured resource: CSV table.
- A code resource: Jupyter notebook with partial outputs.

The model can consult, reason about, compare, or transform these resources more naturally and precisely, instead of operating only on plain text.

## 6. Benefits and Limitations

The Model Context Protocol (MCP) introduces a fundamental abstraction in the interaction between language models and external environments. While its design represents a significant improvement in flexibility and standardization, it also poses certain technical, operational, and adoption challenges that need to be considered. This section evaluates both sides of the protocol: its key benefits and current limitations.

### 6.1 Benefits

#### 6.1.1 Standardization of Contextual Interfaces

One of MCP’s main achievements is providing a **common reference framework** for different systems to expose resources, tools, and prompts coherently to models. This eliminates the need to integrate each application in an ad hoc manner, enabling interoperability among providers and platforms.
 
#### 6.1.2 Improved Model Contextual Capability

Thanks to the protocol, the model can operate on **structured and up-to-date data**, not just free text. This enables richer interactions, more complex tasks, and a more precise understanding of the environment.

Example: comparing two legal contracts, running unit tests on code, or reviewing metrics of a data pipeline.

#### 6.1.3 Extensibility and Modularity

MCP is designed to be **modular**, allowing developers to create their own custom servers, tools, or resources without needing to modify the core of the host or model. This accelerates innovation and enables domain-specific adaptations.

#### 6.1.4 Alignment with Emerging Paradigms

The protocol is compatible with current trends such as:

- **Autonomous agents** based on LLM models,
- **LLMs with external tools and functions** (*tool use*),
- **Context-assisted cognitive systems in real time**.

Its architecture positions it as a key infrastructure for the future of applied AI.

#### 6.1.5 Separation of Responsibilities and Security

By delegating access to sensitive data or critical functions to the MCP server, a clear **separation of responsibilities** is promoted. This facilitates permission control, usage auditing, and regulatory compliance.

### 6.2 Limitations

#### 6.2.1 Initial Adoption Curve

Implementing an MCP client and server from scratch requires understanding the protocol specifications, managing transport, authentication, and resource and tool structures. This can represent a **technical barrier** for small teams or those with little experience in protocols.

#### 6.2.2 Dependency on Model Compatibility

The MCP protocol is only a transport layer. For it to work, the LLM must be capable of:

- Correctly interpreting structured resources,
- Deciding when and how to invoke tools,
- Maintaining coherence between external context and generation.

This limits its application to sufficiently advanced models configured for *tool use* or agents.

#### 6.2.3 Latency and Performance

Each interaction with an MCP server (e.g., resource query or tool invocation) adds latency to the model session. In applications where immediacy is key, this can affect the user experience if not properly managed through caching, contextual anticipation, or execution queues.

#### 6.2.4 Security and Access Control

Although the protocol encourages clear separation between model and data, the exposure of resources or tools must be carefully controlled:

- What tools can the model invoke without supervision?
- What sensitive data is available as resources?
- What validation or traceability mechanisms exist?

Designing secure and auditable MCP servers is essential for use in corporate or regulated environments.

#### 6.2.5 Still Emerging Ecosystem

As a recent protocol, the ecosystem of **clients, servers, libraries, and reusable templates** is still under development. This limits its immediate adoption to pioneering or experimental projects, although sustained growth is expected in the coming years.
 
## 7. Alternatives and Related Protocols

Although the Model Context Protocol (MCP) represents a novel and structured approach to connecting language models with their environment, it is not the only solution addressing this goal. There are various technologies, frameworks, and architectural patterns that seek to tackle the problem of contextualization, external tool execution, and functional integration of LLMs. This section presents a comparative analysis between MCP and the main alternatives currently available.

### 7.1 LangChain

LangChain is one of the best-known libraries for building applications that integrate LLMs with tools, databases, documents, and APIs. Unlike MCP, **LangChain is a library oriented towards execution flows within Python environments**, and not an interoperable protocol.

**Comparison:**
 
| Feature                | MCP                               | LangChain                         |
|------------------------|----------------------------------|----------------------------------|
| Type                   | RPC Protocol                     | Python Library                   |
| Interoperability       | High (model-agnostic)            | Low (tied to execution environment) |
| Modularity             | High (separate client, server, tools) | Moderate (all in the same process) |
| Observability          | Standardized by design           | Depends on implementation        |
| Agents and reasoning   | Supported if the model allows it | Highly integrated                |
 
LangChain provides useful tools for quickly building agents, but it does not define a standard interface to expose context to third-party models.

### 7.2 Semantic Kernel

Semantic Kernel, powered by Microsoft, allows building agents that combine prompts, functions, and contextual memory. It is designed to integrate with enterprise services (Microsoft Graph, Azure, etc.).

**Key comparison:**

- **Semantic Kernel** promotes composing LLM + function execution flows but does not define a standardized protocol to interoperate with external tools.
- MCP can integrate with Semantic Kernel as a transport layer or function discovery mechanism.

### 7.3 OpenAI Function Calling / Tool Use API

OpenAI introduced `function_call` as a mechanism for GPT models to invoke user-declared functions. More recently, its **Tool Use API** extends this model to more complex environments with multiple tools.

**Comparison with MCP:**

- Both allow the model to invoke external functions with structured arguments.
- However, **OpenAI defines a closed protocol, specific to its models**, whereas MCP proposes an open standard, vendor-independent.
- The semantics of tools and resources in MCP are more explicit and auditable.

### 7.4 ReAct and Toolformer

Although not protocols per se, these methods define patterns for tool use by LLMs:

- **ReAct** (Reasoning and Acting) promotes interleaved reasoning and acting.
- **Toolformer** introduces a supervised training strategy so models learn to use tools at the right moment.

**MCP can be considered an infrastructure layer** that enables implementing ReAct or Toolformer strategies in a controlled and structured way, within a traceable execution environment.

### 7.5 LSP (Language Server Protocol)

A direct inspiration for MCP, the **Language Server Protocol** revolutionized the interaction between code editors and static analysis servers through a universal RPC protocol.

The analogy:

- **LSP** connects editors with code semantic analysis tools.
- **MCP** connects models with external tools and contexts.

Both separate client and server, define standard methods, and promote a modular and reusable ecosystem.

## 8. Current Ecosystem and Available Tools

Although the Model Context Protocol (MCP) is an emerging standard, its adoption has begun to take shape through reference implementations, open-source tools, and community contributions. This section details the main technical resources available, their maturity level, and how they can be used to start working with MCP in real applications.
 
### 8.1 Official Repositories

The MCP specification and its initial implementations are hosted on GitHub under the [modelcontext](https://github.com/modelcontext) organization.

Key repositories:

- [`modelcontext/spec`](https://github.com/modelcontext/spec): Formally defines the MCP protocol, including JSON-RPC messages, resource and tool types, and usage conventions.
- [`modelcontext/server-py`](https://github.com/modelcontext/server-py): Reference implementation of an **MCP server in Python**, useful for exposing tools and resources to compatible models.
- [`modelcontext/client-js`](https://github.com/modelcontext/client-js): MCP client in JavaScript for integration in web applications or Node.js environments.

These repositories provide documentation, examples, and tests to enable implementing and deploying a functional MCP environment.

### 8.2 Third-Party Clients and Servers

Beyond official implementations, some developers have started building MCP clients, servers, and adapters tailored to various environments:

- **Bindings for other platforms**: adaptations to Rust, Go, and Java are being explored, though still in early stages.
- **Experimental integrations**: some users have integrated MCP with LangChain and Semantic Kernel as backends, using MCP as a tool discovery layer.

### 8.3 Model Compatibility

As of 2025, MCP support by LLM providers remains limited and experimental, but growing:

- **Claude (Anthropic)**: Natively supports MCP in some enterprise environments. It is the model most aligned with the protocol.
- **GPT-4**: Does not directly support MCP, but can be adapted via tool use if the client implements the protocol.
- **Open-source models (Mistral, LLaMA, etc.)**: Can work with MCP as long as they receive the necessary structured context through prompt engineering or tool-use methods like ReAct.

### 8.4 Complementary Tools

MCP can integrate with other parts of the AI ecosystem to offer more complete solutions:

- **LLM orchestration frameworks**: such as LangGraph, Flowise, or CrewAI can incorporate MCP as a backend for tools or resource nodes.
- **Observability and auditing**: logging and tracing tools (OpenTelemetry, Prometheus, etc.) can connect to the MCP server.
- **Context managers**: middlewares are being developed to dynamically enrich resources served to models (e.g., transforming documents, applying filters or validations).

### 8.5 Community Resources

The development of MCP is supported by technical community resources:

- Official documentation: https://modelcontextprotocol.io
- Discord channel: [Model Context Protocol](https://discord.gg/xhquc5RSpT)
- Practical examples and notebook templates
- Technical videos explaining protocol design and use cases
 
### 8.6 Maturity Status

| Aspect                 | Current Status (2025)                  |
|------------------------|--------------------------------------|
| Specification          | Stable, version 1.0 published         |
| Official client        | JS available, others in development   |
| Official server        | Python functional and extensible      |
| Third-party ecosystem  | Growing, still in early stages        |
| Model adoption         | Limited, led by Anthropic              |

## 9. Use Cases and Application Scenarios

The Model Context Protocol (MCP) enables a broad range of practical applications by allowing language models to directly interact with structured information, programmatic tools, and real operational environments. This section analyzes concrete use cases where MCP provides differential value compared to other architectures, organized by domain and task type.

### 9.1 Enterprise Assistants with Operational Context

#### 9.1.1 Data System Analysis

An assistant can use MCP to:

- Query the status of data pipelines exposed as JSON resources,
- Invoke tools to restart processes or validate integrity,
- Obtain real-time traces and logs from internal tools.

This prevents the model from "imagining" answers based only on text and forces it to work with live, verified information.

#### 9.1.2 Performance Reports and Alerts

An analyst might ask: "What happened with the conversion metrics after the last deployment?" Through resources connected to analytics dashboards, the model can:

- Read metrics before and after deployment,
- Detect anomalies using a statistical tool,
- Generate an explanatory report or mitigation suggestions.

### 9.2 Technical and Programming Assistants

#### 9.2.1 Linters, Tests, and Code Execution

By connecting to tools like linters, test runners, or compilers via MCP, an assistant can:

- Read code from a resource,
- Invoke the `run_tests` tool with arguments,
- Analyze results and suggest corrections directly.

Example: running `pytest` on a specific folder and summarizing errors in natural language.

#### 9.2.2 Context-Guided Refactoring

MCP allows the model to query a dependency resource or class graph, identify bottlenecks, and propose a refactoring plan based on real system data, not just learned heuristics.

### 9.3 Legal and Financial Applications

#### 9.3.1 Contract and Clause Analysis

Through structured resources containing contract versions, specific clauses, or negotiation logs, a model can:

- Compare clauses across documents,
- Detect inconsistencies or risky clauses,
- Propose well-founded modifications.

#### 9.3.2 Regulatory Task Automation

An assistant can query regulations (e.g., GDPR or financial directives) exposed as resources and execute tools that validate compliance over real documents or databases.

### 9.4 Technical Support and Troubleshooting

MCP enables a model to act as a first-level technical assistant, with access to logs, system metrics, diagnostic tools, and knowledge bases.

Example flow:

1. User reports an error,  
2. Model accesses logs of the affected instance,  
3. Invokes an automated analysis tool,  
4. Informs the user of the fault and corrects it if possible.

### 9.5 Multi-Model Agent Composition

By standardizing how tools and resources are described, MCP allows coordination of multiple LLM agents, each with distinct roles and capabilities, collaborating on the same environment.

Example:

- One agent extracts data from an API (tool),  
- Another analyzes it and detects outliers (tool),  
- Another generates the final report (no tool).

All of this can be orchestrated in a structured, auditable, and repeatable manner via MCP.

### 9.6 Integration with Human-in-the-Loop Processes

MCP is not limited to automatic tasks: it can coordinate human-in-the-loop workflows, allowing the model to:

- Request human validations at certain steps,  
- Record manually made decisions,  
- Propose progressive action plans.

This makes it useful in fields like medicine, consulting, research, or law, where human control is essential.

## 10. Future of the MCP Protocol

The Model Context Protocol (MCP) is a young proposal with a solid architecture and foundations that position it as a potential de facto standard for integrating language models with operational environments. This section examines possible future directions for the protocol as well as technical, community, and ethical challenges that will influence its adoption.

### 10.1 Standardization and Governance

MCP is currently in open development, driven by a technical community led by Anthropic and other independent contributors. Near-future plans include:

- **Formal standardization proposals** through bodies like W3C, OpenAI Alliance, or even consortia such as IEEE or IETF.  
- **Stable versioning and compatibility contracts** that guarantee interoperability across different client and server versions.  
- **Open governance models** that allow change proposals, community discussion, and progressive adoption.

A robust and long-lasting protocol requires an active community and a transparent technical evolution process.

### 10.2 Protocol Extensions and Specializations

The current specification covers the fundamentals: tool discovery, execution, context resources, and metadata. However, areas for future extensions are already identified:

#### 10.2.1 Advanced Semantic Typing

- Support for **richer validation schemas** (JSON Schema, Protobuf, Avro).  
- Inclusion of **ontologies** or **shared semantic types** enabling cross-domain interoperability (e.g., financial, clinical, legal types).

#### 10.2.2 Streaming Data Access

Currently, MCP works in synchronous pull mode. Future changes might enable:

- Subscribing to changes in resources,  
- Receiving continuous data streams (streaming) such as logs, sensors, or events.

#### 10.2.3 Dynamic Tool Composition

A mechanism could be added for the model itself, or an external system, to **dynamically compose tools** from basic primitives, generating ad hoc functions depending on the task.
 
### 10.3 Native Integration in Models

One of the most important challenges is to enable **LLM models themselves to natively interpret and act on the MCP protocol**, without the need for prompt engineering or translation layers.

- Anthropic is advancing in this direction with their Claude models.
- Other companies like OpenAI or Mistral could follow this approach if the community shows enough traction.
- Including the "tool protocol" as part of pretraining or fine-tuning is seen as a viable strategy.

### 10.4 Collaborative Ecosystems

MCP could become the backbone for multi-agent, distributed, and collaborative execution environments:

- Platforms like AutoGen, LangGraph, or CrewAI could adopt MCP as the underlying protocol.
- Specialized models (e.g., data extraction, planning, reasoning) can interoperate through MCP by sharing tools.
- Hybrid environments (human + LLM + symbolic agent) can use MCP as an orchestration layer.

### 10.5 Risks and Challenges

#### 10.5.1 Security and Control

Allowing a model to execute external tools involves:

- Risk of executing malicious code,
- Loss of control if tools are not properly audited,
- Exposure to sensitive data.

Strict **validation, sandboxing, authentication, and traceability** mechanisms will be necessary.

#### 10.5.2 Model Cognitive Overload

Not all models can manage multiple tools with multiple arguments. An overly rich protocol could become unusable if the model is not prepared to navigate it.

#### 10.5.3 Fragmentation

If multiple dialects, incompatible extensions, or closed implementations of the protocol emerge, there is a risk of losing interoperability. This must be mitigated with compliance test suites and formal contracts.

### 10.6 Long-term Vision: Infrastructure for Autonomous LLMs

MCP could play a key role as a foundational infrastructure in:

- Autonomous systems driven by natural language,
- AGI models that interact robustly with their environment,
- Platforms where AI can observe, act, plan, and execute complex tasks using verified programmatic tools.

Just as the **Language Server Protocol (LSP)** revolutionized development environments, **MCP could be the equivalent for interactive inference environments**.

## 11. Conclusions and Final Recommendations

The Model Context Protocol (MCP) represents a significant advance in how language models interact with their environment. Throughout this article, it has been shown how MCP introduces a clear, auditable, and extensible abstraction to connect models with tools, data, and real processes, solving one of the most important limitations of contemporary LLMs: their contextual isolation.

### 11.1 MCP as an Infrastructure Layer

MCP should be understood as a **fundamental piece of infrastructure** for building intelligent, interoperable, and secure agents. Its adoption promotes good practices such as:

- Separation between model capabilities and business logic,
- Auditability of inputs and outputs,
- Modularity and component reuse,
- Standardization of interfaces between AI and systems.

Just as HTTP enabled the growth of the web or SQL allowed declarative data manipulation, MCP can be the catalyst for sustainable growth of generative AI-based systems.

### 11.2 MCP Is Not a Magic Solution

Despite its potential, MCP does not solve all LLM-related problems:

- It does not guarantee correct reasoning: it only structures the environment.
- It does not ensure precise results: it depends on the quality of connected data and tools.
- It does not eliminate the need for supervision: human control remains essential in sensitive environments.

Therefore, its implementation must be accompanied by **rigorous evaluations**, **security testing**, and **continuous monitoring**.
 
### 11.3 Recommendations for Adoption

#### 11.3.1 For Engineers and Developers

- Explore the official specifications at [modelcontextprotocol.io](https://modelcontextprotocol.io),
- Start by creating a minimal MCP server implementation with controlled resources and tools,
- Use clients like Claude or experimental wrappers to validate simple workflows,
- Keep JSON-RPC contracts well documented and tested.

#### 11.3.2 For Architects and Technical Leads

- Evaluate MCP as a middleware layer between internal systems and language models,
- Design architectures where resources are versionable, secure, and decoupled from the model,
- Study how MCP can integrate with existing data pipelines, corporate APIs, and legacy systems.

#### 11.3.3 For Open Source Communities

- Create client and server libraries in multiple languages,
- Develop reusable MCP toolkits across different domains,
- Publish examples, tutorials, and test environments to ease adoption.

### 11.4 Emerging Future

As language models become **autonomous agents capable of executing complex tasks**, the need for structured, auditable, and controllable environments becomes critical. MCP provides a solid framework for this, and its adoption — though still early — could be decisive for the next generation of interactive, reliable, and modular AI systems.

## 12. Web Resources and References

#### 12.1 Official Documentation

- **Anthropic – Model Context Protocol (MCP) Documentation**  
  MCP is an open protocol unifying how applications provide context to LLMs: [docs.anthropic.com/en/docs/mcp](https://docs.anthropic.com/en/docs/mcp)
- **ModelContextProtocol.io – Introduction and Guides**  
  Official site with resources, SDKs, and detailed specification: [modelcontextprotocol.io](https://modelcontextprotocol.io/)
- **GitHub – ModelContextProtocol Organization**  
  Main repository with SDKs (Python, TypeScript, Java, C#, Kotlin, Rust): [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)

#### 12.2 Technical Articles and Papers

- **“Model Context Protocol (MCP) at First Glance” (Jun 2025)**  
  Large-scale study of vulnerabilities and health metrics in MCP servers: [arXiv:2506.13538](https://arxiv.org/abs/2506.13538)
- **“Unveiling Attack Vectors in the Model Context Protocol Ecosystem” (Jun 2025)**  
  Identification of attack vectors (tool poisoning, puppet, rug pull): [arXiv:2506.02040](https://arxiv.org/abs/2506.02040)
- **“Enterprise‑Grade Security for the Model Context Protocol” (Apr 2025)**  
  Advanced threats and mitigation strategies at the enterprise level: [arXiv:2504.08623](https://arxiv.org/abs/2504.08623)
- **“ETDI: Mitigating Tool Squatting and Rug Pull Attacks in MCP” (Jun 2025)**  
  Security extension proposal (OAuth, version control, policies): [arXiv:2506.01333](https://arxiv.org/abs/2506.01333)
- **“Model Context Protocol: Landscape, Security Threats, and Future Research Directions” (Mar 2025)**  
  Comprehensive review of status, threats, and future research lines: [arXiv:2503.23278](https://arxiv.org/abs/2503.23278)

### 12.3 Wikipedia and Other General Resources

- **Wikipedia – Model Context Protocol (MCP)**  
  Descriptive summary, timeline, and adoption of MCP: [es.wikipedia.org/wiki/Protocolo_de_Contexto_de_Modelo](https://es.wikipedia.org/wiki/Protocolo_de_Contexto_de_Modelo)
