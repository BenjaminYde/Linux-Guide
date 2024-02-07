# Users and Permissions

## Users and Groups

**Owner (User)**: The owner of a file or directory is typically the creator or the user who executed the file creation command. This user has permissions that can be set independently of others, allowing for granular control over the file.

**Group**: Every file in Linux is associated with a group. Groups help organize users with similar access needs, allowing for easier permission management. A user who is a member of a file's group can have different permissions than someone who is not.

**Others**: This category represents all other users who have access to the system but are not the owner nor part of the group associated with the file. Permissions can be set to control what actions these users can perform on the file.

### Groups

A group is a collection of users that can have common access rights to files and directories. Linux uses groups to organize users with similar access levels or needs. Each group has a unique group ID (GID). When a user creates a file, the file's group ownership is set to the creator's primary group by default.

To see what groups exist on your system and which ones a user belongs to, you can use the following commands:

`groups [username]`: Lists all the groups that the specified user belongs to. If no username is specified, it shows the groups of the current user.

`cat /etc/group`: Displays all the groups defined on the system, along with their GID and the users belonging to them.

### Super User and Default User

**Super User (root)**: As mentioned, the superuser has full access to the system and can perform any operation. The superuser's UID is 0.

**Default User**: This refers to the first regular user account created during the installation of the Linux system. Often, this user has sudo privileges, allowing them to execute commands as the root user. This user's UID is typically `1000`, which is the first UID assigned to a regular user by many Linux distributions.

### User ID and Group ID Instead of Names

In Linux, you can specify a user or group by their numeric ID (UID/GID) instead of their name. This can be useful in scripts or when the username or group name is unknown. For example:

`chown 1001:1002 file.txt` changes the ownership of `file.txt` to the user with UID `1001` and the group with GID `1002`.

### Viewing Permissions and Ownership

The `ls -l -a` command is used to list files and directories with detailed information, including permissions and ownership. The output includes:

```
-rw-r--r-- 1 john developers 4096 Jan  1 12:34 file.txt
```

This means:

- The file is a regular file (`-` at the beginning).
- The owner (john) has read and write permissions (`rw-`), while the group (developers) and others have only read permissions (`r--`).
- The file is owned by user john and group developers.
- It has a size of 4096 bytes.
- It was last modified on Jan 1 at 12:34.

## Using Traditional Unix Permissions

In Ubuntu, as in all Unix and Linux-based systems, file permissions and ownership are fundamental concepts that control access to files and directories. The commands `chmod` (change mode) and `chown` (change owner) are crucial tools for managing these access rights. Here's a deeper look into how chmod and chown work, providing the groundwork before introducing Access Control Lists (ACLs).

### chmod: Changing File Permissions

The `chmod` command is used to change the file's mode, which includes its permission settings. Permissions determine who can read, write, or execute a file. There are three types of users who can have different permissions:

- **Owner (u)**: The user who owns the file.
- **Group (g)**: The group that owns the file. Each file is associated with a single group.
- **Others (o)**: Everyone else who has access to the file system.

Permissions are represented either numerically or symbolically:

- **Numeric (Absolute) Mode**: 
  - Consists of three or 3 digits. 
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

### Summary

**Simplicity**: The traditional model is straightforward, using chmod to set read, write, and execute permissions for the owner, group, and others. chown changes the ownership of a file or directory to a specified user and/or group. This model is sufficient for basic file permission management.

**Limitations**: The major limitation is its simplicity itself. Permissions are applied broadly to three categories (owner, group, others), making it difficult to set permissions for individual users beyond the owner or for multiple groups. If you need to give specific permissions to several users who do not fit neatly into a single group, traditional Unix permissions fall short.

## Access Control Lists (ACLs) in Linux File Systems

Access Control Lists (ACLs) are a sophisticated method for defining more granular permissions on files and directories within a file system than the traditional owner/group/other model. They provide a flexible framework for specifying detailed access rights for multiple users and groups, enhancing the security and management of file access in complex environments.

### Benefits

- **Granularity**: ACLs provide the ability to specify permissions for any number of individual users or groups. This allows for much more detailed access control policies, making ACLs ideal for environments where multiple users require different levels of access to the same files or directories.

- **Flexibility**: With ACLs, you can configure permissions that extend beyond the capabilities of the traditional model. For instance, you can allow several users to have different types of access to a file without having to create a new group for each unique permission set. This flexibility is particularly useful in collaborative environments where users' access needs may vary widely.

- **Default Permissions for Directories**: ACLs allow you to set default permissions on directories, ensuring that all files and directories created within inherit these ACL settings. This feature is not available with traditional Unix permissions and is valuable for maintaining consistent permission policies.

### Key Components

- **Entries**: An ACL consists of entries, each specifying access permissions for a user or group. These entries define the ability to read, write, or execute the file or directory.
- **Types of ACLs**: There are typically two types of ACLs:
  - **Access ACLs**: Determine the permissions for files and directories.
  - **Default ACLs**: Used only for directories; they define the permissions that any new files and directories created within will inherit.

### How ACLs Work

When a user or a process attempts to access a file or directory, the file system checks the ACL entries in order, from top to bottom, to determine whether the access should be allowed or denied. If no matching entry is found, the traditional Unix/Linux permissions are evaluated.

### Managing ACLs with getfacl and setfacl

#### getfacl

`getfacl`: This command is used to display the ACLs associated with a file or directory. It provides a detailed view of all the access entries, including the default ACLs for directories. The output includes information on the file owner, group, and the specific permissions granted to each user or group mentioned in the ACL.

```bash
getfacl file.txt
```

### setfacl

`setfacl`: This command allows for the modification or creation of ACL entries on a file or directory. It can be used to add, modify, or remove access entries, making it a powerful tool for managing detailed access controls.

Adding or modifying an ACL entry:

```bash
setfacl -m u:username:rwx file.txt
```

Removing a specific ACL entry:

```bash
setfacl -x u:username file.txt
```

Setting default ACLs on a directory:

```bash
setfacl -d -m u:username:rwx directory/
```

## AppArmor in Ubuntu

Ubuntu uses AppArmor as its primary mandatory access control (MAC) system by default. AppArmor is more user-friendly and straightforward to configure compared to SELinux, making it a popular choice for many Ubuntu administrators and users. It focuses on confining individual programs to a set of listed capabilities, defined in profiles located in `/etc/apparmor.d/`. These profiles determine what files and permissions the program can access.

AppArmor is designed to be easier to manage and to create policies for, with a focus on application-specific confinement. It comes pre-configured with profiles for many common applications and services, enhancing security without requiring significant additional configuration from the user.

## SELinux on Ubuntu

While SELinux is not enabled by default on Ubuntu, it is available and can be installed and configured by users who prefer its security model. SELinux offers a more complex and granular approach to security policies, allowing for comprehensive control over system resources and processes. It uses security labels and policy rules to manage access controls, operating on the principle of least privilege.

SELinux is known for its robust and fine-grained security capabilities, often used in environments that require stringent security measures. However, its complexity and the need for detailed policy management can make it more challenging to configure and maintain than AppArmor.