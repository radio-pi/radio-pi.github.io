---
layout: post
title: Get the software running
category: tutorial
---

This post is asuming that you have setup your Raspberry Pi
and your audio interface. If not check out the blog post: [here]( /2016-01-12-setup-a-radio-pi ) 

# Install mpd

For now the only usable backend to play music in Radio Pi is mpd so 
we need to install it. It's planed to have at least vlc as fallback 
but nobody wrote that feature so it's just a plan.

```
sudo apt-get install mpd lame mpc
sudo service mpd stop
```

The only thing I changed in the mpd config (`/etc/mpd.conf`) is the mixer_control to Digital.

```
audio_output {
	type		"alsa"
	name		"My ALSA Device"
	#device		"hw:0,0"	# optional
	#format		"44100:16:2"	# optional
	#mixer_device	"hw:0"	        # optional
	mixer_control	"Digital"       # optional
	mixer_index	"0"		# optional
}
```

Start the mpd and enable it on startup.

```
sudo service mpd start
sudo update-rc.d mpd enable
```

# App Backend

At the moment the only way to install the web api backend is to clone it from git.
And here is how:


Install the required tools: python, python-virtualenv and git with apt-get.

```
sudo apt-get install python python-virtualenv git
```

Get the source:

```
sudo git clone https://github.com/radio-pi/python-websocket-backend.git /usr/local/src/radiopi
sudo chown -R pi /usr/local/src/radiopi
```


Create the enviroment and install the dependencys. 

```
virtualenv /usr/local/src/radiopi/env
. env/bin/activate
pip install -r requirements.txt 
```

Copy the init script to /etc/init.d/ and and run it on startup.

```
sudo cp /usr/local/src/radiopi/radiopi /etc/init.d/
sudo service radiopi start
sudo update-rc.d radiopi defaults
sudo update-rc.d radiopi enable
```
