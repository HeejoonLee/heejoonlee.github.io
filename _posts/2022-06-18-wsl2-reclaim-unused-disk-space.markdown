---
title: "[WSL2] Reclaim Unused Disk Space"
date: 2022-06-18 20:04:00 +0900
categories: [Tools, WSL2]
tags: [WSL2, Ubuntu, Linux, guide]
---

For each instance of WSL2, a hard disk image file *(.vhdx)* is created which acts as the main storage device for that instance.

The file grows in size as new files are created in WSL2. However, *the storage is never reclaimed(returned to Windows host)* when the space is no longer in use by the WSL2 instance. For example, when you delete a file in WSL2, the file does not show up in the filesystem, but the .vhdx(in Windows host) file will not shrink in size. 

Thus, it is essential to manually reclaim such unused space once in a while, otherwise Windows host will eventually give you a warning that there is no storage space available when actually there is.


## Reclaim Unused Disk Space

1. Open *PowerShell* and make sure the WSL2 instance using the storage you are reclaiming from is not running:

    ```powershell
    > wsl --list --running
    Windows Subsystem for Linux Distributions:
    Ubuntu-22.04
    
    > wsl --shutdown
    > wsl --list --running
    There are no running distributions.
  ```

2. Locate the path of the .vhdx file that you are reclaming space from:

    > The default location of the hard disk image file of a WSL2 instance is `%PROFILE%\AppData\Local\Packages\CanonicalGroupLimited.[InstanceName]onWindows_79rhkp1fndgsc\LocalState\ext4.vhdx`
    {: .prompt-info }

3. Run *diskpart*

    ```powershell
    > diskpart
    ```

4. Run the following commands in the *diskpart* cmd window that opens up:

    ```
    DISKPART> select vdisk file="[.vhdx path]"
    DISKPART> attach vdisk readonly
    DISKPART> compact vdisk
    DISKPART> detach vdisk
    DISKPART> exit
    ```

5. When step 4 is completed, the .vhdx file will have shrunken in size
