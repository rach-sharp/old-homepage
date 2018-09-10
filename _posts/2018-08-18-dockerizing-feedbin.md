---
layout: post
title: Dockerizing Feedbin
categories: [docker]
---

[Feedbin](https://github.com/feedbin/feedbin){:target="_blank"} is the best open source
RSS Reader out there, but if you're trying to set it up on your own server, the installation
and configuration process is fiendish. It has a lot of undocumented configuration options
which aren't clearly optional or mandatory. It has a lot of components, including a separate
Feedbin Refresher service, Redis, an image proxy, and others, so structuring it as a single
Docker container is impractical. For my own homelab, I've built:
 
 - a Docker image for the core Feedbin code
 - a Docker image for the Feedbin Refresher code
 - a docker-compose file with all the components included
 
to make the deployment process super simple - a lot of the Feedbin config was just there
to connect all the separate components. Hopefully easier installation for developers looking to host their own instances will help to get more people involved with the Feedbin project.

You can get it from [the project page](https://github.com/rachel-sharp/freedbin){:target="_blank"}!
