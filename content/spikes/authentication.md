+++
title = "Authentication"
weight = "20"
+++

Reference: [/engine/technology-stack/authentication](/engine/technology-stack/authentication)

## Spike 1: OpenCrypto in browser: key generation, persistence, & retrieval

### Spike questions

1. Shall we use the OpenCrypto library?

### Spike tasks

#### 1. Create a public and private key-pair and persist the private key

This will result in:

  1. The unencrypted private key (“unencryptedPrivateKey”)
  2. The public key (“publicKey”)

Persistence:

Store the unencryptedPrivateKey as an unextractable key in IndexedDB (see [level-browserify](https://www.npmjs.com/package/level-browserify) for a simpler LevelDB interface for IndexedDB).

This means that:

  * The private key material is not accessible from JavaScript (we – or a malicious future script – can still use the key but can’t extract it)
  * The owner of the site doesn’t have to keep entering their password (they will have to – once – the first time they use a different browser or a different device to access their personal federated web site)

#### 2. Create an unextractable symmetric key from a master password that the site owner chooses (“passwordKey”) and use it to encrypt the unencryptedPrivateKey

This key will be used to encrypt the private key that we will use for publickey authentication with the server, signed messages for authenticating unencrypted messages with federated servers, and for implementing end-to-end encrypted messages.

#### 3. Encrypt the private key with the passwordKey

You will end up with the encrypted private key (“encryptedPrivateKey”).

#### 4. Transfer and store the publicKey and the encryptedPrivateKey on the server

  1. Make the publicKey accessible from a route on the server (just use plain Express): `/public-key` → returns the publickey.
  2. Make the encryptedPrivateKey accessible from a route on the server: `/private-key` → returns the encryptedPrivateKey

This means that:

  * The publicKey is accessible to any other instance at a well-known location (it will also be advertised within the actor object in ActivityPub, but don’t worry about that right now)

  * The encryptedPrivateKey can be downloaded to any client that the owner uses in the future. When the owner provides the password they used in Step #2, we can recreate the symmetric key and decrypt the encryptedPrivateKey to obtain the privateKey (which we will persist, as in Step 1, on the new client).
