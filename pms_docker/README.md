# Plex documentation
## A lot of useful information can be found here: https&#65279;://hub.docker.com/r/plexinc/pms-docker/
- **We're assuming you've already created a plex user and plex group in Linux.** \
The UID (1001) and GID (1001) will be needed for docker compose and you'll need to check /etc/passwd to make sure they match.

![#1589F0]sudo useradd -M plex`#1589F0` \
![#1589F0]cat /etc/passwd | grep plex`#1589F0` \
![#1589F0]plex:x:1001:1001:plex::/bin/bash`#1589F0`

- **We're assuming you already have docker installed. This is distro dependent so you'll need to look it up for your specific install.** \
You may need to add a repo to install docker and its components.

![#1589F0]sudo dnf config-manager --add-repo https&#65279;://download.docker.com/linux/fedora/docker-ce.repo`#1589F0`

![#1589F0]dnf repolist`#1589F0`  
<pre>repo name                                                repo id</pre>
<pre>docker-ce-stable                                         Docker CE Stable - x86_64</pre>

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin.

**Afer installing docker, you'll want to make sure it's enabled and started** \
ex. ![#1589F0]sudo systemctl enable docker, sudo systemctl start docker, sudo systemctl is-enabled docker, sudo systemctl is-active docker`#1589F0`

A note about Docker permissions. By default, Docker commands require sudo permissions. \
**If you want to run docker commands from a different user context, that user needs to be added to the docker group.**

sudo usermod -aG docker your-non-root-username

- **We're assuming you have the NFS packages installed. You may need to install these depending on distribution.**

sudo dnf install nfs-utils

- **We're assuming you already have folders created and permissions assigned to allow Plex access.**

**NOTE: rwx is ONLY needed for /appdata/plex_config, not for /appdata/plex and may pose a security risk.** \
/appdata/plex is only used as a mount point, and the NFS permissions are configured to read-only on the NAS side.

This has been tested with rm -rf and access is denied due to read-only for the mounted filesystem. \
Probably best to come back and adjust these permissions later as they're possibly over permissive.

sudo chgrp plex /appdata/ \
sudo chgrp plex /appdata/plex \
sudo chgrp plex /appdata/plex_config \
sudo chmod g+rwx /appdata/ \
sudo chmod g+rwx /appdata/plex/ \
sudo chmod g+rwx /appdata/plex_config

### NFS mounts will need to be created in /etc/fstab (replace IP, source volume, and mount path with what's applicable for your environment)

x.x.x.x:/volumexx/plex /appdata/plex nfs auto,user 0 0 \
It's probably best to start with noauto first to make sure there's not a problem with mounting the filesystem. \
**NOTE: systemctl daemon-reload will be required after altering etc/fstab.**

You can test the mount after creating the noauto entry in fstab using the mount command. \
mount x.x.x.x:/volumexx/plex /appdata/plex/ \
The /appdata/plex path must already exist with appropriate permissions configured for plex

- **We're assuming at least a foundational understanding of docker networking in the example compose.yml**

In the provided example, we're using a macvlan docker network, and exposing the container on it's own IP within \
the same subnet of playback devices. This is a personal preference and many prefer to use the host network by default. \
Your address space will almost certainly be different, so you'd have to create something that works in your environment. \
In the example, we're working with a network of 192.168.20.0/26, but DHCP is only managing x.x.x.2-x.x.x.50, leaving twelve \
addresses within the scope that can be assigned to other resources without colliding with DHCP or the broadcast address. \
### NOTE: If you are new to docker networking, there's a great tutorial here: https&#65279;://www&#65279;.youtube.com/watch?v=5grbXvV_DSk