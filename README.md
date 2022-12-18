# MicroOS Desktop
A small Github Page to list road blockers for openSUSE MicroOS to be the perfect beginner and power user distribution.

## Proton/Wine and SELinux
There is currently a wierd sittuation where some games randomly not work no matter if they are rated Platinum or SteamDeck verified on SteamDB or Steam.  
This is due to a default SELinux configuiration of MicroOS.  
In order to fixy this try running: `sudo setsebool -P selinuxuser_execmod 1` in a terminal  
If your games now run successfully you want this change to be permanent:

1) Open terminal  
2) `sudo tranactional-update -c shell`  
3) `sudo setsebool -P selinuxuser_execmod 1`  
4) exit

After the next reboot this setting should be permanent.
Related bugzilla report: https://bugzilla.opensuse.org/show_bug.cgi?id=1206292

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
