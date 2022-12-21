# Automatics updates / Reboots
By default MicroOS will try to make a system update every day at 0:00 o' clock.  
If it couldn't do this because the computer was turned off before it will re-try as soon as it gets the chance.  
This means it can pretty much happen that your computer shutsdown right after you turned it on or are in the middle of something.  
Not a behaviour you may want but there is an "easy" fix for this as this situation is currently re-worked on the maintainers side.  

## Prevent auto reboot
To prevent MicroOS for automatically reboot do the following:  

1) `transactional-update -c shell`  
2) `cp /usr/etc/transactional-update.conf /etc`  
3) `vim /etc/transactional-update.conf` and `Ins`  
Check if the line `REBOOT_METHOD=notify` is set to notify  
4) `esc :x`  

After the next reboot transactional-upate will use the transactional-update notifyer instead of rebooting on it own.  
As the notifyer is not yet ready you will not get notified but your MicroOS does not reboot on it's own,  
updates happen silently in the background and as soon as the notifyer is ready you'll get notifyed about a finished update.  
Since this is what the maintainers are aming for we're just putting the right config in the right place a little bit earlier.