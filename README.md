# CinemaAtHome

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
- **Subscrition fees**: 0€
  - You will not need to pay any subscription fees to use the services. Everything is self-hosted and free to use.

> ☝🏻 *Compared to a subscription to netflix or another streaming platform, the investment is derisory.*  

## Prerequisites

### Mandatory

#### Hardware

- A computer or a Raspberry Pi to host the services.
- A high-capacity storage device to store your movies and series.

#### Software

- A Linux distribution installed on your host and a basic knowledge of the CLI.
- An Linux account with sudo privileges.
- Docker and Docker Compose installed on your host. [See the official documentation based on your distribution](https://docs.docker.com/get-docker/)

### Optional

- A domain name to access your services from the internet.
- A Cloudflare account to manage your tunnels and generate SSL certificates.

> 💡 *We'll be using Docker throughout this project. You don't necessarily need to know anything about this technology, but it's always better to know and understand what you're doing.*

> ☝🏻 *I'll try to be as clear as possible, so that anyone with even the slightest computer/network knowledge can understand what I'm talking about.*

# Table of contents

- [CinemaAtHome](#cinemaathome)
  - [Introduction](#introduction)
  - [ DISCLAIMER ](#-disclaimer-)
  - [Estimated cost](#estimated-cost)
  - [Prerequisites](#prerequisites)
    - [Mandatory](#mandatory)
      - [Hardware](#hardware)
      - [Software](#software)
    - [Optional](#optional)
- [Table of contents](#table-of-contents)
- [Tree Infrasctructure](#tree-infrasctructure)
- [How is this sh\*t gonna work?](#how-is-this-sht-gonna-work)
- [Video Streaming with JELLYFIN](#video-streaming-with-jellyfin)
- [Reverse Proxy with NGINX REVERSE PROXY (NPM)](#reverse-proxy-with-nginx-reverse-proxy-npm)
- [(OPTIONAL but really cool) WAN access with CLOUDFLARE](#optional-but-really-cool-wan-access-with-cloudflare)
- [(OPTIONAL but useful) Bypassing Cloudflare's CAPTCHA with FLARESOLVERR](#optional-but-useful-bypassing-cloudflares-captcha-with-flaresolverr)

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