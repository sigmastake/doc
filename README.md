# 🌐 Set up Offline Signing for Cardano with CNTools

`sudo apt install cryptsetup`

sudo umount /usb \* make sure usb not mounted

sudo fdisk -l \* take note of device id (sda)

sudo fdisk /dev/sda&#x20;

<mark style="color:green;background-color:blue;">Delete old partition if any: d Create new partition: n Select default for all except last sector Enter +2GB for last sector Create new partition: n Select default for all Write: w</mark>

sudo mkfs.fat /dev/sda1 sudo mkfs.fat /dev/sda2

sudo dd bs=4K if=/dev/urandom of=/dev/sda1 \* random noise sudo cryptsetup -h sha256 -c aes-xts-plain -s 256 luksFormat /dev/sda1 \* encrypt

sudo cryptsetup luksOpen /dev/sda1 priv sudo mkfs.ext4 /dev/mapper/priv Mount encrypted USB script

cd

sudo mkdir /priv \* mount point

sudo nano \~/priv.sh

\#!/usr/bin/bash

sudo cryptsetup luksOpen /dev/sda1 priv sudo mount /dev/mapper/priv /priv sudo mount -t vfat /dev/sda2 /usb -o uid=1000,gid=1000,utf8,dmask=027,fmask=137

sudo chmod 700 priv.sh sudo chown cardanocore.cardanocore priv.sh

Unmount encrypted USB Script

sudo nano \~/unmount.sh

\#!/usr/bin/bash sudo umount /usb sudo umount /priv sudo cryptsetup luksClose priv

sudo chmod 700 unmount.sh sudo chown cardanocore.cardanocore unmount.sh

Perform Backup to Cold Encrypted USB \~/priv.sh sudo cp -a /opt/cardano/cnode/priv/. /priv/cardano/ OCH Use cntools Backup and Restore - Offline
