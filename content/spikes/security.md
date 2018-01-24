+++
title = "Security"
weight = "20"
+++

## Goals

Security goals for Indienet are:

  1. Implement end-to-end encrypted private messages between federated personal web sites (with the same level of protection as PGP in email).

  2. Enable people to access their federated personal web site, and their entire list of end-to-end encrypted private messages, from any browser/device using a master password. If they haven’t authenticated on a certain device before, they will have to enter their master password the first time only.

  3. Research and use the latest cryptography knowledge and best practices whenever possible. (We will be consulting with cryptographers on our choices as we go.)

## General notes

  1. Please keep all spikes in separate projects in the /spikes/authentication subgroup  and push to the source repo regularly as you’re working on them.

  2. Please create issues for the various tasks you need to perform for each spike in the source repo and update and close the issues as you go.

  3. Please document findings and questions on the notes sections of the spikes on this page as you work.

  4. JavaScript should be in [JavaScript Standard Style](https://github.com/feross/standard).

## Source

* https://source.ind.ie/indienet/spikes/security

# Week 2

![xkcd comic on password strength](/images/spikes/password_strength.png)

## General notes / conclusions

The goal of this week is to take what we’ve learned from Week 1 and build upon that to spike out authentication with the tech stack that we’re considering (FeathersJS + Nuxt).

Based on what we learned last week:

## General cryptography policy

Use best practices, do not roll any custom crypto:

  * We use standard [libsodium](https://libsodium.org) methods
  * We encourage use of strong passwords to project the account (e.g., see [zxcvbn](https://github.com/dropbox/zxcvbn); [blog post/demo](https://blogs.dropbox.com/tech/2012/04/zxcvbn-realistic-password-strength-estimation/))
  * We use `crypto_pwhash` to derive the key we encrypt the private key with (uses Argon2)
  * We use `crypto_sign_detached` to sign (uses Ed25519)
  * We use `crypto_box_easy` to encrypt (uses X25519)
  * We use one keypair (We will use libsodium’s built-in methods to convert the Ed25519 keys to Curve25519 keys for  for signing and Curve25519 for encryption.)

## Spike 5: Publickey authentication with Feathers

### Goals

  * Sign in to a FeathersJS API from a single-page web app using a publickey authentication strategy.

### Details

  * Client: Use plain HTML, CSS, JavaScript (no framework)
  * Use libsodium
  * Adhere to general cryptography policy, above.

### Tests

  * Sign in (via sign-in form)
  * Sign out (via sign-out button)
  * Access public route: access allowed regardless of authentication state
  * Access private route when authenticated: access allowed
  * Access private route when unauthenticated: access denied

### Notes

  * None yet.

---

## Spike 6: Publickey authentication with Feathers, Vuex, and Nuxt

### Goals

  * Build on Spike 5 to add Nuxt and Vuex
  * Move from a client-side rendered FeathersJS/HTML single-page app to server-side rendered/universal Nuxt/Vuex client with FeathersJS API back-end (single project).
  * Evaluate: FeathersJS/Nuxt/JWT publickey authentication – there seems to be issues with auth in Feathers/Nuxt integrations; can we work around those? Is this a good/stable stack?

### Details

  * Use [feathers-vuex](https://github.com/feathers-plus/feathers-vuex)
  
### Tests

Repeat all tests from Spike 5, plus the following server-side rendered route tests:

  * Access server-side rendered private route when authenticated: access allowed
  * Access server-side rendered private route when unauthenticated: access denied
  * Access server-side rendered public route: access allowed regardless of authentication state

### Resources

  * [Nuxt Authentication from Scratch](https://medium.com/@kasheftin/nuxt-authentication-from-scratch-a7a024c7201b): a good tutorial on implementing authentication with Nuxt.

### Notes

  * None yet.

---

# Week 1

## Spike 1: OpenCrypto in browser: key generation, persistence, & retrieval

![Authentication flow](/images/spikes/authentication.png)

### Spike goals

1. Explore the design; does it work? Do we encounter any pain points or obvious flaws?

2. Explore the WebCrypto API-based OpenCrypto library. Is it fit for purpose?

### Spike tasks

Please keep the Web interface as simple as possible. Plain HTML.

Please also keep the server (Node) as simple as possible. Plain [Express](https://expressjs.com).

1. **Create a public key and an *unextractable* private key**

    This will result in:

    * The an <strike>*unextractable*</strike> extractable unencrypted private key (`unencryptedPrivateKey`)

    * The public key (`publicKey`)

    **Update:** note that we are now creating an _extractable_ private key at this point (otherwise, as pointed out by Wim during the spike, we couldn’t encrypt it with the secret key in the next steps). Also note that this changes the order of the steps below:

3. **Create an ephemeral symmetric key from a master password** that the site owner chooses (`passwordKey`) and use it to .

4. **Encrypt the `unencryptedPrivateKey`** with the `passwordKey` to get the `encryptedPrivateKey`

5. **Import the `unencryptedPrivateKey` as an unextractable key using the WebCrypto API and store it in IndexedDB** (see [level-browserify](https://www.npmjs.com/package/level-browserify) for a simpler LevelDB interface for IndexedDB).

    **Note:** see [this example](https://gist.github.com/saulshanabrook/b74984677bccd08b028b30d9968623f5) of storing unextractable keys in IndexedDB (in that example they create an unextractable key, we will be importing an the extractable key we created as an unextractable key during the import call).

    This means that:

    * From this first session on, the private key material is not accessible from JavaScript (we – or a malicious future script – can still use the key but can’t extract it)

    * The owner of the site doesn’t have to keep entering their password (they will have to – once – the first time they use a different browser or a different device to access their personal federated web site)

6. **Transfer `publicKey` and `encryptedPrivateKey` to the server** and store them there. You can store them on the file system.

7. **Make `publicKey` accessible from a route on the server** (just use plain Express on the server):

    `GET /public-key` → returns `publicKey`

8. **Make `encryptedPrivateKey` accessible from a route on the server:**

    `GET /private-key` → returns `encryptedPrivateKey`

    This means:

      * `publicKey` is accessible to any other instance at a well-known location (it will also be advertised within the actor object in ActivityPub, but don’t worry about that right now)

      * `encryptedPrivateKey` can be downloaded to any client that the owner uses in the future. When the owner provides the password they used in Step #2, we can recreate the symmetric key and decrypt `encryptedPrivateKey` to obtain `privateKey` (which we persist, as in Step 1, on the new client).

Note: at this point, the server has been set up/configured for the first time. It has been claimed by the owner. From here on, to authenticate the owner, we will use publickey authentication (see Spike 3).

### Notes

  * The flow has been updated based on feedback from the team to remove a logic error in the original (the private key is now created as an extractable key as, otherwise, it would not be possible to encrypt it and send it to the server). We do, however, still import the unencrypted private key as an unextractable key and store it in IndexedDB as such.

  * Wim pointed out that we should note that IndexedDB can be deleted by the browser due to space constraints. We should plan for this eventuality and handle it gracefully. The impact to the site owner will be that they will have to enter their password again so we can reauthenticate them.

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

  * We have chosen to go with the [Natrium Browser](https://github.com/wilhelmmatilainen/natrium-browser) because this library implements promises instead of callbacks. It also combines the [libsodium.js](https://github.com/jedisct1/libsodium.js) library with [Natrium](https://github.com/wilhelmmatilainen/natrium) so we have the best of both.
    ==> After trying to implement this, we came to the conclusion that both Natrium Browser and Natrium are very difficult to implement and rely on different build tools. That's why we choose to implement the [libsodium.js](https://github.com/jedisct1/libsodium.js) library

  * The problem with using libsodium is that you have to make choices about different settings for the encryption yourself. In contrast to the OpenCrypto library, the libsodium one is "big" (42kb ~ 512kb + 57,5kb).

  * [Libsodium quick reference](https://dev.to/paragonie/libsodium-quick-reference)

---

## Spike 3: Publickey authentication

To authenticate with the server for the REST and WebSocket APIs, we will be using JWT with a publickey authentication scheme, implemented within Passport.

For publickey authentication, we will:

1. Receive a `nonce` from the server

2. Sign the `nonce` with our `unencryptedPrivateKey` to get a `signedNonce`

3. Transfer the `signedNonce` to the server

4. On the server, verify the signature using our `publicKey`

5. If the signature is verified, the server will return the JWT, which we will persist on the client and use in subsequent requests. (We should import the JWT using the WebCrypto API as an unextractable key.)

For this spike, please explore two versions, in order:

1. Version 1: Vanilla Express and Passport:

   * [Express](https://expressjs.com)
   * [Passport](http://www.passportjs.org)
   * Test: does [passport-publickey](https://github.com/timfpark/passport-publickey) do what we want, or can we build what we want on top of it, or do we need to look into implementing our own publickey authentication scheme on top of Passport
   * Research: are there any other publickey authentication schemes implmented in JavaScript. Quickly test them out and report back if there are.

2. Version 2: [FeathersJS](https://feathersjs.com)

### Notes

At the moment we created a keypair with the OpenCrypto library. The library generates keys based on the RSA-OAEP algorithm. But this algorithm can only be used for encryption and decryption.
So in order to sign and verify a nonce, we have to create a new keypair with the RSA-PKCS1-v1_5 algorithm.
So a site will have two keypairs, one for encryption/decryption and one for sign/verify.

We will have to make a pull request to the OpenCrypto library, so we can choose which algorithm he will use to generate a keypair.

More info about which algorithm to use for what:
* https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API/Supported_algorithms
* https://blog.engelke.com/tag/webcrypto/
* https://stackoverflow.com/questions/33929699/using-webcrypto-to-generate-a-key-pair-useful-for-both-encryption-and-signing
* https://crypto.stackexchange.com/questions/12090/using-the-same-rsa-keypair-to-sign-and-encrypt

---

## Spike 4: End-to-end encrypted private messages

Mock a separate, second node (`node2`) that has a:

  * private key (`privateKeyNode2`)
  * public key (`publicKeyNode2`)
  * a message it wants to send to our node (`messageNode2`)

The goal is for this second node is to send us a private message, encrypted with our public key, that we will decrypt and read using our private key (as created in Spikes 1 & 2; please keep the spikes separate).

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

  * In this spike we send an encrypted message from server 1 (port 8080) to server 2 (port 8181).
  * We only saved the session key encrypted with the public key of the receiving server. In a real two way communication system, we should also encrypt the session key with the public key of the sending server. This encrypted session key should be stored together with the encrypted message on the sending server.
  * This spike only works with ASCII encoded strings, since we have to convert strings to base64. In future releases we should support UNICODE instead.


## Related resources

For more resources, references, etc., see:  [/engine/technology-stack/authentication](/engine/technology-stack/authentication)
