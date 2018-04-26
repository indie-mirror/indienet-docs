---
title: "API"
weight: 10
---

This document describes the public interface between [Indie Onboarder](/onboarder) and its [Magic Website Factory](/magic-website-factory).

## Security

All communication is over TLS (HTTPS) only. Server setups on both ends have HTTP access disabled.

## Authentication

All calls between servers MUST pass a cryptographically-secure shared secret access token for authentication. The secret access token is created and initially communicated out of band via a cryptographically secure channel between Hallo.gent (Smart Citizen Lab at Stad Gent) and the Magic Website Factory (Combell). For authentication purposes, the secret access token is provided in the Authorization header of all calls between the servers.

```
Authorization: Bearer <secret-access-token>
```

For local development-time testing, a mock secret access token is hardcoded between Hallo.gent and the local testing version of Magic Website Factory.

## API versioning

The API endpoints should be versioned, starting with the current version at `/v1`. All locations, below, must be prefixed with the version identifier.

e.g., https://live.magicwebsitefactory.gent/v1/domain

## Live and test subdomains

  - Live: https://live.magicwebsitefactory.gent
  - Test: https://test.magicwebsitefactory.gent

## Domain service

Location: `/domain`

### GET

Check if the domain is available. Returns a JSON object with a boolean status.

#### Parameters

`/domain/<domain>`

Where the following known values for `domain` returns the following responses:

#### Responses

* Domain available: `{"available": true}`
* Domain not available: `{"available": false}`
* Domain check service not functional/available: 503 Service Unavailable error.

On the testing servers, the following known values for `domain` returns the following responses:

* available.gent: `{"available": true}`
* unavailable.gent: `{"available": false}`
* error.gent: 503 Service Unavailable error.

## Site service

Location: `/site`

### POST

Asks for the passed domain to be registered and for an Indie Site to be set up at it.

The secret sign-up code is the code that the person used to claim their Indie Site from Hallo.gent.

When the Magic Website Factory receives a successfully-authenticated call, it returns a synchronous response (see below). It then, asynchronously, registers the requested domain name, [deploys and configures](api) an [Indie Site](/site) instance at it, and calls the passed callback URL with the status of the operation.

#### Parameters (Body; JSON)

```json
  {
    "domain": "<the domain to register (string)>",
    "callback": "<callback URL>",
    "installerSecret": "<secret sign-up code>"
  }
```

#### Responses

* Registration request received and process successfully started: 201 Created 
* Domain not available: 409 Conflict
* Domain registration service is unavailable: 503 Service Unavailable

## Callback

On successful registration of the domain and creation of the Indie Site, the Magic Website Factory carries out a PATCH request to the `callback` URL passed in the Site service’s POST method with the following arguments:

### PATCH

Location: `<callback-url>/site/`

#### Parameters

* `<callback-url>/site/<domain>`: domain to update the status for
* `?status=<code>`: 201 = created, 409 = conflict (domain not available), 500 = internal server error (the site setup process failed and will not be retried)

#### Responses

The Magic Website Factory expects the following responses to its callback request:

* 200 OK: Response successfully received
* Error code (depending on error): callback failed. Log the error and queue the callback to be tried again (exponential falloff).

(When a 200 OK response is received, Hallo.gent POSTs the full owner settings, along with the secret sign-up code to the Indie Site. The Indie Site will then prompt for password creation and complete its configuration.)
