```yml
hostname br-rtr.au-team.irpo
interface int0
  description "to isp"
  ip address 172.16.5.2/28
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
  description "br-srv"
  ip address 192.168.0.1/27
  ex
port te1
  service-instance te1/int1
    encapsulation untagged
    ex
  ex
interface int1
  connect port te1 service-instance te1/int1
  ex
ip route 0.0.0.0 0.0.0.0 172.16.5.1
username net_admin
  password P@ssw0rd
  role admin
  ex
int tunnel.0
  ip add 172.16.0.2/30
  ip mtu 1400
  ip tunnel 172.16.5.2 172.16.4.2 mode gre
  ex
router ospf 1
  router-id 2.2.2.2
  network 172.16.0.0/30 area 0
  network 192.168.0.0/27 area 0
  passive-interface default
  no passive-interface tunnel.0
  ex
int int1
  ip nat inside
  ex
int int0
  ip nat outside
  ex
ip nat pool NAT_POOL 192.168.0.1-192.168.0.30
ip nat source dynamic inside-to-outside pool NAT_POOL overload interface int0
ntp timezone utc+5
ex
show ntp timezone
```
