# Automation of the BornHack network

The BornHack network is not very big, but we have a few things that we think is needed anyway.

First of all the network is critical for the event, so any changes that might break the network - will affect all participants.

Second we have very old Brocade devices which are a bit hard to automate - RANCID runs OK, and Oxidized do too. We aim to not make a lot of changes to these after initial setup though.

So the main areas of automation are the core switches, management servers and routers. These are:

* Core switches - currently Juniper EX3300
* Core router - currently Juniper SRX
* Core management network system, Soekris OpenBSD conserver / OpenVPN
* LibreNMS server - currently Ubuntu VM https://www.librenms.org/
* Oxidized https://github.com/ytti/oxidized

Main focus is to control the network after setup, and save configs - if a device should start failing, due to rain, powers problems, age 
