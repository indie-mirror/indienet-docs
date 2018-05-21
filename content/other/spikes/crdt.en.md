---
title: "CRDT: Conflict-free/Commutative Replicated Data Types"
weight: 5
---

Indie Site is expected to:

  1. Work offline (in web parlance/marketing speak: “offline first”/“progressive web app”)
  2. Be able to sync changes between the server and clients
  3. Eventually be able to support peer-to-peer communication between clients

All of these require a robust sync solution for sharing state between the various nodes (the server is merely seen as another node in this approach). And sync is hard.

## OT vs CRDT

We can rule out OT from the outset as there is a body of evidence that [it is overly-complicated to implement](https://en.wikipedia.org/wiki/Operational_transformation#Critique_of_OT):

“Joseph Gentle who is a former Google Wave engineer and an author of the Share.JS library wrote, "Unfortunately, implementing OT sucks. There's a million algorithms with different tradeoffs, mostly trapped in academic papers. The algorithms are really hard and time consuming to implement correctly. […] Wave took 2 years to write and if we rewrote it today, it would take almost as long to write a second time.”

Our spikes will focus on evaluating various CRDT algorithms and, when possible, existing JavaScript implementations of the underlying algorithms.

## CRDTs

CRDT stands for [conflict-free/commutative replicated datatype](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type).

### Process

Over the course of little over a week, I’ve reviewed a large amount of academic papers as well as implementations of CRDTs.

Some of the most relevant resources are:

  * [Data Laced with History: Causal Trees & Operational CRDTs](http://archagon.net/blog/2018/03/24/data-laced-with-history/): A great starting point for an excellent, contemporary review of the state of the art in CRDTs by Alexei. See the References section of the article for links to further documents.

  * Logoot: [paper: “Logoot: A Scalable Optimistic Replication Algorithm for Collaborative Editing on P2P Networks”](https://hal.archives-ouvertes.fr/inria-00432368/document), [Collaborative text editing with Logoot](https://medium.com/@ravernkoh/collaborative-text-editing-with-logoot-a632735f731f): an easy-to-understand summary of the algorithm. Implementations: [bnoguchi/logoot](https://github.com/bnoguchi/logoot), [usecanvas/logoot-js](https://github.com/usecanvas/logoot-js) (has a great readme, by the way)

  * LSEQ: [paper: “LSEQ: an Adaptive Structure for Sequencing in Distributed Collaborative Editing”](https://hal.archives-ouvertes.fr/hal-00921633/document) – builds upon Logoot and “belongs to the variable-size identifiers class of sequence CRDTs”. Implementation: see [LSEQTree](https://github.com/Chat-Wane/LSEQTree)

  * Causal Trees: [paper: “Deep Hypertext with Embedded Revision Control Implemented in Regular Expressions”](http://www.ds.ewi.tudelft.nl/~victor/articles/ctre.pdf)

  Other important resources that I consulted include [the DAT paper](https://datproject.org/paper), [δ-CRDT](https://www.researchgate.net/publication/266798509_Efficient_State-based_CRDTs_by_Delta-Mutation); [implementation](https://github.com/asonge/loom), [RGA](http://csl.skku.edu/papers/CS-TR-2009-318.pdf).

  Finally, the paper [Evaluating CRDTs for Real-time Document Editing](https://hal.inria.fr/inria-00629503/document) has a review of the performance metrics of various CRDTs.

### Outcomes

Copied over from the my [stream of consciousness post on 2018-05-17](https://forum.ind.ie/t/stream-of-consciousness-soc-2018-05-17/2146/1) from the Indie Forum:

There are three experience-related problems with fully peer-to-peer approaches (e.g., scuttlebutt, etc.):

1. __Discovery/availability:__ They rely on discovery mechanisms that are not as performant as centralised discovery. Availability is not guaranteed as nodes exist on sometimes-connected clients.

2. __Greedy replication:__ They’re either based on data types that require a complete merge/feed or attempt (in the background) to replicate as much of the full dataset on every node as possible. This leads to long (especially initial) sync times, and can lead to huge amounts of bandwidth usage (My Scuttlebutt client was transferring gigabytes of data within the first few hours.)

3. __Ignoring social aspects__: e.g., Lack of ability to delete posts. While tombstone-based systems are easier to replicate, they do lead to social issues. When you share something on Scuttlebutt, for example, that’s it. You cannot undo. 

A fourth issue (related to #3) with some systems (e.g., DAT, IPFS), is that their problem domain is more about protecting against censorship (a worthy and important goal) than ensuring individual sovereignty / protecting privacy. 

How Indie Site will attempt to address these issues:

1. __Discovery/availabiltiy:__ Of course, we don’t want centralised discovery in a peer to peer system so a core tenet of Indie is that _everyone_ should have their own highly available and easily findable node on the Internet. Let’s call it an Always On Node (AON). Within the context of the current web, you can think of it as a web site. And, indeed, it will fulfil the role of a personal web site. As much as possible, this node should not be privileged over other nodes in its capabilities beyond having the two traits mentioned above. Addressability via domain name makes it findable within the mainstream Internet and third-party hosting makes it highly available. I see this as a necessary bridge between the client/server (centralised) web and a fully peer-to-peer topology in the future. 

2. __Lazy/partial replication__: When you visit a web site, you do not download all of the data from that web site. The experience of the Web would be markedly worse if this was the case. Instead, you only download the content you’re interested in at that time. P2P systems should be the same. The difference being that every node is a web server. (This is possible to implement even between two web clients using WebRTC.) The always-on node (AON) would be the fallback source of truth and, by its highly available and always findable nature, will most likely – but not necessarily – contain the most information about the system at any one time (especially when we factor in server-to-server federation). We should embrace the fact that different nodes will most likely rarely if ever converge to contain a complete copy of all of the data but that any node can diverge on the same view of the data for whatever transaction it is currently involved in (e.g., viewing the latest posts or a private conversation).

3. __Focus on social aspects:__ We must be aware of and sensitive to the social aspects of the system we’re designing. Eugen is a shining example of this with his approach to Mastodon. When it comes to issues like “delete”, for example, while we cannot violate the laws of physics (what is made public cannot provably be made private again),  we can be as sensitive as possible in how we design and expose such functionality. 

My research over the last week into CRDTs (conflict-free/commutative data types) as well as Directed Acyclic Graph / Merkle Tree-based data types, etc., and some of the latest turn-key replicated data type solutions out there (like Automerge) as well as the more Web-centric/offline-first approaches to solve similar problems (e.g., Logux) lead me to believe that we need our own data types and protocols for Indie Site as they, more than anything else, will determine the “shape” of this platform and what it can do (and whether it will be fit for purpose both today as a federated personal web site system and, possibly, tomorrow as a bridge to an entirely p2p system).

Some initial quick thoughts on the core principles of our data type(s)/replication strategy, authorisation, etc:

  * All nodes do not have to have the same data (messages) but, given the same data, will converge on the same ordering (commute) and views that are rendered on the same subset of the data should match.

  * Views are never exchanged, only messages (the loading of a server-side rendered web view might be seen as an exception to this as the view is reduced from the data on the server-side node and communicated to the client but it is really, if anything, an experience-based optimisation on the first render before the view is cached by the service worker. After which, every further render doesn’t violate this principle.)

* Conflicts cannot happen (we use conflict-free data types)

* Support paging and streaming

* Work offline

* All messages are signed

* Messages have partial order and refer to the hash of their parent (messages may only have a single parent). A special root node marks the start of each tree. 

*  Messages contain their own list of authorised appenders (via their public keys)

* Access control is managed via end-to-end encryption of the messages.

* Messages are both URL-addressable (via the AON’s domain) and content addressable by their hash.

Some implementation thoughts:

* The persistence/replication interface for the view layer should be entirely transparent and expose reactive model objects. For the web client, initial views should be server-side rendered. (See Nuxt/VueJS)

* Messages received that reference parent messages that have not been received are kept in a pool (e.g, see the WOOT implementation). Owners may or may not be alerted to the presence of these messages depending on use case. 

* Domain-specific (non-replicated) objects should be accessible via API (e.g., posts for embedding in other sites). These collections should be considered rendered views of the original messages and should not otherwise be exchanged between nodes.

* Regarding the conflict-free/commutative structure of the data type(s), there is much to be liked about the approaches of Logoot/LSEQ (see LSEQTree) and Causal Trees.

### Spike: Automerge

Spike: https://source.ind.ie/indienet/spikes/crdt/automerge

Automerge is a JavaScript library for building distributed systems using a conflict-free JSON data structure. It is based on the paper [A Conflict-Free Replicated JSON Datatype](https://arxiv.org/abs/1608.03960) by Martin Kleppmann and Alastair R. Beresford.

I summarised my initial (very basic) performance/storage-related findings at:
https://github.com/automerge/automerge/issues/89

In its current state, I’m not sure we can use this. To have a conflict-free JSON structure would be amazing but it currently comes with a lot of overhead. There’s nothing we want to achieve that we cannot using a linear structure such as the ones used by more traditional CRDTs like Logoot. My next spike will be to explore this option.

### Ruled out

The following algorithms/approaches have been evaluated and ruled out:

#### WOOT (and WOOTO, WOOTH, etc.)

[Aral] My own research in this area dates back six years when I was looking at the WOOT algorithm ([Objective-C implementation](https://github.com/aral/trovedata), talk at (Warning: YouTube link) [Realtime Conf 2013](https://www.youtube.com/watch?v=NSTZ4mIv_wk))

In [Evaluating CRDTs for Real-time Document Editing](https://hal.inria.fr/inria-00629503/document) (PDF) by Mehdi Ahmed-Nacer, et. al, even the original researchers rule out WOOT as a real-world algorithm due to performance/scalability issues. The optimised WOOTO and WOOTH algorithms do not fair much either.
