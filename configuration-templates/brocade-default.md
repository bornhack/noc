# Notes

The main distribution switches we put out into the field are Brocade switches.

They are various models, but the below config works on most of them.



Sometimes the IP on the management-vlan may have trouble.
Try
```
unset management-vlan
```
and then set it again

Current configuration - as used since 2016 which is the base VLAN ID:
```
!
ver 08.0.30hT311
!
stack unit 1
  module 1 icx6430-24-port-management-module
  module 2 icx6430-sfp-4port-4g-module
!
!
!
!
spanning-tree single
!
vlan 1 name DEFAULT-VLAN by port
 spanning-tree
!
vlan 44 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 spanning-tree
 management-vlan
 default-gateway  10.123.44.1 1
!
vlan 2016 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 untagged ethe 1/1/7 to 1/1/24
 spanning-tree
!
vlan 2017 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 spanning-tree
!
vlan 2018 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 spanning-tree
!
vlan 2019 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 spanning-tree
!
vlan 2020 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 spanning-tree
!
vlan 2021 by port
 tagged ethe 1/1/1 to 1/1/6 ethe 1/2/1 to 1/2/2
 spanning-tree
!
!
!
!
!
aaa authentication login default local
enable aaa console
hostname south1
ip address 10.123.44.15 255.255.255.0
ip dns domain-list bornhack.dk
ip dns server-address 8.8.8.8
no ip dhcp-client enable
!
logging host 10.123.44.1
logging facility local0
no telnet server
username hlk password ...
snmp-server community bornhack ro
snmp-server contact noc@bornhack.dk
snmp-server location south-bornhack-dk
!
!
clock summer-time
clock timezone gmt GMT+01
!
!
ntp
 server 10.123.44.1
!
!
interface ethernet 1/1/1
 dual-mode  44
!
interface ethernet 1/1/2
 dual-mode  44
!
interface ethernet 1/1/3
 dual-mode  44
!
interface ethernet 1/1/4
 dual-mode  44
!
interface ethernet 1/1/5
 dual-mode  44
!
interface ethernet 1/1/6
 dual-mode  44
!
!
!
!
lldp run
!
!
ip ssh  idle-time 20
!
!
end
```
