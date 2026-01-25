# Pi-Hole with FTLDNS using Unbound

This is a simple docker stack for deploying a Pi-Hole with Unbound as the recursive DNS resolver.
~~The Pi-hole container is served using [MACVLAN](https://docs.docker.com/engine/network/drivers/macvlan/), so ensure that the upstream network hardware supports devices with multiple mac addresses.~~
Dropped the MACVLAN feature as it will complicate any mesh network features for other containers sharing the same host.

This is so I can configure the router's DHCP server to use this as the DNS address.

This stack depends on [pihole/pihole](https://hub.docker.com/r/pihole/pihole), [klutchell/unbound](https://hub.docker.com/r/klutchell/unbound), and redis.

Depending on your config, you might want to manually set the root domain hints using the following command:

```bash
wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints
```

## Debugging the network

Since this stack involves multiple network layers (Pi-hole -> Unbound and LAN -> Pi-hole), it might not be too trivial to debug any issues for people who are not used to networking in containerized environments.

Use the following command from the docker host machine to test if unbound is working correctly:

```bash
# docker exec pihole dig <domain name> @unbound -p 5335
docker exec pihole dig +ad dnssec.works @unbound -p 5335
```

You can also try the same command using the pi-hole's address to check if it works as expected locally

```bash
# note that this will default to port 53, whilst the unbound instance running within the container must use port 5335 to prevent any conflicts
dig +ad dnssec.works @pihole-ip
```

## References

- https://docs.pi-hole.net/guides/dns/unbound/
- https://unbound.docs.nlnetlabs.nl/en/latest/getting-started/configuration.html
- https://github.com/pi-hole/docker-pi-hole/blob/master/src/Dockerfile
