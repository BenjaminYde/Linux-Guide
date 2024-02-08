# Package Management

## Packages 

In Linux, a package is a compressed archive file that contains all the necessary files for a particular software application to run. For example, a web browser like Firefox comes in a package that has all the files needed for Firefox to run.

Packages are collections that contain the pre-compiled binary software files, installation scripts, configuration files, dependency requirements, and other details about the software. These packages are typically specific to a particular distribution and formatted in that distribution’s preferred package format, such as .deb for Debian/Ubuntu and .rpm for CentOS/RHEL.

While it’s relatively simple for a user to install a package file, there are other complexities to consider. These complexities include obtaining (downloading) the package, ensuring packages are upgraded with security and bug fixes, and maintaining all the dependencies for the software.

## Repositories 

While a user can obtain package files through any method of file transfer, it is typical to use software repositories (also called repos) to obtain packages. Repositories are simply the location where the packages are stored, commonly accessible via the internet. A repository can contain a single package or thousands of packages. Most Linux distributions have their own unique repositories, sometimes separating out their core’s packages into one while additional features are in others.

## Dependencies 

It’s very common in almost any operating system for software to require other software to run. In Linux, each package contains metadata detailing the additional packages that are required. These additional packages are called dependencies. A single package can sometimes have hundreds of dependencies. When installing, upgrading, or removing packages, these dependencies may also 
need to be installed, upgraded, and optionally removed.

## Package Managers 

A package manager reduces the complexity for the end-user by automating the process of obtaining, installing, upgrading, and removing packages and their dependencies. This dramatically improves the user experience and the ability to properly and efficiently manage the software on your Linux system. Today, package managers can be a defining feature for Linux distributions and many system administrators prefer to use a particular distribution based on its package management system (among other considerations)

### Benefits of package managers

Easily obtain the correct, trusted, and stable package for your Linux distribution. Since most distributions maintain their own repositories, using a package manager can ensure you only install packages that have been thoroughly vetted, are stable, are trusted, and work with your system. The subjective judgments and community standards that guide package management also guide the “feel” and “stability” of a given system.

### Automatically manage all dependencies when taking action on a package. 

While dependencies can be distributed inside the original package to ensure the correct versions of the required software are always present, this increases disk usage and package file size. In most cases, package managers will manage dependencies as separate packages, allowing them to be shared with other software and reducing disk usage and file size.

### Follow the unique conventions for each Linux distribution.

Linux distributions often have conventions for how applications are configured and stored in the `/etc/` and `/etc/init.d/` directories. By using packages, distributions are able to enforce a single standard.

### Stay up-to-date on security patches and software updates.

Most package managers provide a single command to automatically update all packages to the latest versions stored on the configured repositories. Provided those repositories are consistently maintained, this enables you to quickly get important security updates.

## Overview of package managers 

There are lots of package managers in Linux, each working a bit differently. Here is a list of common 
package managers, along with their supported distributions, package file formats, and a description.

### APT 

- **Distributions**: Debian-based, including Debian and Ubuntu
- **Underlying package management tool**: dpkg
- **Package file format**: .deb
- `Advanced Package Tool`, more commonly known as APT.

**YUM**:

- **Distributions**: RHEL/CentOS 7, Fedora 21, and earlier versions of both distributions
- **Underlying package management tool**: rpm
- **Package file format**: .rpm
- `Yellowdog Updater, Modified`, more commonly known as YUM, is a package management tool for a variety of older RHEL-based distributions (such as CentOS 7) and older versions of Fedora.

**DNF**:

- **Distributions**: RHEL/CentOS 8, Fedora 22, and later versions of both distributions
- **Underlying package management tool**: rpm
- **Package file format**: .rpm
- Dandified YUM, or simply DNF, is the successor to YUM.


### Other Package Managers

You might want to mention other package managers that cater to specific needs or are used in other distributions:

- **Pacman**: Used in Arch Linux and derivatives. It is known for its simplicity and effectiveness.
- **Zypper**: The command-line interface of the ZYpp package manager, used in openSUSE and SUSE Linux Enterprise.
- **Snap**: Developed by Canonical, it is designed to work across different Linux distributions, providing self-contained packages.
- **Flatpak**: Similar to Snap, Flatpak is focused on providing sandboxed applications to enhance security and portability across different distributions.

## Common Package Management Commands

You could include a section on common commands for each package manager. This section could be invaluable for beginners and serve as a quick reference for experienced users. Here are examples for APT, YUM, and DNF:

### APT (Debian, Ubuntu)

- Update package index: `sudo apt update`
- Upgrade all packages: `sudo apt upgrade`
- Install a new package: `sudo apt install package_name`
- Remove a package: `sudo apt remove package_name`
- Search for a package: `apt search keyword`
- Show package details: `apt show package_name`
`
### YUM (RHEL/CentOS 7, Fedora 21 and earlier)

- Update package index: `sudo yum makecache`
- Upgrade all packages: `sudo yum update`
- Install a new package: `sudo yum install package_name`
- Remove a package: `sudo yum remove package_name`
- Search for a package: `yum search keyword`
- Show package details: `yum info package_name`

### DNF (RHEL/CentOS 8, Fedora 22 and later)

- Update package index: `sudo dnf makecache`
- Upgrade all packages: `sudo dnf upgrade`
- Install a new package: `sudo dnf install package_name`
- Remove a package: `sudo dnf remove package_name`
- Search for a package: `dnf search keyword`
- Show package details: `dnf info package_name`

## Tips

`apt -qq clean`: This command clears out the local repository of retrieved package files. After packages are installed, the .deb files used for the installation remain in a local cache, which can consume unnecessary disk space. The -qq option runs the command in "quiet" mode, significantly reducing the output to the console, making it less verbose.

`apt -qq autoclean`: Similar to apt clean, but instead of removing all cached .deb files, autoclean only removes package files that can no longer be downloaded (obsolete or replaced by newer versions). This helps in freeing up space while ensuring that you can still reinstall packages from the cache without downloading them again. Again, -qq makes the command run quietly.

`apt -qq autoremove -y`: This command removes packages that were automatically installed to satisfy dependencies for some package and are no longer needed. This helps in keeping your system clean of libraries or dependencies that are not required anymore. The -y flag automatically answers "yes" to prompts, and -qq keeps the operation quiet.

`rm -rf /var/lib/apt/lists/*`: This forcefully removes all files in the /var/lib/apt/lists directory. This directory holds the package lists and metadata retrieved from repositories during an apt update. Clearing this directory can help resolve issues with corrupt package lists and is often done to reduce the footprint of Docker images or other similar use cases. However, you will need to run apt update again before installing any packages since this command removes the data necessary for apt to know what packages are available for installation.

### Why do this?

**Reduce Disk Space Usage**: It cleans up unnecessary files, which is particularly important in environments with limited storage or to create smaller Docker images.