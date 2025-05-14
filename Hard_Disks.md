#How to check number of hard disks installed in a linux system#

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

Count the number of entries that start with Disk /dev/.




