## Ansible simple example demo ## 

### Prerequisites: ###

1. We should have ansible installed on the vm we want to execute the ansible playbook on.

```
sudo dnf install epel-release
sudo dnf install ansible
```

2. We should also have script created / placed in the following directory:

```
cd k8s-experiments/ansible/playbooks/
ls
nano ./playbooks/change-hostnames.yml
```

### Steps: ###

We need hosts.ini file to specify the ip addresses of the hosts we want to change by this script. 
In our case they were placed into directory named cloudinit-vms.
```
cd ..
cd ./inventory/cloudinit-vms
nano ./hosts.ini
```

Once we have specified the ips and the hostnames we would like to be set on those vms,
we need to turn on ssh-agent:
```
cd ~
eval `ssh-agent`
ssh-add ./id_rsa_cloudinit
```

Then we can run our playbook:
```
cd k8s-experiments/ansible/
ansible-playbook -i ./inventory/cloudinit-vms/hosts.ini -v ./playbooks/change-hostnames.yml
```

In case we need info about roles:
```
ansible-galaxy role --help
```

To install a new role:
```
ansible-galaxy role install bodsch.snapd
```
Should appear in the list:
```
ansible-galaxy role list
```

