```yml
hostname hq-rtr.au-team.irpo
interface int0
  description "to isp"
  ip address 172.16.4.2/28
  ex
port te0
  service-instance te0/int0
    encapsulation untagged
    ex
  ex
interface int0
  connect port te0 service-instance te0/int0
  ex
interface int1
  description "to hq-srv"
  ip address 192.168.100.1/26
  ex
interface int2
  description "to hq-cli"
  ip address 192.168.200.1/28
  ex
port te1
  service-instance te1/int1
    encapsulation dot1q 100
    rewrite pop 1
    ex
  service-instance te1/int2
    encapsulation dot1q 200
    rewrite pop 1
    ex
  ex
interface int1
  connect port te1 service-instance te1/int1
  ex
interface int2
  connect port te1 service-instance te1/int2
  ex
ip route 0.0.0.0 0.0.0.0 172.16.4.1
int tunnel.0
  ip add 172.16.0.1/30
  ip mtu 1400
  ex
ip tunnel 172.16.4.2 172.16.5.2 mode gre
router ospf 1
  router-id 1.1.1.1
  network 172.16.0.0/30 area 0
  network 192.168.100.0/26 area 0
  network 192.168.200.0/28 area 0
  passive-interface default
  no passive-interface tunnel.0
int int1
  ip nat inside
  ex
int int2
  ip nat inside
  ex
int int0
  ip nat outside
  ex
ip nat pool NAT_POOL 192.168.100.1-192.168.100.62,192.168.200.1-192.168.200.14
ip nat source dynamic inside-to-outside pool NAT_POOL overload interface int0
ip pool hq-cli 192.168.200.14-192.168.200.14
dhcp-server 1
  pool hq-cli 1
    mask 28
    gateway 192.168.200.1
    dns 192.168.100.62
    domain-name au-team.irpo
    ex
  ex
interface int2
  dhcp-server 1
ntp timezone utc+5
timedatectl status
ex
write memory
```
