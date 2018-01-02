+++
title = "iGent"
weight = "40"
+++

iGent is specific implementations of [Indienet Site](../site) and [Indienet Hub](../hub) for the [City of Ghent](https://stad.gent) in Belgium.

The goal is to empower the citizens of Ghent with their own place on the Internet, at their own domain which they own and control. This is a hosted, interoperable, federated, free and open, and easy to use system.

Citizens use their personal sites to interact with each other, with the greater fediverse (e.g., Mastodon), and with the departments and services (like Gentinfo) provided by the city of Ghent.

This is the opposite of a Smart City platform. It’s a <strong>Smart Citizen</strong> platform.

## Overview

{{<mermaid align="left">}}
sequenceDiagram
  participant A as .Gent
  participant B as City
  participant C as Services
  participant D as Host
  participant E as Citizen
  participant F as Personal Site
  participant G as Other citizens

  A->>B: Domain names
  B-->C: (Provides)
  B->>E: Letter with redemption code
  E->>D: Redeem code
  E->>D: Choose Domain
  D->>F: Setup site (<1 minute)
  E->>F: Use
  F->>C: Interact with
  F->>G: Interact with


{{< /mermaid >}}
