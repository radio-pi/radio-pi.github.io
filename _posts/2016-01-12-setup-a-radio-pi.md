---
layout: post
title: Setup a RadioPi
category: tutorial
---

THIS IS OUTDATET CHECKOUT THE UPDATED GUIDE [HERE]( /2024-03-10-setup-a-radio-pi )

First of all I build it with a [Raspberry Pi 3]( https://www.raspberrypi.org/products/raspberry-pi-3-model-b/ ) and the 
[HiFiBerry DAC+]( https://www.hifiberry.com/dacplus/ ) but if you use other hardware like a USB sound card 
you can easily adapt most of this tutorial. Goal of this article is that you have a running Radio Pi at the end. 

## Raspberry up and running

The first step is to download the latest 2017-11-29-raspbian-jessie-lite.zip image. After you unzip it you can copy the 
the image to your sd card.

Please make sure /dev/sdX is the right device!

```
sudo dd if=2017-11-29-raspbian-jessie-lite.img of=/dev/sdc
```

Now you can connect a monitor and a keyboard and start your Raspberry Pi. This shouldn't 
take to long.

Now you should see a line like 

> My IP Address is 192.168.1.XXX

To login we use pi as the user and raspberry is the password. And we start ssh to do the rest remote.

```
sudo systemctl start ssh
```

### Static IP

With the IP and ssh started we are able to login to our Raspberry Pi with `ssh pi@192.168.1.XXX`.
Before everything else change the password with `passwd`. Cool now we can configure a static IP.
You don't need one but it makes life simpler. (Also I always install vim)


This is my config you can just change the address, gateway and netmask to match your network.
Just add it to the end of the `/etc/dhcpcd.conf` file.

```
interface eth0
static ip_address=192.168.17.61/24
static routers=192.168.17.1
static domain_name_servers=192.168.17.5 192.168.17.6
static domain_search=l33t.network
static domain_name=l33t.network
```

You should also enable ssh that it starts with bootup.

```
sudo update-rc.d ssh enable
```

# HiFiBerry

Since they done a pretty good job in documenting what you should do to get 
there hardware running, here just a TL:DR and the link to [there documentation]( https://support.hifiberry.com/hc/en-us/articles/205377651-Configuring-Linux-4-x-or-higher ).

Replace in `/boot/config.txt` the `dtparam=audio=on` with the one matching your hifiberry which is in my case: `dtoverlay=hifiberry-dacplus`.

Create /etc/asound.conf with 

```
pcm.!default  {
 type hw card 0
}
ctl.!default {
 type hw card 0
}
```

Now you can reboot it and relogin on your new IP. You can test the sound with `speaker-test -c2 -t wav`.
Your Raspberry Pi is now ready for the software installation which is covered in an other post.
