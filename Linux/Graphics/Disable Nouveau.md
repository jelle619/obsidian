# How to Disable the Nouveau Driver for Different Linux Systems

## Fedora
1. Create the **/usr/lib/modprobe.d/blacklist-nouveau.conf** file and add the following information to the file.
```    
blacklist nouveau`
options nouveau modeset=0
```
2. Re-generate initramfs.
`$sudo dracut --force`

## RHEL/CentOS
1. Create the **/etc/modprobe.d/blacklist-nouveau.conf** file and add the following information to the file.
```
blacklist nouveau
options nouveau modeset=0
```
2. Re-generate initramfs.
`$sudo dracut --force`

## OpenSUSE
1. Create the **/etc/modprobe.d/blacklist-nouveau.conf** file and add the following information to the file.
```
blacklist nouveau
options nouveau modeset=0
```
2. Re-generate initrd.
`$sudo /sbin/mkinitrd`

## SLES
The Nouveau driver is not installed in SLES.
    
## Ubuntu
1. Create the **/etc/modprobe.d/blacklist-nouveau.conf** file and add the following information to the file.
```
blacklist nouveau
options nouveau modeset=0
```
2. Re-generate initramfs.
`$sudo update-initramfs -u`