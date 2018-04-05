---
title: "API"
weight: 10
---

All API calls require the passing of a secret that authenticates them. For the test server, this secret is hardcoded.

`secret = "<secret>"`

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

`/domain/<domain>?secret=<secret>`

Where the following known values for `domain` returns the following responses:

#### Responses

* available.gent: `{"status": true}`
* unavailable.gent: `{"status": false}`
* error.gent: triggers the 503 service unavailable error.

On error, the server should return a valid error message.

Valid errors:

* 503: Service unavailable. The domain check/registration service is unavailable.

## Site service

Location: `/site`

### POST

Asks for the passed domain to be registered and for an Indie Site to be set up at it.

#### Parameters

```json
  {
    "domain": "<the domain to register (string)>",
    "callback": "<callback URL>",
    "owner-settings": {
      // Object with owner settings, to be copied
      // into the ~/indie/v1/owner-settings.json file
      // at deployment time.
    },
    "secret": "<secret>"
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
* `&secret=<secret>`

#### Responses

The Magic Website Factory expects the following responses to its callback request:

* 200 OK: Response successfully received
* Error code (depending on error): callback failed. Log the error and queue the callback to be tried again (exponential falloff).