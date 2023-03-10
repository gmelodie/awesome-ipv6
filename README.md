# awesome-ipv6 [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of IPv6 materials and learning resources


## Contents

- [Books](#books)
- [Cheatsheets](#cheatsheets)
- [Courses](#courses)
- [Tools](#tools)
- [Commands](#commands)
  - [ping6](#ping6)
  - [ip](#ip)
  - [Bluetooth 6LoWPAN](#bluetooth-6lowpan)
- [Configurations](#configurations)
  - [Radvd](#radvd)
  - [IPv6 conf](#ipv6-conf)
- [Labs](#labs)
- [RFCs](#rfcs)

## Books
- [IPv6 Fundamentals: A Straightforward Approach to Understanding IPv6](https://www.amazon.com.br/IPv6-Fundamentals-Straightforward-Approach-Understanding-ebook/dp/B07212JBMT/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=IPv6+Fundamentals%3A+A+Straightforward+Approach+to+Understanding+IPv6&qid=1574187978&sr=8-1)
- [IPv6 Security](https://www.amazon.com.br/IPv6-Security-Networking-Technology-English-ebook/dp/B001PBSDKC/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=ipv6+security&qid=1612097420&sr=8-1)
- [Linux IPv6 HowTo](https://www.tldp.org/HOWTO/Linux+IPv6-HOWTO/) - All-things manual for IPv6 on Linux

## Cheatsheets
- [IPv6 Essentials Cheat Sheet v1.7](http://teachmeipv6.com/IPv6-Essentials-Cheat-Sheet.pdf)
- [Internet Protocol Version 6 (IPv6) Basics Cheat Sheet by Jens Roesen](https://www.roesen.org/files/ipv6_cheat_sheet.pdf)
- [IPv6 Addressing Guide (pt-BR)](http://ipv6.br/media/arquivo/ipv6/file/46/enderec-v6.pdf)

## Courses
- [IPv6 Training Recommendations from Internet Society](https://www.internetsociety.org/deploy360/ipv6/training/)
- [Linux IPv6 Crash Course](https://www.linux.com/tutorials/ipv6-crash-course-linux/)

## Tools
- [Bash ULA Generator](https://github.com/adeverteuil/bash-ula-generator)
- [Radvd](https://github.com/reubenhwk/radvd)

### ping6 
[Documentation](https://linux.die.net/man/8/ping6)
- `ping6 <node address>` - Ping a node
- `ping6 ff02::1%<interface>` - Ping all-nodes multicast address on link
  
### ip
[Documentation](https://linux.die.net/man/8/ip)
- `ip -6 neigh show` - Show neighbor cache
- `ip -6 addr`/`ip -6 a`/ `ip -6 address` - Show IPv6 interfaces addresses
- `ip address add 2002:db6::1/64 dev bt0` - Add IPv6 prefix to interface
- `ip -6 addr flush label "en*"` - Clear all IPv6 addresses on an interface

### Bluetooth 6LoWPAN
- Enable 6LoWPAN over Bluetooth
  1. `modprobe bluetooth_6lowpan`
  2. `echo 35 > /sys/kernel/debug/bluetooth/6lowpan_psm`
  3. `echo 1 > /sys/kernel/debug/bluetooth/6lowpan_enable`
- Connect device to host
  - `echo "connect XX:XX:XX:XX:XX:XX 2" | sudo tee /sys/kernel/debug/bluetooth/6lowpan_control`

## Configurations

### Radvd
[Documentation](https://www.systutorials.com/docs/linux/man/5-radvd.conf/)
```
interface $iface
{ 
  AdvSendAdvert on;

  prefix 2b02:6009:aac0:c2a2::/64
  {
    AdvOnLink on;
    AdvAutonomous on;
    AdvRouterAddr on;
  };
};
```

### IPv6 conf
IPv6 configuration in `/etc/sysctl.conf`, `all` can be replaced with interface name
- `net.ipv6.conf.all.accept_ra=2` - Enable or disable SLAAC
  - 1 -> Enable (disabled, if forwarding=1)
  - 0 -> Disable
  - 2 -> Enable (for all)
- `net.ipv6.conf.all.forwarding=1` - Enable IPv6 forwarding
- `net.ipv6.conf.all.accept_ra_defrtr=1` - Accept hop limit settings from RA
- `net.ipv6.conf.all.router_solicitations=1` - Number of Router Solicitations send, before assuming no router available

### Disable Privacy Extensions
When disabling privacy extensions, the Interface ID will be generated from the device's MAC address (EUI-64 format). This setting is only effective for managed devices because many operating systems also have built-in MAC address privacy options.

On Windows, open a command prompt as Administrator and paste the following commands:
```
netsh interface ipv6 set global randomizeidentifiers=disabled store=active 
netsh interface ipv6 set global randomizeidentifiers=disabled store=persistent 
netsh interface ipv6 set privacy state=disabled store=active 
netsh interface ipv6 set privacy state=disabled store=persistent
```

On Linux, go to /etc/netplan and find the YAML configuration file there. Add ```ipv6-privacy: off``` and ```ipv6-address-generation: eui64``` to the appropriate interface:
```
network:
  version: 2
  ethernets:
    enp0s3:
      ipv6-privacy: off
      ipv6-address-generation: eui64
  renderer: NetworkManager
```

## Labs
- [CORE - Open source packet-tracer-like program to create IPv6-enabled environments (pt-BR)](http://ipv6.br/pagina/downloads)

## RFCs
- [RFC8504: IPv6 Node Requirements Best Current Practice 220](https://tools.ietf.org/html/rfc8504)
- [RFC8200: (STD 86) Internet Protocol, Version 6 (IPv6) Specification](https://tools.ietf.org/html/rfc8200)
- [RFC4291: IP Version 6 Addressing Architecture](https://tools.ietf.org/html/rfc4291)
- [RFC4861: Neighbor Discovery for IP version 6 (IPv6)](https://tools.ietf.org/html/rfc4861)
- [RFC4862: IPv6 Stateless Address Autoconfiguration](https://tools.ietf.org/html/rfc4862)
- [RFC4941: Privacy Extensions for Stateless Address Autoconfiguration in IPv6](https://tools.ietf.org/html/rfc4941)
- [RFC4443: Internet Control Message Protocol (ICMPv6) for the Internet Protocol Version 6 (IPv6) Specification](https://tools.ietf.org/html/rfc4443)
- [RFC8106: IPv6 Router Advertisement Options for DNS Configuration](https://tools.ietf.org/html/rfc8106)
- [RFC8415: Dynamic Host Configuration Protocol for IPv6 (DHCPv6)](https://tools.ietf.org/html/rfc8415)
- [RFC3596: DNS Extensions to Support IP Version 6](https://tools.ietf.org/html/rfc3596)
- [RFC6724: Default Address Selection for Internet Protocol Version 6 (IPv6)](https://tools.ietf.org/html/rfc6724)
- [RFC7381: Enterprise IPv6 Deployment Guidelines](https://tools.ietf.org/html/rfc7381)

## Contribute

Contributions welcome! Read the [contribution guidelines](contributing.md) first.

## License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0)

To the extent possible under law, Gabriel Cruz has waived all copyright and
related or neighboring rights to this work.
