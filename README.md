# BeagleBoneBlack_UbuntuImage
Describe step by step how to load a ubuntu image to Beaglebone Black

## Ubuntu (18.04.4)
By default:
|username| password|
|--------|---------|
| ubuntu | temppwd |

### Get prebuilt image:
```
wget https://rcn-ee.com/rootfs/2020-03-12/elinux/ubuntu-18.04.4-console-armhf-2020-03-12.tar.xz
```
Verify Image with:

```sha256sum ubuntu-18.04.4-console-armhf-2020-03-12.tar.xz
abe086f9132dfe8e8b9df8d14da225e0ce89a082abc92515de8a2ac63fc54ae2  ubuntu-18.04.4-console-armhf-2020-03-12.tar.xz
```
Unpack Image:
```
tar xf ubuntu-18.04.4-console-armhf-2020-03-12.tar.xz
cd ubuntu-18.04.4-console-armhf-2020-03-12
```
If you don't know the location of your SD card:
```
sudo ./setup_sdcard.sh --probe-mmc
```
You should see something like:
```
Are you sure? I don't see [/dev/idontknow], here is what I do see...

fdisk -l:
Disk /dev/sda: 500.1 GB, 500107862016 bytes <- x86 Root Drive
Disk /dev/sdd: 3957 MB, 3957325824 bytes <- MMC/SD card

lsblk:
NAME      MAJ:MIN   RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    0 465.8G  0 disk 
├─sda1        8:1    0 446.9G  0 part /  <- x86 Root Partition
├─sda2        8:2    0     1K  0 part 
└─sda5        8:5    0  18.9G  0 part [SWAP]
mmcblk0     179:0    0   3.7G  0 disk 
└─mmcblk0p1 179:1    0   3.7G  0 part 

```
In this example, we can see via mount, /dev/sda1 is the x86 rootfs, therefore /dev/mmcblk0 is the other drive in the system, which is the MMC/SD card that was inserted and should be used by ./setup_sdcard.sh...

### Install Image on BeagleBoneBlack:
```
sudo ./setup_sdcard.sh --mmc /dev/mmcblk0 --dtb beaglebone
```
Unmount ssdCard and after put it on Beaglebone
```
<hostUser>@<PCname>:~/$sudo umount /media/<hostUser>/rootfs
```

### Services Active:
Apache, Port 80: http://arm.local/ 
SSH, Port 22: ssh ubuntu@arm.local