# CinemaAtHome

# 🥖Looking for a full french video-guide?🥖

Coming soon!

## Table of contents

- [CinemaAtHome](#cinemaathome)
- [🥖Looking for a full french video-guide?🥖](#looking-for-a-full-french-video-guide)
  - [Table of contents](#table-of-contents)
  - [Introduction](#introduction)
  - [ DISCLAIMER ](#-disclaimer-)
  - [Estimated cost](#estimated-cost)
  - [Prerequisites](#prerequisites)
    - [Mandatory](#mandatory)
      - [Hardware](#hardware)
      - [Software](#software)
    - [Optional](#optional)
- [Tree Infrasctructure](#tree-infrasctructure)
- [How is this sh\*t gonna work?](#how-is-this-sht-gonna-work)
- [Mount points recap](#mount-points-recap)
- [Video Streaming with JELLYFIN](#video-streaming-with-jellyfin)
- [Reverse Proxy with NGINX REVERSE PROXY (NPM)](#reverse-proxy-with-nginx-reverse-proxy-npm)
- [(OPTIONAL but really cool) WAN access with CLOUDFLARE](#optional-but-really-cool-wan-access-with-cloudflare)
- [(OPTIONAL but useful) Bypassing Cloudflare's CAPTCHA with FLARESOLVERR](#optional-but-useful-bypassing-cloudflares-captcha-with-flaresolverr)
- [**LEGALLY** download movies and series with QBITTORRENT](#legally-download-movies-and-series-with-qbittorrent)
- [Manage your movies with RADARR](#manage-your-movies-with-radarr)
- [Manage your series with SONARR](#manage-your-series-with-sonarr)
- [Manage your managers with PROWLARR](#manage-your-managers-with-prowlarr)
- [Build your library with JELLYSEERR](#build-your-library-with-jellyseerr)


## Introduction

This repo aims to guide you through the creation of you own self-hosted streaming server.
At the end of this documentation, you will have a fully functional video streaming server accessible from your LAN and the internet, without having to expose any ports of your router to the internet nor to expose your public IP address.

## <img src="./.attachment/would_you_download_a_car.png" height="30"> DISCLAIMER <img src="./.attachment/would_you_download_a_car.png" height="30">
This project is for educational purposes only. I do not encourage the use of this project to download or distribute copyrighted material. The project is intended to be used to stream your own movies and series.<br>

Downloading or distributing copyrighted material without permission is illegal and can have serious consequences depending on your local laws.

<p align="center">
  <img src="./.attachment/leave_alone_meme.jpg" />
</p>

## Estimated cost

- **Hardware**: 0€ - 200€
  - This part will depend on the hardware you already have. You can use an old computer or a Raspberry Pi to host the services.
  - You will also need a high-capacity storage device to store your movies and series.
- **Software**: 0€
  - Everything (or almost) is open-source and free to use.
  - You will need to create a free account on [Cloudflare](https://www.cloudflare.com/) to manage your tunnels (more info further).
- **Domain name**: 0€ - 15€/year
  - It depends on the registrar you choose. If you want a paid domain name, it will rarely exceed 15€/year. (If it does, you should consider changing your registrar)
- **Electricity**: ¯\\\_(ツ)_/¯ 
  - It depends on your country and the power consumption of your hardware. You can check the power consumption of your hardware and calculate the cost of running it 24/7.
- **Internet**: ¯\\\_(ツ)_/¯ 
  - It depends on your internet plan and also your country. You will need a decent upload speed to stream your movies and series from outside your LAN.
- **VPN**: 0€ - 30€/year
  - This is optional but recommended if you want to use the full potential of this project. You can use a free VPN but I recommend using a paid one for better security and privacy.
- **Subscrition fees**: 0€
  - You will not need to pay any subscription fees to use the services. Everything is self-hosted and free to use.

> ☝🏻 *A friend told me that compared to a subscription to Netflix or another streaming platform, the investment is derisory. It's true, but remember that it's illegal to download copyrighted stuff :wink:*  


## Prerequisites

### Mandatory

#### Hardware

- A computer or a Raspberry Pi to host the services.
  - > ☝🏻 *If you are using a Raspberry Pi, it's not recommended to store your movies on the SD card. Prefer storing them in dedicated HDD or SSD.*

- A high-capacity storage device to store your movies and series. 

#### Software

- A Linux distribution installed on your host and a basic knowledge of the CLI.
- An Linux account with sudo privileges.
- Docker and Docker Compose installed on your host. [See the official documentation based on your distribution](https://docs.docker.com/get-docker/)
  - > ☝🏻 * If you're using a RedHat-based distro, you can follow [this doc i've made](https://github.com/Marc-Harony/HomeNextcloudServer/blob/master/README.md#step-1-install-dependencies-and-configure-the-host) to install and configure Docker.

### Optional

- A domain name to access your services from the internet.
- A Cloudflare account to manage your tunnels and generate SSL certificates.

> 💡 *We'll be using Docker throughout this project. You don't necessarily need to know anything about this technology, but it's always better to know and understand what you're doing.*

> ☝🏻 *I'll try to be as clear as possible, so that anyone with even the slightest computer/network knowledge can understand what I'm talking about.*

# Tree Infrasctructure

```bash
/docker/
├── jellyfin/
│   ├── config/
│   │   └── [...]
│   └── docker-compose.yaml
├── proxy/
│   ├── certs/
│   │   └── [...]
│   ├── data/
│   │   └── [...]
│   └── docker-compose.yaml
#============= Optional but really cool =============#
├── cloudflare/ # WAN access w/o exposing ports      #
│   └── docker-compose.yaml                          #
#====================================================#
├── flaresolverr/
│   ├── config/
│   │   └── [...]
│   └── docker-compose.yaml
├── qbittorrent/
│   ├── config/
│   │   └── [...]
│   ├── docker-compose.yaml
│   └── gluetun/
│       └── [...]
├── radarr/
│   ├── config/
│   │   └── [...]
│   └── docker-compose.yaml
├── sonarr/
│   ├── config/
│   │   └── [...]
│   └── docker-compose.yaml
├── prowlarr/
│   ├── config/
│   │   └── [...]
│   └── docker-compose.yaml
└── jellyseerr/
    ├── config/
    │   └── [...]
    └── docker-compose.yaml
```

# How is this sh*t gonna work?

*Insert a nice drawn schema here*

# Mount points recap

With all the services we are going to deploy, you'll rapidly get lost with all the mount points. First, I recommand to store your config in the same folder as the service (like in the [tree infrasctructure](#tree-infrasctructure)). This way, you'll know where to find the config files of the services. The part that will be tricky is the media storage and wich service will use it. Here is how I've set this up:

| Mount Point | Services that have access to it | Description |
|-------------|----------|-------------|
| `$SERVICE/config` | $SERVICE | Specific location for the config files of the service. |
| `/mnt/external_drive/jellyfin/media/films` | Jellyfin, Radarr | Location of the movies. This folder points to a dedicated HDD |
| `/mnt/external_drive/jellyfin/media/series` | Jellyfin, Sonarr | Location of the series. This folder points to a dedicated HDD |
| `/mnt/external_drive/qbittorrent/downloads` | qBittorrent, Radarr, Sonarr | Location of the downloaded movies and series. This folder points to a dedicated HDD. qBittorrent will download files there and then Radarr or Sonarr will move them in their respective folders. |


# Video Streaming with JELLYFIN

We start the infrasctructure with a video streaming server. It will be exposed to your LAN so every device connected to the same network will be able to access it.

![Full documentation here!](./docker/jellyfin/README.md)

# Reverse Proxy with NGINX REVERSE PROXY (NPM)

Now that Jellyfin is up and running, we will secure the access to it with a Reverse Proxy. This will allow you to add SSL encryption and access it with a domain name. Additionally, you will be able to access the service without having to expose its ports to the LAN!

![Full documentation here!](./docker/proxy/README.md)

# (OPTIONAL but really cool) WAN access with CLOUDFLARE

This part is optional but really cool. With Cloudflare, you will be able to access your services from the WAN without having to expose any ports of your router to the internet or to expose your public IP address.
You will need to create a free account on Cloudflare to manage your tunnels.
For this part to work, you will need to have a domain name.

![Full documentation here!](./docker/cloudflare/README.md)

# (OPTIONAL but useful) Bypassing Cloudflare's CAPTCHA with FLARESOLVERR

This part is optional but really useful. Cloudflare's CAPTCHA can be annoying, especially when you wan't to automate **COMPLETELY LEGAL** torrent downloads.
This service will allow you to bypass these CAPTCHA and access the services without any human interaction, pretty nice huh?

![Full documentation here!](./docker/flaresolverr/README.md)

# **LEGALLY** download movies and series with QBITTORRENT

What's good with this project is that will be able to watch your favourite movies and series from anywhere, but what about watching series that you don't have yet? That's where qBittorrent comes in handy. **Again**, I do not encourage to download copyrighted material, this is only for educational purposes.

![Full documentation here!](./docker/qbittorrent/README.md)

# Manage your movies with RADARR

Now everything is setup and ready to download, you may need a tool to know what you have and what you don't. Radarr is a tool that will help you manage your movies and download the ones you don't have yet.

![Full documentation here!](./docker/radarr/README.md)

# Manage your series with SONARR

Same as Radarr but for series. Sonarr will help you manage your series and download the ones you don't have yet.

![Full documentation here!](./docker/sonarr/README.md)

# Manage your managers with PROWLARR

Prowlarr is a tool that will help you manage your Radarr and Sonarr instances. It will allow you to have a single interface to manage your movies and series.

![Full documentation here!](./docker/prowlarr/README.md)

# Build your library with JELLYSEERR

Jellyseerr is a tool that will help you build your library. It will scan your movies and series and add them to your Jellyfin library. You'll be able to request movies or series to add to your library. Any user with a Jellyfin account will be able to request a movie or a series to add to the library.

![Full documentation here!](./docker/jellyseerr/README.md)
