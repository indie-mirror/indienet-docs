+++
title = "Deploy"
weight = "50"
+++

If you have deployment privileges, run the `./deploy` script.

Or, manually:

```bash
git push deploy master
```

This will push to the deployment mirror repository on GitHub which will trigger [Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/) to build it via Hugo and deploy it to [indienet.info](https://indienet.info).

See [netlify.toml](https://source.ind.ie/indienet/docs/blob/master/netlify.toml) for the deployment configuration.
