# Home-Lab
Personal Homelab
//My home lab is a virtualized network environment designed for testing, development, and system administration practice. The lab runs on a physical Hyper-V host named "The Thinker", connected to my home network via a Spectrum modem/router. The Hyper-V environment hosts a pfSense virtual firewall that segments the network into a dedicated lab subnet (192.168.100.0/24) using an internal virtual switch.

The pfSense firewall routes traffic between the lab subnet and the external home network (192.168.1.0/24), allowing for NAT, DHCP, and firewall rule customization. The lab network includes:

A Windows Server 2019 VM (192.168.100.101) for Active Directory, DNS, and domain services.

A Docker container host (192.168.100.102) running lightweight services and tools such as Portainer and Pi-hole.

A Linux (Ubuntu) VM (192.168.100.103) used for scripting, SSH management, and testing open-source applications.

The entire setup is isolated from the home network, enabling realistic enterprise simulations and secure experimentation with network configurations, firewalls, and server deployments
//
