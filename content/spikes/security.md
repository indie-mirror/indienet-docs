+++
title = "Security"
weight = "20"
+++

## Goals

Security goals for Indienet are:

  1. Implement end-to-end encrypted private messages between federated personal web sites (with the same level of protection as PGP in email).

  2. Enable people to access their federated personal web site, and their entire list of end-to-end encrypted private messages, from any browser/device using a master password. If they haven’t authenticated on a certain device before, they will have to enter their master password the first time only.

  3. Research and use the latest cryptography knowledge and best practices whenever possible.

## General notes

  1. Please keep all spikes in separate projects in the /spikes/authentication subgroup  and push to the source repo regularly as you’re working on them.

  2. Please create issues for the various tasks you need to perform for each spike in the source repo and update and close the issues as you go.

  3. Please document findings and questions on the notes sections of the spikes on this page as you work.

## Spike 1: OpenCrypto in browser: key generation, persistence, & retrieval

### Spike goals

1. Explore the design; does it work? Do we encounter any pain points or obvious flaws?

2. Explore the WebCrypto API-based OpenCrypto library. Is it fit for purpose?

### Spike tasks

1. **Create a public key and an *unextractable* private key**

    This will result in:

    * The an *unextractable* unencrypted private key (`unencryptedPrivateKey`)

    * The public key (`publicKey`)

2. **Persist the unextractable `unencryptedPrivateKey` in IndexedDB** (see [level-browserify](https://www.npmjs.com/package/level-browserify) for a simpler LevelDB interface for IndexedDB).

    This means that:

    * The private key material is not accessible from JavaScript (we – or a malicious future script – can still use the key but can’t extract it)

    * The owner of the site doesn’t have to keep entering their password (they will have to – once – the first time they use a different browser or a different device to access their personal federated web site)

3. **Create an ephemeral symmetric key from a master password** that the site owner chooses (`passwordKey`) and use it to .

4. **Encrypt the `unencryptedPrivateKey`** with the `passwordKey` to get the `encryptedPrivateKey`

5. **Transfer `publicKey` and `encryptedPrivateKey` to the server** and store them there. You can store them on the file system.

6. **Make `publicKey` accessible from a route on the server** (just use plain Express on the server):

    `GET /public-key` → returns `publicKey`

7. **Make `encryptedPrivateKey` accessible from a route on the server:**

    `GET /private-key` → returns `encryptedPrivateKey`

    This means:

      * `publicKey` is accessible to any other instance at a well-known location (it will also be advertised within the actor object in ActivityPub, but don’t worry about that right now)

      * `encryptedPrivateKey` can be downloaded to any client that the owner uses in the future. When the owner provides the password they used in Step #2, we can recreate the symmetric key and decrypt `encryptedPrivateKey` to obtain `privateKey` (which we persist, as in Step 1, on the new client).

### Notes

  * None yet.

---

## Spike 2: Re-implement Spike 1 using libsodium

Use [libsodium](https://download.libsodium.org/doc/) to implement Spike 1 (please keep all spikes separate and checked into source control; don’t overwrite Spike 1).

### Questions

1. Which implementations should we spike (and why?)

  * [libsodium.js](https://github.com/jedisct1/libsodium.js)
  * [js-nacl](https://github.com/tonyg/js-nacl)
  * [Natrium](https://github.com/wilhelmmatilainen/natrium)
  * [Natrium Browser](https://github.com/wilhelmmatilainen/natrium-browser)

2. What are the differences in workflow between Spike 1 and Spike 2? Which is easier to work with?

3. What are the security implications of using a libsodium port (which gives us better ciphers) instead of the WebCrypto API (which gives us unextractable keys)? (This is a question we should post to some friendly neighbourhood cryptographers in the community.)

### Notes

  * None yet.

---

## Spike 3: End-to-end encrypted private messages

Mock a separate, second node (`node2`) that has a:

  * private key (`privateKeyNode2`)
  * public key (`publicKeyNode2`)
  * a message it wants to send to our node (`messageNode2`)  

The goal is for this second node is to send us a private message, encrypted with our public key, that we will decrypt and read using our private key (as created in Spike 1; please keep the spikes separate – you can copy Spike 1 to start Spike 2).

Continue to mock `node2` to:

  1. Create a secret symmetric session key (`secretSessionKeyNode2`)

  2. Encrypt `messageNode2` with `secretSessionKeyNode2` to get `encryptedMessageNode2`

  3. Encrypt `secretSessionKeyNode2` with our `publicKey` to get `encryptedSecretSessionKeyNode2` (`node2` encrypts `secretSessionKeyNode2` with our public key since the message is being sent to us from `node2` and we should be the only ones able to decrypt it, using our `privateKey`)

**We then assume that node2 communicates, via ActivityPub, `encyptedMessageNode2` and `encryptedSecretSessionKeyNode2` to us.** Please don’t mock separate nodes or networking between them for this spike.

Then, on our node:

  1. Use `privateKey` to decrypt `encryptedSecretSessionKeyNode2` and get `secretSessionKeyNode2`

  2. Use `secretSessionKeyNode2` to decrypt `encryptedMessageNode2` and get `messageNode2`

  3. Display `messageNode2`

### Notes

  * None yet.

---

## Related resources

For more resources, references, etc., see:  [/engine/technology-stack/authentication](/engine/technology-stack/authentication)
