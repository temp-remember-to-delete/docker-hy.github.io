---
path: "/getting-started"
title: "Getting started"
hidden: false
information_page: true
sidebar_priority: 4000
---

The course material is meant to be read sequentially, part by part, from start to finish. To get a passing grade you have to complete each exercise. Except one exercise can be skipped for each part. Some of the exercises are marked as mandatory and the mandatory exercises can not be skipped. The exercises are placed in the material in such a way that you will have learned the necessary skills from the material prior to each given exercise. You can do the exercises as you're going through the material.

The course material does not consider all operating systems and as such some instructions may lack platform-specific details. But we've had students who've made it through the material with varying machines. Please make a [pull request](/contributing) to the course material if you find any mistakes or wish to add something. You can also [contribute](/contributing) through GitHub.

### Completing course

The course is composed of 3 parts. You should read through this page before starting with part 1. This part includes installation instructions and a serious warning. The parts should take 5-25 hours each to complete.

Here are additional instructions for completion after each part and at the end of this page.

### Grading

Passing this course requires you to do the exercises for each part. This means every exercise, except one for each part. Some of the exercises are mandatory and can not be skipped. This course is worth 1-3 credits depending on the completed parts.
Completing part 1 gives you 1 credit. Completing parts 1 and 2 is worth 2 credits. Completing all of the parts will grant you 3 credits.

### Learning objectives

Part 1

Can explain what images and containers are and how they're related.
Can build images with Podman for existing projects and run them.

Part 2

Can manage complex multi-container applications with podman-compose.

Part 3

Can optimize images sizes and security for production.
Knows why podman-compose is not an optimal production solution and what is.

### Course has multiple pages

The DevOps with Podman course is split into 3 parts:

- DevOps with Podman ([TKT21036](https://studies.helsinki.fi/opintotarjonta/cur/otm-43341e49-dfae-4899-95d6-a4da1c5ec47b))
- DevOps with Podman: podman-compose ([TKT21037](https://studies.helsinki.fi/opintotarjonta/cur/otm-29ab3dee-9b30-4fbf-b7e7-dcbecd7218c5))
- DevOps with Podman: security and optimization ([TKT21038](https://studies.helsinki.fi/opintotarjonta/cur/otm-3c890ee9-7598-4b5c-9922-2c26c0c3235b))

You can click the links to visit each of the subcourse pages. However, all the details should be found on this page. If something is missing or unclear after reading this page, please contact jami.kousa@helsinki.fi or get in touch through Discord.

### Discord

This course has a Discord group where we discuss everything about the course. Support is available almost 24/7, with the discussion being in both English and Finnish.

Join our discord group [here](https://study.cs.helsinki.fi/discord/join/podman).

**All** inappropriate, degrading or discriminating comments on the channel are prohibited and will lead to action taken against the commenter.

# Warning: Before installing Podman

Containers leverage the power of your own operating system. As such **by default** any containerized application, or user who has external access to your container, would have super user privileges to your computer.

I will try my best to alert you of potential risks as we encounter them, but due to the structure of the course we will focus on security in part 3.

Please keep this in mind as you move through the installation and exercises. If you ever feel unsure about what you're doing, come to the channel and have a chat with us.

## Installing Podman

Use the official documentation to find download instructions for podman-ce for the platform of your choice:

[Linux](https://podman.io/getting-started/installation#installing-on-linux)

[MacOS](https://podman.io/getting-started/installation#macos)

[Windows](https://podman.io/getting-started/installation#windows)

Confirm that Podman installed correctly by opening a terminal and running `podman -v` to see the installed version.

## Installing podman-compose

Linux - [podman-compose GitHub repository](https://github.com/containers/podman-compose). 

Mac - there is the [MacPorts](https://ports.macports.org/port/podman-compose/) and [pip installation](https://ports.macports.org/port/podman-compose/)

Windows - Best to follow the Linux instructions, whilst using WSL2.

Confirm that podman-compose installed correctly by opening a terminal and running `podman-compose -v` to see the installed podman-compose version.

<text-box name="Rootful vs Rootless" variant="hint">

There is no need to add oneself to the docker group, since podman can be run rootless, do note that if you wish to use the _well-known ports_ (0 to 1023) you will need to use sudo, this comes with an even bigger caveat, which is that, rootful and rootless are two different uses, so in order to run the same image, it has to be downloaded for root as well.

Keep in mind that if you use sudo, containers give you super user access to the computer.

</text-box>

## Installing docker-compose

Linux - For docker-compose, see the [official Docker documentation](https://docs.docker.com/compose/install/), for download/installation instructions.

Mac - `docker-compose` might be easiest to use. There are the [official releases](https://github.com/docker/compose/releases) as well as [Homebrew Formulae](https://formulae.brew.sh/formula/docker-compose#default). To get it to function follow [this guide](https://gist.github.com/kaaquist/dab64aeb52a815b935b11c86202761a3). 

Windows - Best to follow the Linux instructions, whilst using WSL2.

## Deadline

The deadline was in May of 2022 and there may not be a new course.

## General guidance

_Do not alter the code of the projects, unless by pull-requests to the original projects_

You do not need to touch ruby, java, js or python code during this course. You may have to read their error messages.

Visit the Discord channel if you are stuck!

## How to return exercises

Make a repository to GitHub and publish your solutions in clearly ordered files / folders.
If you need help publishing using Git you should refer to their [guide](https://guides.github.com/activities/hello-world/). Make sure that the repository is available to me, either by using a public repository or a private repository and adding Jakousa as a collaborator.

Most of the exercises will be focused on a Podmanfile and/or podman-compose.yml. In those cases, submitting the file is enough. In other cases, a picture or copy-paste from your command line or a link to Podman Hub and/or project inside the repository is enough. For the command line exercises at start the command `script` ([https://linux.die.net/man/1/script](https://linux.die.net/man/1/script)) may be helpful.

When you have completed a part, use the [submission application](https://studies.cs.helsinki.fi/stats/courses/podman2022) to mark your exercises. You can **not** edit a submission, so make sure you have completed enough exercises for a part before submitting.

## Completing

The certificate is available from the small icon beneath your submissions!

After you have returned all of the required exercises and wish to end your course completion and want the ECTS credits press the following button in the submission application (ignore the message about an "exam" as there is no exam in this course):

<img src="./img/incomplete_course.png">

After that, ensure that the application has the message "Course marked as completed" and the date.

<img src="./img/completed_course.png">
