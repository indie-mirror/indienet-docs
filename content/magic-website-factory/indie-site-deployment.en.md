---
title: "Indie Site Deployment"
weight: 20
---

When called with a valid and authenticated [POST request to the Site Service](../api/#site-service), the Magic Website Factory:

  1. Registers the requested domain.
  2. Configures an [Indie Site](/site) (see Configuration, below).
  3. When ready, calls the passed callback URL to inform Hallo.gent of the status of the operation.

## Application details

[Indie Site](/site) ([source](https://source.ind.ie/indienet/site)):

  * Is a pure Node.js application
  * Is single process (uses an in-process LevelDB database)
  * Only has node module dependencies (`npm install`)

## Configuration

Indie Site expects its configuration data to be accessible from the following directory, which must be set up by the Magic Website Factory:

```bash
~/indie
```

Inside that directory, it expects the following file:

```bash
~/indie/installer-secret.json
```

The `installer-secret.json` file contains the secret sign-up code that was passed in the [POST request to the Site Service](../api/#site-service):

```json
{
  "secretSignupCode": "<secret sign up code>"
}
```

If a `~/indie/installer-secret.json` file exists, Indie Site uses the secret sign-up code to authenticate the initial configuration call from Hallo.gent. This is to prevent an edge-case where a malicious actor might gain access and ownership of newly-created Indie Sites before the legitimate owner has had a chance to set up their password and claim ownership.

## Running

To run Indie Site in production:

```bash
npm start
```
