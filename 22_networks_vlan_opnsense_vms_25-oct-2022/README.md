## How to set up VLANs ##

### Prerequisites: ### 
- Proxmox node with ~ 16 GiB RAM and ~ 90 GB local storage
- opnsense ISO
- proxmox ISO
- manjaro ISO

### Steps: ### 

#### 1. Create linux bridge vmbr1
* Proxmox UI: System > Network > [Create] > Linux bridge
  - Name
  - VLAN aware - checked
---------------------------------------------------------------------------
#### 2. Create VM for opnsense (see [Lesson 16](../16_networks_ssl-termination_self-signed_cert_04-oct-2022))
* Set vmbr0 as WAN
* Set vmbr1 as LAN
    - set up LAN (subnet mask)
* Firewall > Rules: close web to untagged VLAN (enable rule LAN to LAN only)
---------------------------------------------------------------------------
#### 3. Create VM for manjaro > convert to template
* Create 1st linked clone from the manjaro template
  - Name: manjaro1
  - Network device: untagged VLAN (vmbr1)

* Check connectivity:
  - gateway
  - web

* Show logs:
  - Firewall > Live Log > filter by interface LAN
---------------------------------------------------------------------------
#### 4. Create 2nd linked clone from the manjaro template
- Name: manjaro2

* Go to opnsense to create VLAN 3:
  - Interfaces > Other Types > VLAN > [+]
  - Interfaces > Assignments > VLAN > [+]
  - In interfaces appeared new options, edit
    - enable
    - description
    - static ip
    - ipv4 - set address
  - Service > DHCPv4 *
  - Firewall > Aliases > RFC
  - Firewall > Rules > [+]
* Go back to manjaro:
  - Open Network Manager (nmtui)
    - edit a connection
    - wired connection to know parent edit
    - add a connection > vlan
      - device vlan03
      - parent ens18
    - activate connection
    - deactivate wired connection
    - activate vlan 3 > Ok
    - Check ip address

* Check connectivity:
  - manjaro1 to manjaro2, gateway, web
  - manjaro2 to manjaro1, gateway, web
* Show logs:
  - Firewall > Live Log

---------------------------------------------------------------------------
#### 5. Create 3rd linked clone from the manjaro template
* Create VLAN21
  - Interfaces > Other Types > VLAN > [+]
  - Interfaces > Assignments > VLAN > [+]
  - In interfaces appeared new options, edit
    - enable
    - description
    - static ip
    - ipv4 - set address
  - Service > DHCPv4
  - Firewall > Aliases > RFC
  - Firewall > Rules > [+]
* Create linked clone from the manjaro template:
  - Name: manjaro3
  - Connect it to VLAN 20 via Proxmox UI (Network settings)
* Check connectivity:
  - manjaro3 to manjaro2, manjaro1, gateway, web
  - manjaro1 to manjaro2, manjaro3
  - manjaro2 to manjaro3, manjaro1
---------------------------------------------------------------------------
#### 6. Create VM for proxmox
* Install SDN
```  
apt update
apt install libpve-network-perl ifupdown2 
```  
* Install qemu agent
```  
apt install qemu-guest-agent
```  
* Set up VLAN3 via update /etc/network/interfaces on each proxmox VM
* Convert to template
* Make 3 linked clones from the proxmox templates
* Update hostname & ip address:
```   
hostnamectl set-hostname <...>
```
```
nano /etc/hosts
```
```
nano /etc/postfix/main.cf
```
```
nano /etc/network/interfaces
```
```
ifreload -s -a
ifreload -a
```
* Re-generate certificates
```
cd /etc/pve
rm pve-root-ca.pem priv/pve-root-ca.key nodes/*/pve-ssl.{key,pem}
pvecm updatecerts --force
systemctl restart pveproxy
```
* Check connectivity:
  - each node to each node
* Open on manjaro2 each node ui in separate tab
* Create cluster

---------------------------------------------------------------------------
#### 7. Send cloudinit folder to each node
* Send ssh key to each node
* Run script to create ubuntu cloudinit template on 1st node
---------------------------------------------------------------------------
#### 8. Create 1st linked clone ubuntu VM on 1st node
* Create VLAN20 in SDN
* Start VM
* In case any correction to network configuration is necessary, it might be done this way:
  ```
  nano /etc/netplan/50-cloud-init.yaml
  netplan apply
  ```
* Check connectivity:
  - gateway
  - web
  - manjaro1
  - manjaro2
  - manjaro3
---------------------------------------------------------------------------
#### 9. Create 2nd ubuntu VM cloned linked from template on 1 node
* Create VLAN21
* Set up VLAN21 in opnsense:
  - Interfaces > Other Types > VLAN > [+]
  - Interfaces > Assignments > VLAN > [+]
  - In interfaces appeared new options, edit
    - enable
    - description
    - static ip
    - ipv4 - set address
  - Service > DHCPv4
  - Firewall > Aliases > RFC
  - Firewall > Rules
    - Set connectivity VLAN20 : VLAN21
* In case any correction to network configuration is necessary, it might be done this way:
```
nano /etc/netplan/50-cloud-init.yaml
netplan apply
```
* Check connectivity:
  - gateway
  - web
  - VLAN3
  - ubuntu1
---------------------------------------------------------------------------
#### 10. Run script to create ubuntu template on 2nd node
- Create 1st linked clone ubuntu VM on 2nd node
- Connect to VLAN21
- In case any correction to network configuration is necessary, it might be done this way:
```
nano /etc/netplan/50-cloud-init.yaml
netplan apply
```
- Check connectivity:
  - gateway
  - web
  - VLAN3
  - ubuntu1
  - ubuntu2
---------------------------------------------------------------------------
#### 11. Create 2nd linked clone ubuntu VM on 2nd node
- Connect to VLAN22
- Create VLAN22
- Set up VLAN22 in opnsense:
  - Interfaces > Other Types > VLAN > [+]
  - Interfaces > Assignments > VLAN > [+]
  - In interfaces appeared new options, edit
    - enable
    - description
    - static ip
    - ipv4 - set address
  - Service > DHCPv4
  - Firewall > Aliases > RFC
  - Firewall > Rules
- In case any correction to network configuration is necessary, it might be done this way:
```
nano /etc/netplan/50-cloud-init.yaml
netplan apply
```
- Check connectivity:
  - gateway
  - web
  - VLAN3
  - VLAN20
  - VLAN21
---------------------------------------------------------------------------
#### 12. Run script to create ubuntu template on 3rd node
- Create 1st linked clone ubuntu VM on 3rd node
- Connect to VLAN20
- In case any correction to network configuration is necessary, it might be done this way:
```
nano /etc/netplan/50-cloud-init.yaml
netplan apply
```
- Check connectivity:
  - gateway
  - web
  - VLAN3
  - VLAN20
  - VLAN21
  - VLAN22
---------------------------------------------------------------------------
#### 13. Create 2nd linked clone ubuntu VM on 3rd node
- Connect to VLAN22
- In case any correction to network configuration is necessary, it might be done this way:
```
nano /etc/netplan/50-cloud-init.yaml
netplan apply
```
- Check connectivity:
  - gateway
  - web
  - VLAN3
  - VLAN20
  - VLAN21
  - VLAN22
- Set up network connection via nmtui
  - edit a connection
  - wired connection to know parent edit
  - add a connection
    - type VLAN 
    - device vlan03
    - parent ens18
  - activate connection
  - deactivate wired connection
  - activate VLAN3
- Check IP address:
```
ip a
```

### References ###

1. [What are VLANs? - The simplest explanation](https://www.youtube.com/watch?v=MmwF1oHOvmg)
2. [Native VLAN - the DEFINITIVE illustration](https://www.youtube.com/watch?v=Fmq1E1Qr2W4)
3. [Set-VMNetworkAdapterVlan](https://learn.microsoft.com/en-us/powershell/module/hyper-v/set-vmnetworkadaptervlan?view=windowsserver2022-ps)
4. [Proxmox Network Configuration: VLAN](https://pve.proxmox.com/wiki/Network_Configuration#_vlan_802_1q)
5. [Software Defined Network (SDN) integration with Proxmox](https://pve.proxmox.com/pve-docs/chapter-pvesdn.html)
6. [Cloud-Init Support in Proxmox](https://pve.proxmox.com/wiki/Cloud-Init_Support)
7. [How to Configure Networking on Ubuntu with Netplan](https://vitux.com/how-to-configure-networking-with-netplan-on-ubuntu/)
8. [Configuring IP Networking with nmtui](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-configuring_ip_networking_with_nmtui)