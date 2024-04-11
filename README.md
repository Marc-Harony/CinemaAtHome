# CinemaAtHome

## Introduction

This repo aims to guide you through the creation of you own self-hosted streaming server.
At the end of this documentation, you will have a fully functional video streaming server accessible from your LAN and the internet, without having to expose any ports of your router to the internet nor to expose your public IP address.

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
  - [Estimated cost](#estimated-cost)
  - [Prerequisites](#prerequisites)
    - [Mandatory](#mandatory)
      - [Hardware](#hardware)
      - [Software](#software)
    - [Optional](#optional)
- [Table of contents](#table-of-contents)
- [Tree Infrasctructure](#tree-infrasctructure)
- [Video Streaming with JELLYFIN](#video-streaming-with-jellyfin)
- [Reverse Proxy with NGINX REVERSE PROXY (NPM)](#reverse-proxy-with-nginx-reverse-proxy-npm)

# Tree Infrasctructure

```bash
/docker/
├── jellyfin/
│   ├── config/
│   │   ├── [...]
│   └── docker-compose.yaml
├── proxy/
│   ├── certs/
│   │   ├── [...]
│   ├── data/
│   │   ├── [...]
│   └── docker-compose.yaml
```

# Video Streaming with JELLYFIN

We start the infrasctructure with a video streaming server. It will be exposed to your LAN so every device connected to the same network will be able to access it.

![Full documentation here!](./docker/jellyfin/README.md)

# Reverse Proxy with NGINX REVERSE PROXY (NPM)

Now that Jellyfin is up and running, we will secure the access to it with a Reverse Proxy. This will allow you to add SSL encryption and access it with a domain name. Additionally, you will be able to access the service without having to expose its ports to the LAN!

![Full documentation here!](./docker/proxy/README.md)