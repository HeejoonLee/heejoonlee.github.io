---
title: "Ubuntu Installation Guide"
date: 2022-05-20 02:24:00 +0900
categories: [Tools, VirtualBox]
tags: [VirtualBox, Ubuntu, Linux, guide]
img_path: /assets/img/vbox_ubuntu/
---

## Prerequisites
* A host machine with VirtualBox installed: [VirtualBox][vbox-home]
* An Ubuntu image file: [ubuntu-21.10][ubuntu-download]


## Create an empty virtual machine
1. Open **Orcale VM VirtualBox**

2. From the top menu, click: 

    `Machine`->`new`

3. Set up the new virtual machine:

    1. Specify a *name* for the machine
    2. Choose `Linux` for *type* and `Ubuntu (64-bit)` for *version*
    3. Set an adequate amount of memory size(*over 2GB is recommended*)
    4. Select `create`

4. With the virtual machine just created highlighted, click `Settings` icon on the top right

5. Click `Storage` on the left tab and add the downloaded *Ubuntu image* to Controller:IDE

## Install Ubuntu

1. Choose your desired language and click *Install Ubuntu*

    ![Choose language](001.PNG)
    _Choose a language_

2. Choose your keyboard layout and click *Continue*

    ![Keyboard layout](002.PNG)
    _Choose a keyboard layout_

3. Select *Minimal Installation* and check *Download updates while installing Ubuntu*. Click *continue*

    ![Updates](003.PNG)
    _Updates and other software_

4. In Installation Type, select *Erase disk and install Ubuntu*. Click *continue*

    ![Installation type](004.PNG)
    _Installation type_

5. Choose your region

    ![Region](006.PNG)
    _Region_

6. Fill in all the fields and click *conitnue*

    ![Who are you](007.PNG)
    _Login info_

7. Wait for Ubuntu installation to finish(~10 min)

8. Once the installation is finished, shut down the machine *manually* by `File`->`Close`, then `Power off machine`

9. Double click the virtual machine and wait for Ubuntu to boot

10. Update package:

    ```bash
    $ sudo apt update && sudo apt upgrade -y
    ```

## Install Virtual Box Guest Additions

1. Install prerequisites:

    ```bash
    $ sudo apt -y install build-essential dkms
    ```

2. Insert VBox Guest additions image from the top menu:

    `Devices`->`Inset Guest Additions CD image..`

3. Mount Guest Additions image:

    ```bash
    $ sudo mount /dev/cdrom /mnt
    ```

4. Run `VBoxLinuxAdditions.run`{: .filepath} in the mounted directory:

    ```bash
    $ cd /mnt
    $ sudo ./VBoxLinuxAdditions.run
    ```

5. Reboot the virtual machine

## Create a Snapshot

Create a snapshot of the Ubuntu setup just installed. This allows us to easily restore to the saved state in case of any critical errors.

1. Save a snapshot:

    `Machine` -> `Take a snapshot`

2. Specify a name and description for the snapshot

3. The snapshot will appear in the window to the right:

4. Restore to this snapshot in case of an error


[vbox-home]: https://www.virtualbox.org/wiki/Downloads
[ubuntu-download]: https://ubuntu.com/download/desktop