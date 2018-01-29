# Notes

The core switching devices on BornHack has been two Juniper EX3300 switches with a very basic config, running in virtual chassis mode.

Below is the old config.
```
root@born-core-01> show configuration | no-more
## Last commit: 2016-08-27 19:44:30 CEST by hlk
version 15.1R2.9;
system {
    host-name born-core-01;
    time-zone Europe/Copenhagen;
    root-authentication {
        encrypted-password "...."; ## SECRET-DATA
    }
    name-server {
        10.0.42.1;
    }
    login {
        message "\n\n==========================================================\n\nAccess to this device is limited to authorized users only.\n\n WARNING: All unauthorized access is prohibited.\n\n==========================================================\n\n";
        class rancid {
            permissions [ access admin firewall interface routing secret security snmp system trace view view-configuration ];
        }
        user hlk {
            uid 6000;
            class super-user;
            authentication {
                encrypted-password "..."; ## SECRET-DATA
                ssh-dsa "ssh-dss AAAAB3NzaC1kc3MAAACBAMzGjfcnofM9W8WAAZ03O+gX/susUi1prJYRpomsnHMwICiiqLL1R5J/FUb/E79hhcVtyXLh833YeEQlCm25Jw4gmYABfRGyvcWYDs2zQd7Li1kBXlXnRoxd1pxDTZ4trUSSklosKCZYHqOhFNntc0789xLkjuZWe/IzYVJCW8wrAAAAFQCQiVrdhQWLXzogUR+mRL5WmWYPtwAAAIBvRrUEsjLMvl+EZRGnX/UtOaNyFjKvGh0dzLddjJ03H9dkAVeJDkLx9LSyUEvGDMYSY+gbBoakHCWVDnF1109y2m2UQ23NkJdgkXyWBkf4M4VKGe+Yl9M1FaR2Lvr8o5qW4hZJf8XfC7MXWTiyPxuonc7xufBj1ibU53nQVcEzlAAAAIB5cgVBZTVGKznH0/65JyhJmcMNMimtX7EV/VjnCK+JK3s8sJnNo2fu297IR+XQKlVO8oyw7n/9peYEMkksWrOPW/jvdK9ce1nLGJ81T36Ppiui8G/4GsSkSd6x3igLCxGgB+fEv0tyOyEsIbKVVZ8ySTRg+LvtNlfjA/+faAw2DA== hlk@kramse.dk"; ## SECRET-DATA
            }
        }
        user rancid {
            uid 2005;
            class rancid;
            authentication {
                encrypted-password "..."; ## SECRET-DATA
                ssh-rsa "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAwV/Lvzhr4OGlrqIZokrkI9WwNVi1vLvmHmZ1U+NIV3tYmqP3H9res6MV6Cqiuj+uKIvfW5aW0dEMpYcV/Zp9PvOzcgD4x2KhcItf59fm4HkUO2SoNFvIw8lFtRb1L2MzIX8Bk8e1usHmDPfX5zywW3Gcjp3fGAjBzf99yLtToudA4+aWm0RIZ3do18sf2VgnW0XqTFq7Y+y6O6jnIs+qbh3kLepo1Br5W8vDAmUrIASu0oMt87R70wYIqfREbevbhqT3J4SFauiBpwzTEp/EC4vlw/xAZp3B/wt9EKLarCYbjS7fgObNe7TfIW9HLcY05daVTHpSWtc/y35wIy2t6Q== rancid" from 10.100.0.129; ## SECRET-DATA
            }
        }
    }
    services {
        ssh {
            root-login deny;
            protocol-version v2;
        }
        netconf {
            ssh;
        }
        dhcp {
            traceoptions {
                file dhcp_logfile;
                level all;
                flag all;
            }
        }
    }
    syslog {
        user * {
            any emergency;
        }
        file messages {
            any notice;
            authorization info;
        }
        file interactive-commands {
            interactive-commands any;
        }
    }
    commit synchronize;
    ntp {
        boot-server 10.123.44.1;
        server 10.123.44.1;
    }
}
chassis {
    redundancy {
        graceful-switchover;
    }
    aggregated-devices {
        ethernet {
            device-count 4;
        }
    }
    auto-image-upgrade;
}
interfaces {
    interface-range mgmt {
        member-range ge-0/0/20 to ge-0/0/23;
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan44_mgmt;
                }
            }
        }
    }
    interface-range camp-net {
        member-range ge-0/1/0 to ge-0/1/1;
        member-range ge-1/1/0 to ge-1/1/1;
        member-range ge-0/0/18 to ge-0/0/19;
        member-range ge-1/0/18 to ge-1/0/19;
        unit 0 {
            family ethernet-switching {
                port-mode trunk;
                vlan {
                    members [ vlan44_mgmt ssid-bornhack ssid-bornhack-nat ssid-bornhack-open ssid-bornhack-ipv6 ohmled ];
                }
            }
        }
    }
    interface-range transit {
        member ge-0/1/2;
        member ge-1/1/2;
        member ge-0/0/13;
        member ge-1/0/13;
        description "Transit: Bornfiber uplink";
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan10_bornfiber;
                }
            }
        }
    }
    interface-range ap-net {
        member-range ge-0/0/14 to ge-0/0/16;
        member-range ge-1/0/14 to ge-1/0/16;
        unit 0 {
            family ethernet-switching {
                port-mode trunk;
                vlan {
                    members [ ssid-bornhack ssid-bornhack-nat ssid-bornhack-open ssid-bornhack-ipv6 ];
                }
                native-vlan-id 44;
            }
        }
    }
    ge-0/0/0 {
        ether-options {
            802.3ad ae0;
        }
    }
    ge-0/0/1 {
        ether-options {
            802.3ad ae1;
        }
    }
    ge-0/0/8 {
        description "Cust: random uplink for testing network";
        unit 0 {
            family ethernet-switching {
                vlan {
                    members ssid-bornhack;
                }
            }
        }
    }
    ge-0/0/9 {
        description "Cust: conserver outside";
        unit 0 {
            family ethernet-switching {
                vlan {
                    members ssid-bornhack;
                }
            }
        }
    }
    ge-0/1/0 {
        description "Core: SouthBound fiber 1";
    }
    ge-0/1/1 {
        description "Core: northBound fiber 1";
    }
    ge-1/0/0 {
        ether-options {
            802.3ad ae0;
        }
    }
    ge-1/0/1 {
        ether-options {
            802.3ad ae1;
        }
    }
    ge-1/0/2 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members ohmled;
                }
            }
        }
    }
    ge-1/0/3 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members ssid-bornhack;
                }
            }
        }
    }
    ge-1/1/0 {
        description "Core: SouthBound fiber 2";
    }
    ge-1/1/1 {
        description "Core: northBound fiber 2";
    }
    ae0 {
        description "Core: born-srx-01 reth0";
        aggregated-ether-options {
            lacp {
                active;
                periodic slow;
            }
        }
        unit 0 {
            family ethernet-switching {
                port-mode trunk;
                vlan {
                    members [ ssid-bornhack ssid-bornhack-nat ssid-bornhack-open ssid-bornhack-ipv6 ];
                }
            }
        }
    }
    ae1 {
        description "Core: born-srx-02 reth0";
        aggregated-ether-options {
            lacp {
                active;
                periodic slow;
            }
        }
        unit 0 {
            family ethernet-switching {
                port-mode trunk;
                vlan {
                    members [ ssid-bornhack ssid-bornhack-nat ssid-bornhack-open ssid-bornhack-ipv6 ];
                }
            }
        }
    }
    lo0 {
        unit 0;
    }
    vlan {
        unit 44 {
            family inet {
                address 10.123.44.10/24;
            }
        }
    }
    inactive: vme {
        unit 0 {
            family inet {
                address 10.0.42.21/24;
            }
        }
    }
}
snmp {
    description "Zencurity EX3300";
    location core-bornhack-dk;
    contact "noc@zencurity.com";
    community bornhack {
        authorization read-only;
        clients {
            10.123.44.0/23;
        }
    }
}
routing-options {
    static {
        route 0.0.0.0/0 next-hop 10.123.44.1;
    }
    router-id 10.0.10.1;
    autonomous-system 65002;
}
protocols {
    igmp-snooping {
        vlan all;
    }
    rstp {
        bridge-priority 4k;
    }
    lldp {
        interface all;
    }
    lldp-med {
        interface all;
    }
}
firewall {
    family ethernet-switching {
        filter webserver {
            term tcp-syn {
                from {
                    destination-address {
                        10.0.10.254/32;
                    }
                    protocol tcp;
                    destination-port 80;
                    tcp-initial;
                }
                then policer tcp-syn-policer;
            }
            term tcp-connection {
                from {
                    destination-address {
                        10.0.10.254/32;
                    }
                    protocol tcp;
                    destination-port 80;
                    tcp-established;
                }
                then {
                    accept;
                    policer tcp-policer;
                }
            }
            term tcp-block {
                from {
                    protocol tcp;
                }
                then discard;
            }
            term icmp-connection {
                from {
                    destination-address {
                        10.0.10.254/32;
                    }
                    protocol icmp;
                }
                then {
                    accept;
                    policer icmp-policer;
                }
            }
            term udp-connection {
                from {
                    destination-address {
                        10.0.10.254/32;
                    }
                    protocol udp;
                }
                then policer udp-policer;
            }
            inactive: term block {
                then discard;
            }
            term block-everything {
                from {
                    protocol [ 2-5 18-255 7-16 ];
                }
                then discard;
            }
        }
    }
    policer icmp-policer {
        if-exceeding {
            bandwidth-limit 5m;
            burst-size-limit 1k;
        }
        then discard;
    }
    policer tcp-policer {
        if-exceeding {
            bandwidth-limit 1m;
            burst-size-limit 30k;
        }
        then discard;
    }
    policer udp-policer {
        if-exceeding {
            bandwidth-limit 1m;
            burst-size-limit 30k;
        }
        then discard;
    }
    policer tcp-syn-policer {
        if-exceeding {
            bandwidth-limit 10m;
            burst-size-limit 10k;
        }
        then discard;
    }
}
ethernet-switching-options {
    inactive: storm-control {
        interface all;
    }
}
vlans {
    default;
    ohmled {
        vlan-id 2020;
    }
    ssid-bornhack {
        vlan-id 2016;
    }
    ssid-bornhack-ipv6 {
        vlan-id 2019;
    }
    ssid-bornhack-nat {
        vlan-id 2017;
    }
    ssid-bornhack-open {
        vlan-id 2018;
    }
    vlan10_bornfiber {
        vlan-id 10;
    }
    vlan44_mgmt {
        vlan-id 44;
        l3-interface vlan.44;
    }
}

{linecard:1}
root@born-core-01>
```
