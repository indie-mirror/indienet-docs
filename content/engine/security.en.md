+++
title = "Security"
weight = "20"
+++

Indienet apps based on Indienet Engine are hosted on a server and accessed through clients. The default client is a web client that is, itself, served by the server.

Depending on how the app is installed and hosted, different security threats will exist.

In order to be entirely sure of the security of the end-to-end encryption of private messages, you would have to:

  1. Install the app from source yourself (to verify that it was not tampered with)
  2. Host the server on hardware that you have physical control over (to verify that it is not tempered with by the third-party host)

In such a scenario, you can be reasonably certain that the client that is being served is the one that you intend to be served.

This, however, is a negligible use case for Indienet apps, which will mostly be installed, hosted, and updated by third-party hosts.

Just like any web app, this means that we must trust the host not to:

  1. Add a back door to the source and serve a malicious client (with which they could, for example, capture your password, etc.)
  2. Not to update to a malicious version sometime in the future (a web server could serve you a different client on each connection)

While these issues are blatantly apparent for web clients, they are also faced by native apps today in a world of App Stores and automatic updates. 

## Resources

  * [End-To-End Web Crypto: A Broken Security Model](https://www.indolering.com/e2e-web-crypto/)
  * [What’s wrong with in-browser cryptography?](https://tonyarcieri.com/whats-wrong-with-webcrypto)
  * [A Few Thoughts on Cryptographic Engineering](https://blog.cryptographyengineering.com/2013/06/17/how-to-backdoor-encryption-app/)
