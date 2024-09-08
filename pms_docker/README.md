# Plex documentation
## A lot of useful information can be found here: https://hub.docker.com/r/plexinc/pms-docker/
- We're assuming you've already created a plex user and plex group in Linux. \
The UID (1001) and GID (1001) will be needed for docker compose and you'll need to check /etc/passwd to make sure they match.

sudo useradd -M plex \
Example from /etc/passwd: \
plex:x:1001:1001:plex::/bin/bash

- We're assuming you already have docker installed. This is distro dependent so you'll need to look it up for your specific install. \
You may need to add a repo to install docker and its components.

sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo

dnf repolist \
repo id                                                       repo name \
docker-ce-stable                                              Docker CE Stable - x86_64

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin.

A note about Docker permissions. By default, Docker commands require sudo permissions. \
If you want to run commands from a different user context, that user needs to be added to the Docker group.

sudo usermod -aG docker username

- We're assuming you have the NFS packages installed. You may need to install these depending on distribution.

dnf install nfs-utils

- We're assuming you already have folders created and permissions assigned to allow Plex access.

NOTE: rwx is ONLY needed for /appdata/plex_config, not for /appdata/plex and may pose a security risk. \
/appdata/plex is only used as a mount point, and the NFS permissions are configured to read-only on the NAS side.

This has been tested with rm -rf and access is denied due to read-only for the mounted filesystem. \
Probably best to come back and adjust these permissions later as they're overly permissive.

sudo chgrp plex /appdata/ \
sudo chgrp plex /appdata/plex \
sudo chgrp plex /appdata/plex_config \
sudo chmod g+rwx /appdata/ \
sudo chmod g+rwx /appdata/plex/ \
sudo chmod g+rwx /appdata/plex_config

### NFS mounts will need to be created in /etc/fstab (replace IP and mount path with what is applicable for your environment)

x.x.x.x:/volumexx/plex /appdata/plex nfs auto,user 0 0 \
It's probably best to start with noauto first to make sure there's not a problem with mounting the filesystem. \
NOTE: systemctl daemon-reload will be required after altering etc/fstab.

You can test the mount after creating the noauto entry in fstab using the mount command. \
mount x.x.x.x:/volumexx/plex /appdata/plex/ \
The /appdata/plex path must already exist with appropriate permissions configured for plex