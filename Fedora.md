Install from live cd without burning it to cd:-

http://forums.fedoraforum.org/showthread.php?t=191888

## Post installation

```bash
# cat - >> /etc/rc.d/rc.local
echo -n 255 > /sys/devices/platform/i8042/serio1/sensitivity 
echo -n 200 > /sys/devices/platform/i8042/serio1/speed
```
http://www.thinkwiki.org/wiki/How_to_configure_the_TrackPoint#configure-trackpoint