# `consul`

[Consul](https://www.consul.io) is used for service discovery within and between datacenters, in situations where there will be a cluster of Consul servers per datacenter (the Consul documentation recommends that you have either 3 or 5 Consul servers running in each datacenter to avoid data loss in the event of a server failure. An odd number of servers is necessary to avoid stalemate issues during leader elections). Consul servers are the component that do the heavy lifting. They store information about services and key/value information. Every other node that provides a service to Consul will have a Consul agent installed. These Consul agents will not be configured as servers.

This role sets up the target hosts with Consul as either servers or clients. It also sets up [Consul Template](https://www.hashicorp.com/blog/introducing-consul-template.html) on all client nodes. After the role finishes its job, every Consul node (server and client included) will be communicating with each other.

To verify that nodes are communicating with each other, simply SSH into any node and do:

```
$ consul members
```

This should yield a list of nodes (server and client) within this datacenter that are Consul-aware. For more details add the `-detailed` flag at the end of the command.

## OS

Ubuntu 14.04+ with `systemd`

## Created Files

1. `/usr/local/bin/consul`
2. `/etc/systemd/system/consul.service`
3. `/opt/consul/deploy`
4. `/etc/consul.d/config.json`

## Steps

1. Installs Consul, managed by `root:root`
2. Creates directory for storing config, log, and data files
3. Deploys `systemd` script
4. Deploys configuration file depending on agent type (i.e. server or client)
5. Installs Consul Template if agent type is client
6. Runs Consul as a `systemd` service

# Ports

- `8500`: Default port for HTTP API
- `8600`: Default port for DNS interface

## Usage

Once nodes are linked via Consul, you can lookup information stored in Consul from **any** node within the cluster. This can be done via the [HTTP API](https://www.consul.io/docs/agent/http.html) (with `curl`) running on port 8500 or the [DNS interface](https://www.consul.io/docs/agent/dns.html) (with `dig`) running on port 8600.

To get a list of all nodes using the HTTP API:

```
$ curl localhost:8500/v1/catalog/nodes
```

To get a list of all nodes belong to a service using the HTTP API:

```
$ curl http://localhost:8500/v1/catalog/service/<service_name>
```

To query a specific node via the DNS interface:

```
$ dig @127.0.0.1 -p 8600 <node_name>.node.consul
```

To query a specific service via the DNS interface:

```
$ dig @127.0.0.1 -p 8600 <service_name>.service.consul
```

To query a specific service filtered by node tag via the DNS interface:

```
$ dig @127.0.0.1 -p 8600 <tag>.<service_name>.service.consul
```

You can also use the DNS API to retrieve the entire address/port pair as a SRV record:

```
$ dig @127.0.0.1 -p 8600 <service_name>.service.consul SRV
```

Consul servers are by default registered with the service name `consul`, hence `consul.service.consul`.

## Web UI

Consul comes with a web UI pre-bundled in its binary. Simply enable it in the config file by setting `ui: true`. You can access it via its HTTP API port (defaults to 8500). By default the UI is only accessible from within the server. To access it from another machine, create an SSH tunnel to the Consul server:

```
$ ssh -N -f -L 8500:localhost:8500 ubuntu@<consul_server_ip>
```

Now you can access the UI via `localhost:8500`. To disable the SSH tunnel, find the PID of the running task and kill it:

```
$ ps aux | grep 8500
$ kill <pid>
```
