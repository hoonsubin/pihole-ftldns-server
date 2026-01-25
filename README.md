# Pi-Hole with FTLDNS using Unbound

This is a simple docker stack for deploying a Pi-Hole with Unbound as the recursive DNS resolver.
~~The Pi-hole container is served using [MACVLAN](https://docs.docker.com/engine/network/drivers/macvlan/), so ensure that the upstream network hardware supports devices with multiple mac addresses.~~
Dropped the MACVLAN feature as it will complicate any mesh network features for other containers sharing the same host.

This is so I can configure the router's DHCP server to use this as the DNS address.

This stack depends on [pihole/pihole](https://hub.docker.com/r/pihole/pihole), [klutchell/unbound](https://hub.docker.com/r/klutchell/unbound), and redis.

Depending on your config, you might want to manually set the root domain hints using the following command:

```sh
wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints
```

## References

- https://docs.pi-hole.net/guides/dns/unbound/
- https://unbound.docs.nlnetlabs.nl/en/latest/getting-started/configuration.html
