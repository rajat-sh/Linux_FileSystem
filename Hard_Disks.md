### How to check number of hard disks installed in a linux system

To check the number of hard disks installed on a Linux system, you can use several tools and commands. Below is an overview of some of the most common methods:

**1. Using the "lsblk" Command**

The lsblk command lists information about block devices (e.g., hard drives, SSDs, partitions).

lsblk -d


The -d option ensures that only the disk devices (not partitions) are displayed.
Look at the "NAME" column to count the number of disks.

Example output:

root@fmc:/Volume/home/admin# lsblk -d
NAME MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda    8:0    0  250G  0 disk 

In this example, there is one disk sda

**2. Using the "fdisk" Command**

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

**3. Using the "ls /dev/" Command**

You can manually check the disk devices in the /dev directory.

ls /dev/sd* /dev/nvme* 2>/dev/null

sd* lists SATA/SCSI drives, while nvme* lists NVMe SSDs.
Ignore partition entries (e.g., /dev/sda1, /dev/sda2), and count only the base names (e.g., /dev/sda, /dev/sdb).

Example output:

root@rajat-virtual-machine:/home/rajat# ls /dev/sd* /dev/nvme* 2>/dev/null
 /dev/sda   /dev/sda1   /dev/sda2   /dev/sda5


**4. Using the "cat /proc/partitions" Command**

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

**5. Using the "df -h" Command**

The df -h command shows mounted file systems and their sizes. While it doesn't explicitly show unmounted disks, it gives an idea of the storage devices in use.

df -h

Example output:

root@fmc:/Volume/home/admin# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda6       3.7G  2.3G  1.2G  67% /
none             16G     0   16G   0% /dev
/dev/sda1        87M   37M   43M  47% /boot
/dev/sda7       237G   72G  154G  32% /Volume
none             16G  460K   16G   1% /dev/shm


Count the unique devices (/dev/sdX or /dev/nvmeXnY) listed under "Filesystem."

In above example there is one  unique devices sda

**6. Using the "dmesg" Command**

The dmesg command displays kernel logs, which include information about detected disks during boot or when plugged in.

dmesg | grep -i "disk"

Look for lines mentioning sdX or nvmeXnY.

Example output:

root@fmc:/Volume/home/admin# dmesg | grep -i "disk"
[    0.007154] RAMDISK: [mem 0x7f810000-0x7fffffff]
[    2.072898] scsi 0:0:0:0: Direct-Access     VMware   Virtual disk     2.0  PQ: 0 ANSI: 6
[    2.090996] sd 0:0:0:0: [sda] Attached SCSI disk


**7. Using the "ls /sys/block/" Directory**

The /sys/block/ directory contains information about all block devices.

ls /sys/block/

This command lists all block devices (disks and partitions).

Ignore loop devices,ram (loop*,ram*) and count the entries corresponding to disks (sdX, nvmeXnY)

Example output:

root@fmc:/Volume/home/admin# ls /sys/block/
loop0  loop1  loop2  loop3  loop4  loop5  loop6  loop7	ram0  ram1  ram10  ram11  ram12  ram13	ram14  ram15  ram2  ram3  ram4	ram5  ram6  ram7  ram8	ram9  sda
root@fmc:/Volume/home/admin# 

**8. Using "lshw" for Hardware Details**

The lshw utility provides detailed hardware information, including disks.

sudo lshw -class disk

This outputs detailed information about all detected disks.

Example output:

root@rajat-virtual-machine:/home/rajat# sudo lshw -class disk
  *-cdrom                   
       description: DVD-RAM writer
       product: VMware IDE CDR00
       vendor: NECVMWar
       physical id: 0.0.0
       bus info: scsi@0:0.0.0
       logical name: /dev/cdrom
       logical name: /dev/cdrw
       logical name: /dev/dvd
       logical name: /dev/sr0
       version: 1.00
       capabilities: removable audio cd-r cd-rw dvd dvd-r dvd-ram
       configuration: ansiversion=5 status=open
  *-disk
       description: SCSI Disk
       product: Virtual disk
       vendor: VMware
       physical id: 0.0.0
       bus info: scsi@2:0.0.0
       logical name: /dev/sda
       version: 1.0
       size: 250GiB (268GB)
       capabilities: partitioned partitioned:dos
       configuration: ansiversion=2 logicalsectorsize=512 sectorsize=512 signature=ea2d3709




