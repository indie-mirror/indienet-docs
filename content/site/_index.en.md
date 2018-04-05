---
title: "Indie Site"
weight: 10
---

Indie Site is an implementation of a Federated Personal Web Site (FPWS).

But what is a Federated Personal Web Site? Good question. It’s:

  * __Federated:__ FPWS owners can follow each other’s sites, talk to each other, and be notified of updates. Federation is achieved via the W3C ActivityPub protocol so they can also talk to any other ActivityPub compatible service (e.g., Mastodon).

  * __Personal:__ An FPWS is for one person only: the owner of the site. It is a single-tenant application. We do not have (and do not want) the concept of “users.”

  * __Web Site:__ apart from the above two qualifications, an FPWS is a regular web site. It is build using web technologies and experienced through a standard web browsers. Indie site functionality can also be experienced via native apps in the future that use its REST And WebSocket APIs.

## Sections

{{% children %}}

## Screenshots

Indie Site is currently under development. These are some screenshots of the progress. It is currently not ready for testing as key functionality still has to be integrated from our various spikes. We hope to have a very basic functional initial version in April 2018.

### Posting interface

The posting interface aims to be as unobtrusive as possible. We are experimenting with seamless What You See Is What You Get (WYSIWYG) Markdown support for advanced authors as well as a default experience that works for anyone, regardless of existing technical knowledge.

![Indie Site Screenshot: posting interface](/images/screenshots/indie-site/post.jpg)

### Settings screen

The Settings screen is where you can customise the site’s background colour (soon to be the theme colour) or background image, your profile image, and your name and bio. All are optional.

![Indie Site Screenshot: settings screen](/images/screenshots/indie-site/settings.jpg)

### Initial setup

Initial setup involves picking a strong password. This password is not stored anywhere and is used to encrypt your private key for storage on the server. (What that means is that once we have implemented private messages, no one else will have access to them but you.)

![Indie Site Screenshot: initial setup](/images/screenshots/indie-site/setup.jpg)

### Remote follow

As Indie Site is a _federated_ personal web site (FPWS), other people who have a FPWS, Indie Site, Mastodon, or any other ActivityPub-compatible site/app/account can follow you. (And you can follow them.)

![Indie Site Screenshot: remote follow](/images/screenshots/indie-site/remote-follow.jpg)
