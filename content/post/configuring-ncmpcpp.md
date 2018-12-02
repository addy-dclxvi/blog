---
title: "My Way of Configuring Ncmpcpp Music Player"
date: 2018-12-01T21:34:30+07:00
lastmod: 2018-12-01T21:34:30+07:00
draft: false
keywords: ["linux", "ricing", "music", "ncmpcpp", "terminal"]
description: "My simple way how to setup ncmpcpp music player"
tags: ["linux", "ricing", "music", "ncmpcpp", "terminal"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
**ncmpcpp** is an mpd client. In short, it's the frontend of mpd.
The actual music player is mpd, but managing mpd with barehand is not possible.
So, the most common way to control mpd is using ncmpcpp,
to list the library, arranging the playlist, choosing the music, and other actions.
**mpd + ncmpcpp** duo usually also paired with **mpc** for control.

{{% notice info %}}
Don't forget that ncmpcpp is a terminal applications.
{{% /notice %}}

## Installation
They are available in almost every distro repository.
So, just get them all easily. I use Debian by the way.

```Shell
sudo apt-get install mpd mpc ncmpcpp
```

## Configurations
### mpd
On Debian, after installing mpd, it will be launched as system service automatically,
which is annoying. I want to run mpd as user process instead of system service.
So, I disable the mpd system service.

```shell
sudo systemctl disable mpd.service
sudo systemctl disable mpd.socket
```

Now, mpd will use the user configuration in **~/.mpd/mpd.conf**
instead of the system wide mpd configuration in **/etc/mpd.conf**.
Here is my mpd configuration..

{{< highlight conf "linenos=table" >}}
music_directory "/media/addy/Media/musik/"
playlist_directory "/media/addy/Media/musik/"
db_file "/home/addy/.mpd/mpd.db"
log_file "/home/addy/.mpd/mpd.log"
pid_file "/home/addy/.mpd/mpd.pid"
state_file "/home/addy/.mpd/mpdstate"

audio_output {
type "pulse"
name "pulse audio"
}

audio_output {
type "fifo"
name "my_fifo"
path "/tmp/mpd.fifo"
format "44100:16:2"
}

bind_to_address "127.0.0.1"
port "6600"
{{< / highlight >}}

I do dualboot with Windows 7.
Even I'm very very rare to boot Windows, I'm still keeping it.
Just in case one day I will need it.
I store my musics in an NTFS partition called **Media**.
So, if boot Windows I'm still be able to enjoy my metal playlist.
I mount that media partition automatically on boot to **/media/addy/Media/musik/**
by editing the **/etc/fstab** file.

```fstab
UUID=01D2AF3E91F4D320 /media/addy/Media ntfs rw,nosuid,nodev,noatime,allow_other 0 0
```

That's the line I add to my fstab file. I put it in the most bottom line.
I get the UUID of my partition using `sudo blkid` command.
The change will be applied on the next boot.

Okay, back to the mpd configuration.
Replace *addy* in line number 3 to 6 with your own username.
Just in case you forgot your username, you could get it using `whoami` command.
I use PulseAudio for my sound. If you're using pure ALSA,
try to replace the line number 60 to 63 with..

{{< highlight conf "linenos=table,linenostart=60" >}}
audio_output {
type "alsa"
name "alsa for audio soundcard"
mixer_type "software"
}
{{< / highlight >}}

### ncmpcpp
ncmpcpp configuration is placed in **~/.ncmpcpp/config** file.

{{< figure src="https://u.cubeupload.com/addy15/ncmpcppconfig.png" >}}

I tried to paste the code to this page, but the syntax was conflicted with
[Hugo Shortcodes](https://gohugo.io/content-management/shortcodes/).
So, get the text file [here](/file/ncmpcpp/config). The square brackets are *comments*,
just like *\#* symbol. Just a matter of personal preference.
The explanations of my ncmpcpp config would be very long if wrote them all here.
The config is very rational, it's very easy to be understood only by reading it.
But if you need more detailed explanations, it's available inside the manual pages.
Use `man ncmpcpp` command to open it.

### mpc
mpc has no configuration file. The tasks of mpc are just for simply controlling mpd.
Like `mpc next`, `mpc prev`, `mpc toggle`, etc.
The advantage is, we could bind those command to our multimedia keys.
So, even the ncmpcpp is closed we still be able to control the mpd.
I use Openbox, so I put those keybinds in **rc.xml**

{{< highlight xml "linenos=table,linenostart=478" >}}
        <!-- Keybindings to control mpd -->
        <keybind key="XF86AudioStop">
            <action name="Execute">
                <command>mpc stop</command>
            </action>
        </keybind>
        <keybind key="XF86AudioPlay">
            <action name="Execute">
                <command>mpc toggle</command>
            </action>
        </keybind>
        <keybind key="XF86AudioPrev">
            <action name="Execute">
                <command>mpc prev</command>
            </action>
        </keybind>
        <keybind key="XF86AudioNext">
            <action name="Execute">
                <command>mpc next</command>
            </action>
        </keybind>
{{< / highlight >}}

## Launch
By default, Debian (the king of automations) also placed the autostart for mpd in
**/etc/xdg/autostart/**. If not, just simply add to your DE/WM autostart configuration.
So, after we reach the desktop after boot, we only need to hit the Play button to listen
the music (if there is a playlist stored).

{{% notice info %}} 
XDG global autostart needs **python-xdg** packages.
{{% /notice %}}

After mpd process is running (you could also manually run it by typing `mpd`
in terminal), you can launch ncmpcpp music player by typing `ncmpcpp` in terminal.
It will be blank by default, because we haven't load any playlist.

I recommend You to update the playlist first. On ncmpcpp, hit **U** keyboard button.
Just in case it doesn't work, Launch a new terminal then type `mpc update`.
After it got finished. You can access your music library in ncmpcpp.
Hit number 2 then navigate using Arrows keys, Enter, and Backspace.
Hit Space to add an album or an artist
recursively to the playlist. Hit number one to go back to the playlist.
Hit enter to select and play music.

{{< figure src="https://u.cubeupload.com/addy15/albums.png" >}}
{{< figure src="https://u.cubeupload.com/addy15/playlist.png" >}}

My theme is pretty simple & ugly. The only good thing of my setup is the
glorious Dream Theater playlist. I really recommend you to listen **Octavarium** album.
It's one of their best album. You could visit https://dotshare.it to discover
more Configuration examples. And of course, visit https://reddit.com/r/unixporn
for even more configuration examples. You will see a lot of ncmpcpp in that Subreddit.

{{% notice info %}} 
ncmpcpp has a lot other keybinds. Just read the manual pages to discover them.
{{% /notice %}}

And as always.. Thanks for reading!
