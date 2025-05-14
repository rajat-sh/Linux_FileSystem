### How to check number of hard disks installed in a linux system

To check the number of hard disks installed on a Linux system, you can use several tools and commands. Below is an overview of some of the most common methods:

**1. Using the lsblk Command**

The lsblk command lists information about block devices (e.g., hard drives, SSDs, partitions).

lsblk -d


The -d option ensures that only the disk devices (not partitions) are displayed.
Look at the "NAME" column to count the number of disks.

Example output:

root@fmc:/Volume/home/admin# lsblk -d
NAME MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda    8:0    0  250G  0 disk 

In this example, there is one disk sda

**2. Using the fdisk Command**

The fdisk utility lists the available disks and their partitions.

sudo fdisk -l

This command displays detailed information about all disks and their partitions.
Each disk will be listed as /dev/sdX, /dev/nvmeXnY, or /dev/vdX.

Example output:

Disk /dev/sda: 250 GiB, 268435456000 bytes, 524288000 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xea2d3709

Count the number of entries that start with Disk /dev/.

**3. Using the ls /dev/ Command**

You can manually check the disk devices in the /dev directory.

ls /dev/sd* /dev/nvme* 2>/dev/null

sd* lists SATA/SCSI drives, while nvme* lists NVMe SSDs.
Ignore partition entries (e.g., /dev/sda1, /dev/sda2), and count only the base names (e.g., /dev/sda, /dev/sdb).

Example output:

root@rajat-virtual-machine:/home/rajat# ls /dev/sd* /dev/nvme* 2>/dev/null
 /dev/sda   /dev/sda1   /dev/sda2   /dev/sda5


**4. Using the cat /proc/partitions Command**

The /proc/partitions file contains information about the available block devices.

cat /proc/partitions

This lists all block devices, including disks and partitions.
Look for entries with major numbers corresponding to disks (e.g., sda, sdb, nvmeXnY).

Example output:

root@fmc:/Volume/home/admin# cat /proc/partitions
major minor  #blocks  name

   1        0      16384 ram0
   1        1      16384 ram1
   1        2      16384 ram2
   1        3      16384 ram3
   1        4      16384 ram4
   1        5      16384 ram5
   1        6      16384 ram6
   1        7      16384 ram7
   1        8      16384 ram8
   1        9      16384 ram9
   1       10      16384 ram10
   1       11      16384 ram11
   1       12      16384 ram12
   1       13      16384 ram13
   1       14      16384 ram14
   1       15      16384 ram15
 **8        0  262144000 sda**
   8        1      95232 sda1
   8        2    1000448 sda2
   8        3          1 sda3
   8        5    3905536 sda5
   8        6    3905536 sda6
   8        7  253233152 sda7


