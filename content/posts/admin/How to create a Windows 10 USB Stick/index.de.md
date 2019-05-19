---
title: How to create a Windows 10 bootable USB-Stick on Linux
date: 2019-02-22T13:35:00+01:00
author: Djordje Atlialp
draft: false
toc: false
images:
tags:
  - linux
  - system administration
  - admin
  - windows
  - microsoft
---

Dieser Artikel dient irgendwie auch der eigenen Dokumentation ...

Also, wie erstellt man unter Linux einen bootbaren USB-Stick mit Windows 10 drauf?

```bash
# USB Device
DEVICE=/dev/sdX

# Delete every bit on it
dd if=/dev/zero of=$DEVICE bs=1M count=1

# Create new partition
fdisk ${DEVICE}
n
p
1
ENTER
ENTER
t
c
a
w

# Format and mount
mkfs.vfat ${DEVICE}1
mount ${DEVICE}1 /mnt/

# Copy data
mkdir Win10
mount -o loop Win10_1709_German_x64.iso Win10
cp -a Win10/* /mnt/usb

# Unmount
umount /mnt/
umount Win10
```
