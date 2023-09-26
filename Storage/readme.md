# Linux Block Storage
## Physical Device
Perhaps a physical spinning SATA disk, USB disk, SCSI disk or even remote storage device with iSCSI.
## Device Driver
The kernel modules used to access the physical device. The SCSI disk module is sd_mod.
```sh
modinfo sd_mod
```
## Device File
For example, /dev/sda. Having a file to represent the disk, standard I/O- can be used to read and write to disk.

### Listing Block Devices
```sh
ls # to list files on a directory
lscpu # to list cpu
lsmod # to list kernel modules or device driver
modinfo <module_name> 
modinfo sd_mod
```
To list all block devices in a tree layout:
```sh
lsblk
```
Only list the tree format from a specific partition of a disk (such as sda), the first SCSI disk
```sh
lsblk /dev/sda
lsblk -s /dev/sda # to print reversely
```
# Loop Device
Loop devices have a file as the backend storage in place of physical storage. One way of using them is to access ISO files.
#### Loop Device as ISO File
```sh
sudo losetup -s <ISO_file_name> # -f option will find the next available loop device
```
## losetup Command
```sh
lsblk /dev/loop? # to check next available loop device
losetup # to list all loop device
sudo losetup -d loop0 # it will delete loop0 device
```
## Mount a LOOP Device
```sh
sudo mount /dev/loop0 /mnt
ls /mnt
sudo unmont /mnt # it will unmount the loop device
```

## Major Number
Major Number are assigned by the kernel maintainers. Some example of kernel numbers are 8 for sd_mod and 7 for the loop devices.
## Minor Number
Using the Linux SCSI driver, partitions are limited to a maximum 15 per disk. Each disk has pre-assigned. sda=8:0; sdb=8:16; sdc=8:32 to sdo=8:240.

# Creating Raw Disk File
We have already known that we can mount ISO files as loop devices. Well, We may also make use of raw disk files, these can be created with either ***dd*** or ***fallocate*** . The command time can prefer any command to display execution time.

```sh
time dd if=/dev/zero of=dd.disk bs=1M count=500
time fallocate -l 500M fa.disk
```
## Creating Loop Device
```sh
sudo losetup -f <disk_file> # attch next available loop device
sudo losetup /dev/loop1 /var/disks/disk1 # attach loop 1
losetup # list all loop disks
sudo losetup -d /dev/loop0 # delete or detach loop0
sudo losetup -D # detach all loop devices
```
## Usage of above Commands
- With ***fallocate*** command, we can create the disk file
- WIth ***losetup*** command, we can use to create the device file linking to the backend storage file.

## Why Partition
Complete disks can be used for filesystems, but breaking the disk into smaller elements may be more practical in some situations.

## Partition Tables

| MBR or MSDOS TABLE | GPT/GUID Partition Table |
|---|---|
|Max 2TB in size|Max 8ZB in size|
|Max 4 primary or 3 primary partitions, 1 extended plus logical| Max 255 partitions per disk in Linux depending on the  driver used|

# Partitioning Disks
Loop devices appear exactly the same as any other block device. So, partitioning is the same.
The traditional utility is fdisk(mbr) and gdisk(gpt) disks and it is interative parted can be used interactively but also directly at the cli.

```sh
sudo mkdir /var/disks
sudo fallocate -l LG /var/disks/disk1
sudo losetup /dev/loop1 /var/disks/disk1
sudo fdisk /dev/loop1
sudo parted /dev/loop1 mklabel1 msdos mkpart primary 0% 25%
```
# File System
To use the partition, we need to add a filesystem.

# Device Name Transitory
- The disk sda may be sda today but if it is not the first disk detected on the next boot it will not be sda, it could be sdb.
- File systems can be optional assigned with a label to identify them.
- All FIle systems have a UUID that uniquely identifies that file system.
- Adding a filesystem, we can mount the device using persistant names.
```sh
sudo mkfs.xfs -L "DATA" /dev/loop1
sudo mount LABEL=DATA /mnt
mount -t XFS # lists all xfs filesystems
sudo unmount /mnt
sudo blkid /dev/loop1p1 # prints UUID
sudo mount UUID="uuid" /mnt # another way 
```
To make thr mount at the system boot time, set it in /etc/fstab

# Persisting Loop Devices
If you ever need to reboot the system, loop devices will be lost and have to be restored.
```sh
sudo reboot
lsblk /dev/loop1 # loop device will not be found
sudo losetup /dev/loop1 /var/disks/disk1
lsblk /dev/loop1 # loop device create. now need to create partitions
sudo partprobe /dev/loop1
```
### Service Unit persisting loop device
```sh
[Unit]
Description=Setup Loop Device
DefaultDependencies=no
Before=local-fs.target
After=systemd-udevd.service
Required=systemd-udevd.service

[Service]
Type=oneshot
ExecStart=/sbin/losetup /dev/loop1 /var/disks/disk1
ExecStart=/sbin/partprobe /dev/loop1
TimeoutSec=60
RemainAfterExit=no

[Install]
WantedBy=local-fs.target
```
# Logical Volume Management
Aggregating block storage to re-allocate as needed in the form of device-mapper volumes.

### Viewing LVM
By default, LVM setups are used in many distribution including RHEL. LVM volumes use the device mapper driver, dm_mod
```sh
lsblk /dev/sda2
sudo dmsetup ls --tree # list device mapper devices in tree format
```

### Partitions Types
Altrough not strictly neccessary we can set the partition type to LVM allowing the system to know more about how the partition should be used.
```sh
sudo parted /dev/loop1 mkpart primary 25% 50% set 2 lvm on
sudo parted /dev/loop1 mkpart primary 50% 75% set 3 lvm on
sudo parted /dev/loop1 mkpart primary 75% 100% set 4 lvm on
sudo parted /dev/loop1 print
sudo parted /dev/loop1
```
# LVM2 Storage Layers
### Logical Volumes
Dev-mapper devices which are formatted and presented to the consumer as a block device.
### Volume Group
Volume group act as storage pools, aggregating storage together and overcoming the limitations of physical storage size.
### Physical Volumes
Physical storage existing on the host as disks, partitions and raw files.
## Managing LVM
|Physical Volume|Volume Group|Logical Volumes|
|--|--|--|
|pvs, pvmove, pvcreate|vgcreate, vgs, vgdisplay|lvs, lvcreate, lvsize, lvdisplay|
```sh
sudo pvs
pv # to show all command start with pv
sudo vgs # scan volume group
sudo vgdisplay # print all volume group details
```
## Physical Volume Options
We can skip the seperate definition of our physical volume unless we need to control the meta-data storage. Physical volumes will be defined with the volume group.
### Volume Group Physical Extent size
The default PE size is 4M and affects all physical volumes in a volume group. The setting was more important in LVM1 when there was a limitation of 65k extents within a volume group, this is not the case with LVM2. Setting a larger PE size now has more of an affect when striping data with RAID in LVM.
```sh
sudo vgcreate vg1 -v -s 8m /dev/loop1p1
```
### Creating Logical Volumes
WHen defining logical volumes we can use -L to specify the desired size that will be adjusted to the nearest matching multiples of the PE size.
Using -l we can specify the number of extents to use 12 extents would be 96M where 8M extent size is set.
```sh
sudo lvcreate -n lv1 -L 100M vg1
sudo lvcreate -n lv2 -l 12 vg1
sudo lvs vg1
```

```sh
pvs
pvcreate /dev/loop1p2
vgcreate -v vg1 /dev/loop1p2
vgdisplay vg1
vgremove vg1
pvremove /dev/loop1p2
pvcreate -- /dev/loop1p1 # will show all options for pvcreate
lvs vg1
lvcreate -n lv1 -L 100M vg1
lvcreate -n lv2 -L 13M v1
lvs vg1
```

