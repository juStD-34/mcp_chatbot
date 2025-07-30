# Model Context Protocol (MCP) Architecture

This overview of the Model Context Protocol (MCP) discusses its scope and core concepts, and provides examples demonstrating each core concept.

## Scope

The Model Context Protocol includes the following projects:

* **MCP Specification**: A specification of MCP that outlines the implementation requirements for clients and servers.
* **MCP SDKs**: SDKs for different programming languages that implement MCP.
* **MCP Development Tools**: Tools for developing MCP servers and clients, including the MCP Inspector
* **MCP Reference Server Implementations**: Reference implementations of MCP servers.

## Concepts of MCP

### Participants

MCP follows a client-server architecture where an MCP host — an AI application like Claude Code or Claude Desktop — establishes connections to one or more MCP servers. The MCP host accomplishes this by creating one MCP client for each MCP server. Each MCP client maintains a dedicated one-to-one connection with its corresponding MCP server.

The key participants in the MCP architecture are:

* **MCP Host**: The AI application that coordinates and manages one or multiple MCP clients
* **MCP Client**: A component that maintains a connection to an MCP server and obtains context from an MCP server for the MCP host to use
* **MCP Server**: A program that provides context to MCP clients

**For example**: Visual Studio Code acts as an MCP host. When Visual Studio Code establishes a connection to an MCP server, such as the Sentry MCP server, the Visual Studio Code runtime instantiates an MCP client object that maintains the connection to the Sentry MCP server.

Note that **MCP server** refers to the program that serves context data, regardless of where it runs. MCP servers can execute locally or remotely.

### Layers

MCP consists of two layers:

* **Data layer**: Defines the JSON-RPC based protocol for client-server communication, including lifecycle management, core primitives, such as tools, resources, and prompts, and notifications.
* **Transport layer**: Defines the communication mechanisms and channels that enable data exchange between clients and servers, including transport-specific connection establishment, message framing, and authorization.

#### Data layer

The data layer implements a JSON-RPC 2.0 based exchange protocol that defines the message structure and semantics.
This layer includes:

* **Lifecycle management**: Handles connection initialization, capability negotiation, and connection termination between clients and servers
* **Server features**: Enables servers to provides core functionality including tools for AI actions, resources for context data, and prompts for interaction templates from to the client
* **Client features**: Enables servers to ask the client to sample from the host LLM, elicit input from the user, and log messages to the client
* **Utility features**: Supports additional capabilities like notifications for real-time updates and progress tracking for long-running operations

#### Transport layer

The transport layer manages communication channels and authentication between clients and servers. It handles connection establishment, message framing, and secure communication between MCP participants.
MCP supports two transport mechanisms:

* **Stdio transport**: Uses standard input/output streams for direct process communication between local processes on the same machine, providing optimal performance with no network overhead.
* **Streamable HTTP transport**: Uses HTTP POST for client-to-server messages with optional Server-Sent Events for streaming capabilities. This transport enables remote server communication and supports standard HTTP authentication methods.

### Data Layer Protocol

A core part of MCP is defining the schema and semantics between MCP clients and MCP servers. MCP uses JSON-RPC 2.0 as it's underlying RPC protocol.

#### Lifecycle management

MCP is a protocol that requires lifecycle management. The purpose of lifecycle management is to negotiate the capabilities that both client and server support.

#### Primitives

MCP primitives are the most important concept within MCP. They define what clients and servers can offer each other.

MCP defines three core primitives that *servers* can expose:

* **Tools**: Executable functions that AI applications can invoke to perform actions (e.g., file operations, API calls, database queries)
* **Resources**: Data sources that provide contextual information to AI applications (e.g., file contents, database records, API responses)
* **Prompts**: Reusable templates that help structure interactions with language models (e.g., system prompts, few-shot examples)

Each primitive type has associated methods for discovery (`*/list`), retrieval (`*/get`), and in some cases, execution (`tools/call`).

MCP also defines primitives that *clients* can expose:

* **Sampling**: Allows servers to request language model completions from the client's AI application
* **Elicitation**: Allows servers to request additional information from users
* **Logging**: Enables servers to send log messages to clients for debugging and monitoring purposes

#### Notifications

The protocol supports real-time notifications to enable dynamic updates between servers and clients. Notifications are sent as JSON-RPC 2.0 notification messages (without expecting a response) and enable MCP servers to provide real-time updates to connected clients.