## Using fstrim, Cloud-Init, Ansible, LVM resize additional PV, systemctl ##

### Table of Contents: ###

- [fstrim setup and usage](./fstrim_timer.md)
- [Generating VMs from cloud-init images](./cloudinit-exp/cloudinit.md)
- [Configuring VMs using Ansible](./ansible/ansible.md)
- [How to resize LVM with additional PV](./lvm_resize_additional_pv.md)
- [Managing a simple HTTPS server with systemctl](./simple-https/systemctl.md)

### References ###

1. [Хабр Q&A: Нужно ли в виртуалках создавать LVM, если я захочу увеличить объем диска?](https://qna.habr.com/q/671189)
2. [How to resize a logical volume with 5 simple LVM commands](https://www.redhat.com/sysadmin/resize-lvm-simple)
3. [Proxmox with LVM-thin and why we should use Trim/Discard](https://gist.github.com/hostberg/86bfaa81e50cc0666f1745e1897c0a56)
4. [Proxmox VE Administration Guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.html)
5. [Solid state drive (more about trim here)](https://wiki.archlinux.org/title/Solid_state_drive)
6. [Object vs. File vs. Block Storage: What’s the Difference?](https://www.ibm.com/cloud/blog/object-vs-file-vs-block-storage)
7. [List of file systems](https://en.wikipedia.org/wiki/List_of_file_systems)
8. [systemd](https://en.wikipedia.org/wiki/Systemd)
9. [Why Systemd is the Devil but I Love It](https://www.youtube.com/watch?v=hc7J-zWEty8)
10. [How To Use Systemctl to Manage Systemd Services and Units](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)
11. [Writing Init Scripts on Alpine Linux](https://wiki.alpinelinux.org/wiki/Writing_Init_Scripts)
12. [Working with OpenRC](https://docs.alpinelinux.org/user-handbook/0.1a/Working/openrc.html)
13. [SSH handshake explained](https://goteleport.com/blog/ssh-handshake-explained/)
14. [Diffie Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)
15. [Man-in-the-middle attack in SSH - How does it work?](https://www.ssh.com/academy/attack/man-in-the-middle#various-ways-to-prevent-the-attack)
16. [Understanding the SSH Encryption and Connection Process](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process)
17. [Cloud-Init Support in Proxmox](https://pve.proxmox.com/wiki/Cloud-Init_Support)
18. [Ansible Galaxy site](https://galaxy.ansible.com/)
19. [Terraform provider plugin for Proxmox](https://github.com/Telmate/terraform-provider-proxmox)
