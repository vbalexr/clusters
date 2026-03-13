Install Ubuntu 25.10 and apply this netplan modify it for each node

```yaml
network:
  version: 2
  ethernets:
    enp87s0: {}
    enp90s0: {}
    # for services comunication, do not have interneth access
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
  bridges:
    br0:
      interfaces:
        - enp2s0f1np1
      addresses:
        - 172.16.128.X/24
      parameters:
        stp: true
        forward-delay: 4
        
        
```

baltasar

/etc/keepalived/keepalived.conf


vrrp_instance VI_1 {
    state MASTER
    interface enp2s0f0np0
    virtual_router_id 41
    priority 200
    advert_int 1
    virtual_ipaddress {
        10.255.0.5/24
    }
}
vrrp_instance VI_2 {
    state MASTER
    interface bond0
    virtual_router_id 42
    priority 200
    advert_int 1
    virtual_ipaddress {
        10.1.0.5/24
    }
}
sudo systemctl restart keepalived

Casper


/etc/keepalived/keepalived.conf


vrrp_instance VI_1 {
    state BACKUP
    interface enp2s0f0np0
    virtual_router_id 41
    priority 100
    advert_int 1
    virtual_ipaddress {
        10.255.0.5/24
    }
}
vrrp_instance VI_2 {
    state BACKUP
    interface bond0
    virtual_router_id 42
    priority 100
    advert_int 1
    virtual_ipaddress {
        10.1.0.5/24
    }
}
sudo systemctl restart keepalived

melchior


/etc/keepalived/keepalived.conf


vrrp_instance VI_1 {
    state BACKUP
    interface enp2s0f0np0
    virtual_router_id 41
    priority 50
    advert_int 1
    virtual_ipaddress {
        10.255.0.5/24
    }
}
vrrp_instance VI_2 {
    state BACKUP
    interface bond0
    virtual_router_id 42
    priority 50
    advert_int 1
    virtual_ipaddress {
        10.1.0.5/24
    }
}
sudo systemctl restart keepalived