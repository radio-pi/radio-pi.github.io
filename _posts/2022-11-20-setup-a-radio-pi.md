---
layout: post
title: Setup a RadioPi
category: tutorial
---

The hardware I used for this tutorial is the [Raspberry Pi 4 B]( https://www.raspberrypi.com/products/raspberry-pi-4-model-b/ )
and the [HiFiBerry DAC+]( https://www.hifiberry.com/dacplus/ ).
If you use other hardware like a USB sound card you might need to adapt parts of this tutorial.
Goal of this tutorial is that you have a running Radio Pi at the end.

## Raspberry up and running

The first step is to download the latest [Raspberry Pi OS Lite image](https://downloads.raspberrypi.org/raspios_lite_arm64/images/raspios_lite_arm64-2022-09-26/2022-09-22-raspios-bullseye-arm64-lite.img.xz).
After that unpack the image and copy it to the SD card.

Please make sure /dev/sdX is the correct device!

```
unxz 2022-09-22-raspios-bullseye-arm64-lite.img.xz
sudo dd if=2022-09-22-raspios-bullseye-arm64-lite.img of=/dev/sdX
```

Connect a monitor, a keyboard and network and start your Raspberry Pi.
To login use the user account just created.
To setup the next steps start ssh and enable it on startup.

```
sudo systemctl enable ssh
sudo systemctl start ssh
```

### Static IP

It is recommended to configure a static IP address to be able to connect to the RadioPi Raspberry Pi.
(It is recommend to install your favorite editor).

This is my config you can just change the address, gateway and netmask to match your network.

```
$ cat /etc/network/interfaces.d/eth0
auto eth0
iface eth0 inet static
 address 192.168.17.190
 netmask 255.255.255.0
 gateway 192.168.17.1
 dns-nameservers 192.168.17.5
```

Restart networking and check with `ip` if your configuration worked.

```
sudo systemctl restart networking.service
ip a
```


# HiFiBerry

HiFiBerry has done a pretty good job in documenting what you should do to get there hardware running.
Here just a TL:DR and the link to [there documentation]( https://www.hifiberry.com/docs/software/configuring-linux-3-18-x/ ).

Replace in `/boot/config.txt` the `dtparam=audio=on` with the one matching your HiFiBerry.
And disable `force_eeprom_read` see [Configuration changes in Linux 5.4](https://www.hifiberry.com/blog/configuration-changes-in-linux-5-4/).
In my case:

```
dtoverlay=hifiberry-dacplus
force_eeprom_read=0
```

Create /etc/asound.conf with 

```
pcm.!default  {
 type hw card 2
}
ctl.!default {
 type hw card 2
}
```

It is possible that the HiFiBerry HAT shows up as a different card,
the id can be checked with `aplay -l` adjust the configuration accordingly.

Now you can reboot it and re-login on your new static IP.
To test the sound with use `speaker-test -c2 -t wav`.
The Raspberry Pi is now ready for the software installation which is covered in an other tutorial.
