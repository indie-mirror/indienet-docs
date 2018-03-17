---
title: "API"
date: 2018-03-17T18:09:23+01:00
draft: false
weight: 10
---

## Overview

The API can be consumed via [Feathers Client](https://docs.feathersjs.com/api/client.html), [WebSocket](https://docs.feathersjs.com/api/client/primus.html#direct-connection), and [REST](https://docs.feathersjs.com/api/client/rest.html#http-api).

e.g., to get the owner of a site’s encrypted private signing key, you would:

### Feathers Client

```javascript
// Where app is an instance of Feathers Client, with a socket or REST connection configured for your Indie Site.
const encryptedPrivateSigningKey = await app.service('owner').get('encrypted-private-signing-key')
```

### WebSocket

```javascript
// Where socket is a web socket connection to an Indie Site.
const encryptedPrivateSigningKey = await socket.send('get', 'owner', 'encrypted-private-signing-key')
```

### REST

```
curl -H "Accept: application/json" http://localhost:3030/owner/encrypted-private-signing-key
```

In the documentation below, call types are listed in Feathers/WebSocket, REST verb order.

## Owner

The Owner API deals with registering the site owner and getting and setting details of the owner for authentication and site customisation.

### create, POST

Registers the owner of the site. This route may only be successfully called once. Future attempts to register an owner once an owner has already been registered will result in `403: Forbidden` errors.

Expects the owner’s keys object:

```json
  "derivedKeySalt": "string",
  "encryptedPrivateSigningKeyNonce": "string",
  "encryptedPrivateSigningKey": "string",
  "publicSigningKey": "string"
```

On success, returns `201: Created` and the owner’s key object that it was passed.

### find, GET

On success, returns `200: OK` and the owner’s keys object.

### get, GET /key-type

Returns necessary information for the requested key type. 

Valid key types are:

#### encrypted-private-signing-key

On success, returns `200: OK` and the following JSON object:

```json
  "derivedKeySalt": "string",
  "encryptedPrivateSigningKeyNonce": "string",
  "encryptedPrivateSigningKey": "string",
```

This object has all information necessary to:

1. Create a derived key from the owner’s password (secret) using Argon2.
2. Decrypt the ed25519 encrypted private signing key using the derived key.

(This steps are prerequisites on the client for using public-key authentication to authenticate the owner.)

#### public-signing-key

On success, returns `200: OK` and a JSON object with the owner’s ed25519 public signing key:

```json
  "publicSigningKey": "string"
```

#### Errors

  * `404: Not Found`: requested key type does not exist or key not set yet (see the error message accompanying the error to see which) 