+++
title = "Database"
weight = "70"
+++

## RethinkDB

RethinkDB is a realtime database that has a changefeeds feature that could simplify our production footprint/deployment requirements, server-side code dramatically and enable us to keep an event-driven workflow throughout.

Although RethinkDB supports clustering natively, scalability of the database should not be an issue for personal web sites/apps (as there’s a single owner).

### Possible issues

  * Changefeed connections might fail: [ReQL proposal: restarting feeds](https://github.com/rethinkdb/rethinkdb/issues/3471)

### Related projects

  * [node-rethinkdb-job-queue](https://github.com/grantcarthew/node-rethinkdb-job-queue): A persistent job or task queue backed by RethinkDB. (Also see, regarding changefeed connection failures: [Add rethinkdb-changefeed-reconnect](https://github.com/grantcarthew/node-rethinkdb-job-queue/issues/77))


### Useful links

  * [Troubleshooting common RethinkDB problems](https://rethinkdb.com/docs/troubleshooting/)


## Other options

A more traditional database / message queue combination.

  * [rsmq](https://github.com/smrchy/rsmq): Redis Simple Message Queue (Node)
  * (RabbitMQ, etc.…)
  * (For database, PostgreSQL, etc., or Mongo, etc.)
  * …?
