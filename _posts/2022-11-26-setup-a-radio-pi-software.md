---
layout: post
title: Get the software running
category: tutorial
---

This post assumes that you have setup your Raspberry Pi and your audio interface.
If not check out the tutorial post: [here]( /2022-11-20-setup-a-radio-pi )

# App Backend

The installation process right now is still very manual,
and involves a handful of steps which can be copy pasted.

Install [VLC](https://www.videolan.org/vlc/) which is used a player to play the audio streams.

```
sudo apt install vlc
```

Install the required tools: Python, virtualenv and git with apt-get.

```
sudo apt install python3 python3-dev virtualenv git
```

Get the source:

```
sudo git clone https://github.com/radio-pi/python-websocket-backend.git /usr/local/src/radiopi
sudo chown -R $USER /usr/local/src/radiopi
```

Create the environment and install the dependency.

```
virtualenv --python=python3 /usr/local/src/radiopi/env
source /usr/local/src/radiopi/env/bin/activate
pip install /usr/local/src/radiopi/
pip install uvicorn
```

Copy the systemd service to `/lib/systemd/system/` reload it and enable it to run on startup.

```
sudo cp /usr/local/src/radiopi/radiopi.service /lib/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable radiopi
sudo systemctl start radiopi
```
