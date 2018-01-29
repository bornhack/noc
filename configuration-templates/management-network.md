# BornHack Management network

We have used a very nice management setup for BornHack.

Using a Soekris 5501 with multi-port serial card we could have a small OpenBSD controlling the core parts of the network.

The management-vlan 44 and the IP prefix 10.123.44.0/23 has been used for this.

The OpenBSD was also used as the default-gateway, NTP server, DNS etc. for the devices on the management network.

It is also possible to create VPN on the OpenBSD, so we added OpenVPN access in 2017.
