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

    <img src="brew_update.png" alt="brew_update" width="500"/>

3. Check outdated packages: `$ brew outdated`

    <img src="brew_outdated.png" alt="brew_outdated" width="500"/>

4. Upgrade outdated packages: `$ brew upgrade`

    <img src="brew_upgrade.png" alt="brew_upgrade" width="500"/>

    This process may take a while depending on the number of packages that need to be upgraded.


## Deleting Brew Packages

> TODO
{: .prompt-danger}
