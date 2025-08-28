# Docker

Necesarry docker images to easily deploy an Arrowhead Local Cloud are aviable in Docker Hub. What you need is to create your own [docker-compose](https://docs.docker.com/compose/) setup that fulfills your requirements the most.

## Startegies

There are multiple proper approches that are requiring different docker compose solutions. Most common approaches:

* [Aggregated](./docker/docker_aggregated.md): Compose all systems and their databases at once. A complete Local Cloud can deployed to a single host machine.
* [Coupled](./docker/docker_coupled.md): Compose a system with its database. Local Cloud components can be separated to different host machines.
* [Dispersed](./docker/docker_dispersed.md): Compose every system and every database independently. Docker containers can be distributed between host machines as desired. 

## Not familiar with docker?

**Quick review**

<iframe width="756" height="415" src="https://www.youtube.com/embed/QEzbZKtLi-g?si=Lh7xgx9-XJeBvNng" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Dive deep** with the [official docs](https://www.docker.com/get-started/).