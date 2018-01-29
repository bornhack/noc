# Physical Setup at BornHack NOC

The physical setup has multiple parts, some can be executed in parallel.

## Uplink fiber

We have used a small cheap TP-Link media-converter Gigabit with single-mode 1310nm SPF for converting the fiber connecting into copper cable, which was then routed to the main room, skolestuen.

## Core switches

The core switching devices are two Juniper EX3300 switches with each 4xSFP+ 10Gbit. We have used a twinax copper cable for connecting these into a virtual chassis.

## Distribution Switches and datenklos.

We have used boxes borrowed from the municipality for PoPs, datenklos.

These were then equippped with each:
* Powercable with ground!
* Brocade switch
* One or more Access Points
* Fiber uplinks and downlinks using single-mode 1310nm Flexoptics SFPs

## Server Hosting and Virtual Setup

We have used VMware ESXi for creating a few helpful virtual machines. The main ones are:
* Network Management System NMS, LibreNMS
* Oxidized for gathering configurations
* UniFi Wireless Controller

These servers were connected to the core switches.
