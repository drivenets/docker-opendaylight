# OpenDaylight controller Docker image

[![](https://img.shields.io/docker/pulls/sfuhrm/opendaylight?style=plastic)](https://hub.docker.com/repository/docker/sfuhrm/opendaylight/general)
[![](https://img.shields.io/docker/image-size/sfuhrm/opendaylight/latest?style=plastic)](https://hub.docker.com/repository/docker/sfuhrm/opendaylight/general)
[![](https://img.shields.io/microbadger/layers/sfuhrm/opendaylight?style=plastic)](https://hub.docker.com/repository/docker/sfuhrm/opendaylight/general)
[![](https://img.shields.io/docker/cloud/automated/sfuhrm/opendaylight?style=plastic)](https://hub.docker.com/repository/docker/sfuhrm/opendaylight/general)
[![](https://img.shields.io/docker/cloud/build/sfuhrm/opendaylight?style=plastic)](https://hub.docker.com/repository/docker/sfuhrm/opendaylight/general)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

OpenDaylight is a SDN controller. It can be used to control and orchestrate a network of switches.

OpenDaylight supports many southbound protocols and can be integrated in Openstack or even Kubernetes.

## Relevant links

The sites relevant for this docker image:

- [Github docker-opendaylight](https://github.com/sfuhrm/docker-opendaylight): The Github repository.
- [The Docker Hub repository with version specific images](https://hub.docker.com/repository/docker/sfuhrm/opendaylight)

More reading can be found here:

- [OpenDaylight Home](https://www.opendaylight.org/)
- [OpenDaylight Aluminium Documentation](https://docs.opendaylight.org/en/stable-aluminium/)

## Supported tags

- latest
- 0.13.1
- 0.13.0
- 0.12.2
- 0.12.1
- 0.12.0
- 0.11.4
- 0.11.3
- 0.11.2
- 0.11.1
- 0.11.0
- 0.10.3
- 0.10.2
- 0.10.1
- 0.10.0
- 0.9.3
- 0.9.2
- 0.9.1
- 0.9.0
- 0.8.4
- 0.8.3
- 0.8.2
- 0.8.1
- 0.8.0

## What's inside

- Based on [`openjdk:11-jre-slim-buster`](https://hub.docker.com/_/openjdk) for OpenDaylight 10+ and `openjdk:8-jre-slim-buster` for older versions.
- No modules installed (you can install modules at boot time by setting the [`KARAF_FEATURES`](#customization-via-env-variables) env variable).
- Exposed TCP ports:
  - 6633 Openflow,
  - 8101 Karaf CLI via SSH (see below),
  - 8181 RESTCONF

## How to use it

### Run container

`docker run -d -p 6633:6633 -p 8101:8101 -p 8181:8181 --name=opendaylight sfuhrm/opendaylight`

### Access OpenDaylight Karaf CLI

#### Using SSH

To access the Karaf CLI, issue the command

`ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -p 8101 karaf@localhost`

The default SSH credentials are:

- Username: `karaf`
- Password: `karaf`

Note: `StrictHostKeyChecking` and `UserKnownHostsFile` file is turned off because different container invocations will have different host keys.

#### Using Docker

An interactive Karaf shell can also be spawned with the following command line:

```bash
docker exec -ti opendaylight bin/client
```

where `opendaylight` is the name of the running container.

## Customization via ENV variables

The following environment variables can be set to customize the started
server:

| Variable                | Default   |  Possible Values |   |
|-------------------------|-----------|------------------|---|
| `KARAF_INTERACTIVE`     | `0`       | `0`,`1`              | Set to `1` to run Karaf interactive, meaning that the docker instance will be interactive. As an alternative, you can connect in a running server instance using `docker exec -ti opendaylight /odl/bin/client`  |
| `KARAF_FEATURES`        |           | string           | Comma separated features to load on boot of OpenDaylight. Example: `odl-yangtools-parser,odl-yangtools-parser-api` |
| `KARAF_USER`            | `karaf`   | string           | The user name of the Karaf service.  |
| `KARAF_PASSWORD`        | `karaf`   | string           | The password of the Karaf service.  |
| `ODL_LOGLEVEL`          | `INFO`    | `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL` |  The log level to log runtime information to the Docker console with.  |
| `ODL_ADMIN_PASSWORD`    | `admin`   | string           |  The password of the AAA admin user.  |

Example:

```bash
docker run  -e KARAF_USER=michael -e KARAF_PASSWORD=knight -p 6633:6633 -p 8101:8101 -p 8181:8181 --name=opendaylight sfuhrm/opendaylight:latest
```

## Credits

This is a fork of [Guillaume Lefevres](https://github.com/guillaumelfv/docker-opendaylight)
repository, but nothing of Guillaumes work is left.
