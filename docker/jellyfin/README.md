# Service
- [Jellyfin](https://jellyfin.org/)

*Jellyfin is the Free Software Media System that puts you in control of managing and streaming your media. There are no strings attached, no premium licenses or features, and no hidden fees.*

# Table of contents

- [Service](#service)
- [Table of contents](#table-of-contents)
- [Compose file](#compose-file)
- [Explanation of the compose file:](#explanation-of-the-compose-file)
  - [General information about the container](#general-information-about-the-container)
  - [Volumes](#volumes)
  - [Transcoding (Intel Integrated GPU ONLY)](#transcoding-intel-integrated-gpu-only)
  - [Network](#network)
- [Run the container](#run-the-container)
- [Service Configuration](#service-configuration)

# Compose file

<details>
<summary>Click to expand</summary>

![compose.yml](./compose.yml)
</details><br>

In this infrasctrucutre, we will use [Jellyfin](https://jellyfin.org/) as a streaming service. It is a completely free and open source alternative to Plex with a modern interface.<br>
You can find client applications for:
- [Android](https://play.google.com/store/apps/details?id=org.jellyfin.mobile)
- [iOS](https://apps.apple.com/us/app/jellyfin-mobile/id1480192618?mt=8)
- [Windows and Linux](https://github.com/jellyfin/jellyfin-media-player/releases)
- [Android TV](https://play.google.com/store/apps/details?id=org.jellyfin.androidtv)

You can also play your favourite movies and series on your browser by accessing your server IP or domain name.

# Explanation of the compose file:

## General information about the container
<details>
<summary>Click to expand</summary>

```yml
---
services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
#    user: 1002:1003
    restart: always
[...]
```
</details><br>

The first `jellyfin` defines the name of the service. This is the equivalent as the hostname of the container inside the docker network.<br>
We are using the official image `jellyfin/jellyfin` from Docker Hub. With no tags, the service will always use the latest version of the image.<br>
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
      - /path/to/your/media:/media
[...]
```
</details><br>

The two first volumes are used to synchronize the time of the container with the host. This is useful for logs and other time-related operations.<br>
Then, we define where we want to store the configuration files of the service. This folder will contain information about the users and how Jellyfin is configured.<br>
Finally, we define where we want to store the media files. This folder will contain all the movies, series, music, etc. that you want to stream. It is recommended to use a dedicated disk for this folder.

## Transcoding (Intel Integrated GPU ONLY)
> 💡 *If you want more info on transcoding configuration or for other GPU, check the [official documentation](https://jellyfin.org/docs/general/administration/hardware-acceleration.html)*

> 💡 *You may need to install drivers for your GPU, check the documentation for more info.*
<details>
<summary>Click to expand</summary>

```yml
[...]
    devices:
      - /dev/dri/renderD128:/dev/dri/renderD128
    group_add:
      - "110"
      - "44"
[...]
```
</details>

We are using the integrated GPU of the CPU for transcoding. We need to add the device `/dev/dri/renderD128` to the container and add the group `110` and `44` to the container. This is the group of the GPU and the video group.

## Network
<details>
<summary>Click to expand</summary>

```yml
[...]
    networks:                #1
      - docker_net
    ports:
      - 8096:8096

networks:                   #2
  docker_net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.69.0.0/24
          gateway: 172.69.0.1
```
</details>

For this part, will first look at `networks: #2`:
 - We define a network called `docker_net` with the driver `bridge`. This means that the containers connected to this network will be able to communicate with the WAN and the LAN.
 - We define a subnet for the network. This is the IP range that the containers will use to communicate with each other. The gateway will be the IP address of the "virtual" docker network card on the host.
<br>

Then, we ask the container to use the network `docker_net` with `networks: #1` and we expose the port `8096` of the container to the host.

> ⚠️ *At the end of the configuration, you will have to comment or remove the `ports` section as we don't want to expose any ports of the host (we will use a Reverse Proxy instead).*


# Run the container
To run the container, you can use the following command:
```bash
docker-compose up -d
```
Alternatively, you can specify the path to the compose file:
```bash
docker-compose -f /path/to/your/compose.yml up -d
```
You'll be able to access the web interface of Jellyfin by going to `http://your-server-ip:8096` or `http://your-domain-name:8096`.

# Service Configuration