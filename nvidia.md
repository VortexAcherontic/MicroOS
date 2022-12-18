[< back to main](README.md)

# MicroOS Desktop - nVidia drivers
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

## Wayland and nVidia
If you also want to use Wayland with nVidia on MicroOS (and other Linux distributions not having this by default) you'll need to tweak the following:

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

If you're using the Gnome spin of MicroOS you alos need to enable a few systemd services for GDM to offer a Wayland session.  
For this do the following:  

1) `sudo systemctl enable nvidia-suspend.service`  
2) `sudo systemctl enable nvidia-resume.service`  
3) `sudo systemctl enable nvidia-hibernate.service`  
4) `sudo reboot`  

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
