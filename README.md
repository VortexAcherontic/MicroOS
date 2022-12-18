# MicroOS Desktop
A small Github Page to list road blockers for openSUSE MicroOS to be the perfect beginner and power user distribution.

## SELinux
[Got to SELinux page](selinux.md)

## Network attached document scanners
As of now network scanners like 2 in 1 printers might not work by default.  
This is due to the fact that MicroOS is missing simple-scan or skanlite and some sane drivers and configs by default.  

What you probably want to do is the following:
1) Open Terminal  
2) `sudo tranactional-update -c shell`  
3) `zypper in skanlite sane-backends` for KDE or `zypper in simple-scan sane-backends` for Gnome  
4) `vim /etc/sane.d/dll.conf`  
5) Uncommend the line: `#escl`  
    * Press: `Insert`  
    * Move to the line `#escl`  
    * Press Del to remove only the `#`  
    * Press `Esc`  
    * Type `:x`  
6) Exit  
7) `sudo reboot`  

This will allow sane to scan for driverless network scaners using the escl backend.  
Most devices should support this.

Side note: Network printers who support driverless access via CUPs do work out of the box.

Related Bugzilla report: https://bugzilla.opensuse.org/show_bug.cgi?id=1206277

## nVidia drivers
[Go to nvidia drivers](nvidia.md)
