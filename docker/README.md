# docker

This role installs Docker on the target hosts. In Sybl, all appllications are containerized running in network mode `host`. The performance impact of Docker is highlighted [here](http://stackoverflow.com/questions/21889053/what-is-the-runtime-performance-cost-of-a-docker-container).

## Prerequisites

- Ubuntu 14.04+ with `systemd`