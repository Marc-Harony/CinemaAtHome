# Service
- [service](https://jellyfin.org/)

*Jellyfin is the Free Software Media System that puts you in control of managing and streaming your media. There are no strings attached, no premium licenses or features, and no hidden fees.*

# Table of contents

# Compose file

<details>
<summary>Click to expand</summary>

![compose.yml](./compose.yml)
</details><br>

In this infrasctrucutre, we will use [SERVICE]() as a service. <br>
You can find client applications for:
- 



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

The part `networks: #2` is different from the `Jellyfin` service. We are using an external network called `jellyfin_docker_net`. This network is created in the `compose.yml` file of the `Jellyfin` service. This way, the `Jellyfin` service and the `Proxy` service will be able to communicate with each other and also with all the other services in the same network.

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