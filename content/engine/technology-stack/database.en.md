+++
title = "Database"
weight = "70"
+++

**Note:** this decision is under review. Options:

## LevelDB

**Note:** What can we learn from our experience with [Heartbeat Node](https://source.ind.ie/project/heartbeat-node) - e.g., regarding the use of LevelDB for this purpose? (e.g., see the [implementation of the StreamWeaver class](https://source.ind.ie/project/heartbeat-node/blob/master/StreamWeaver.coffee)).

## Secure Scuttlebutt

[Secure Scuttlebutt](https://github.com/ssbc/secure-scuttlebutt) is built on LevelDB, specifically for p2p messaging. Would the higher-level abstraction aid or limit us?

Related:

  * [Kappa architecture](http://milinda.pathirage.org/kappa-architecture.com/)
  * [Scuttlebot](https://github.com/ssbc/scuttlebot)
  

## RethinkDB

RethinkDB is a realtime database that has a changefeeds feature that could simplify our production footprint/deployment requirements, server-side code dramatically and enable us to keep an event-driven workflow throughout.

Although RethinkDB supports clustering natively, scalability of the database should not be an issue for personal web sites/apps (as there’s a single owner).

### Possible issues

  * Changefeed connections might fail: [ReQL proposal: restarting feeds](https://github.com/rethinkdb/rethinkdb/issues/3471)

### Related projects

  * [Penseur](https://github.com/hueniverse/penseur): Lightweight RethinkDB wrapper. Changes API includes reconnect functionality:

```
await db[table].changes(criteria, [options])

Subscribe to changes matching the given criteria for the table.

criteria - db criteria functions chained together
options - optional object with the following properties
handler - handler function to execute when changes occur.
reconnect - boolean, reconnect if the connection to the feed is interrupted
initial - boolean, include the initial results in the change feed
```
  * [node-rethinkdb-job-queue](https://github.com/grantcarthew/node-rethinkdb-job-queue): A persistent job or task queue backed by RethinkDB. (Also see, regarding changefeed connection failures: [Add rethinkdb-changefeed-reconnect](https://github.com/grantcarthew/node-rethinkdb-job-queue/issues/77))


### Useful links

  * [Troubleshooting common RethinkDB problems](https://rethinkdb.com/docs/troubleshooting/)


## Other options

A more traditional database / message queue combination.

  * [rsmq](https://github.com/smrchy/rsmq): Redis Simple Message Queue (Node)
  * (RabbitMQ, etc.…)
  * (For database, PostgreSQL, etc., or Mongo, etc.)
  * …?
