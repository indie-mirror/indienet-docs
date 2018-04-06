---
title: "Indie Site Deployment"
weight: 20
---

When called with a valid and authenticated [POST request to the Site Service](../api/#site-service), the Magic Website Factory:

  1. Registers the requested domain.
  2. Configures an [Indie Site](/site) instance with the passed owner settings details.
  3. When ready, calls the passed callback URL to inform Hallo.gent.

[The relevant API call](../api/#site-service) is detailed on the API page.

Here, we present the requirements for Indie Site configuration.

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
~/indie/owner-settings.json
```

The `owner-settings.json` file contains the owner settings that were passed to the [Magic Website Factory Site Service (POST) method](../api/#site-service)

The owner settings object that is passed only contains the `<secret sign-up code>`, so the _~/indie/owner-settings.json_ in a configured Indie Site instance should match the following:

```json
{
  "secretSignupCode": "<secret-sign-up-code>"
}
```

Indie Site uses the `secretSignupCode` to authenticate the initial configuration call from Hallo.gent. Unless the secret sign-up code passed matches the one in _~/indie/owner-settings.json_, attempts to configure the new site are refused. This is to prevent an edge-case where a malicious actor might gain access and ownership of newly-created Indie Sites before the legitimate owner has had a chance to set up their password and claim ownership.

## Running

To run Indie Site in production:

```bash
npm start
```
