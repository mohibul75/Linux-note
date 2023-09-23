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

