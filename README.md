[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/index.php/irc/
[podcasturl]: https://www.linuxserver.io/index.php/category/podcast/

[![linuxserver.io](https://www.linuxserver.io/wp-content/uploads/2015/06/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/quassel-core
[![](https://images.microbadger.com/badges/image/lsioarmhf/quassel-core.svg)](http://microbadger.com/images/lsioarmhf/quassel-core "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/quassel-core.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/quassel-core.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io-armhf/lsioarmhf-quassel)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io-armhf/job/lsioarmhf-quassel/)
[hub]: https://hub.docker.com/r/lsioarmhf/quassel-core/

[Quassel IRC][quassurl] is a modern, cross-platform, distributed IRC client, meaning that one (or multiple) client(s) can attach to and detach from a central core.

This container handles the IRC connection (quasselcore) and requires a desktop client (quasselclient) to be used and configured. It is designed to be always on and will keep your identity present in IRC even when your clients cannot be online. Backlog (history) is downloaded by your client upon reconnection allowing infinite scrollback through time.

[![quassel](http://icons.iconarchive.com/icons/oxygen-icons.org/oxygen/256/Apps-quassel-icon.png)][quassurl]
[quassurl]: http://quassel-irc.org/

## Usage

```
docker create \
	--name=quassel-core \
	-v /etc/localtime:/etc/localtime:ro \
	-v <path to data>:/config \
	-e PGID=<gid> -e PUID=<uid> \
	-p 4242:4242 \
	lsioarmhf/quassel-core
```

**Parameters**

* `-p 4242` - the port quassel-core listens for connections on
* `-v /etc/localtime` for timesync - *optional*
* `-v /config` - database and quassel-core configuration storage
* `-e PGID` for for GroupID - see below for explanation
* `-e PUID` for for UserID - see below for explanation

It is based on ubuntu xenial with s6 overlay, for shell access whilst the container is running do `docker exec -it quassel-core /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application
`IMPORTANT... THIS IS THE ARMHF VERSION`

Quassel wiki: [quassel](http://bugs.quassel-irc.org/projects/quassel-irc/wiki)

A great place to host a quassel instance is a VPS, such as [DigitalOcean](https://www.digitalocean.com/?refcode=501c48b34b8c). For $5 a month you can have a 24/7 IRC connection and be up and running in under 55 seconds (or so they claim).

Once you have the container running, fire up a quassel desktop client and connect to your new core instance using your droplets public IP address and the port you specified in your `docker run` command *default: 4242*. Create an admin user, select SQLite as your storage backend (Quassel limitation). Setup your real name and nick, then press `Save & Connect`.

You're now connected to IRC. Let's add you to our [IRC](http://www.linuxserver.io/index.php/irc/) `#linuxserver.io` room on Freenode. Click 'File' > 'Networks' > 'Configure Networks' > 'Add' (under Networks section, not Servers) > 'Use preset' > Select 'Freenode' and then configure your identity using the tabs in the 'Network details' section. Once connected to Freenode, click `#join` and enter `#linuxserver.io`. That's it, you're done.

## Info

* Monitor the logs of the container in realtime `docker logs -f quassel-core`.

## Versions

+ **13.09.16:** Initial Release.