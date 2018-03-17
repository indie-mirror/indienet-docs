---
title: "ActivityPub"
weight: 10
---

Explore ActivityPub implementation and interoperability (e.g., with Mastodon) for the [Indie Site](/site) project.

## Goals

  * Get a feel for [ActivityPub](/site/engine/technology-stack/protocol/) by hardcoding responses

  * Get a feel for the [Feathers](/site/engine/technology-stack/framework/) framework

  * Get interoperability working with [Mastodon](https://joinmastodon.org) and note any special cases

## Repository

https://source.ind.ie/indienet/spikes/activitypub

## To-Dos

### Key

  * **[AP-n.i]**: Section n, subsection i in W3C ActivityPub spec.
  * **[AS-n.i]**: Section n, subsection i in W3C Activity Streams spec.

### List

  1. [x] Respond with hardcoded Actor object [AP-4]
  2. [x] Send WebFinger Link header in 1. [Not in AP spec]
  3. [x] Respond with hardcoded WebFinger request [Not in AP spec]
  4. [ ] Mock Outbox
  5. [ ] Respond to Follow requests **in progress**
  6. [ ] Mock Followers
  7. [ ] Mock Following

## Resources

  * [JSON-LD Origins](http://manu.sporny.org/2014/json-ld-origins/)

  * [Activity Streams Test Documents](https://github.com/w3c-social/activitystreams-test-documents)

  * [LevelGraph JSON-LD](https://github.com/levelgraph/levelgraph-jsonld) “LevelGraph-JSONLD is a plugin for LevelGraph that adds the ability to store, retrieve and delete JSON-LD objects. In fact, it is a full-blown Object-Document-Mapper (ODM) for LevelGraph.” (Also see: [this video](http://nodejsconfit.levelgraph.io), by the author.)

  * [Mississippi](https://github.com/maxogden/mississippi) “A collection of useful stream utility modules.”

## Notes

### ActivityStrea.ms library

  * Currently lacking some ActivityPub vocabulary (and declared as looking for maintainers on the repo, although Evan Prodromou has been updating it recently). [See this issue](https://github.com/jasnell/activitystrea.ms/issues/24). [My fork.](https://github.com/aral/activitystrea.ms)

### ActivityStreams test documents

“Collection of documents for testing activity streams parsers”

https://github.com/aral/activitystreams-test-documents

### ActivityPub-Mock

Evan Prodromou has just created a project to help unit test ActivityPub implementations. This looks very useful for our use case.

https://gitlab.com/aral/activitypub-mock


### JSON Server

“Get a full fake REST API with zero coding in less than 30 seconds (seriously)”

(Might be great for testing as we develop. See [jsonplaceholder.typicode.com](http://jsonplaceholder.typicode.com/) for a live demo.)

https://github.com/typicode/json-server

### Content-Type note

The content type for the JSON body parser in Express must be set to the ActivityPub content types or the body of the request will come in empty. e.g.,

```javascript
app.use(express.json({type: ['application/json', 'application/activity+json', 'application/ld+json; profile="https://www.w3.org/ns/activitystreams"']}))
```

Note: Mastodon uses the `application/activity+json` content type (which is _should_ not _must_ in the spec. Supporting both is the safest route.)

### FeathersJS Notes

  * **It’s currently not possible to mount a Feathers service at root.** See my comment on https://github.com/feathersjs/feathers/issues/728). Also see the `root-service-error` branch for a non-working example of how it would look if we could. **Workaround:** use Express middleware (see current implementation in `master`).

### Perceived behaviour from Mastodon interactions

#### Initial actor request flow

Source: https://source.ind.ie/indienet/spikes/activitypub/blob/master/src/app.js

  1. **Actor request:** expects actor object + HTTP Link header to be set pointing to WebFinger endpoint.

    Sample Link header:
    ```
    <https://dhobqcaxam.localtunnel.me/.well-known/webfinger?resource=acct%3Aaral%40dhobqcaxam.localtunnel.me>; rel="lrdd"; type="application/xrd+xml", <https://dhobqcaxam.localtunnel.me>; rel="alternate"; type="application/activity+json"
    ```

  2. **WebFinger request:** Expects WebFinger map from `resource` sepcified in the WebFinger URL in the Link header in 1 with a map to the `id` (url) specified in the Actor object in 1.

  3. Outbox request

  4. Following request

  5. Followers request

#### Follow request receipt

  1. **Inbox request**: Follow action

## Issues

### Personal Site Identifier Syntax Simplification

The current fediverse naming convention (@account@si.te) for accounts is biased towards multi-tenant federated web apps (as this is the norm for the fediverse at the moment).

For single-tenant sites (personal sites), this is unnecessarily redundant and can be simplified to the form @si.te as we are certain that @si.te is the identifier of a person and the only account on the site.

#### To-Dos

  * [ ] Define this syntax and propose that it be implemented in other systems (like Mastodon)

#### Temporary workaround

A temporary workaround is to have a well-known default account that is used to interoperate with existing systems like Mastodon.

In the spike, we’re using @person@si.te.

**Examples:** [app.js, line 68](https://source.ind.ie/indienet/spikes/activitypub/blob/master/src/app.js#L68) and [app.js, line 39](https://source.ind.ie/indienet/spikes/activitypub/blob/master/src/app.js#L39).

#### Related discussions

  * W3C: https://github.com/w3c/activitypub/issues/260

### Mastodon interoperability

Although the ActivityPub specification does not include or require WebFinger, Mastodon does in order to interoperate.

So, to interoperate with Mastodon:

  1. We must include a Link header in the ActivityStreams Actor response that points to the WebFinger endpoint.

    **Example:** [app.js, line 68](https://source.ind.ie/indienet/spikes/activitypub/blob/master/src/app.js#L68)

  2. Implement the WebFinger endpoint to map to the ActivityPub endpoint.

    **Example:** [app.js, line 33]( https://source.ind.ie/indienet/spikes/activitypub/blob/master/src/app.js#L33)

#### Related discussions

  * Eugen: https://github.com/tootsuite/mastodon/issues/4906#issuecomment-328844846
  * W3C issue: https://github.com/w3c/activitypub/issues/194
  * Mastodon issue: https://github.com/tootsuite/mastodon/pull/2522
