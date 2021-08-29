# raspberry-pi-arch-linux

Setup guide for Arch Linux on Raspberry Pi 4.

## Optional: overwrite device

```console
$ sudo shred --verbose --random-source=/dev/urandom --iterations 1 /dev/sdb
```

## Start fdisk to partition the SD card

```console
$ fdisk /dev/sdb
```

## At the fdisk prompt, delete old partitions and create a new one

1. Type **o**. This will clear out any partitions on the drive.
2. Type **p** to list partitions. There should be no partitions left.
3. Type **n**, then **p** for primary, **1** for the first partition on the drive
, press ENTER to accept the default first sector, then type **+200M** for the last sector.
4. Type **t**, then **c** to set the first partition to type W95 FAT32 (LBA).
5. Type **n**, then **p** for primary, **2** for the second partition on the drive, and then press ENTER twice to accept the default first and last sector.
6. Write the partition table and exit by typing **w**.

## Create and mount the FAT filesystem

```console
$ mkfs.vfat /dev/sdb1
$ mkdir boot
$ mount /dev/sdb1 boot
```

## Create and mount the ext4 filesystem

```console
$ mkfs.ext4 /dev/sdb2
$ mkdir root
$ mount /dev/sdb2 root
```
## Download and extract the root filesystem (as root, not via sudo)

```console
$ wget http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-4-latest.tar.gz
$ bsdtar -xpf ArchLinuxARM-rpi-4-latest.tar.gz -C root
$ sync
```

## Move boot files to the first partition

```console
$ mv root/boot/* boot
```

## Unmount the two partitions

```console
$ umount boot root
```

## Remove root and boot folders as they are not needed any longer

```console
$ sudo rm -r boot root
```

## 

# Sources

https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-4

https://illuad.fr/2020/07/06/install-arch-linux-arm-on-a-raspberry-pi-4.html

https://ladvien.com/installing-arch-linux-raspberry-pi-zero-w/

https://docs.termius.com/termius-handbook/connecting-to-a-host