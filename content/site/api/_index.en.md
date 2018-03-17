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

In the API documentation, call types are listed in Feathers/WebSocket, REST verb order.

## Services

{{% children %}}