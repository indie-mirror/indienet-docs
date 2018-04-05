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

Indie Site expects its configuration data to be accessible from the following directory, which must be set up by the Magic Website Factory.

`~/indie/v1/`

Inside that directory, it expects the following file:

`owner-settings.json`: the owner settings that were passed to the [Magic Website Factory Site Service (POST) method](../api/#site-service)

## Running

To run Indie Site in production:

```bash
npm start
```
