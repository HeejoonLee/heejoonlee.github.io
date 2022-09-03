---
title: "[macos] Managing Brew Packages"
date: 2022-09-03 21:47:00 +0900
categories: [macos, Brew]
tags: [macos, Brew, packages, guide]
img_path: /assets/img/macos_brew/
---

## TL;DR

```zsh
$ brew update
$ brew outdated
$ brew upgrade
```

## Upgrading Brew Packages

1. Check installed Brew packages: `$ brew list`
2. Update repository: `$ brew update`

    ![brew_update](brew_update.png)

3. Check outdated packages: `$ brew outdated`

    ![brew_outdated](brew_outdated.png)

4. Upgrade outdated packages: `$ brew upgrade`

    ![brew_upgrade](brew_upgrade.png)

    This process may take a while depending on the number of packages that need to be upgraded.


## Deleting Brew Packages

> TODO
{: .prompt-danger}
