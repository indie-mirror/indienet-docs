+++
title = "Design"
weight = "10"
+++

The Indienet Engine is a loosely-coupled ActivityPub server implementation for single-person web apps in JavaScript (Node.js) that handles the following features via REST and WebSocket APIs and [message persistence](/site/engine/technology-stack/database/) (see [technology stack](../technology-stack)):

  * [ ] Authentication
  * [ ] Server-to-server interactions
  * [ ] Client-to-server interactions

{{<mermaid align="left">}}
sequenceDiagram
  participant A as AP Server
  participant B as Engine
  participant C as Database
  participant D as AP Server
  participant E as Client

  A->>B: delivery
  B->>C: persistence
  B->>D: delivery
  D->>B: delivery
  E->>B: publickey authentication request
  B->>E: Authentication OK (JWT token)
  E->>B: new message
  B->>C: persistence
  C->>B: event
  B->>A: delivery
  B->>D: delivery
  B->>E: delivery
{{< /mermaid >}}

## Setup

An installation must be configurable to set:

  * The domain that it runs on (this is the domain that will act as the ActivityPub server)
  * Account details of the owner (name, description, photo)

This configuration will have to be carried out automatically for seamless deployments (to be designed).
