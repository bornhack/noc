
# Notes from router configuration

The core router on BornHack has been Juniper SRX with a very basic config, running BGP an announcing our ASN, IPv4 and IPv6 to BornFiber.

We have tried running DHCP on the devices and in VMs, both methods work fine.

We used them in PACKET MODE - so no firewall

Below is the config.
```
## Last changed: 2017-08-25 16:18:12 UTC
version 12.1X46-D66.1;
system {
    host-name born-srx-01;
    root-authentication {
        encrypted-password "..."; ## SECRET-DATA
    }
    name-server {
        91.239.100.100;
    }
    login {
        user bhadmin {
            uid 2000;
            class super-user;
            authentication {
                encrypted-password "..."; ## SECRET-DATA
            }
        }
        user hlk {
            uid 6000;
            class super-user;
            authentication {
                encrypted-password "..."; ## SECRET-DATA
                ssh-ed25519 "ssh-ed25519 AA...5 user@Personal"; ## SECRET-DATA
            }
        }
    }
    services {
        ssh {
            root-login deny-password;
            protocol-version v2;
        }
        dhcp {
            router {
                172.16.0.1;
            }
            pool 172.16.0.0/22 {
                address-range low 172.16.0.20 high 172.16.2.254;
            }
        }
    }
    syslog {
        archive size 100k files 3;
        user * {
            any emergency;
        }
        file messages {
            any critical;
            authorization info;
        }
        file interactive-commands {
            interactive-commands error;
        }
    }
    max-configurations-on-flash 5;
    max-configuration-rollbacks 5;
    license {
        autoupdate {
            url https://ae1.juniper.net/junos/key_retrieval;
        }
    }
}
interfaces {
    ge-0/0/0 {
        unit 0 {
            family inet {
                address 185.108.252.2/27;
            }
            family inet6 {
                address 2a00:4820:bad:beef::2/64;
            }
        }
    }
    ge-0/0/1 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/2 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/3 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/4 {
        description "Core: Camp Networks";
        vlan-tagging;
        unit 2016 {
            vlan-id 2016;
            family inet {
                address 151.217.0.1/22;
            }
            family inet6 {
                address 2001:7fc:2:2016::1/64;
            }
        }
        unit 2017 {
            vlan-id 2017;
            family inet {
                address 172.16.0.1/22;
            }
        }
    }
    ge-0/0/5 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/6 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/7 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/8 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/9 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/10 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/11 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/12 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/13 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    ge-0/0/14 {
        unit 0 {
            family ethernet-switching {
                vlan {
                    members vlan-trust;
                }
            }
        }
    }
    vlan {
        unit 0 {
            family inet {
                address 192.168.1.1/24;
            }
        }
    }
}
routing-options {
    rib inet6.0 {
        static {
            route 2001:7fc:2::/48 discard;
        }
    }
    static {
        route 151.217.0.0/21 discard;
    }
    autonomous-system 58367;
}
protocols {
    router-advertisement {
        interface ge-0/0/4.2016 {
            managed-configuration;
            prefix 2001:7fc:2:2016::/64;
        }
    }
    bgp {
        group bornfiber_ipv4 {
            authentication-key "..."; ## SECRET-DATA
            export export-bornfiber-ipv4;
            peer-as 24800;
            neighbor 185.108.252.1;
        }
        group bornfiber_ipv6 {
            authentication-key "..."; ## SECRET-DATA
            export export-bornfiber-ipv6;
            peer-as 24800;
            neighbor 2a00:4820:bad:beef::1;
        }
    }
    stp;
}
policy-options {
    prefix-list bornhack_ipv4 {
        151.217.0.0/21;
    }
    prefix-list bornhack_ipv6 {
        2001:7fc:2::/48;
    }
    policy-statement export-bornfiber-ipv4 {
        term bornhack_v4 {
            from {
                prefix-list bornhack_ipv4;
            }
            then accept;
        }
    }
    policy-statement export-bornfiber-ipv6 {
        term bornhack_v6 {
            from {
                prefix-list bornhack_ipv6;
            }
            then accept;
        }
    }
}
security {
    forwarding-options {
        family {
            inet6 {
                mode packet-based;
            }
        }
    }
    screen {
        ids-option untrust-screen {
            icmp {
                ping-death;
            }
            ip {
                source-route-option;
                tear-drop;
            }
            tcp {
                syn-flood {
                    alarm-threshold 1024;
                    attack-threshold 200;
                    source-threshold 1024;
                    destination-threshold 2048;
                    timeout 20;
                }
                land;
            }
        }
    }
    nat {
        source {
            pool src-nat-pool-1 {
                address {
                    151.217.0.11/30;
                }
                port {
                    no-translation;
                }
                address-shared;
            }
            rule-set trust-to-untrust {
                from zone trust;
                to zone untrust;
                rule source-nat-rule {
                    match {
                        source-address 0.0.0.0/0;
                    }
                    then {
                        source-nat {
                            pool {
                                src-nat-pool-1;
                            }
                        }
                    }
                }
            }
        }
    }
    policies {
        from-zone trust to-zone untrust {
            policy trust-to-untrust {
                match {
                    source-address any;
                    destination-address any;
                    application any;
                }
                then {
                    permit;
                }
            }
        }
        from-zone untrust to-zone untrust {
            policy allow-all {
                match {
                    source-address any;
                    destination-address any;
                    application any;
                }
                then {
                    permit;
                }
            }
        }
    }
    zones {
        security-zone trust {
            host-inbound-traffic {
                system-services {
                    all;
                }
                protocols {
                    all;
                }
            }
            interfaces {
                vlan.0;
                ge-0/0/4.2017 {
                    host-inbound-traffic {
                        system-services {
                            ping;
                            dhcp;
                            ssh;
                        }
                    }
                }
            }
        }
        security-zone untrust {
            screen untrust-screen;
            host-inbound-traffic {
                system-services {
                    ping;
                    ssh;
                }
                protocols {
                    bgp;
                }
            }
            interfaces {
                ge-0/0/0.0;
                ge-0/0/4.2016 {
                    host-inbound-traffic {
                        system-services {
                            ping;
                        }
                    }
                }
            }
        }
    }
}
firewall {
    family inet {
        filter bypass-flow-filter {
            term bypass-flow-term-1 {
                from {
                    source-address {
                        151.217.0.0/21;
                    }
                    destination-address {
                        0.0.0.0/0;
                    }
                }
                then packet-mode;
            }
            term accept-rest {
                then accept;
            }
        }
    }
}
vlans {
    vlan-trust {
        vlan-id 3;
        l3-interface vlan.0;
    }
}

[edit]
bhadmin@born-srx-01#
```
