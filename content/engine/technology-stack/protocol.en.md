+++
title = "Protocol"
weight = "10"
+++

## ActivityPub

[ActivityPub](https://www.w3.org/TR/activitypub/) is a W3C Proposed Recommendation as of 5 December, 2017:

“The ActivityPub protocol is a decentralized social networking protocol based upon the [ActivityStreams 2.0](https://www.w3.org/TR/activitystreams-core/) data format. It provides a client to server API for creating, updating and deleting content, as well as a federated server to server API for delivering notifications and content.” – [W3C](https://www.w3.org/TR/activitypub/#abstract)

ActivityPub is already implemented in the popular federated microblogging platform Mastodon.

As of the time of this writing, Mastodon alone had over 1M active accounts spread across 1,000 active instances.

<iframe src="https://lou.lt/@mastodonusercount/99252518927131758/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="600"></iframe><script src="https://lou.lt/embed.js" async="async"></script>

A major goal of our project is to raise the number of ActivityPub instances by at least an order of magnitude as Indienet apps are, by definition, instances of one.

### Gotchas

  * Note: if we want to federate with Mastodon (we do), we must also implement WebFinger as Mastodon doesn’t accept Actor objects without a WebFinger. Discussion: https://github.com/w3c/activitypub/issues/194

### Resources

  * [ActivityStreams 2.0 implementation in JavaScript](https://github.com/aral/activitystrea.ms)

  * [jsonld.js](https://github.com/digitalbazaar/jsonld.js): “This library is an implementation of the JSON-LD specification in JavaScript.”

  * [See Radar](../../../radar) for links to other ActivityPub implementations.
