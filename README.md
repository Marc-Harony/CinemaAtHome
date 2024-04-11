# CinemaAtHome
This repo aims to guide you through the creation of you own self-hosted streaming server.

# Table of contents

- [CinemaAtHome](#cinemaathome)
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

![README.md](./docker/jellyfin/README.md)

# Reverse Proxy with NGINX REVERSE PROXY (NPM)

![README.md](./docker/proxy/README.md)