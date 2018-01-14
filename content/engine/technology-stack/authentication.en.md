+++
title = "Authentication"
weight = "30"
+++

The requirements for authentication are affected by the following requirements and aspects of the Indienet:

  * Indienet sites/apps are personal sites, there is only a single owner that uses each site/app. (We do not have the concept of users and we do not need usernames.)

  * Private messages must be end-to-end encrypted (see [security](/engine/security))

  * Private messages must be accessible at any time in the future from the server from any authenticated client.

As such, the current plans for authentication are:

## Key Generation And Storage

  1. On first use, a private and public key keypair is generated on the client.

  2. The private key is symmetrically encrypted (e.g., via [Triplesec]( https://keybase.io/triplesec/)) using a strong password (we will recommend the use of a password manager) and sent to the server, along with the public key.

  3. A copy of the private key is kept on the client but can be re-requested from the server at any time and from any client. (The private key is useless unless it is decrypted with the strong password set in Step 2.)

  4. The public key is also served at a well-known location and is used by other clients to encrypt private messages for the owner of the instance.

## Private Key Retrieval

  1. If the client doesn’t have a copy of the private key, it requests it from the server.

  2. The client decrypts the private key using the owner’s strong password.

  3. It uses the decrypted private key to authenticate using public key authentication (see below).

## Future thoughts:

  * Use a Service Worker to handle cryptographic functions in the browser to keep the private key safe from a server that turns malicious at some point. (Keep an eye on [browser compatibility](https://caniuse.com/#search=service%20worker) – once all evergreen browsers support this, let’s take a look.)

## General resources

  * [OpenCrypto](https://github.com/safebash/OpenCrypto): OpenCrypto is a Cryptographic JavaScript library built on top of [WebCrypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

  * [Web Crypto Live Table](https://diafygi.github.io/webcrypto-examples/)

  * [WebKit: Update on Web Cryptography](https://webkit.org/blog/7790/update-on-web-cryptography/): “When developing with pure JavaScript crypto libraries, secret or private keys are often stored in the global JavaScript execution context. It is extremely vulnerable as keys are exposed to any JavaScript resources being loaded and therefore allows XSS attackers be able to steal the keys. WebCrypto API instead protects the secret or private keys by storing them completely outside of the JavaScript execution context.”

  * [Storing Cryptographic Keys in Persistent Browser Storage](https://pomcor.com/2017/06/02/keys-in-browser/)

  * [WebCrypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

  * [WebCrypto examples](https://github.com/diafygi/webcrypto-examples)

  * [Cryptobench.js](https://github.com/mnasyrov/cryptobench-js)

  * An older article (2013) by Alex Maccaw on [end-to-end encryption in web apps](https://blog.alexmaccaw.com/end-to-end-encryption-in-js-web-apps).

## Public Key Authentication

### Resources

  * For a general guide on application of cryptography for deelopers, see the book [Serious Cryptography: A Practical Introduction to Modern Encryption](https://nostarch.com/seriouscrypto)

  * [feathers-authentication-publickey](https://github.com/amaurymartiny/feathers-authentication-publickey): “Public Key authentication strategy for feathers-authentication using Passport” ([Example.](https://github.com/amaurymartiny/feathers-authentication-publickey/tree/master/example))

  * [passport-publickey](https://github.com/timfpark/passport-publickey): “Passport strategy for authenticating using a public/private key pair to sign a nonce challenge.”

  * [passport-keyverify](https://github.com/phutchins/passport-keyverify): “Passport strategy for authenticating using a public/private key pair to sign a nonce challenge.”

  * [PiPo](https://github.com/phutchins/pipo): “A secure chat client with client side encryption written in NodeJS”

  * [Asymmetric Public / Private Key Encryption (RSA) in Node.js](https://coolaj86.com/articles/asymmetric-public--private-key-encryption-in-node-js/) “” ([Code.](https://git.daplie.com/coolaj86/examples-rsa-keypairs))

  * [TripleSec](https://keybase.io/triplesec/): “TripleSec is a simple, triple-paranoid, symmetric encryption library for a whole bunch of languages. It encrypts data with Salsa 20, AES, and Twofish, so that a someday compromise of one or two of the ciphers will not expose the secret.”

  * [JWT using RSA public/private key pairs (video)](https://www.youtube.com/watch?v=F0HLIe3kNvM)

  * [Lib Sodium](https://github.com/paixaop/node-sodium)

## JWT
