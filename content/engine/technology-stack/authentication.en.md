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

## Public Key Authentication

### Resources

  * [feathers-authentication-publickey](https://github.com/amaurymartiny/feathers-authentication-publickey): “Public Key authentication strategy for feathers-authentication using Passport” ([Example.](https://github.com/amaurymartiny/feathers-authentication-publickey/tree/master/example))

  * [passport-publickey](https://github.com/timfpark/passport-publickey): “Passport strategy for authenticating using a public/private key pair to sign a nonce challenge.”

  * [passport-keyverify](https://github.com/phutchins/passport-keyverify): “Passport strategy for authenticating using a public/private key pair to sign a nonce challenge.”

  * [PiPo](https://github.com/phutchins/pipo): “A secure chat client with client side encryption written in NodeJS”

  * [Asymmetric Public / Private Key Encryption (RSA) in Node.js](https://coolaj86.com/articles/asymmetric-public--private-key-encryption-in-node-js/) “” ([Code.](https://git.daplie.com/coolaj86/examples-rsa-keypairs))

  * [TripleSec](https://keybase.io/triplesec/): “TripleSec is a simple, triple-paranoid, symmetric encryption library for a whole bunch of languages. It encrypts data with Salsa 20, AES, and Twofish, so that a someday compromise of one or two of the ciphers will not expose the secret.”

  * [JWT using RSA public/private key pairs (video)](https://www.youtube.com/watch?v=F0HLIe3kNvM)

## JWT
