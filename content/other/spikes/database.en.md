---
title: "Database"
weight: 10
---

**ACTIVE SINCE: 2018-05-21**

We need a database choice to support the “offline first”/p2p nature of indie site and its relationship to the always on node (server) and other nodes.

## Requirements

  * Isomorphic (browser and Node.js at a minimum)
  * In browser: IndexedDB
  * In Node: in-process database (LevelDB or SQLite)
  * Changes/events

## Nice-to-haves

  * Simple API
  * Easy migrations
  * Vue.js integration

## Candidates

Spike:

  * [ ] NanoSQL (in Node: with LevelDB store)
  * [ ] Dexie.js (in Node: with [IndexedDBShim](https://github.com/axemclion/IndexedDBShim) and SQLite store)

Test:

  * Performance
  * Easy of use
  * Fitness for purpose
