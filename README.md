[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)

The [LinuxServer.io](https://linuxserver.io) team brings you another container release featuring :-

 * regular and timely application updates
 * easy user mappings (PGID, PUID)
 * custom base image with s6 overlay
 * weekly base OS updates with common layers across the entire LinuxServer.io ecosystem to minimise space usage, down time and bandwidth
 * regular security updates

Find us at:
* [Discord](https://discord.gg/YWrKVTn) - realtime support / chat with the community and the team.
* [IRC](https://irc.linuxserver.io) - on freenode at `#linuxserver.io`. Our primary support channel is Discord.
* [Blog](https://blog.linuxserver.io) - all the things you can do with our containers including How-To guides, opinions and much more!
* [Podcast](https://anchor.fm/linuxserverio) - on hiatus. Coming back soon (late 2018).

# PSA: Changes are happening

From August 2018 onwards, Linuxserver are in the midst of switching to a new CI platform which will enable us to build and release multiple architectures under a single repo. To this end, existing images for `arm64` and `armhf` builds are being deprecated. They are replaced by a manifest file in each container which automatically pulls the correct image for your architecture. You'll also be able to pull based on a specific architecture tag.

TLDR: Multi-arch support is changing from multiple repos to one repo per container image.

# [linuxserver/couchpotato](https://github.com/linuxserver/docker-couchpotato)
[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/couchpotato.svg)](https://microbadger.com/images/linuxserver/couchpotato "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/couchpotato.svg)](https://microbadger.com/images/linuxserver/couchpotato "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/couchpotato.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/couchpotato.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-couchpotato/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-couchpotato/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/couchpotato/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/couchpotato/latest/index.html)

[Couchpotato](https://couchpota.to/) is an automatic NZB and torrent downloader. You can keep a `movies I want` list and it will search for NZBs/torrents of these movies every X hours. Once a movie is found, it will send it to SABnzbd or download the torrent to a specified directory.

[![couchpotato](https://couchpota.to/media/images/full.png)](https://couchpota.to/)

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list). 

Simply pulling `linuxserver/couchpotato` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v6-latest |


## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=couchpotato \
  -e PUID=1001 \
  -e PGID=1001 \
  -e TZ=Europe/London \
  -e UMASK_SET=022 \
  -p 5050:5050 \
  -v </path/to/appdata/config>:/config \
  -v </path/to/downloads>:/downloads \
  -v </path/to/movies>:/movies \
  --restart unless-stopped \
  linuxserver/couchpotato
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  couchpotato:
    image: linuxserver/couchpotato
    container_name: couchpotato
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/London
      - UMASK_SET=022
    volumes:
      - </path/to/appdata/config>:/config
      - </path/to/downloads>:/downloads
      - </path/to/movies>:/movies
    ports:
      - 5050:5050
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 5050` | http gui |
| `-e PUID=1001` | for UserID - see below for explanation |
| `-e PGID=1001` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London |
| `-e UMASK_SET=022` | for umask setting of couchpotato, optional , default if left unset is 022 |
| `-v /config` | Couchpotato Application Data. |
| `-v /downloads` | Downloads Folder. |
| `-v /movies` | Movie Share. |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

&nbsp;
## Application Setup

Access the webui at `<your-ip>:5050`, for more information check out [CouchPotato](https://couchpota.to).



## Support Info

* Shell access whilst the container is running: `docker exec -it couchpotato /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f couchpotato`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' couchpotato`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/couchpotato`

## Versions

* **14.01.19:** - Multi-arch builds.
* **16.08.18:** - Rebase to alpine 3.8.
* **06.11.17:** - Rebase to alpine 3.7.
* **20.07.17:** - Internal git pull instead of at runtime, add UMASK_SET variable.
* **12.07.17:** - Add inspect commands to README, move to jenkins build and push
* **25.05.17:** - Rebase to alpine 3.6
* **07.02.17:** - Rebase to alpine 3.5.
* **11.11.16:** - Stop cp logging to docker log (they are accessible in the webui and the config folder).
* **30.09.16:** - Fix umask.
* **09.09.16:** - Add layer badges to README.
* **27.08.16:** - Add badges to README.
* **08.08.16:** - Rebase to alpine linux.
* **12.11.15:** - Misc Code Cleanup.
* **02.10.15:** - Change to python baseimage.
* **28.07.15:** - Updated to latest baseimage (for testing), and a fix to autoupdate.
