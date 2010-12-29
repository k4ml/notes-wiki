Install from live cd without burning it to cd:-

1. Mount downloaded iso.
2. Copy all content in the iso to the root of the device that need to be booted.
3. Change all reference in /EFI/boot/grub.conf and syslinux.cfg to point root device to the UUID of the partition that we want to boot. Use blkid to get UUID.

If the boot device is home partition, make sure to copy iso content to the root of the partition, not to /home/userdir or something.

http://forums.fedoraforum.org/showthread.php?t=191888

## Post installation

```bash
# cat - >> /etc/rc.d/rc.local
echo -n 255 > /sys/devices/platform/i8042/serio1/sensitivity 
echo -n 200 > /sys/devices/platform/i8042/serio1/speed
```
http://www.thinkwiki.org/wiki/How_to_configure_the_TrackPoint#configure-trackpoint