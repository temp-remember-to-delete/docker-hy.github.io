---
path: '/part-3/2-deployment-pipelines'
title: 'Deployment pipelines'
hidden: false
---

Let's set up a deployment pipeline from GitHub to a host machine. We will demonstrate this using your local machine, but the same steps can be used for Raspberry Pi or even a virtual machine in the cloud (such as one provided by [Hetzner](https://www.hetzner.com/cloud)).

We will use GitHub Actions to build an image, push the image to Podman Hub, and then use a project called "Watchtower" automatically pull the image from there.

Let's work with the repository [https://github.com/podman-hy/podman-hy.github.io](https://github.com/podman-hy/podman-hy.github.io) as it already has a Podmanfile and the GitHub Actions config for our convenience.

First either fork the repository or clone it as your own.

Let's go over the GitHub Actions instructions. We will be using the official actions offered by [podman](https://github.com/podman), but we could've just installed podman and ran podman build. Most of the following is simply copied from the action [usage instructions](https://github.com/marketplace/actions/build-and-push-podman-images):

```yaml
name: Release DevOps with Podman # Name of the workflow

# On a push to the branch named master
on:
  push:
    branches:
      - master

# Job called build runs-on ubuntu-latest
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    # Checkout to the repository (the actions don't actually need this since they use the repository context anyway)
    - uses: actions/checkout@v2

    # We need to login so we can later push the image without issues.
    - name: Login to PodmanHub
      uses: podman/login-action@v1
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    # Builds devopspodmanuh/podman-hy.github.io
    - name: Build and push
      uses: podman/build-push-action@v2
      with:
        push: true
        tags: devopspodmanuh/coursepage:latest
```

Before this will work we will need to add 2 Secrets to the repository: `DOCKERHUB_TOKEN` and `DOCKERHUB_USERNAME`. This is done by opening the repository in browser and first pressing *Settings* then *Secrets*. The `DOCKERHUB_TOKEN` can be created in Podman Hub, click your username and then *Account Settings* and *Security*.

Now create a podman-compose.yml. We will use [watchtower](https://github.com/containrrr/watchtower) to automate the updates.

Watchtower is an open source project that automates the task of updating images. It will poll the source of the image (in this case podmanhub) for changes in the containers that are running. The container that is running will be updated when a new version of the image is pushed to podman hub. Watchtower respects tags e.g. container using ubuntu:18.04 will not be updated unless a new version of ubuntu:18.04 is released.

<text-box name="Security reminder: Podman Hub accessing your computer" variant="hint">

Note that now anyone with access to your Podman Hub also has access to your PC through this. If they push a malicious update to your application watchtower will happily download and start the updated version.

</text-box>

```yaml
version: "3.8"

services:
  coursematerial:
    image: devopspodmanuh/coursepage
    ports:
      - 4000:80
    container_name: coursematerial
  watchtower:
    image: containrrr/watchtower
    environment:
      -  WATCHTOWER_POLL_INTERVAL=60 # Poll every 60 seconds
    volumes:
      - /var/run/podman.sock:/var/run/podman.sock
    container_name: watchtower
```

Before running podman-compose up here, beware that watchtower will try to update **every** image running in case there is a new version. Check the [documentation](https://containrrr.github.io/watchtower/) if you want to prevent this.

Run this with `podman-compose up` and commit something new into the repository. When you do the `git push`, follow how the github actions pushes a new image to PodmanHub and then watchtower pulls the new image to your machine.

<exercise name="Exercise 3.1: A deployment pipeline to Heroku">

  Let's create our first deployment pipeline!

  For this exercise you can select which ever web application you already have containerized.

  If you don't have any web applications available you can use any one from this course and modify it. (Such as the course material itself)

  Use GitHub, Github Actions, and Heroku to deploy a container to heroku. You can also use other CI/CD tools instead of GitHub Actions.

  Submit a link to the repository with the config.

</exercise>

<exercise name="Exercise 3.2: Building images inside of a container">

  Watchtower uses volume to podman.sock socket to access Podman daemon of the host from the container. By leveraging this ourselves we can create our own simple build service.

  Create a project that downloads a repository from github, builds a Podmanfile located in the root and then publishes it into Podman Hub.

  You can use any programming language / technology for the project implementation. A simple bash script is viable.

  Then create a Podmanfile for it so that it can be run inside a container.

  Make sure that it can build at least some of the example projects.

</exercise>
