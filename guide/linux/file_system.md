# File System Layout

The Linux File Hierarchy Structure or the Filesystem Hierarchy Standard (FHS) defines the directory structure and directory contents in Unix-like operating systems. If you’re coming from Windows, the Linux file system structure can seem particularly alien. The C:\ drive and drive letters are gone, replaced by a / and cryptic-sounding directories, most of which have three letter names.

When opening the terminal, list all directories in the root directory with 1 level depth. As you can see, some folders are references that are of the current user.

## Important File System Characteristics

File systems are critical components of an operating system that manage how data is stored and retrieved on storage devices. They provide a structure for organizing files and directories on disks and other media. Here are some key characteristics of file systems:

### Type

Defines the file system's architecture and rules for managing data. Common types include FAT32, NTFS, ext4, XFS, and Btrfs, each with unique features and capabilities.

### Capacity

Refers to the maximum volume size and the maximum file size that the file system can handle. For example, FAT32 has a maximum file size of 4 GB and a maximum volume size of 2 TB, whereas NTFS and ext4 support much larger sizes.

### Allocation Unit

Also known as the block size, it is the smallest amount of disk space that can be allocated to hold a file. Larger blocks can reduce overhead but may waste space when storing small files.

### Metadata Handling

Stores essential data about files, such as size, access permissions, file type, modification date-time, and other attributes, crucial for file management and security.

### Filename Restrictions

Sets rules for file naming, including the maximum length, use of special characters, and case sensitivity, which varies between file systems.

### Performance

Influenced by file system design, block size, and handling of metadata, impacting read/write speeds and efficiency in managing files.

### Reliability

Features such as journaling (logs changes before they are made to the file system) and checksums enhance data integrity and enable recovery from errors or system crashes.

### Snapshot and Versioning

Supports creating point-in-time captures of the file system, allowing restoration to previous states for data recovery.

## File System Types

### NTFS

**Description**: NTFS is a file system developed by Microsoft with support for data recovery, large volumes, and files, as well as advanced features like file compression and encryption.

**Use Cases**: While primarily used by Windows, NTFS is supported in Linux through NTFS-3G for scenarios requiring compatibility with Windows systems.

### FAT32 and exFAT

**Description**: FAT32 is widely used for its compatibility with a broad range of devices and operating systems. exFAT is an updated version that supports larger files and volumes.

**Use Cases**: Commonly used for removable storage devices such as SD cards and USB flash drives, especially when cross-platform compatibility is needed.

### XFS

**Description**: XFS is a high-performance 64-bit journaling file system designed for scalability in the storage of files and the allocation of disk space.

**Use Cases**: Often used in high-performance servers and data centers where large files and volumes are common.

### Btrfs (B-tree Filesystem)

**Description**: Btrfs is a modern file system that focuses on fault tolerance, repair, and easy administration. It introduces advanced features like copy-on-write, snapshots, and dynamic inode allocation.

**Use Cases**: Suitable for systems where data integrity and flexibility for managing data snapshots are critical.

### Ext4 (Fourth Extended Filesystem)

**Description**: Ext4 was designed to be backward compatible with ext3 and ext2, its previous generations. It’s better than the previous generations in the following ways:

- It provides a large file system.
- Utilizes extents that improve large file performance and reduces fragmentation.
- Provides persistent pre-allocation which guarantees space allocation and contiguous memory.
- Delayed allocation improves performance and reduces fragmentation by effectively allocating larger amounts of data at a time.
- It uses HTree indices to allow unlimited number of subdirectories.
- Performs journal checksumming which allows the file system to realize that some of its entries are invalid or out of order after a crash.
- Support for time-of-creation timestamps and improved timestamps to induce granularity.
- Transparent encryption.
- Allows cleaning of inode tables in background which in turn speeds initialization. The process is called lazy initialization.
- Enables writing barriers by default. Which ensures that file system metadata is correctly written and ordered on disk, even when write caches lose power.

However, ext4 has some limitations. Ext4 does not guarantee the integrity of your data, if the data is corrupted while already on disk then it has no way of detecting or repairing such corruption.

**Use Cases**: Ext4 is the default file system for many Linux distributions, including Ubuntu, because of its reliability, performance, and compatibility with larger storage devices. Ext4 is well-suited for both desktop and server environments.

## The File System Explained

### / (Root)

- The root directory is the starting point of the file system. All other directories, files, drives, and devices are attached to this directory.
- Only root user has write privilege under this directory.
- Please note that `/root` is root user’s home directory, which is not same as `/`.

### /bin (User Binaries)

- Contains essential user command binaries (programs) that need to be available in single user mode and for all users
- For example: `ps`, `ls`, `ping`, `grep`, `cp`, ...

### /sbin (System Binaries)

- Similar to `/bin`, but holds binaries that are essential for the system's administration, 
- For example: 
  - `/etc/bashrc`: It is used by bash shell that contains system defaults and aliases.
  - `/etc/crontab`: A shell script to run specified commands on a predefined time interval.
  - `/etc/fstab`: Information of the Disk Drive and their mount point.
  - `/etc/grub.conf`: It is the grub bootloader configuration file.
  - `/etc/init.d`:  Service startup Script.
  - `/etc/hosts`: Information of IP and corresponding hostnames

### /etc (Configuration Files)

- Houses all the system-wide configuration files. It also contains a collection of shell scripts that start the system at boot time. Individual user configurations are not found here.
- This also contains startup and shutdown shell scripts used to start/stop individual programs.
- For example: `/etc/resolv.conf`, `/etc/logrotate.conf`, ...

### /dev (Device Files)

- Contains device files, including terminal devices, usb, or any device attached to the system. 
- For example, `/dev/sda` represents the first SATA drive.

### /proc (Process Information)

- A virtual file system that provides a mechanism for the kernel to send information to processes. 
- It contains information about system resources, running processes, and kernel configurations 
- For example: `/proc/cpuinfo`, `/proc/meminfo`, ...
- For example: `/proc/{pid}` directory contains information about 
the process with that particular pid.

### /var (Variable Files)

- Intended for files whose content is expected to continually change during normal operation of the system, such as logs, spool files, and temporary e-mail files.
- For example: 
  - system log files (`/var/log`); 
  - packages and database files (`/var/lib`); 
  - emails (`/var/mail`); 
  - print queues (`/var/spool`); 
  - lock files (`/var/lock`); 
  - temp files needed across reboots (`/var/tmp`);

### /tmp (Temporary Files)

- Provides a storage space for temporary files that are created by system and user applications. 
- Files in `/tmp` can be deleted to free up space but may be necessary for currently running or suspended applications.

### /usr (User Programs)

- Contains the majority of user utilities and applications, including the system's default set of user tools. It's subdivided into several directories such as:
  - `/usr/bin` for user programs.
  - `/usr/sbin` for system administration programs that are not needed in single user mode.
  - `/usr/lib` for libraries.
  - `/usr/local` for locally installed software.

### /home (Home Directories)

- The directory where user-specific files and directories are located. Each user is assigned a directory within /home named after their user account.
- For example: `/home/john`, `/home/benjaminyde`

### /boot (Boot Loader Files)

- Contains the essential files needed to boot the system, including the Linux kernel, initrd image, and bootloader configuration files (e.g., GRUB).

### /lib (System Libraries)

- Contains library files that support the binaries located in /bin and /sbin. These libraries usually include vital shared libraries needed by system programs.
- Library filenames are either ld* or lib*.so.*
- For example: ld-2.11.1.so, libncurses.so.5.7

### /opt (Optional Add-on Applications)

- A directory for installing and storing additional (optional) software and packages that are not part of the default installation.

### /mnt and /media (Mount Directories)

- `/mnt` is traditionally used for mounting temporary file systems, like network file system (NFS) mounts. 
- `/media` is used for removable media devices, such as USB drives, CD-ROMs, etc.

### /srv (Service Data)

- Contains data for services provided by the system, such as serving websites or file storage.

## Using Traditional Unix Permissions

In Ubuntu, as in all Unix and Linux-based systems, file permissions and ownership are fundamental concepts that control access to files and directories. The commands `chmod` (change mode) and `chown` (change owner) are crucial tools for managing these access rights. Here's a deeper look into how chmod and chown work, providing the groundwork before introducing Access Control Lists (ACLs).

### chmod: Changing File Permissions

The `chmod` command is used to change the file's mode, which includes its permission settings. Permissions determine who can read, write, or execute a file. There are three types of users who can have different permissions:

- **Owner (u)**: The user who owns the file.
- **Group (g)**: The group that owns the file. Each file is associated with a single group.
- **Others (o)**: Everyone else who has access to the file system.

Permissions are represented either numerically or symbolically:

- **Numeric (Absolute) Mode**: 
  - Consists of three or four digits. 
  - Each digit is a sum of values for read (4), write (2), and execute (1). 
  - For example, the permission code 766 in numeric mode would break down as follows:
    - 7 for the owner: read (4), write (2), and execute (1) permissions added together (4+2+1=7).
    - 6 for the group: read (4) and write (2) permissions (4+2=6), but no execute permission.
    - 6 for others: same as the group, read and write permissions.

- **Symbolic Mode**: 
  - Uses letters (r, w, x, -) combined with symbols (+, -, =) to represent permissions. 
  - For example, `chmod u+rwx,g+rx,o+rx` achieves the same as `chmod 755`.


#### Examples

Making a script executable by the owner:

```bash
chmod u+x script.sh
```

Setting read and write permissions for the owner and group, and read-only for others:

```bash
chmod 664 file.txt
```

Adding write permission for the group:

```bash
chmod g+w file.txt
```

### chown: Changing File Ownership

The `chown` command changes the ownership of a file or directory. This involves changing the owner and/or the group associated with the file.

Syntax: chown [OPTION]... [OWNER][:[GROUP]] FILE...

- **OWNER**: The user ID to become the new owner of the file(s).
- **GROUP**: The group ID to become the new group owner of the file(s).

#### Examples

Changing the owner of file.txt to john:

```bash
chown john file.txt
```

Changing both the owner to john and the group to developers:

```bash
chown john:developers project/
```

Changing the group ownership of report.doc to staff without altering the file's owner:

```bash
chown :staff report.doc
```

## Access Control Lists (ACLs) in Linux File Systems

Access Control Lists (ACLs) are a sophisticated method for defining more granular permissions on files and directories within a file system than the traditional owner/group/world model. They provide a flexible framework for specifying detailed access rights for multiple users and groups, enhancing the security and management of file access in complex environments.

### Overview

In the traditional Unix/Linux permissions model, access control is managed through a set of permissions for three categories: the file owner, the group, and others. While effective for basic use cases, this model can be limiting when more nuanced access control is required. ACLs address this limitation by allowing detailed permissions to be set for any number of users and groups.

### Key Components

- **Entries**: An ACL consists of entries, each specifying access permissions for a user or group. These entries define the ability to read, write, or execute the file or directory.
- **Types of ACLs**: There are typically two types of ACLs:
  - **Access ACLs**: Determine the permissions for files and directories.
  - **Default ACLs**: Used only for directories; they define the permissions that any new files and directories created within will inherit.

### Benefits

- **Granular Permissions**: ACLs allow permissions to be finely tuned beyond the owner/group/world model, enabling scenarios where multiple users or groups need specific access rights.

- **Enhanced Security**: By providing detailed control over who can access what, ACLs enhance the security of a system. They ensure that only authorized users or groups can access sensitive data.

- **Flexibility**: ACLs offer the flexibility needed in environments with complex access control requirements, such as collaborative projects or shared directories.

### How ACLs Work

When a user or a process attempts to access a file or directory, the file system checks the ACL entries in order, from top to bottom, to determine whether the access should be allowed or denied. If no matching entry is found, the traditional Unix/Linux permissions are evaluated.

