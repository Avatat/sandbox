# Knowledge base
Zero-copy:
* [Zero Copy I: User-Mode Perspective](https://www.linuxjournal.com/article/6345)

Grow LVM:
1. Resize partition(s) under LVM using `parted`
2. Grow LV: `lvextend -l +100%FREE /dev/volume/lv`
3. Grow FS: `resize2fs /dev/volume/lv`

SCSI, local disks:
* `dmesg | grep sda`
* `lsblk --scsi`
* `lshw -C disk`
* `ls -ld /sys/block/sd*`
* `udevadm info -n /dev/sda -a`
* `hdparm -i /dev/sda`
