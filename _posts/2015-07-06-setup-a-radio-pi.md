# setup a radiopi 

First of all I build it with a [Raspberry Pi B+]( https://www.raspberrypi.org/products/model-b-plus/ ) and the 
[HiFiBerry DAC+]( https://www.hifiberry.com/dacplus/ ) but if you use other hardware like a USB sound card 
you can easily adapt most of this tutorial. Goal of this article is that you have a running Radio Pi at the end. 

## Raspberry up and running

The first step is to download the latest 2015-05-05-raspbian-wheezy.zip image. After you unzip it you can copy the 
the image to your sd card.

Please make sure /dev/sdX is the right device!

```
sudo dd if=2015-05-05-raspbian-wheezy.img of=/dev/sdc
```

Now you can connect a keyboard and a monitor and start your Raspberry Pi.
You should see a config menu, this are the things I changed:

#### rpi start menu (raspi-config)
Expand filesystem
Change User Password
Advanced Options
    Hostname: radio-pi
    Memory Split: 16mb
    SSH: enable
    Device Tree: enable

It's time for a reboot!

### Static IP

I always use static IP addresses but if you prefer (Static)DHCP it should also work.
As long as your DNS setup can handle this.


This is my config you can just change the address, gateway and netmask to match your network.

/etc/network/interfaces

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 192.168.17.66
    netmask 255.255.255.0
    gateway 192.168.17.1
```

Now you should be able to ssh in with pi@IP and you password


## The setup 

Less packages are less things you need to keep up to date, less stuff that can break. 
So I have removed many packages which are not used. If you need anything from this packages
you should probably not deinstall them, but hey you can install them again later.

```
sudo apt-get purge --auto-remove wolfram-engine
sudo apt-get purge --auto-remove scratch
sudo apt-get purge --auto-remove lightdm gnome-themes-standard gnome-icon-theme gvfs-backends gvfs-fuse desktop-base lxpolkit netsurf-gtk zenity gtk2-engines lxde lxtask menu-xdg gksu xserver-xorg xinit xserver-xorg-video-fbdev dbus-x11 libx11-6 libx11-data libx11-xcb1 x11-common x11-utils lxde-icon-theme gconf-service gconf2-common
sudo apt-get autoremove
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install rpi-update
sudo rpi-update
```

# HiFiBerry

Since they done a pretty good job in documenting what you should do to get 
there hardware running, here just a TL:DR and the link to [there documentation]( https://www.hifiberry.com/guides/configuring-linux-3-18-x/ ).

Remove the line `snd_bcm2835` from /etc/modules. And add `dtoverlay=hifiberry-dacplus` to /boot/config.txt.

Create /etc/asound.conf with 
```
pcm.!default  {
 type hw card 0
}
ctl.!default {
 type hw card 0
}
```

Your Raspberry Pi is now ready for the software installation which is covered in an other post.


