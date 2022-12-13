## How to resize a logical volume with LVM ##

### Summary ###

What needs to be done: 
1. Attach the new storage to the system. 
2. Create a new Physical Volume (PV) from that storage. 
3. Add the PV to the Volume Group (VG) 
4. Extend the Logical Volume (LV).

The xdb disk is the new disk attached to it. 
Extend the root partition to make it 11G in size (initially 9 in our example).

lsblk command displays volume sizes:
```
lsblk
```

### Create Physical Volume ###

pvcreate command to create a new physical volume
Use the pvcreate command to designate a disk as a PV.
```
sudo pvcreate /dev/xdb
```

Physical volume "/dev/xdb" successfully created.

When you attach the new storage /dev/xdb, you need to use the pvcreate command in order for the disk to be initialized and be seen by the Logical Volume Manager (LVM).

### Identify Volume Group ###

Identify the Volume Group (VG) to which you are extending the new disk with the vgs command. 
```
sudo vgs
```
vgs command displays volume group information

### Extend Volume Group ###

The vgextend adds one or more initialized Physical Volumes to an existing VG to extend its size.

We are going to extend the rl Volume Group.

The vgextend command adds capacity to the VG.
```
sudo vgextend rl /dev/xdb
```
vgdisplay provides a detailed overview of the VG (all the VGs in the LVM and the complete information about them)
```
sudo vgs
```
vgs command displays volume group information
```
sudo vgdisplay
```

Now we have 2GB free. 
It is possible to extend all or some amount of storage size to it.

### Identify Logical Volume ###

The lvs or lvdisplay command shows the Logical Volume associated with a Volume Group. 
We've already extended the VG, so now it's necessary to extend the Logical Volume.
```
sudo lvs
```

### Extend Logical Volume ###

Extend the LV from the Volume Group with the lvextend command:
```
sudo lvextend -l +100%FREE /dev/rl/root
```

### Extend filesystem ###

You might need to confirm the file system type you're using with one of the following commands:
```
lsblk -f
```
 or 
```
df -Th.
```

Resize the filesystem on the Logical Volume after it has been extended to show the changes. 
Resize the XFS filesystem by using the xfs_growfs command.
```
sudo xfs_growfs /dev/rl/root
```

Finally, verify the size of your extended partition using the df -h command to display storage information:
```
df -h
```

