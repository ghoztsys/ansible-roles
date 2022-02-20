# `nomad`

[Nomad](https://www.nomadproject.io) is used to schedule jobs in server clusters. It is responsible for scheduling jobs in the API node cluster, running Docker containers containing the API app. See official [introduction](https://www.nomadproject.io/intro/) and [documentation](https://www.nomadproject.io/docs/index.html) for more details.

This role installs Nomad on all nodes that need to be scheduled. Nomad, like Consul, has two agent types: server and client. Nomad servers are responsible for doing the heavylifting and schedule jobs on the clients, while the clients simply execute the jobs given to them by the servers. It is recommended to have multiple Nomad servers running in a datacenter to prevent data loss should the lone server fails, and an odd number of servers is required to avoid stalemate situations when electing a server leader.

To check connected server nodes:

```
$ nomad server-members
```

To check connected client nodes:

```
$ nomad node-status
```

## OS

Ubuntu 14.04+ with `systemd`

## Created Files

1. `/usr/local/bin/nomad`
2. `/etc/nomad.d/nomad.cfg`
3. `/var/nomad/`
4. `/opt/nomad/jobs/api.nomad`
5. `/opt/nomad/tmp/logs/`
6. `/opt/nomad/tmp/uploads/`
7. `/etc/systemd/system/nomad.service`

## Steps

1. Install Nomad
2. Deploy `systemd` script
3. Deploy job files
4. Start Nomad service

## Ports

- `4646`: HTTP API
- `4647`: RPC
- `4648`: Serf

## Usage

Nomad schedules [jobs](https://www.nomadproject.io/docs/job-specification/index.html) on its clients. Jobs should be located in `/opt/nomad` and named `<job_name>.nomad`. Job names should be unique.

To dry-run a job:

```
$ nomad plan <job_name>.nomad
```

To proceed with scheduling a job:

```
$ nomad run <job_name>.nomad
```

To stop a scheduled job:

```
$ nomad stop <job_name>
```

To inspect the status of a scheduled job:

```
$ nomad status <job_name>
```

To inspect an allocation (add `-stats` flag for more details):

```
$ nomad alloc-status <alloc_id>
```

To inspect the logs of a task:

```
$ nomad logs <alloc_id> <task_name>
```

## Permission Issues

In the case the Nomad does not have permission to access Docker, try the following:

```sh
$ sudo usermod -aG docker $USER
$ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
$ sudo chmod g+rwx "/home/$USER/.docker" -R
```