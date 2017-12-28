+++
title = "Technology Stack"
weight = "10"
+++

Indienet Engine is an implementation of the ActivityPub [protocol](protocol/) written in the JavaScript (ES6) [language](language/). It runs on a Node.js [server](server/) and is built using the FeathersJS [framework](framework/) and a RethinkDB [database](database/). It exposes websocket and REST [interfaces](interfaces/) and implements publickey and JWT [authentication](authentication/).

The overriding design goal of Indienet Engine is to be a basic foundational component for creating federated apps and sites for individual use. As such, we want it to be as accessible and loosely-coupled as possible. Our technology choices attempt to reflect these goals.

{{% children %}}
