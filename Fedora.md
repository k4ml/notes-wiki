Install from live cd without burning it to cd:-

1. Mount downloaded iso.
2. Copy all content in the iso to the root of the device that need to be booted.
3. Change all reference in /EFI/boot/grub.conf and syslinux.cfg to point root device to the UUID of the partition that we want to boot. Use blkid to get UUID.
4. Add entry to /boot/grub/menu.lst and point the kernel and initrd to `/EFI/boot/vmlinuz0` and `/EFI/boot/initrd0.img`. Root device to pass is the UUID of the partition we want to boot.

### Notes:-
* If the boot device is home partition, make sure to copy iso content to the root of the partition, not to /home/userdir or something.
* Need to use UUID for root device, otherwise would get error `/dev/sdaX already mounted'.

http://forums.fedoraforum.org/showthread.php?t=191888

## Post installation
### Trackpoint
```bash
# cat - >> /etc/rc.d/rc.local
echo -n 255 > /sys/devices/platform/i8042/serio1/sensitivity 
echo -n 200 > /sys/devices/platform/i8042/serio1/speed
```
http://www.thinkwiki.org/wiki/How_to_configure_the_TrackPoint#configure-trackpoint

### YUM
```bash
$ wget -O timemirrorbandwidth.py "http://fedorapeople.org/gitweb?p=izhar/public_git/hack-patches.git;a=blob_plain;f=yum-plugin-timemirrorbandwidth/timemirrorbandwidth.py"
# mv timemirrorbandwidth.py /usr/lib/yum-plugins/
# cat - >> /etc/yum/pluginconf.d/timemirrorbandwidth.conf
[main]
enabled=1
^D
# yum time-mirrors --disableplugin=fastestmirror
```
http://blog.kagesenshi.org/2010/12/yum-plugin-timemirrorbandwidth.html

### Flash

```bash
$ wget "http://get.adobe.com/flashplayer/completion/?installer=Flash_Player_10.1_for_Linux_%28YUM%29"
# rpm -ivh adobe-release-i386-1.0-1.noarch.rpm
# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux
# cat /etc/yum.repos.d/adobe-linux-i386.repo 
[adobe-linux-i386]
name=Adobe Systems Incorporated
baseurl=http://linuxdownload.adobe.com/linux/i386/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux
# yum install nspluginwrapper alsa-plugins-pulseaudio flash-plugin
```