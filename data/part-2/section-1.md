---
path: '/part-2/1-migrating-to-podman-compose'
title: 'Migrating to podman compose'
hidden: false
---

Even with a simple image, we've already been dealing with plenty of command line options in both building, pushing and running the image.

Next we will switch to a tool called podman-compose to manage these.

podman-compose is designed to simplify running multi-container applications to using a single command.

In the folder where we have our Dockerfile with the following content:

```podmanfile
FROM ubuntu:18.04

WORKDIR /mydir

RUN apt-get update
RUN apt-get install -y curl python
RUN curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
RUN chmod a+x /usr/local/bin/youtube-dl

ENV LC_ALL=C.UTF-8

ENTRYPOINT ["/usr/local/bin/youtube-dl"]
```

we create a file called `podman-compose.yml`:

```yaml
version: '3.8'

services:
    youtube-dl-ubuntu:
      image: <username>/<repositoryname>
      build: .
```

The version setting is not very strict, it just needs to be above 2 because otherwise the syntax is significantly different. See <https://docs.podman.com/compose/compose-file/> for more info. The key `build:` value can be set to a path (ubuntu), have an object with `context` and `podmanfile` keys or reference a `url of a git repository`.

Now we can build and push with just these commands:

```console
$ podman-compose build
$ podman-compose push
```

## Volumes in podman-compose ##

To run the image as we did previously, we will need to add the volume bind mounts. Volumes in podman-compose are defined with the following syntax `location-in-host:location-in-container`. Compose can work without an absolute path:

```yaml
version: '3.8'

services:

    youtube-dl-ubuntu:
      image: <username>/<repositoryname>
      build: .
      volumes:
        - .:/mydir
      container_name: youtube-dl
```

We can also give the container a name it will use when running with container_name. And the service name can be used to run it:

```console
$ podman-compose run youtube-dl-ubuntu https://imgur.com/JY5tHqr
```

<exercise name="Exercise 2.1">

  *Exercises in part 2 should be done using podman-compose*

  Without a command `devopspodmanuh/simple-web-service` will create logs into its `/usr/src/app/text.log`.

  Create a podman-compose.yml file that starts `devopspodmanuh/simple-web-service` and saves the logs into your
  filesystem.

  Submit the podman-compose.yml, make sure that it works simply by running `podman-compose up` if the log file exists.


</exercise>

## Web services in podman-compose ##

Compose is really meant for running web services, so let's move from simple binary wrappers to running a HTTP service.

<https://github.com/jwilder/whoami> is a simple service that prints the current container id (hostname).

```console
$ podman container run -d -p 8000:8000 jwilder/whoami
  736ab83847bb12dddd8b09969433f3a02d64d5b0be48f7a5c59a594e3a6a3541
```

Navigate with a browser or curl to localhost:8000, they both will answer with the id.

Take down the container so that it's not blocking port 8000.

```console
$ podman container stop 736ab83847bb
$ podman container rm 736ab83847bb
```

Let's create a new folder and a podman-compose file `whoami/podman-compose.yml` from the command line options.

```yaml
version: '3.8'

services:
    whoami:
      image: jwilder/whoami
      ports:
        - 8000:8000
```

Test it:

```console
$ podman-compose up -d
$ curl localhost:8000
```

Environment variables can also be given to the containers in podman-compose.

```yaml
version: '3.8'

services:
  backend:
      image:
      environment:
        - VARIABLE=VALUE
        - VARIABLE
```

<exercise name="Exercise 2.2">

  Read about how to add command to podman-compose.yml from the [documentation](https://docs.podman.com/compose/compose-file/compose-file-v3/#command).

  The familiar image `devopspodmanuh/simple-web-service` can be used to start a web service.

  Create a podman-compose.yml and use it to start the service so that you can use it with your browser.

  Submit the podman-compose.yml, make sure that it works simply by running `podman-compose up`

</exercise>

<exercise name="Exercise 2.3">

  <b style="color:firebrick;">This exercise is mandatory</b>

  As we saw previously, starting an application with two programs was not trivial and the commands got a bit long.

  In the previous part we created Dockerfiles for both [frontend](https://github.com/podman-hy/material-applications/tree/main/example-frontend) and [backend](https://github.com/podman-hy/material-applications/tree/main/example-backend).
  Next, simplify the usage into one podman-compose.yml.

  Configure the backend and frontend from part 1 to work in podman-compose.

  Submit the podman-compose.yml

</exercise>
