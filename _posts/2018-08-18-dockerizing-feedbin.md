---
layout: post
title: Dockerizing Feedbin
categories: [docker]
---

[Feedbin](https://github.com/feedbin/feedbin){:target="_blank"} is the best open source RSS Reader out there, but if you're trying to set it up on your own server, the installation and configuration process is fiendish. It has a lot of undocumented configuration options which aren't clearly optional or mandatory. It has a lot of separate components, including a separate Feedbin Refresher service, Redis, an image proxy, and others, which make structuring it as a single Docker container impractical.  For my own homelab, I've built a Docker image and docker-compose file to make the deployment process super simple - structuring the external services like Redis as private sidecar docker containers cuts out a lot of the Feedbin config parameters. Hopefully easier installation for developers looking to host their own instances will help to get more people involved with the Feedbin project.

You can get it from [the project page](https://github.com/rachel-sharp/freedbin){:target="_blank"}! For the moment these changes are in a fork of the original project, so when moving fast and breaking things, production-ready Feedbin won't be impacted.
