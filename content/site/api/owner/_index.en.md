---
title: 'Owner'
weight: 10
---

The Owner API deals with registering the site owner and getting and setting details of the owner for authentication and site customisation.

## create, POST

Registers the owner of the site. This route may only be successfully called once. Future attempts to register an owner once an owner has already been registered will result in `403: Forbidden` errors.

Expects the owner’s keys object:

```json
  "derivedKeySalt": "string",
  "encryptedPrivateSigningKeyNonce": "string",
  "encryptedPrivateSigningKey": "string",
  "publicSigningKey": "string"
```

On success, returns `201: Created` and the owner’s key object that it was passed.

## find, GET

On success, returns `200: OK` and the owner’s keys object.

## get, GET /key-type

Returns necessary information for the requested key type. 

Valid key types are:

### encrypted-private-signing-key

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

### public-signing-key

On success, returns `200: OK` and a JSON object with the owner’s ed25519 public signing key:

```json
  "publicSigningKey": "string"
```

### Errors

  * `404: Not Found`: requested key type does not exist or key not set yet (see the error message accompanying the error to see which) 