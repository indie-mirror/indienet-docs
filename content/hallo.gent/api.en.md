The Magic Website Factory is used to query for .Gent domain availability and to request domain registration and Indie Site setup.

The Magic Website Factory is being developed in conjunction with [Combell](http://combell.gent), patrons of the .Gent top-level domain, and this specific instance is hosted by them on their infrastructure. The API presented here forms the public interface between Hallo.gent and Combell.

There is a development-time version of the Magic Website Factory used for local testing at https://source.ind.ie/indienet/magic-website-factory

All API calls require the passing of a secret that authenticates them. For the test server, this secret is hardcoded.

`secret=ethical-design`

## API versioning

The API endpoints should be versioned, starting with the current version at `/v1`. All locations, below, should be prefixed with the version identifier.

e.g., https://magicwebsitefactory.gent/v1/domain

## Domain service

Location: `/domain`

### GET

Check if the domain is available. Returns a JSON object with a boolean status.

#### Parameters

`/domain/<domain>?secret=ethical-design`

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

* `domain=<domain>`: the domain to register
* `callback=<callback-url>`: the callback URL to inform when the domain is ready
* `secret=ethical-design`

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
* `&secret=ethical-design`

#### Responses

The Magic Website Factory expects the following responses to its callback request:

* 200 OK: Response successfully received
* Error code (depending on error): callback failed. Log the error and queue the callback to be tried again (exponential falloff).