# Proxmox
I've recently started to play with Proxmox. 

In my home country I had full control of my home router, here in Poland the router is managed by the ISP; so I have no control
over port openings, DHCP reservations, etc.

Configuring Proxmox for WLAN is hard to do it (Or so they say) Ref: https://pve.proxmox.com/wiki/WLAN

These are the things I've done so far for my setup, I'll add more as needed.

1. DHCP

Proxmox likes static IPs, however is possible to use it with DHCP without reservations.
Edit /etc/network/interfaces and change:
```
iface vmbr0 inet static
       address 192.168.100.2/24
       gateway 192.168.100.1
       bridge-ports enp2s0f0
       bridge-stp off
       bridge-fd 0
```
For:
```
iface vmbr0 inet dhcp
        bridge-ports enp2s0f0
        bridge-stp off
        bridge-fd 0
```
