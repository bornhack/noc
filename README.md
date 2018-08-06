# BornHack NOC

Stuff for the BornHack NOC team

We will try to assemble our scripts, configurations and other things needed to run the network at BornHack.

See more about BornHack at www.BornHack.dk

## Generic checklist NOC stuff

### Before camp

- [ ] Apply for temp resources IPv4, IPv6 and ASN from RIPE NCC

Things to find and bring

- [ ] Bring armored 100m fiber cables
- [ ] Core switches Juniper EX3300
- [ ] Core routers Juniper SRX
- [ ] SFPs, thank you flexoptics!
- [ ] Wifi Access points, dont forget power injectors and cables
- [ ] Patch cables, BornHack owns a box with small and longer, bring more
- [ ] Extension cords for network stuff
- [ ] Electrical tools, screwdriver with voltage tester
- [ ] Wire stripper, cable crimping tools whatever

Notes:
in 2018 wifi is handled by a dedicated resource, outside of NOC
Also servers are handled by the server team

### Build-up - prioritized
- [ ] Setup core router
- [ ] Setup core switching
- [ ] Setup South PoPs, south3 closest to house first
- [ ] Setup North PoPs, direct fiber from core
- [ ] Setup family area West1 PoP, direct fiber from core
- [ ] Setup West2 PoP left from South3, fiber from south3
- [ ] Setup PoP Speaker tent south1, fiber from west2

It is recommended to setup a few priority PoPs in this order:
1. Core in house
2. South3 central PoP
3. West2 - to reach speacker tent
4. South1 - speaker tent


Details are copied into project plan on Github 2018 edition
