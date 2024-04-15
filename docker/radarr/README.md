# Service
- [RADARR](https://radarr.video/)

*RADARR is a service that allows you to automatically download movies from your favorite indexers. It can also keep track of all your movies and notify you when a new one is available.*

# Table of contents

- [Service](#service)
- [Table of contents](#table-of-contents)
- [Compose file](#compose-file)
- [Explanation of the compose file:](#explanation-of-the-compose-file)
  - [General information about the container](#general-information-about-the-container)
  - [Volumes](#volumes)
  - [specific options](#specific-options)
  - [Network](#network)
- [Run the container](#run-the-container)
- [Service Configuration](#service-configuration)


# Compose file

<details>
<summary>Click to expand</summary>

![compose.yml](./compose.yml)
</details><br>

In this infrasctructure, we will use [RADARR](https://radarr.video/) as a service. <br>


# Explanation of the compose file:

## General information about the container
<details>
<summary>Click to expand</summary>

```yml
---
services:

```
</details><br>

The first `SERVICE` defines the name of the service. This is the equivalent as the hostname of the container inside the docker network.<br>
We are using the official image `SERVICE/SERVICE` from Docker Hub. With no tags, the service will always use the latest version of the image.<br>
We define a `container_name` so it's easier to manage containers on our host.<br>
Finally, we define the `restart` policy to `always` so the container will always restart if it stops or if the host reboots.<br>

## Volumes
<details>
<summary>Click to expand</summary>

```yml
[...]
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /etc/localtime:/etc/timezone:ro
      - /path/to/your/config:/config
      - 
[...]
```
</details><br>

The two first volumes are used to synchronize the time of the container with the host. This is useful for logs and other time-related operations.<br>
Then, we define where we want to store the configuration files of the service. This folder will contain information about the users and how Jellyfin is configured.<br>


## specific options
> 💡 *If you want more info on transcoding configuration or for other GPU, check the [official documentation](https://jellyfin.org/docs/general/administration/hardware-acceleration.html)*

> 💡 *You may need to install drivers for your GPU, check the documentation for more info.*
<details>
<summary>Click to expand</summary>

```yml
[...]
[...]
```
</details>

## Network
<details>
<summary>Click to expand</summary>

```yml
[...]
    networks:                #1
      - docker_net
    ports:
      - 
networks:
  docker_net:
    external:
      name: jellyfin_docker_net
```
</details>

Then, we ask the container to use the network `docker_net` with `networks: #1` and we expose the port `PORTS` of the container to the host.

- 80:80   is the default port for HTTP
- 

> ⚠️ *At the end of the configuration, you will have to comment or remove the `- 81:81` section as we don't want to expose the WebGUI ports of the host.*

The part `networks: #2` is different from the [`Jellyfin` service](../jellyfin/compose.yml). We are using an external network called `jellyfin_docker_net`. This network is created in the `compose.yml` file of the `Jellyfin` service. This way, the `Jellyfin` service and the `Proxy` service will be able to communicate with each other and also with all the other services in the same network.

# Run the container
To run the container, you can use the following command:
```bash
docker-compose up -d
```
Alternatively, you can specify the path to the compose file:
```bash
docker-compose -f /path/to/your/compose.yml up -d
```
You'll be able to access the web interface of SERVICE by going to `http://your-server-ip:PORT` or `http://your-domain-name:PORT`.

# Service Configuration

Coming soon!