# Proxmox
I've recently started to play with Proxmox. 

In my home country I had full control of my home router, here in Poland the router is managed by the ISP; so I have no control
over port openings, DHCP reservations, etc.

Configuring Proxmox for WLAN is hard to do it (Or so they say) Ref: https://pve.proxmox.com/wiki/WLAN

These are the things I've done so far for my setup, I'll add more as needed.

## 1. DHCP

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

Create a new file /etc/dhcp/dhclient-exit-hooks.d/update-etc-hosts with:
```
if ([ $reason = "BOUND" ] || [ $reason = "RENEW" ])
then
  sed -i "s/^.*\spve.local\s.*$/${new_ip_address} pve.local pve/" /etc/hosts
fi
```
NOTE: The entry MUST exist previously, the hook WON'T create it. Of course pve.local is my system, you need to replace names accordingly.

## 2. Repositories

Although it can be used for free, Proxmox has a subscription licensing model.
Comment all the enterprise repositories if listed in sources.list or under sources.list.d and add this one:
```
# PVE pve-no-subscription repository provided by proxmox.com
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
```
NOTE: Replace bookworm for the corresponding Debian version, if needed.


