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

