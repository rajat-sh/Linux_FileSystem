Disk partitioning in Linux (or any operating system) is the process of dividing a physical storage device, such as a hard drive or SSD, into separate, logical sections called partitions. Each partition acts as a virtual, independent storage unit, and it can be used to install an operating system, store data, or manage different file systems.


**1. Why Disk Partitioning is Important**

Partitioning a disk allows you to:

    Organize Data: Separate the operating system, user data, and other files for better organization.
    
    Run Multiple Operating Systems: Install different OSes (e.g., Linux, Windows) on separate partitions.
    
    Improve Performance: By isolating different parts of the system, you can reduce fragmentation and improve access times.
    
    Backup and Recovery: Data in one partition can remain unaffected if another partition gets corrupted.
    
    File System Flexibility: Use different file systems (e.g., ext4, NTFS, FAT32) for different partitions.
    
    Simplify Maintenance: For example, you can reinstall Linux on one partition without affecting user data on another.

**2. Disk Partitioning Basics**

A physical disk can be divided into one or more partitions. Here are the basic concepts:

a. Types of Partitions

    Primary Partition:
        A physical disk can have up to 4 primary partitions.
        Primary partitions can directly hold file systems or an operating system.

    Extended Partition:
        If more than 4 partitions are needed, one of the primary partitions can be designated as an extended partition.
        Inside the extended partition, you can create multiple logical partitions.

    Logical Partition:
        Logical partitions exist inside the extended partition.
        These are treated as independent partitions by the operating system.


b. Partition Table

The partition table is a small region of the disk that stores information about the partitions. There are two common partition table formats:

    MBR (Master Boot Record):
        Supports up to 4 primary partitions.
        Maximum disk size: 2 TiB.
        Used in legacy systems.

    GPT (GUID Partition Table):
        Supports an unlimited number of partitions (though Linux tools typically support 128).
        Maximum disk size: 9.4 ZB (virtually unlimited).
        Required for disks larger than 2 TiB and modern UEFI systems.




**3. Common Linux Partitions**

Linux systems often use multiple partitions for better organization and performance. Common partitions include:

a. Root Partition (/)

    Contains the main operating system files.
    All other directories (e.g., /home, /var) are subdirectories under /.


b. Home Partition (/home)

    Stores user-specific files and settings.
    Useful for keeping personal data separate from the OS.


c. Boot Partition (/boot)

    Contains bootloader files (e.g., GRUB) and kernel images.
    Typically small (around 500 MB).


d. Swap Partition

    Used as virtual memory when the system runs out of RAM.
    Size recommendation:
        Equal to RAM size for systems with ≤8 GB RAM.
        Half the RAM size for systems with >8 GB RAM.


e. Other Optional Partitions

    /var: For frequently changing data, such as logs, mail, and databases.
    /tmp: For temporary files.
    /usr: For user-installed applications and libraries.



4. How Disk Partitioning Works

a. Partitioning Tools

Linux provides several tools to create and manage partitions:

    Command-Line Tools:
        fdisk: For managing MBR partitions.
        parted: For managing both MBR and GPT partitions.
        gdisk: Specifically for GPT partitions.

    GUI Tools:
        GParted: A graphical partition manager.
        Disks (GNOME Disk Utility): User-friendly for simple partitioning tasks.


b. Partitioning Workflow

    Identify the Disk: Use lsblk or fdisk -l to list available disks.
    lsblk
    fdisk -l

Create Partitions: Use a tool like fdisk or parted to create primary, extended, or logical partitions. Example with fdisk:
sudo fdisk /dev/sdX  # Replace X with your disk name

Format the Partition: Format the partition with a file system (e.g., ext4, xfs, etc.).
sudo mkfs.ext4 /dev/sdX1  # Replace X1 with the partition name

Mount the Partition: Mount the partition to a directory.
sudo mount /dev/sdX1 /mnt

Update /etc/fstab: To mount the partition automatically at boot, add an entry in /etc/fstab.



**5. File Systems**

Each partition needs a file system to store and organize data. Common Linux file systems include:

    ext4: Default in most Linux distributions, reliable, and widely used.
    xfs: High-performance file system, often used for large-scale storage.
    btrfs: Advanced file system with features like snapshots and checksumming.
    vfat/fat32: Compatible with Windows and other operating systems.
    NTFS: Used primarily by Windows.



**6. Example Partitioning Scenario**

For a typical Linux desktop system:

    / (root): 20-50 GB
    /home: 100 GB (or more, based on user needs)
    /boot: 500 MB
    Swap: 4-8 GB (depending on RAM size)


For a server:

    / (root): 20-50 GB
    /var: 50-100 GB (for logs and databases)
    /home: 100 GB (optional, if user accounts are needed)
    Swap: 8-16 GB
    /boot: 500 MB



**7. Commands for Partition Management**

Here are some commonly used Linux commands for partitioning and managing disks:

    List Disks:
    lsblk
    sudo fdisk -l

    Partition a Disk (using fdisk):
    sudo fdisk /dev/sdX

    Format a Partition:
    sudo mkfs.ext4 /dev/sdX1

    Create a Swap Partition:

    sudo mkswap /dev/sdX2
    sudo swapon /dev/sdX2


    Mount a Partition:

    sudo mount /dev/sdX1 /mnt




File systems usually are stored on disks. Most disks can be divided up into partitions, with independent file systems on each partition.Sector 0 of the disk is called the MBR (Master Boot Record) and is used to boot the computer. The end of the MBR contains the partition table. This table gives the starting and ending addresses of each partition. One of the partitions in the table may be marked as active. When the computer is booted, the BIOS reads in and executes the code in the MBR. The first thing the MBR program does is locate the active partition, read in its first block, called the boot block, and execute it. The program in the boot block loads the operating system contained in that partition. For uniformity, every partition starts with a boot block, even if it does not contain a bootable operating system. Besides, it might contain one in the some time in the future, so reserving a boot block is a good idea anyway.

The above description must be true, regardless of the operating system in use, for any hardware platform on which the BIOS is to be able to start more than one operating system. The terminology may differ with different operating systems. For instance the master boot record may sometimes be called the IPL (Initial Program Loader), Volume Boot Code, or simply masterboot. Some operating systems do not require a partition to be marked active to be booted, and provide a menu for the user to choose a partition to boot, perhaps with a timeout after which a default choice is automatically taken. Once the BIOS has loaded an MBR or boot sector the actions may vary. For instance, more than one block of a partition may be used to contain the program that loads the operating system. The BIOS can be counted on only to load
the first block, but that block may then load additional blocks if the implementer of the operating system writes the boot block that way. An implementer can also supply a custom MBR, but it must work with a standard partition table if multiple operating systems are to be supported.


On PC-compatible systems there can be no more than four primary partitions because there is only room for a four-element array of partition descriptors between the master boot record and the end of the first 512-byte sector. Some operating systems allow one entry in the partition table to be an extended partition which points to a linked list of logical partitions. This makes it
possible to have any number of additional partitions. 


Here’s a simple text-based diagram of the Master Boot Record (MBR) structure and its Partition Table. The MBR is located at the very beginning of a storage device (the first 512 bytes of the disk) and is essential for booting legacy systems that use the BIOS boot process.

+-------------------------+-------------------------+
|  Bootloader Code        |  446 bytes             |
+-------------------------+-------------------------+
|  Partition Table Entry 1|  16 bytes              |
+-------------------------+-------------------------+
|  Partition Table Entry 2|  16 bytes              |
+-------------------------+-------------------------+
|  Partition Table Entry 3|  16 bytes              |
+-------------------------+-------------------------+
|  Partition Table Entry 4|  16 bytes              |
+-------------------------+-------------------------+
|  Boot Signature         |  2 bytes (0x55AA)      |
+-------------------------+-------------------------+

Explanation of MBR Components

    Bootloader Code (446 bytes):
        This is the first part of the MBR.
        Contains the machine code (assembly instructions) for bootstrapping the operating system.
        If the disk is bootable, this code loads the OS or a secondary bootloader.

    Partition Table (64 bytes):
        Contains 4 entries of 16 bytes each, allowing up to 4 primary partitions.
        Each entry describes a partition on the disk, including its type, start sector, and size.
        If more than 4 partitions are needed, one of these entries can describe an extended partition, which can contain additional logical partitions.

    Boot Signature (2 bytes):
        A magic number (0x55AA) that marks the end of the MBR.
        Indicates that the disk is bootable and contains a valid MBR.



Partition Table Entry Structure (16 bytes per entry)

Each partition table entry is 16 bytes long and contains the following fields:


+----------------+------------------+
| Boot Flag      | 1 byte          |  (0x80 = bootable, 0x00 = not bootable)
+----------------+------------------+
| Starting CHS   | 3 bytes         |  (Cylinder, Head, Sector address)
+----------------+------------------+
| Partition Type | 1 byte          |  (e.g., 0x07 for NTFS, 0x83 for Linux)
+----------------+------------------+
| Ending CHS     | 3 bytes         |  (Cylinder, Head, Sector address)
+----------------+------------------+
| Starting LBA   | 4 bytes         |  (Logical Block Address of the partition)
+----------------+------------------+
| Size in Sectors| 4 bytes         |  (Total number of sectors in the partition)
+----------------+------------------+

Full Text Diagram for MBR and Partition Table


+-------------------------+
|  Bootloader Code        | 446 bytes
+-------------------------+
|  Partition Table Entry 1|
|   - Boot Flag           | 1 byte
|   - Starting CHS        | 3 bytes
|   - Partition Type      | 1 byte
|   - Ending CHS          | 3 bytes
|   - Starting LBA        | 4 bytes
|   - Size in Sectors     | 4 bytes
+-------------------------+
|  Partition Table Entry 2| 16 bytes
|   ... same structure ...
+-------------------------+
|  Partition Table Entry 3| 16 bytes
|   ... same structure ...
+-------------------------+
|  Partition Table Entry 4| 16 bytes
|   ... same structure ...
+-------------------------+
|  Boot Signature         | 2 bytes (0x55AA)
+-------------------------+


Key Points

    The MBR is exactly 512 bytes in size.
    It supports up to 4 primary partitions because the partition table has space for only 4 entries (16 bytes each).
    To bypass the 4-partition limit, one of the primary partitions can be an extended partition, which can hold multiple logical partitions.
    The Boot Signature (0x55AA) is critical for booting; if it’s missing or corrupted, the system won’t recognize the disk as bootable.



Example

If the disk has two primary partitions and one extended partition, the partition table might look like this:



+-------------------------+-------------------------+
|  Bootloader Code        |  446 bytes             |
+-------------------------+-------------------------+
|  Partition 1 (Primary)  |  Boot Flag: 0x80       |
|                         |  Type: 0x83 (Linux)    |
|                         |  Start LBA: 2048       |
|                         |  Size: 100000 sectors  |
+-------------------------+-------------------------+
|  Partition 2 (Primary)  |  Boot Flag: 0x00       |
|                         |  Type: 0x07 (NTFS)     |
|                         |  Start LBA: 102048     |
|                         |  Size: 200000 sectors  |
+-------------------------+-------------------------+
|  Partition 3 (Extended) |  Boot Flag: 0x00       |
|                         |  Type: 0x05 (Extended) |
|                         |  Start LBA: 302048     |
|                         |  Size: 400000 sectors  |
+-------------------------+-------------------------+
|  Partition 4 (Unused)   |  Empty (all 0s)        |
+-------------------------+-------------------------+
|  Boot Signature         |  2 bytes (0x55AA)      |
+-------------------------+-------------------------+


This layout represents a disk with:

    Partition 1: Linux (ext4).
    Partition 2: Windows (NTFS).
    Partition 3: Extended partition containing logical partitions.
    Partition 4: Unused.


This diagram helps visualize how the MBR and partition table work together to define the structure of the disk.




