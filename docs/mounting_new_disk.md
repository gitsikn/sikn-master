### Disk Management

melihat partisi/disk yang tersedia pada server, misalnya sda, sdb

```
fdisk -l
atau 
lsblk -l
```
### membuat partisi
```
fdisk /dev/sdb
```
Formating Disk
```
sudo mkfs.ext4 /dev/sdb
sudo mkfs.xfs /dev/sdb
```
### Mounting 
```
sudo mount /dev/sdb /mnt/sdb
```
Permanent (To mount it automatically after each reboot)
```
nano /etc/fstab
```
``
/dev/sdb     /mnt/sdb      ext4        defaults      0       0
``

### memeriksa hasil mounting
``
mount | grep sdb
``