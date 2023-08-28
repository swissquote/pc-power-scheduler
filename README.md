# Auto shutdown and wake up daemon for desktop usage

This repo build a debian package, test it, and release it.

# How to install it
Download the latest [release](https://github.com/swissquote/pc-power-scheduler/releases)
    
    sudo apt install ./pc-power-scheduler.deb

# What it does
It install and configure, shutdown & wakeup services, to automatically boot and halt your PC.

The config file `/etc/default/pc-power-scheduler` let your choice between different options
- Enable / Disable actions
- The method you want to use for the halt process => [ shutdown, suspend, hibernate ]
- Which day you want to enable the automatic shutdown
- Specify the time you want to shutdown up your computer
- Specify the time you want to wake up your computer

**pc-power-scheduler.service** => manage shutdown & wakeup services \
**pc-power-scheduler.path** => monitoring change on /etc/default/pc-power-scheduler and exec nedded actions \
**shutdown.service** => notify and exec shutdown process \
**shutdown.timer** => trigg shutdown service define in the config file \
**wakeup.service** => set the wakeup time define in the config file

[//]: # (/etc/systemd/system/wol.service => setup the NIC to be able to receive magic packets send by IT)


# Dev / test
Build manually the package 
```
dpkg-deb --build pc-power-scheduler
```
Trigg the shutdown manually
```
shutdown-popup
```
Get the wakeup time set
```
$ date --date "@$(cat /sys/class/rtc/rtc0/wakealarm)"
Friday 3 March 2023, 07:00:00 (UTC+0100)
```
Get the shutdown time set
```
$ systemctl list-timers shutdown.timer
NEXT                        LEFT    LAST PASSED UNIT           ACTIVATES       
Thu 2023-03-02 19:30:00 CET 9h left n/a  n/a    shutdown.timer shutdown.service

1 timers listed.
```

