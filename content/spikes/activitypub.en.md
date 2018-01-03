+++
title = "ActivityPub"
weight = "10"
+++

Explore ActivityPub implementation and interoperability (e.g., with Mastodon) for the [Indienet Engine](../../engine/) project.

## Goals

  * Get a feel for [ActivityPub](../../engine/technology-stack/protocol/) by hardcoding responses

  * Get a feel for the [Feathers](../../engine/technology-stack/framework/) framework

  * Get interoperability working with [Mastodon](https://joinmastodon.org) and note any special cases

## Repository

https://source.ind.ie/indienet/spikes/activitypub

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
