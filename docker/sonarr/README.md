# Service
- [SONARR](https://sonarr.tv/)

*SONARR is a service that allows you to automatically download movies from your favorite indexers. It can also keep track of all your movies and notify you when a new one is available.*

# Table of contents

- [Service](#service)
- [Table of contents](#table-of-contents)
- [Compose file](#compose-file)
- [Explanation of the compose file:](#explanation-of-the-compose-file)
  - [General information about the container](#general-information-about-the-container)
  - [Volumes](#volumes)
  - [Specific options](#specific-options)
  - [Network](#network)
- [Run the container](#run-the-container)
- [Service Configuration](#service-configuration)


# Compose file

<details>
<summary>Click to expand</summary>

![compose.yml](./compose.yml)
</details><br>

In this infrasctructure, we will use [SONARR](https://sonarr.tv/) as a service. <br>

# Explanation of the compose file:

## General information about the container
<details>
<summary>Click to expand</summary>

```yml
services:
  sonarr:
    image: ghcr.io/hotio/sonarr
    container_name: sonarr
    restart: always
[...]
```
</details><br>

The first `sonarr` defines the name of the service. This is the equivalent as the hostname of the container inside the docker network.<br>
We are using the official image `ghcr.io/hotio/sonarr` from GitHub repositoryes. With no tags, the service will always use the latest version of the image.<br>
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
      - /path/to/your/downloads:/data/downloads
      - /path/to/your/media/series:/data
[...]
```
</details><br>

The two first volumes are used to synchronize the time of the container with the host. This is useful for logs and other time-related operations.<br>
Then, we define where we want to store the configuration files of the service. This folder will contain information about the users and how Jellyfin is configured.<br>
The third volume corresponds to the folder where [qBittorrent service](../qbittorrent/README.md) stores the downloaded files. You **HAVE TO** enter the same path as the one you entered in the `qbittorrent` service.<br>
Finally, the last volume is the folder where the service will store the series. I recommend using the same folder as the one used by the `Jellyfin` service. This way, the `Jellyfin` service will be able to access the series downloaded by the `Sonarr` service.


## Specific options

> 🔴 *There is no specific option for this service*


## Network
<details>
<summary>Click to expand</summary>

```yml
[...]
    networks:
      - docker_net
    ports:
      - "7878:7878"

networks:
  docker_net:
    external:
      name: jellyfin_docker_net
```
</details>

Then, we ask the container to use the network `docker_net` with `networks: #1` and we expose the port `7878` of the container to the host.

- 7878:7878   is the default port for Sonarr's WebGUI.

> ⚠️ *At the end of the configuration, you will have to comment or remove the entire `ports:` section as we don't want to expose the WebGUI ports of the host.*

The part `networks: #2` is different from the [`Jellyfin` service](../jellyfin/compose.yml). We are using an external network called `jellyfin_docker_net`. This network is created in the `compose.yml` file of the `Jellyfin` service. This way, the `Jellyfin` service and the `Sonarr` service will be able to communicate with each other and also with all the other services in the same network.

# Run the container
To run the container, you can use the following command:
```bash
docker-compose up -d
```
Alternatively, you can specify the path to the compose file:
```bash
docker-compose -f /path/to/your/compose.yml up -d
```
You'll be able to access the web interface of SERVICE by going to `http://your-server-ip:7878` or `http://your-domain-name:7878`.

# Service Configuration

Coming soon!