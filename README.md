# Home Lab Infrastructure

Enterprise-style personal lab environment built for hands-on learning in networking, systems administration, virtualization, cybersecurity, and infrastructure management.

## Overview

This home lab is hosted on a physical Hyper-V server named **The Thinker** and connected to a residential network through a Spectrum modem/router. A virtualized **pfSense firewall** provides network segmentation, routing, NAT, DHCP, and security controls for a dedicated internal lab environment.

The lab is isolated from the primary home network, allowing safe testing, realistic enterprise simulations, and deployment of infrastructure services.

## Network Architecture

* Home Network: `192.168.1.0/24`
* Lab Network: `192.168.100.0/24`
* Virtual Firewall: pfSense
* Hypervisor: Microsoft Hyper-V

## Infrastructure Components

### Windows Server 2019 VM

* Active Directory Domain Services
* DNS management
* User/account administration
* Group policy practice

### Docker Host

* Portainer container management
* Pi-hole DNS filtering
* Lightweight self-hosted services

### Ubuntu Linux VM

* SSH administration
* Bash scripting
* Linux system management
* Open-source software testing

## Skills Demonstrated

* Network segmentation and subnetting
* Firewall configuration and access control
* Virtualization with Hyper-V
* Windows Server administration
* Linux administration
* Docker container deployment
* DNS and directory services
* Troubleshooting and system maintenance

## Purpose

This environment is used to build real-world IT skills through practical experience in:

* Enterprise networking concepts
* Infrastructure deployment
* Security hardening
* Server management
* Automation and scripting
* Troubleshooting scenarios

## Future Enhancements

* VLAN implementation
* VPN remote access
* Centralized logging / SIEM
* Monitoring with Grafana or Zabbix
* Automated backups
* Additional Windows/Linux servers

## Author

Built and maintained by pipped as a continuous learning platform for IT and network engineering development.
