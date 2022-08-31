---
title: "[Linux] GRUB"
date: 2022-08-31 01:38 +0900
categories: [Linux, Bootloader]
tags: [Linux, guide]
---

# GRUB(Grand Unified Bootloader)

## Configuration file

File | Description
-----| ----
`/boot/grub2/grub.cfg` | GRUB2 configuration script
`/etc/grub2.cfg` | Symbolic link to `/boot/grub2/grub.cfg`
`/etc/default/grub` | GRUB configuration file
`/etc/grub.d/00_header` | GRUB password configuration file

> After editing the content of the GRUB configuration file, run `grub2-mkconfig` to apply the changes:
```shell
# grub2-mkconfig -o /boot/grub2/grub.cfg
```

## GRUB Configuration Examples

### GRUB timeout settings

Set GRUB timeout to 5 seconds:

```shell
# vi /etc/default/grub
GRUB_TIMEOUT=5
```

### Change terminal resolution
Set terminal resolution to *1024x768*:
```shell
# vi /etc/default/grub
GRUB_CMDLINE_LINUX="rhgb quiet vga=791"
```

### Set GRUB password
Add a user named *heejoon* and set a password:
```shell
# vi /etc/grub.d/00_header
cat <<EOF
set superusers="heejoon"
password heejoon 1234
EOF
```
