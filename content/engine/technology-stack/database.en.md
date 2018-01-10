+++
title = "Database"
weight = "70"
+++

**Note:** this decision is under review.

**Note:** What can we learn from our experience with Heartbeat Node (e.g., use of LevelDB?): https://source.ind.ie/project/heartbeat-node

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
