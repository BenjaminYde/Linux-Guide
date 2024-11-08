---
sidebar_position: 10
title: Dual Boot
---

## Install a Second SSD

### Why a Second Drive?

Installing Ubuntu on a separate ssd drive instead of partitioning a single drive for dual-boot with Windows is recommended due to:

- **Reduced Risk of Interference**: Each OS operates independently on its own drive, minimizing the chances of one affecting the other's system files or operation.
- **Simplified Partition Management**: Avoids the complexity and risks associated with partitioning a single drive, such as data loss or partition errors.
- **Improved System Stability**: If one operating system encounters issues, it's less likely to impact the other, enhancing overall system stability.
- **Easier Maintenance and Recovery**: Troubleshooting and recovery become simpler, as each OS can be managed separately without affecting the other.

### Check Hardware

First of all check if you can add a second SSD to your device? Maybe you can add an NVMe SSD.
Using an NVMe SSD over a regular SATA SSD for installing Ubuntu offers:

- **Faster Performance**: NVMe drives provide much higher speeds, leading to quicker boot times and faster file operations.
- **Efficiency**: They utilize the PCIe interface, offering lower latency and better CPU efficiency.
- **Ideal for Demanding Tasks**: Great for applications needing rapid data access, like gaming, video editing, or data analysis.
- **Future-Proofing**: NVMe is a newer, increasingly standard technology for high-performance storage.

It is also quite expensive, please use a regular 2.5" sata drive if you don't need the speed.

> [!CAUTION]
> Do not install the SSD yet! Please read the following section!      
Else you will risk your windows to be corrupted due to Bitlocker!

## Windows

### Create a bootable USB

1. Get a USB that is > 16GB.
2. Download Ubuntu LTS ISO [here](https://Ubuntu.com/download/desktop).
3. Download Rufus [here](https://rufus.ie/en/).
4. Open Rufus, select the ISO and press START to create a bootable USB.

### Bitlocker

Setting up a dual-boot system, especially when BitLocker is involved, requires careful steps to avoid locking yourself out of your Windows installation. BitLocker is a disk encryption feature included with Windows that can be sensitive to changes in hardware configuration. Here's a breakdown of the necessary steps:

1. **Understanding BitLocker Sensitivity**: BitLocker is designed to protect against unauthorized changes to your system. It's tied to the **Trusted Platform Module (TPM)** in your computer, which means significant hardware changes can be detected as potential security breaches. This triggers BitLocker to lock the drive, requiring a recovery key to regain access.
2. **Link BitLocker to Your Account**: Before making any changes, ensure that your BitLocker recovery key is linked to your Microsoft account. You can typically do this through the BitLocker control panel by choosing to back up your recovery key to your Microsoft account.
3. **Disable BitLocker Before Hardware Changes**: Before you start setting up your dual-boot system (which involves adding a new ssd or altering the boot sequence), disable BitLocker encryption. This is to prevent it from detecting the dual-boot setup process as a security threat. To disable BitLocker, go to the Control Panel, find BitLocker Drive Encryption, and select "Turn Off BitLocker." This process will decrypt your drive, which can take some time depending on the size and speed of your drive.
4. **Apply Hardware Changes for Dual-Boot**: Now that BitLocker is disabled, you can safely proceed with partitioning your hard drive or making other necessary hardware changes for your dual-boot setup.
5. **Re-Enable BitLocker After Setup**: Once you've installed your second OS and enasured everything is working correctly, you can re-enable BitLocker encryption on your Windows partition. Keep in mind that after re-enabling BitLocker, you should again ensure that your new recovery key is backed up to your Microsoft account.

## Set UEFI Settings (BIOS)

### 1. How To get into the BIOS

- **Option 1**: Restart your computer, enter the BIOS/UEFI settings by pressing a key (often F2, F10, F12, DEL, or ESC). It may be possible that these options are not visible. See option 2.
- **Option 2**: In windows, open search bar, type in "BIOS", enter, which opens the settings. Now click on a button that says to reboot with advanced settings.

### 2. Boot Menu: Change boot order

To set up a dual-boot system, you need to change the boot order in your computer's BIOS or UEFI settings to prioritize a bootable USB drive. This is essential for installing a second operating system, as it ensures the computer boots from the USB instead of the internal hard drive. 

### 3. Turn Off Fast Boot

When dual-booting Linux and Windows on separate drives, disabling Fast Boot in Windows is important for the following reasons:

- **Windows Fast Boot State**: With Fast Boot enabled, Windows partially hibernates instead of fully shutting down, affecting all connected drives, not just the one where Windows is installed.
- **Linux File System Access**: If Linux detects that the Windows file system (on its separate drive) is in this hibernated state, it may mount it in read-only mode to prevent data corruption. This happens because Windows' Fast Boot 'locks' the file systems in a state that Linux recognizes as potentially unsafe for writing.
- **Preventing Data Corruption**: Disabling Fast Boot ensures that when Windows shuts down, it fully releases its hold on all drives, allowing Linux to safely access its file system without the risk of corruption.

### 4. Turn Off Secure Boot

### About Secure Boot

- **Purpose of Secure Boot**: Secure Boot is a security standard designed to ensure that your PC boots using only firmware that is trusted by the manufacturer. It helps protect against low-level threats and rootkits that could compromise the boot process.
- **Reduced Security Layer**: Disabling Secure Boot removes a layer of protection. Without Secure Boot, there's a higher risk that malicious software could interfere with the boot process or load before the operating system.
- **Controlled Environment**: If your computer usage is in a controlled environment, where the risk of encountering malware is low and software sources are reliable, the absence of Secure Boot might not be a significant issue. (Everything is mostly done in Docker)

Disabling Secure Boot is often necessary in Linux for optimal use of NVIDIA GPUs.
Understanding Secure Boot, NVIDIA GPUs, and OS Compatibility in Linux and Windows: 

#### Linux Context:

- **Disabling Secure Boot for NVIDIA GPUs**: In Linux, disabling Secure Boot is often necessary to ensure proper functionality of NVIDIA GPUs. This is due to:
- **Driver Signing Issues**: Secure Boot requires digital signatures for drivers, but NVIDIA's proprietary drivers for Linux may not always meet these requirements.
Impact on GPU Functionality: Enabled Secure Boot can block unsigned NVIDIA drivers, potentially leading to reduced GPU performance or accessibility issues, as seen with tools like nvidia-smi.
- **Ensuring Full Access**: Disabling Secure Boot allows these NVIDIA drivers to load, which is crucial for tasks requiring advanced GPU capabilities in Linux.

#### Windows Comparison:

- **Driver Signing and Certification**: In contrast to Linux, NVIDIA's proprietary drivers in Windows are digitally signed and certified by Microsoft, aligning with Secure Boot requirements.
- **Unified Driver Architecture**: Windows benefits from a more consistent driver architecture, with NVIDIA drivers designed and tested specifically for Windows systems, ensuring compatibility with Secure Boot.
- **Operating System Design**: Windows' standardized approach to hardware and driver management, including collaboration with NVIDIA, ensures seamless integration within its ecosystem.
Regular Updates and Compatibility: NVIDIA's regular updates for Windows drivers maintain compatibility with the latest Windows updates and security protocols.

## Tips

### Do not combine NFTS with Ubuntu

Using NTFS (New Technology File System) as the primary or mounted secondary file system using Ubuntu is not recommended. NTFS is a file system developed by Microsoft for Windows. While Ubuntu can read and write to NTFS partitions, it is optimized for ext4 and other Linux file systems. Using NTFS with Ubuntu can lead to several issues:

- **Performance Degradation**: NTFS is not as efficient in Ubuntu as ext4, leading to slower read/write speeds and overall system performance.
- **Lack of Linux File Permissions**: NTFS does not support Linux file permissions natively, which can cause security and functionality problems for Ubuntu.
- **Increased Risk of Data Corruption**: Frequent writes to an NTFS partition from Ubuntu increase the risk of data corruption, especially if the partition is accessed by both Ubuntu and Windows in a dual-boot setup.

## Troubleshooting

### Grub Menu: Does Appear When Booting

#### Set the Ubuntu drive above windows to load

If the GRUB menu does not appear when booting into your dual-boot system, it's possible that the boot order in your BIOS/UEFI settings is not correctly set. Ensure that the drive where Ubuntu is installed is prioritized above the Windows drive in the BIOS boot menu. This change should prompt the GRUB menu to appear on boot, allowing you to select between Ubuntu and Windows.

#### Check Ubuntu settings if the grub is enabled

1. Open `/etc/default/grub`
    ```bash
    sudo nano /etc/default/grub
    ```
2. Check if the following values are present:
    ```
    GRUB_TIMEOUT_STYLE=menu
    GRUB_TIMEOUT=10
    GRUB_CMDLINE_LINUX=""
    ```

### Grub Menu: Windows is not in the list

1. Install `os-prober`:
    ```bash
    sudo apt install os-prober
    ```
2. Check if os-prober is set to true:
    ```bash
    GRUB_DISABLE_OS_PROBER=false
    ```
3. Update the GRUB menu config:
```bash
sudo update-grub
```
4. Reboot and check if it works

### My time on Windows is incorrect due to Ubuntu!?

> [!CAUTION]
> Todo