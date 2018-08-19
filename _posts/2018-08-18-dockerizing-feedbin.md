---
layout: post
title: Dockerizing Feedbin
categories: [docker]
---

[Feedbin](https://github.com/feedbin/feedbin){:target="_blank"} is the best open source RSS Reader out there, but if you're trying to set it up on your own server, the installation and configuration process is fiendish. It has a lot of separate components, including a separate Feedbin Refresher service, an image proxy and others, which make structuring it as a single Docker container impractical. It has a lot of undocumented configuration options which aren't clearly optional or mandatory. For my own homelab, I've built a Docker image and docker-compose file to make the deployment process super simple - the isolated networks that Docker can provide mean that for a lot of the Feedbin config parameters, sensible defaults can be good enough for most users. The database is automatically set up for you. Hopefully easier installation for developers looking to host their own instances will help get more people involved with the Feedbin project.

You can get it from [the project page](https://github.com/rachel-sharp/freedbin){:target="_blank"}! For the moment these changes are in a fork of the original project, so when moving fast and breaking things, production-ready Feedbin won't be impacted.
