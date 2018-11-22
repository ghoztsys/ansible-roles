# consul_template

[Consul Template](https://github.com/hashicorp/consul-template) provides a convenient way to modify the file system whenever values in [[Consul]] changes. For instance, when new app nodes are discovered, [[HAProxy]] needs to know about them, add them to its config and restart so traffic can be directed to these nodes.

This role sets up the target hosts with Consul Template and the logic for updating HAProxy when Consul discovers new nodes.

## Prerequisites

- Ubuntu 14.04+ with `systemd`

## Created Files

1. `/etc/systemd/system/consul-template.service`
2. `/etc/consul-template.d/consul_template.cfg`
3. `/usr/local/bin/consul-template`
4. `/opt/consul-template/haproxy.ctmpl` (when used with [[HAProxy]])
