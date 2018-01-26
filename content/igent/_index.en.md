+++
title = "iGent"
weight = "40"
+++

<img src='/images/iGent@3x.png' alt='iGent logo' style='width: 10vmax;'>

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

## Onboarding scenarios

### A. Synchronous

1. You receive a message from the city via an out-of-band channel (snail mail, email, text message, etc.) with a unique, easier to type URL, valid for one domain and a web site on which to redeem it. (We could also include a QR code option so, e.g., people with iPhones could just point their camera at it to visit the URL.)

2. You visit the URL and choose a .Gent domain name

3. Within 30 seconds you are up and running at your own domain with your own federated personal web site.

#### Design requirements

1. Domain registration TTL close to zero (requires coordination with .Gent registrar – we are currently in talks)

2. Server instance TTL within above-stated 30 second period (this is not an unsolved problem – we could have a buffer of N instances ready to go at all times).

3. City cooperation for out-of-band communication. Not a problem; already have buy-in.

### B. Asynchronous

First two steps are the same as A.

3. You also enter your email address (or phone number)

4. The web site informs you that you will receive an email (or a text message) when the site is ready.

5. You get an email (or text message) when the web site is ready with a link to your own domain.

## Welcome/setup experience on the site itself

1. On the welcome screen, you pick a password that is never communicated to the host.

2. You are shown some people, city services, etc., that you can follow and given brief initial instructions.

3. You’re up and running.
