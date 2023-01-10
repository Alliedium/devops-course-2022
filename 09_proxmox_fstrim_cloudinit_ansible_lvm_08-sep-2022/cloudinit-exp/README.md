## Creating Cloud-Init-initialized VM template and clones via Proxmox command line ## 

### Prerequisites: ###

1. The cloud image has to be installed on the node level, for example this one:
```
wget https://download.rockylinux.org/pub/rocky/9.0/images/x86_64/Rocky-9-GenericCloud-9.0-20220706.0.x86_64.qcow2
```
2. There should be a copy of SSH key (passed either via scp or ssh-copy-id from the another machine user is going to manage the VMs from)

### Steps: ###
               
1. Open the bash script:
```
cd ./cloudinit-exp
nano ./create-rocky9-dc0.sh
```

create-rocky9-dc0.sh :
```
#!/bin/bash
set -e
#pvesh create /pools --poolid k3s-rocky9-dc0
qm create 6000 --memory 4096 --cores 12  --name rocky9-cloud --net0 virtio,bridge=e1v44 --pool k3s-rocky9-dc0
# qemu-img resize /root/Rocky-9-GenericCloud-9.0-20220706.0.x86_64.qcow2 +10G
qm importdisk 6000 /root/Rocky-9-GenericCloud-9.0-20220706.0.x86_64.qcow2 xpool_vmstorage
qm set 6000 --scsihw virtio-scsi-pci --scsi0 xpool_vmstorage:vm-6000-disk-0
qm set 6000 --ide2 xpool_vmstorage:cloudinit
qm set 6000 --boot c --bootdisk scsi0
qm set 6000 --serial0 socket --vga serial0
#for Rocky9
qm set 6000 --machine q35 --bios ovmf --cpu host
qm set 6000 --efidisk0 xpool_vmstorage:0
#
qm clone 6000 6001 --name rocky9-cloud-template -pool k3s-rocky9-dc0 --full
#
qm set 6001 --sshkey ~/.ssh/id_rsa_cloudinit.pub
qm set 6001 --ipconfig0 ip=10.44.106.11/20,gw=10.44.111.1
qm set 6001 --nameserver 10.44.111.1
qm set 6001 --ciuser peter
#
qm template 6001
#
qm clone 6001 6011 --name k3s-r9-dc0-master1 --pool k3s-rocky9-dc0 --full
qm set 6011 --ipconfig0 ip=10.44.106.11/24,gw=10.44.111.1
#
qm clone 6001 6012 --name k3s-r9-dc0-master2 --pool k3s-rocky9-dc0 --full
qm set 6012 --ipconfig0 ip=10.44.106.12/24,gw=10.44.111.1
#
qm clone 6001 6013 --name k3s-r9-dc0-master3 --pool k3s-rocky9-dc0 --full
qm set 6013 --ipconfig0 ip=10.44.106.13/24,gw=10.44.111.1
```

#### Note: It might be necessary to edit parameters according to your needs. ####

2. Run the script (or commands from it one by one): 
```
./create-rocky9-dc0.sh
```
