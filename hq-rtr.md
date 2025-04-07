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
```
