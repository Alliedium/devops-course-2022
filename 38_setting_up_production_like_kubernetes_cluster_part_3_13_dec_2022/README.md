# Demo on: Setting up a production-like Kubernetes cluster for the first time, part 4, 13 Dec 2022

## Creating VMs on Proxmox node
  
  ### Create 3 VMs on Proxmox node for `k3s` cluster via [awesome-linux-config's scripts](https://github.com/Alliedium/awesome-linux-config). Follow step 4 form [29 lecture](../29_configuring_opnsense_and_creating_vms_via_scripts_and_manual_10_nov_2022/README.md).

## Install and manual configure VyOS
  
  ### 1. [Download VyOS iso image](https://vyos.net/get/nightly-builds/)
  ### 2. [Install VyOS](https://docs.vyos.io/en/equuleus/installation/install.html).
  - Create VM from VyOS image.
  - Start it.
  - Log into the VyOS live system (use the default credentials: `vyos`, `vyos`).
  - [Run command](https://docs.vyos.io/en/latest/installation/install.html)
  
  ```
  install image
  ```
  - Reboot the system.
  - Take snapshots 

  ### 3. VyOS configuration.
   - By default, VyOS is in operational mode, and the command prompt displays a $. To [configure VyOS](https://docs.vyos.io/en/equuleus/quick-start.html), you will need to enter configuration mode, resulting in the command prompt displaying a #, as demonstrated below:
  
  ```
  configure
  ```
  
  or

  ```
  config
  ```
   - Configure VyOS for `ssh` connection. Set WAN IP address
  
  ```
  set interfaces ethernet eth0 address '10.44.99.74/20'
  ```
   * Default getway
  
  ```
  set protocols static route 0.0.0.0/0 next-hop '10.44.111.1'
  ```
   * SSH Management
  
  ```
  set service ssh port '22'
  ```

  From your work station go to `VyOS` via ssh.

  ```
  ssh -o UserKnownHostsFile=/dev/null -i ~/.ssh/id_rsa_cloudinit_k3s vyos@10.44.99.74 -o StrictHostKeyChecking=no
  ```

   - enter configuration mode
  
  ```
  config
  ```

   - Interface Configuration

  * Your outside/WAN interface will be eth0. It will use a static IP address of 10.44.99.74/20.
  * Internal traffic in VLAN ID 10.
  * Your internal/LAN interface will be eth0.10. It will use a static IP address of 10.10.0.1/24.
  
  ```  
  set interfaces ethernet eth0 description 'OUTSIDE'
  set interfaces ethernet eth0 vif 10 address '10.10.0.1/24'
  set interfaces ethernet eth0 vif 10 description 'INSIDE'
  ```
  where `10` - is VLAN ID.

  * DHCP
  
  ```
  set service dhcp-server shared-network-name LAN subnet 10.10.0.0/24 default-router '10.10.0.1'
  set service dhcp-server shared-network-name LAN subnet 10.10.0.0/24 name-server '10.10.0.1'
  set service dhcp-server shared-network-name LAN subnet 10.10.0.0/24 domain-name 'vyos.net'
  set service dhcp-server shared-network-name LAN subnet 10.10.0.0/24 lease '86400'
  set service dhcp-server shared-network-name LAN subnet 10.10.0.0/24 range 0 start '10.10.0.100'
  set service dhcp-server shared-network-name LAN subnet 10.10.0.0/24 range 0 stop '10.10.0.200'
  ```

  * DNS 
  
  ```
  set service dns forwarding cache-size '0'
  set service dns forwarding listen-address '10.10.0.1'
  set service dns forwarding allow-from '10.10.0.0/24'
  set service dns forwarding system
  ```

  * NAT
  
  ```
  set nat source rule 100 outbound-interface 'eth0'
  set nat source rule 100 source address '10.10.0.0/24'
  set nat source rule 100 translation address 'masquerade'
  ```

  * Firewall
  
  Set net group 
  
  ```
  set firewall group network-group RFC1918 network '10.0.0.0/8'
  set firewall group network-group RFC1918 network '172.16.0.0/12'
  set firewall group network-group RFC1918 network '192.168.0.0/16'
  ```

  Add a set of firewall policies for our inside/LAN interface.
  This configuration creates a proper stateful firewall that blocks all traffic which was initiated from the internal/LAN side first and send to private subnets.

  ```
  set firewall name INSIDE-OUT default-action 'drop'
  set firewall name INSIDE-OUT rule 10 action 'accept'
  set firewall name INSIDE-OUT rule 10 state established 'enable'
  set firewall name INSIDE-OUT rule 10 state related 'enable'
  set firewall name INSIDE-OUT rule 20 action 'accept'
  set firewall name INSIDE-OUT rule 20 description 'accept internet'
  set firewall name INSIDE-OUT rule 20 destination group network-group '!RFC1918'
  ```

  Set firewall rule that blocks all traffic to VyOS, except on 22 port.
  
  ```
  set firewall name OUTSIDE-LOCAL default-action 'drop'
  set firewall name OUTSIDE-LOCAL rule 10 action 'accept'
  set firewall name OUTSIDE-LOCAL rule 10 state established 'enable'
  set firewall name OUTSIDE-LOCAL rule 10 state related 'enable'
  set firewall name OUTSIDE-LOCAL rule 31 action 'accept'
  set firewall name OUTSIDE-LOCAL rule 31 destination port '22'
  set firewall name OUTSIDE-LOCAL rule 31 protocol 'tcp'
  set firewall name OUTSIDE-LOCAL rule 31 state new 'enable'
  ```

  Apply the firewall policies:

  ```
  set firewall interface eth0 local name 'OUTSIDE-LOCAL'
  set firewall interface eth0.10 in name 'INSIDE-OUT'
  ```
  
  * Hostname
  
  ```
  set system host-name 'vyos-1'
  ```
  * Add new user `vyos-user`
  
  ```
  set system login user vyos-user full-name 'vyos-user'
  set system login user vyos-user authentication plaintext-password 'vyos-user'
  set system login user vyos-user authentication public-keys Adam key 'AAAA ..... ik@DESKTOP-D8C0475'
  set system login user vyos-user authentication public-keys Adam type 'ssh-rsa'
  ```

  * DNS
  
  ```
  set system name-server '10.44.111.1'
  ```
  * NTP servers
  
  ```
  set system ntp server 1.pool.ntp.org
  set system ntp server 2.pool.ntp.org
  ```

  * Port forwarding. In the example below, traffic comes in from the WAN subnet on port 4722 and is redirected to port 22 on the LAN subnet for host 10.10.0.116, thus you can establish ssh connection  to LAN host 10.10.0.116 on 4722 port using WAN ip address.
  
  ```
  set nat destination rule 70 description 'Port Forward public ssh port 4722 to bastion 10.10.0.116 port 22'
  set nat destination rule 70 destination port '4722'
  set nat destination rule 70 inbound-interface 'eth0'
  set nat destination rule 70 protocol 'tcp'
  set nat destination rule 70 translation address '10.10.0.116'
  set nat destination rule 70 translation port '22'
  set nat destination rule 110 description 'NAT Reflection: INSIDE'
  set nat destination rule 110 destination port '8006'
  set nat destination rule 110 inbound-interface 'eth0'
  set nat destination rule 110 protocol 'tcp'
  set nat destination rule 110 translation address '10.10.0.116'
  ```

  * Show changes
  
  ```
  show
  ```

  * Show rules

  ### 4. Commit and Save

  * After every configuration change, you need to apply the changes by using the following command:

  ```
  commit
  ```

  * Once your configuration works as expected, you can save it permanently by using the following command:
  
  ```
  save
  ```

  * exit from configuration mode
  
  ```
  exit
  ```

  * If you do not want to commit and save changes run commands
  
  ```
  exit discard
  ```

  ### 5. Show `VyOS` configuration.

  ```
  show configuration commands
  ```

  * Show network rules using [nftables](https://wiki.nftables.org/wiki-nftables/index.php/Quick_reference-nftables_in_10_minutes)
  
  ```
  sudo nft list ruleset
  ```

## Create `VyOS` vm on Proxmox node via [ansible playbook](https://github.com/Alliedium/awesome-proxmox)
  
  ### 1. Preparing [`VyOS` cloud-init image](https://github.com/vyos/vyos-vm-images).

  * Create `Debian` VM via Scripts
  * Go to `Debian` via ssh
  
  ```
  ssh -o UserKnownHostsFile=/dev/null -i ~/.ssh/id_rsa_cloudinit_k3s k3s-user@10.44.99.81 -o StrictHostKeyChecking=no
  ```
  
  * Clone `vyos/vyos-vm-images` project
  
  ```
  git clone https://github.com/vyos/vyos-vm-images.git
  ```

  * Follow steps from `Requirements` sections.
  * Create `VyOS` cloud-init image.
  
  ```
  sudo ansible-playbook qemu.yml -e disk_size=2  -e iso_local=/tmp/vyos.iso -e grub_console=kvm -e vyos_version=1.4.5  -e cloud_init=true -e keep_user=true -e enable_ssh=true -e cloud_init_ds=NoCloud
  ```
  * Copy created image where you need
  
  ### 2. Clone [Alliedium/awesome-proxmox](https://github.com/bbs-md/awesome-proxmox/edit/main/vyos-proxmox-kvm/README.md) project on a certain Linux host from which Ansible playbooks are to be run, this host is called for brevity below `Ansible host`.

  ```
  git clone https://github.com/Alliedium/awesome-proxmox.github
  ```

  ### 3. Follow steps in README.md from the `Prerequisites` and `Clone awesome-proxmox project and configure the files` sections

  ### 4. Enable ssh access on Proxmox node for `root` user
  * on Proxmox node edit `etc/ssh/sshd_config` file and uncomment the line
  
  ```
  PermitRootLogin yes
  ```

  * then run the command to restart sshd service
  
  ```
  systemctl restart sshd
  ```
  
  ### 5. Run ansible playbooks on `Ansible host`

  ```
  ansible-playbook -i ./inventory/my-vyos ./playbooks/batch-create-start.yml
  ```

## Install `k3s` cluster via [ansible playbook](https://github.com/techno-tim/k3s-ansible) on VMs

  ### 1. Clone `techno-tim/k3s-ansible` project on your host.

  ```
  git clone https://github.com/techno-tim/k3s-ansible.git
  ```

  ### 2. Follow steps from `System requirements` and `Preparation`
  - Navigate to `k3s-ansible` folder
  - Copy `./inventory/sample`  to `/inventory/my-cluster` folder
  - Edit `./inventory/my-cluster/hosts.yml` and  `./inventory/my-cluster/group_vars/all.yml` files
  
  ### 3. Create `k3s` Cluster

  * Start provisioning of the cluster using the following command:
  
  ```
  ansible-playbook site.yml -i inventory/my-cluster/hosts.yml
  ```

  ### 4. Kube Config
  * To copy your kube config locally so that you can access your Kubernetes cluster run:
  
  ```
  scp -o StrictHostKeyChecking=no -i ~/.ssh/id_rsa_cloudinit_k3s k3s-user@10.10.0.11:~/.kube/config ~/.kube/config
  ```
  ### 5. Using `OpenLens` open `~/.kube/config` file.

  ## References on: Setting up a production-like Kubernetes cluster for the first time, part 4, 13 Dec 2022 ##

1. [download vyos image](https://vyos.net/get/nightly-builds/)
2. [Vyos Installation](https://docs.vyos.io/en/equuleus/installation/install.html)
3. [zengkid/build-vyos-lts](https://github.com/zengkid/build-vyos-lts/releases)
4. [VyOS cloud-init](https://docs.vyos.io/en/latest/automation/cloud-init.html)
5. [vyos-vm-images project](https://github.com/vyos/vyos-vm-images)
6. [How to paste configuration in vyos](https://forum.vyos.io/t/how-to-paste-configuration-in-vyos/612/5)
7. [Alliedium/awesome-proxmox](https://github.com/Alliedium/awesome-proxmox)
8. [Ansible role proxmox_create_kvm](https://github.com/UdelaRInterior/ansible-role-proxmox-create-kvm)
9. [Provision Proxmox VMs with Ansible, quick and easy](https://vectops.com/2020/01/provision-proxmox-vms-with-ansible-quick-and-easy/)
10. [techno-tim/k3s-ansible](https://github.com/techno-tim/k3s-ansible)
11. [kube-vip](https://kube-vip.io/)
12. [MetalLB](https://docs.k0sproject.io/head/examples/metallb-loadbalancer/)
