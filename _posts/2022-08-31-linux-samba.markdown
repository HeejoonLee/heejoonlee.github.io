---
title: "[Linux] SAMBA"
date: 2022-08-31 23:04 +0900
categories: [Linux, Network]
tags: [Linux, guide]
---

# SAMBA

**SAMBA** allows Unix and Windows systems to communicate with each other using the CIFS protocol.

## License

GPL

## Daemons

Daemon | Description
--- | ---
`smdb` | User authentication daemon
`nmdb` | WINS(Windows Internet Name Service) daemon

## Server Configuration

`/etc/samba/smb.conf`

```shell
$ vi /etc/samba/smb.conf
[global]
    workgroup = SAMBA
    server string = Samba Server
    netbios name = MYSERVER  // Name used to connect from Windows machine
    interfaces = lo eth0 192.168.12.2/24 192.168.13.2/24
    hosts allow = 127. 192.168.12.  // Allowed IP of SMB client
   #hosts allow = 192.168.1. EXCEPT 192.168.1.220  // Allow all IPs in network 192.168.1. except for 192.168.1.220
   #hosts allow = 192.168.1.0/255.255.255.0  // Network IPs can be set using netmask
   #hosts allow = heejoon_pc, hj_macbook  // Specify using host name

[shared]  # Disk name shown in Windows when connected
    comment = Shared directory
    path = /home/shared  # Path to the Linux directory to be shared
    valid users = heejoon devel  # Users who can access the directory
    writable = yes
    write list = heejoon @home  # @ indicates a group name
```

## Commands

### Client Commands(Linux->Windows)

1. `smbclient`

    * Usage: `smbclient [option] [hostname]`

    Hostname can be an IP address, or hostname(Linux)/machine name(Windows).
    A directory name can be specified after the hostname.

    Correct usage of / and \\:

    ```
    Windows->Linux: \\192.168.12.22\shared
    Linux->Windows: //192.168.12.23/shared, \\\\192.168.12.23\\shared
    ```
    * Options

    | Option | Description|
    |--- | ---|
    | -L | List all Samba directories |
    | -U | Specify user name. Specify password using % |

    * Examples

    `$ smbclient -L 203.247.40.248` List all shared directories in 203.247.40.248

    `$ smbclient \\\\hjwin\\shared -U heejoon` Access shared directory in machine 'hjwin' using the user name 'heejoon'

2. `mount.cifs`

    * Usage: `mount.cifs //server/dir /mount`

    Mount a shared folder in Windows system to a directory in Linux system.

### Server Commands(Windows->Linux)

1. `smbstatus`

    Show client connections

2. `testparm`

    * Usage: `testparm [file path] [hostname IP]`

    Check the validity of the config file(By default, `/etc/samba/smb.conf`)

3. `smbpasswd`

    Manage Samba users
    
    * Usage: `smbpasswd [option] [username]`

    | Options | Description |
    |---- | ----|
    | -a | Add a Samba user. Must exist as a user in the system(/etc/passwd) |
    | -x | Remove a Samba user |
    | -d | Disable a Samba user |
    | -e | Enable a disabled Samba user |
    | -n | Allow login without password. Must also specify in `/etc/samba/smb.conf` 'null passwords = yes' |

    * Examples

    `$ smbpasswd -a heejoon` Add a Samba user heejoon. heejoon must also exist in `/etc/passwd`

    `$ smbpasswd heejoon` Change the password of the Samba user heejoon

4. `pdbedit`

    Manage Samba users

    * Usage: `pdbedit [option] [username]`

    | Options | Description |
    | ---- | ---- |
    | -a | Add a Samba user. Must exist as a user in the system |
    | -L | List all Samba users |
    | -V | Verbose |


    




