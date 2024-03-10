---
title: Docker Commands
date: 2024-03-10 13:00:00 +0000
categories: [geek, web]
tags: [web, n100, docker]     # TAG names should always be lowercase
---

## Docker

I have started using docker to run my Home Assistant and Flight Tracking systems.

This setup is documented here: <{{ site.url }}\assets\posts\Flight-Tracking\>

This pages captures some of the most useful commands.


## Commands

List all Images:

> docker images 

Pull image for services with a build section in the compose file:

>docker compose pull

Start all containers in the compose file:

> docker compose up -d

Removes everything not being used by a running container

> docker system prune --volumes 

View Docker Logs (-f returns logs for a specific container)

> docker logs -f watchtower



	









