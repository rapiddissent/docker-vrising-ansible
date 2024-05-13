# Ansibleated V Rising Linux server installer

## General Overview

Based on the great work done by [TrueOsiris](https://github.com/TrueOsiris/docker-vrising).

By default, this playbook will create a service account 'steam', and dump a bunch of files under /home/steam/vrising;
a few Docker-specific files, the vrising server install provided by SteamCMD, and a `game_files` dir containing game
savedata and configuration.

Docker is used both to standardize the Wine runtime environment, and to simplify service management using docker-compose.

## Prerequisites

1. Python3 - Minimum version tested == 3.8.10.
2. Ansible - Minimum version tested is ansible-core==2.13.13, ansible==6.7.0.
3. SteamCMD - [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD#Downloading_SteamCMD) includes a EULA I'm not comfortable auto-accepting for you; don't wanna get sued.

## Configuration

Modify the top-level config.yml to meet your deployment requirements.

## Installation

Once your `config.yml` has been updated to your liking, simply run the following, entring your `sudo` password when prompted-

```
ansible-playbook --ask-become-pass install_vrising.yml
```

The `vrising` service will automatically start, and is configured to start automatically at system startup.

### Service Management

You can stop the service with-

```
sudo systemctl stop vrising.service
```

You can keep the service from starting at system startup with-

```
sudo systemctl disable vrising.service
```

Start / Start-on-boot-

```
sudo systemctl start vrising.service
sudo systemctl enable vrising.service
```

## Limitations

Currently this is only written for and tested on Ubuntu 20.04 and 22.04. I don't care to extend this matrix much beyond that,
though I will try to keep an eye on PRs against this repo if you wish to implement the steps required for other distros.
To that end, I've attempted to architect the tasks to be easily extensible.

The role doesn't support multiple installs per host- it could, fairly easily, but I don't care to test it to my standards,
so I'm leaving that as an exercise for the reader.

## Issues

The `build_image.sh` script's command line args are acting wonky- it appears `getopt` has changed behavior a bit in the
years since I last used it. It's a corner case, but currently only honors one flag passed. Since there's only `--help`,
and `--force-rebuild`, I don't expect it'll cause much headache.
