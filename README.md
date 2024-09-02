# Docker compose, blocklists, and exclusion settings for Pi-Hole and Unbound in recursive mode

This repo is a backup for Pi-Hole and Unbound running in docker containers leveraging IP addresses exposed in a 192.168.20.0/26 address space\
IP addresses, macvlan configuration, timezone, and file/path bindings will require adjustment to operate in other environments.

# Note on TTL

min-ttl settings in the provided unbound.conf are somewhat liberal and may not be suitable for all scenarios (40 mins in the provided .conf file)\
This can be adjusted in unbound.conf on line 18 'cache-min-ttl: 2400'

# Blocklists and Exclusions

The provided blocklists are not de-duplicated and result in a large block list comprised of over 1M entries

The exclusions have been found to resolve the most common issues encountered, but have not been tested for an extended period of time