### Enable bluetooth on the board NanoPi M3 (works with NanoPC-T3)

I found this tuto here : http://www.friendlyarm.com/Forum/viewtopic.php?f=44&p=958#p958 , thanks Awl !   

Download "nanopi_bluetooth_fix.tar.bz2" to your board  
As user root :  
> cd /  
> tar xvjf nanopi_bluetooth_fix.tar.bz2  

 Remove the two dangling services:  
 > rm /lib/systemd/system/hwservice.service  
 > rm /lib/systemd/system/hwservice_monitor.service  

Execute the following commands: 
> systemctl daemon-reload  
> systemctl enable brcm_patchram_plus  

Reboot your NanoPI-M3 and bluetooth is working.

If you want to install this patch with a newer version of bluez, you need to run the following commands at boot:
> systemctl start bluetooth
> sudo /usr/bin/hciattach /dev/ttySAC1 bcm43xx 921600 flow  
> sudo hciconfig hci0 up  