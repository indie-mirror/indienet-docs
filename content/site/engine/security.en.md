---
title: "Security"
weight: "30"
---

***
**Update:** See the [owner-keys configuration file details](/site/configuration) for discussion of the cryptographic properties of the Indie Site implementation.
***

An Indie Site is a web site (a Node JS web application) hosted on a server and accessed via clients. The default client is a web client that is, itself, served by the server.

Depending on how the app is installed and hosted, different security threats exist.

To absolutely guarantee the security of the end-to-end encryption of private messages, you would have to:

  1. Install the app from source yourself (to verify that it was not tampered with)
  2. Host the server on hardware that you have physical control over (to verify that it is not tempered with by the third-party host)

In such a scenario, you can be reasonably certain that the client that is being served is the one that you intend to be served.

This, however, is an edge case that some highly-skilled technical administrators might use. Indie Site  will mostly be installed, hosted, and updated by third-party hosts.

Just like any web app, this means that we must trust the host to:

  1. Not add a back door to the source and serve a malicious client (with which they could, for example, capture your password, etc.)
  2. Not update to a malicious version sometime in the future (a web server could serve you a different client on each connection)

While these issues are blatantly apparent for web clients, they are also faced by native apps today in a world of App Stores and automatic updates.

## Spikes

  * Documentation: [/spikes/security](/other/spikes/security)
  * Source: https://source.ind.ie/indienet/spikes/security
