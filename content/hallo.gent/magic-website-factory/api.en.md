---
title: "API"
weight: 10
---

All API calls require the passing of a cryptographically-secure shared secret access token to authenticate them. The secret access token is communicated out of band via a cryptographically secure channel between Hallo.gent (Smart Citizen Lab at Stad Gent) and the Magic Website Factory (Combell). For the test server, a mock secret access token is hardcoded.

All communication is over TLS (HTTPS) only. Server setups on both ends have HTTP access disabled.

## API versioning

The API endpoints should be versioned, starting with the current version at `/v1`. All locations, below, should be prefixed with the version identifier.

e.g., https://live.magicwebsitefactory.gent/v1/domain

## Live and test subdomains

  - Live: https://live.magicwebsitefactory.gent
  - Test: https://test.magicwebsitefactory.gent

## Domain service

Location: `/domain`

### GET

Check if the domain is available. Returns a JSON object with a boolean status.

#### Parameters

`/domain/<domain>?secret=<secret-access-token>`

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

#### Parameters (Body; JSON)

```json
  {
    "domain": "<the domain to register (string)>",
    "callback": "<callback URL>",
    "owner-settings": {
      "secretSignUpCode": "<secret-sign-up-code>"
    },
    "secret": "<secret-access-token>"
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
* `&secret=<secret-access-token>`

#### Responses

The Magic Website Factory expects the following responses to its callback request:

* 200 OK: Response successfully received
* Error code (depending on error): callback failed. Log the error and queue the callback to be tried again (exponential falloff).

(When a 200 OK response is received, Hallo.gent POSTs the full owner settings, along with the secret sign-up code to the Indie Site. The Indie Site will then prompt for password creation and complete its configuration.)
