---
title: "[WSL2] Installing Multiple Instances"
date: 2022-05-22 03:51:00 +0900
categories: [Tools, WSL2]
tags: [WSL2, Ubuntu, Linux, guide]
---

WSL2 allows the user to install and manage multiple instances of Linux subsystems. This makes running programs that are dependent on a specific Ubuntu version much more manageable.


## Install an instance from Microsoft Store
1. Open *PowerShell* and check currently installed instances:

    ```powershell
    > wsl -l -v
      NAME            STATE           VERSION
    * Ubuntu          Running         2
      Ubuntu-16.04    Stopped         2
      Ubuntu-22.04    Stopped         2
    ```

2. Check the distributions that you can install from Microsoft Store:

    ```powershell
    > wsl --list --online
    NAME            FRIENDLY NAME
    Ubuntu          Ubuntu
    Debian          Debian GNU/Linux
    kali-linux      Kali Linux Rolling
    openSUSE-42     openSUSE Leap 42
    SLES-12         SUSE Linux Enterprise Server v12
    Ubuntu-16.04    Ubuntu 16.04 LTS
    Ubuntu-18.04    Ubuntu 18.04 LTS
    Ubuntu-20.04    Ubuntu 20.04 LTS
    ```

3. Install the desired distribution with: `wsl --install -d [distro name]`

    For example, to install `Ubuntu 18.04 LTS`:
    ```powershell
    > wsl --install -d Ubuntu-18.04
    ```

4. Follow the steps to finish installing your chosen distribution

    > You may need to set up a *username* and *password* in this step.
    {: .prompt-info }

5. Confirm installed distributions:

    ```powershell
    > wsl -l -v
      NAME            STATE           VERSION
    * Ubuntu          Running         2
      Ubuntu-16.04    Stopped         2
      Ubuntu-18.04    Stopped         2
      Ubuntu-22.04    Stopped         2
    ```

## Install an instance from a local .tar file

> TODO
{: .prompt-danger}
