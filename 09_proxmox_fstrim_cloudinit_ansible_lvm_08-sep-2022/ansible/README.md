## Ansible simple example demo ## 

### Prerequisites: ###

1. Ansible should be installed on the control VM (where the playbooks will be run).

```
sudo dnf install epel-release
sudo dnf install ansible
```

2. The playbook is placed in the following directory:

```
cd ./ansible
nano ./playbooks/change-hostnames.yml
```

3. The SSH key to managed machines should be placed in .ssh directory:

```
nano ~/.ssh/id_rsa_cloudinit
```

### Steps: ###

A hosts.ini file is necessary to specify the IP addresses of the hosts supposed to get affected by the playbook. 
The file is placed into subdirectory named cloudinit-vms. Edit the IP addressees if necessary.
```
nano ./inventory/cloudinit-vms/hosts.ini
```

Once the IPs and the preferable hostnames are specified, ssh-agent has to be enabled:
```
eval `ssh-agent`
ssh-add ~/.ssh/id_rsa_cloudinit
```

Then run the playbook:
```
ansible-playbook -i ./inventory/cloudinit-vms/hosts.ini -v ./playbooks/change-hostnames.yml
```

In case info about roles is necessary:
```
ansible-galaxy role --help
```

To install a new role:
```
ansible-galaxy role install bodsch.snapd
```
The role should appear in the list:
```
ansible-galaxy role list
```

