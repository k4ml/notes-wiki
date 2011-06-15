# Notes about Linux

## Burn multisession cd
```bash
$ mkisofs -o /tmp/sess2.iso -R -V session2 -C $(wodim -dev=/dev/scd0 -msinfo) -M /dev/scd0 ~/Music
$ wodim -dev=/dev/scd0 -multi -data -v -eject -dummy -speed=4 /tmp/sess2.iso
```
http://www.cyberciti.biz/tips/linux-burning-multi-session-cds-on-linux.html

## XFCE Terminal - move tab to bottom
> $EDITOR ~/.config/Terminal/terminalrc
> Change
>     MiscTabPosition=GTK_POS_TOP
> to
>     MiscTabPosition=GTK_POS_BOTTOM

http://foo-projects.org/pipermail/xfce/2007-November/022350.html