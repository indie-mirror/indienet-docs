---
title: "Install"
weight: 20
---

1. [Install Hugo](https://gohugo.io/getting-started/installing/)

2. **Clone the repository:**
  * HTTPS (doesn’t require authentication):
  ```bash
  git clone https://source.ind.ie/indienet/docs.git
  ```

  * SSH (if you have a development account):
  ```bash
  git clone git@source.ind.ie:indienet/docs.git
  ```

3. **Run the `./install` script.**

    Or, manually:

    1. Add the deployment remote:
    ```bash
    git remote add deploy git@github.com:indie-mirror/indienet-docs.git
    ```

    2. Install and update the theme:
    ```bash
    git submodule update --init
    ```
