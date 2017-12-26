+++
title = "Docs"

+++

This is the documentation on the documentation project (this). Very meta.

## Process

The Docs project is used both for planning artefacts and to document the various projects under the Indienet banner as they are developed.

## Development

Docs is built using [Hugo](https://gohugo.io). See the Hugo [quick start guide](https://gohugo.io/getting-started/quick-start/) to install Hugo on your system.

We use [our fork](https://github.com/aral/hugo-theme-docdock) of the [Hugo DocDock Theme](http://docdock.netlify.com/). The fork currently fixes [an issue with search highlighting breaking in Mermaid sequence diagrams](https://github.com/vjeantet/hugo-theme-docdock/issues/112)). (The fix [has now been merged into master](https://github.com/vjeantet/hugo-theme-docdock/pull/113).)

To contribute to the project, team members should [fork the repository on source.ind.ie](https://source.ind.ie/indienet/docs) and submit merge requests.

## Installation

1. Clone the repository
2. Install the theme (`git submodule update --init`)

## Running

```bash
hugo server -D
```

(This will run a local dev server and locally publish the site, including drafts.)

## Deployment

If you have deployment privileges, you can deploy via Git:

```bash
git push deploy master
```

This will push to the deployment mirror repository on GitHub which will trigger [Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/) to build it via Hugo and deploy it to [indienet.info](https://indienet.info).

See [netlify.toml](https://source.ind.ie/indienet/docs/blob/master/netlify.toml) for the deployment configuration.
