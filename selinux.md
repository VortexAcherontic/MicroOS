[< back to main](README.md)

# MicroOS Desktop - SELinux
On this page you'll finde several issues which come up by MicroOS using a rather default SELinux policity set.

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

## Stack smashing attacks
By default SELinux prevents applications and libraries to execute code on a stack.  
This might lead to unwanted side effects such as some games (Stellaris) not to work.  
When ever you encounter an issue lokking similar to:  

```
some_library.so: cannot enable executable stack as shared object requires: Permission denied
```

Try temporarily turn off SELinux by using:

```
sudo setenforce 0
```

to turn SELinux back on simply flipt the 0 to a 1 in the above command.
