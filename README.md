In progress :bomb:

# Docker-based Chat Gateway (Alpine Linux + Bitlbee + ZNC)

This project is a combination of an IM to IRC service (Bitlbee) and
an IRC bouncer (ZNC) which keeps you connected, managing away status
and message buffers for all your devices.

This is a setup I have personally used and evolved slightly since around 2008
(of course back then without Docker or the improved message playback features).
The setup has served me well and continues to be useful. This repository is a
way to document my configuration as well as to share what I've learned and get
feedback.

Feel free to fork and modify for your own needs or issue a pull request!

## Bitlbee

Bitlbee is an IRC server that acts as a bridge between the IRC protocol
and the protocols used by various instant messaging systemms
(Jabber/XMPP, Facebook, AIM/Oscar, Yahoo, MSN, ICQ, Skype, etc).

The Docker image for Bitlbee that is part of this project includes
additional packages for Facebook chat and OTR that you may optionally
use.

### Starting

Create a directory for storing your user `data/`, then run:

```
docker run \
  -d \
  -v /etc/localtime:/etc/localtime:ro \
  -v $PWD/data:/var/lib/bitlbee:rw \
  --name=bitlbee \
  --restart=always \
  stanback/alpine-bitlbee
```

### Configuring

Once you add the Bitlbee server in ZNC (below), you will be prompted
to register and configure. If you want to bypass ZNC and connect
to Bitlbee directly, simply add a port mapping using `-p 6667:6667`
when running the container, then connect up your client to the Docker
host.

In addition to the settings you can configure from your IRC client,
there are some global options that you may set in
`/etc/bitlbee/bitlbee.conf` as well as the message in
`/etc/bitlbee/motd`. You can copy these locally into a `config`
directory and map with `-v $PWD/config:/etc/bitlbee:ro`.

### Commands Quickstart

TODO: Setting up user, adding networks, basic management

## ZNC

ZNC is an IRC bouncer, a system designed to keep you always connected to
your favorite IRC servers. I'm primarily using it with Bitlbee (above)
and Slack's IRC gateway. Other networks include places like Freenode,
Gitter, DALNet, EFNet, etc.

ZNC includes a variety of useful plugins for keeping track of missed
messages, automatically setting away status, perforing commands with
ChanServ/NickServ, watching for particular words or phrases, and many more.

The Docker image for ZNC that is part of this project includes additional
3rd party modules for push notifications with the Palaver iOS client (below),
partial message buffer playback, and separate buffers for your differnet clients.

### Configuring ZNC

Create a directory for storing your `data/`, then run:

```
docker run \
  -it \
  --rm=true \
  -v $PWD/data:/var/lib/znc \
  stanback/alpine-znc --makeconf
```

### Starting

Replace 6697 with whatever port you configured in the previous step.
Symlinking localtime is optional, but was required for my IRC and system
timestamps to sync up. The `--link` command connects with a running
Bitlbee container, with this you can use the hostname `bitlbee` when
adding a new IRC server.

```
docker run \
  -d \
  -p 6697:6697 \
  -v /etc/localtime:/etc/localtime:ro \
  -v $PWD/data:/var/lib/znc \
  --name=znc \
  --restart=always \
  --link=bitlbee:bitlbee \
  stanback/alpine-znc
```

### Commands Quickstart

TODO: Setup user, add network for Bitlbee, load and configure modules

## Client Recommendations

### OSX

Recommendation: Textual

### iOS

Recommendation: Palaver, followed by Mobile Colloquy

### Windows

Recommendation: HexChat

### Android

Recommendation: HoloIRC

### Linux

Recommendation: Communi
