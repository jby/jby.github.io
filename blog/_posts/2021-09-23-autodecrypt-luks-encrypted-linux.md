---
layout: post
title: How to configure LVM & LUKS to autodecrypt partition
comments: false
---

Shamelessly stolen from <a href="https://askubuntu.com/questions/59487/how-to-configure-lvm-luks-to-autodecrypt-partition">Askubuntu.com</a>

Setup:

/dev/sda1 is my unencrypted /boot partition<br>
/dev/sda5 is my lvm partition which contains everything else -- root, swap, and home<br>
/dev/sdc1 is the partition on my USB flash drive where I'll store the keyfile<br>
First, I created a keyfile, just in my home directory:

```
dd if=/dev/urandom of=keyfile bs=512 count=4
```
(you can use a larger blocksize or count for a larger key)

Tell cryptsetup the new key (it's the contents that are important, not the filename):

```
sudo cryptsetup luksAddKey /dev/sda5 keyfile
```
Then, I formatted my USB flash drive with ext2 and gave it a label. I used a label, so that later I can mount it by label, and replace the USB flash drive in case something goes wrong with it.

```
sudo mkfs -t ext2 /dev/sdc1
sudo e2label /dev/sdc1 KEYS
```
(of course, your device will vary)

Now, copy the keyfile to the USB flash drive, owned by root mode 400:

```
mkdir KEYS
sudo mount /dev/sdc1 KEYS
sudo cp keyfile KEYS
sudo chown root KEYS/keyfile
sudo chmod 400 KEYS/keyfile
```
Modify /etc/crypttab. Mine originally contained

sd5_crypt UUID=(...) none luks
which I changed to

sd5_crypt UUID=(...) /dev/disk/by-label/KEYS:/keyfile luks,keyscript=/lib/cryptsetup/scripts/passdev
Finally, update the initramfs:

sudo update-initramfs -uv
It now boots using the keyfile on the USB flash drive. If I remove the flash drive (say, when I go on holiday) it doesn't boot and my data is secure.

