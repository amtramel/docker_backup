# Docker compose, blocklists, and exclusion settings for Pi-Hole and Unbound in recursive mode

This repo is a backup for Pi-Hole and Unbound running in docker containers leveraging IP addresses exposed in a 192.168.20.0/26 address space\
IP addresses, macvlan configuration, timezone, and file/path bindings will require adjustment to operate in other environments.

# Note on TTL

min-ttl settings in the provided unbound.conf are somewhat liberal and may not be suitable for all scenarios (40 mins in the provided .conf file)\
This can be adjusted in unbound.conf on line 18 'cache-min-ttl: 2400' (seconds 2400/60=40)

# Blocklists and Exclusions

The provided blocklists are not de-duplicated but result in a large blocklist comprised of over 1M unique entries

The exclusions have been found to resolve issues encountered thus far, but have not been tested for an extended period of time\
This list will be updated as issues are encountered and changes are successfully implemented

Issues addressed thus far include:
- TikTok booting user from app after an indeterminate period of time
- Microsoft blocking downloads due to "obfuscation of identity"
- Broken playback of Google TV channels on Shield Pro devices
- Issues with Facebook application loading images and other foundational functionality
- Issues with internally hosted Plex that required whitelisting of analytics.plex.tv