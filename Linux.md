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

## Change keyboard layout to JP106

```bash
setxkbmap -model jp106 -layout jp
```
https://bugs.launchpad.net/ubuntu/+source/xkeyboard-config/+bug/53206

## Change locale per user
In `$HOME/.profile`:

```bash
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
export LANGUAGE=en_US.UTF-8
```
http://ubuntuforums.org/showthread.php?t=815362

## Update No-IP using ddclient
    use=web
    web=http://ip1.dynupdate.no-ip.com/
    protocol=noip, login=username, password='password' group_or_comma_separated_host_list

http://machine-cycle.blogspot.com/2010/11/update-no-ipcom-and-dyndnscom-dynamic.html