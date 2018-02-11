+++
title = "Hallo.gent"
weight = "40"
+++

<style>h1 { text-indent: -9999px; white-space: nowrap; overflow: hidden; height: 0;}</style>

<a href='https://source.ind.ie/indienet/hallo-gent'><img src='/images/hallo.gent-colour.png' alt='Hallo.gent logo' style='width: 20vmax;'></a>

<h2 style='color: black; font-size: 3vmax;'>Say hello to your friends, the city, and to the world.</h2>

Hallo.gent empowers the citizens of the [City of Ghent](https://stad.gent) in Belgium with their own [federated personal web site](/site) at their own domain. It’s the home of Hallobot, who guides you seamlessly through the process of registering, setting up, and getting onto your own site.

Citizens use their personal sites to interact with each other, with the greater fediverse (e.g., Mastodon), and with the departments and services (like Gentinfo) provided by the city of Ghent.

This is the opposite of a Smart City platform. It’s a <strong>Smart Citizen</strong> platform.

## Wireframes

These are our initial thoughts on a friendly and seamless on-boarding process that takes people from the Hallo.gent site (which looks like an Indie Site) to your own [Indie Site](/site) by customising it.

### Hallobot onboarding: Step 1

Get authorisation for free domain. (This is a mock of Onboarding Scenario A)

![Hallobot onboarding: Step 1](/images/wireframes/hallo.gent/hallobot-onboarding.png)

### Hallotbot onboarding: Step 2

Pick a domain.

![Hallobot onboarding: Step 2](/images/wireframes/hallo.gent/hallobot-onboarding-2.png) 

## Sequence diagrams

### S1: Overview

{{<mermaid align="left">}}
sequenceDiagram
  participant A as .Gent TLD
  participant B as City
  participant C as Services
  participant C2 as Hallo.gent
  participant D as Host
  participant E as Citizen
  participant F as Personal Site
  participant G as Other citizens

  A->>B: Domain names
  B-->C: (Provides)
  B->>E: Letter with redemption code
  E->>C2: Redeem code
  E->>C2: Choose Domain
  C2->>D: Register domain
  D->>D: Register domain (<1 min)
  D->>D: Setup site (<30 seconds)
  D->>C2: Site ready (see S2)
  C2->>E: Site ready!
  E->>F: Use
  F->>C: Interact with
  F->>G: Interact with
{{< /mermaid >}}

### S2: Detail: Domain registry and site setup

![Domain registry and site setup sequence diagram](/images/sequence-diagrams/hallo.gent/site-registration-blackbox-communication.svg)

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

### C. Geofenced

Authorisation for a free Indie Site with a free .gent domain may also use geographical location as the criteria. This would also include people whom the city calls “users” (anyone who uses the City and its services). It could also provide a turism advantage (e.g., “take a little piece of the city with you…”, etc.)

## Welcome/setup experience on the site itself

1. On the welcome screen, you pick a password that is never communicated to the host.

2. You are shown some people, city services, etc., that you can follow and given brief initial instructions.

3. You’re up and running.
