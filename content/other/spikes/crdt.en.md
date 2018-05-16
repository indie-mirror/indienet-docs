---
title: "CRDT: Conflict-free/Commutative Replicated Data Types"
weight: 10
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

### Contenders

#### Logoot

The algorithm that comes up on top in the above-mentioned paper is [Logoot](https://hal.archives-ouvertes.fr/inria-00432368/document) (PDF).

(Also, there is a variant of Logoot called Logoot-undo that implements undo/redo anywhere.)

TODO:
  * [ ] Spike logoot
  * [ ] Share findings

JavaScript implementations in the wild:

  * [bnoguchi/logoot](https://github.com/bnoguchi/logoot)
  * [usecanvas/logoot-js](https://github.com/usecanvas/logoot-js) (has a great readme, by the way)

#### Automerge

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



## Resources

* [Data Laced with History: Causal Trees & Operational CRDTs](http://archagon.net/blog/2018/03/24/data-laced-with-history/): excellent, contemporary review of the state of the art in CRDTs by Alexei. See the References section for links to further documents. 