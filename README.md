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
3) `zypper in skanlite snae-backends` for KDE or `zypper in simple-scan sane-backends` for Gnome  
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

## nVidia drivers
As of now nVidia drivers need to be installed manually using MicroOS build-in tools to modify the next read-only snapshot.  

1) Open a terminal  
2) `sudo transactional-update -c shell`  
3) `zypper addrepo --refresh https://download.nvidia.com/opensuse/tumbleweed NVIDIA`  
4) `zypper ref`  
5) Accept the repository key by pressing "a" for always  
6) Find out your GPU model and supported drivers: https://www.nvidia.com/Download/index.aspx  
    * \*G06 => 525 driver series  
    * \*G05 => 470 driver series  
    *  \*G04 => 390 driver series  
7) `zypper in nvidia-computeG0* nvidia-gfxG0*-kmp-default nvidia-glG0*`  
8) `exit`  
9) `reboot`  

## Gnome Wayland and nVidia
If you also want to use Wayland with nVidia on MicroOS (and gnome in general) you'll need to tweak the following:

1) Open a terminal  
2) `sudo transactional-update -c shell`  
3) `touch /etc/modprobe.d/50-nvidia.conf`  
4) `vim /etc/modprobe.d/50-nvidia.conf`  
5) Press Insert on your keyboard and insert:  
```
options nvidia_drm modeset=1
options nvidia NVreg_PreserveVideoMemoryAllocations=1
```
6) Then press: `Esc`  
7) Type `:x`  
8) Then run `dracut -f`  
9) `exit`  
10) `sudo systemctl enable nvidia-suspend.service`  
11) `sudo systemctl enable nvidia-resume.service`  
12) `sudo systemctl enable nvidia-hibernate.service`  
13) `sudo reboot`  

The systemD services are required for GDM to propose the Gnome Wayland session to the user.

Hower the out of the box experience with nVidia drivers might not change as long as teh open source drivers are not mature enough to be shipped with MicroOS.

## Using open nVidia module
If you want to use the open-nvidia-kernel you need to add the following to your `50-nvidia.conf` as well:
```
options nvidia NVreg_OpenRmEnableUnsupportedGpus=1
```

Addionally install: `nvidia-open-gfxG06-kmp-default` instead of the kpm-default package.  
The open module is served by the repository: X11:Drivers:Video it is best to add that repo b using opi:

1) Open Terminal  
2) `sudo transactional-update -c shell`  
3) `zypper in --no-recommends opi`  
4) `opi nvidia`  
5) Choose `nvidia-open-gfxG06-kmp-default`  
6) Choose `X11:Drivers:Video`  
7) `exit`  
8) `sudo reboot`  
