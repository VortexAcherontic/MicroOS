[< back to main](README.md)

# MicroOS Desktop - SELinux
On this page you'll finde several issues which come up by MicroOS using a rather default SELinux policity set.

## Proton/Wine and SELinux
There is currently a wierd situation where some games randomly do not work no matter if they are rated Platinum or SteamDeck verified on SteamDB or Steam.  
This is due to a default SELinux configuiration of MicroOS.  
In order to fix this try running: `sudo setsebool -P selinuxuser_execmod 1` in a terminal and see if your game now runs as expected.  

If you want this change to be permanent do the following:

1) Open terminal  
2) `sudo tranactional-update -c shell`  
3) `sudo setsebool -P selinuxuser_execmod 1`  
4) exit

After the next reboot this setting should be permanent.
Related bugzilla report: https://bugzilla.opensuse.org/show_bug.cgi?id=1206292

What this does:  

> If you want to allow all unconfined executables to use libraries requiring text relocation that are not labeled textrel_shlib_t, you must turn on the selinuxuser_execmod boolean. Enabled by default. 

## Stack smashing attacks
By default SELinux prevents applications and libraries to execute code on a stack.  
This might lead to unwanted side effects such as some games ([Stellaris for example](https://forum.paradoxplaza.com/forum/threads/stellaris-does-not-launch-on-selinux-enabled-systems.1562601/)) not to work.  
When ever you encounter an issue looking similar to:  

```
some_library.so: cannot enable executable stack as shared object requires: Permission denied
```

Try temporarily allow stacking by using:

```
sudo setsebool -P selinuxuser_execstack 1
```

to turn it back on simply flipt the 0 to a 1 in the above command.

If you want this change to be permanent do the following:

1) Open terminal  
2) `sudo tranactional-update -c shell`  
3) `sudo setsebool -P selinuxuser_execstack 1`  
4) exit

What this does:  

> If you want to allow unconfined executables to make their stack executable. This should never, ever be necessary. Probably indicates a badly coded executable, but could indicate an attack. This executable should be reported in bugzilla, you must turn on the selinuxuser_execstack boolean. Enabled by default. 