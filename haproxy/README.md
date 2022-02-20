# `haproxy`

[HAProxy](http://www.haproxy.org/) is the load balancer of choice. It collects health check statistics which can be viewed at `http://<haproxy_server_ip>:1936/stats`. It is dependent on [[Consul Template]].

## OS

Ubuntu 14.04+ with `systemd`

## Created Files

1. `/etc/haproxy/haproxy.cfg` (rendered by [[Consul Template]])
