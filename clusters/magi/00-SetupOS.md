Install Ubuntu 25.10 and apply this netplan modify it for each node

```yaml
network:
  version: 2
  ethernets:
    enp87s0: {}
    enp90s0: {}
    enp2s0f0np0:
      addresses:
        - 10.255.0.X/24
        - "fd7a:cafe::X/64"
    enp2s0f1np1: {}
  bonds:
    bond0:
      interfaces:
        - enp87s0
        - enp90s0
      parameters:
        mode: 802.3ad
        lacp-rate: fast
        transmit-hash-policy: layer2+3
      addresses:
        - 10.1.0.X/22
        - "fd7a:6e5b:cafe:10::X/64"
      routes:
        - to: default
          via: 10.1.0.1
        - to: default
          via: "fd7a:6e5b:cafe:10::1"
      nameservers:
        addresses:
          - 10.1.0.1
          - "fd7a:6e5b:cafe:10::1"
  vlans:
    bond0.15:
      id: 15
      link: bond0
    bond0.20:
      id: 20
      link: bond0
    bond0.25:
      id: 25
      link: bond0
```
