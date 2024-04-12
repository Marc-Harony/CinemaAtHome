# Service
- [qBitTorrent](https://www.qbittorrent.org/)
- [Gluetun](https://github.com/qdm12/gluetun)

*qBitTorrent is a free and open-source BitTorrent client. In this project you'll be able to connect to your qBitTorrent client using your browser and download torrents directly to your server.*
*Gluetun is a VPN client that will allow you to connect to a VPN provider and download torrents anonymously.*

# DISCLAIMER

This project is for educational purposes only. I do not condone the use of this project for illegal activities. Downloading copyrighted material without permission is illegal and can have serious consequences.

The goal of this project is to see what it's possible to do with Docker's Network and how to use it to connect services together.

# Table of contents

- [Service](#service)
- [DISCLAIMER](#disclaimer)
- [Table of contents](#table-of-contents)
- [Compose file](#compose-file)
- [Explanation of the compose file:](#explanation-of-the-compose-file)
  - [qBitTorrent service](#qbittorrent-service)
    - [General information about the container](#general-information-about-the-container)
    - [Volumes](#volumes)
    - [Connection with the VPN](#connection-with-the-vpn)
    - [Network](#network)
  - [Gluetun service](#gluetun-service)
- [Run the container](#run-the-container)
- [Service Configuration](#service-configuration)


# Compose file
<details>
<summary>Click to expand</summary>

![compose.yml](./compose.yml)
</details><br>

In this infrasctrucutre, we will use [qBitTorrent](https://www.qbittorrent.org/) as a Bittorrent client that will be running behind a dockerized VPN client [Gluetun](https://github.com/qdm12/gluetun). <br>


# Explanation of the compose file:

## qBitTorrent service


### General information about the container
<details>
<summary>Click to expand</summary>

```yml
services:
  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent
    container_name: qbittorrent
    restart: always
[...]
```
</details><br>

The first `qbittorrent` defines the name of the service. This is the equivalent as the hostname of the container inside the docker network.<br>
We are using the official image `lscr.io/linuxserver/qbittorrent` from GitHub repositories. With no tags, the service will always use the latest version of the image.<br>
We define a `container_name` so it's easier to manage containers on our host.<br>
Finally, we define the `restart` policy to `always` so the container will always restart if it stops or if the host reboots.<br>

### Volumes
<details>
<summary>Click to expand</summary>

```yml
[...]
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /etc/localtime:/etc/timezone:ro
      - /path/to/your/config:/config
      - /path/to/your/downloads:/data/downloads
[...]
```
</details><br>

The two first volumes are used to synchronize the time of the container with the host. This is useful for logs and other time-related operations.<br>
Then, we define where we want to store the configuration files of the service. This folder will contain information about the users and how qBitTorrent is configured.<br>
Finally, we define where we want to store the downloaded files. This folder will contain the files that you download with qBitTorrent.<br>


### Connection with the VPN

<details>
<summary>Click to expand</summary>

```yml
[...]
    network_mode: "service:gluetun"
    depends_on:
      - gluetun  # Déclarez la dépendance à gluetun
    environment:
      - PUID=1000
      - PGID=1000
      - WEBUI_PORT=8080
      - TORRENTING_PORT=6881
[...]
```
</details>

The `network_mode` is set to `service:gluetun`. This means that the container will use the network of the service `gluetun` that will makes it to run "behind" the VPN.<br>
The `depends_on` is used to declare a dependency to the service `gluetun`. This means that the service `qbittorrent` will only start if the service `gluetun` is running.<br>
The `environment` variables are used to configure the service. `PUID` and `PGID` are used to define the user and group ID of the user that will run the service. `WEBUI_PORT` is used to define the port that the web interface will be available on. `TORRENTING_PORT` is used to define the port that the service will use to download torrents.<br>

### Network

> 🔴 *The network is configured in the [previous part](#connection-with-the-vpn)

## Gluetun service

The config I'm using is for the provider CyberghostVPN. Check the [Gluetun documentation](https://github.com/qdm12/gluetun-wiki) to get information about the configuration for your provider.

# Run the container
To run the container, you can use the following command:
```bash
docker-compose up -d
```
Alternatively, you can specify the path to the compose file:
```bash
docker-compose -f /path/to/your/compose.yml up -d
```
You'll be able to access the web interface of qBitTorrent by going to `http://your-server-ip:8080` or `http://your-domain-name:8080`.

You can also check that the VPN is correctly configured by connecting to the `qBitTorrent` container and checking its public IP address:
```bash
docker exec -it qbittorrent bash # Connect to the container
# Inside the container, type:
curl ifconfig.me # Check the public IP address
```

If the IP returned by the command is different from your public IP address, then the VPN is correctly configured. YAY! \\o/

# Service Configuration

Coming soon!