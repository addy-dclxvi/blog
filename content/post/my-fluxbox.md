---
title: "My Fluxbox Configurations"
date: 2018-12-16T10:45:56+07:00
lastmod: 2018-12-16T10:45:56+07:00
draft: false
keywords: ["linux", "ricing", "fluxbox", "guide", "configurations"]
description: "Explanation of my Fluxbox Configurations and how to use it"
tags: ["linux", "ricing", "fluxbox"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Intruduction
This post is an explanation about my Fluxbox configurations and how to use them. Fluxbox is my second
window manager after Openbox. They are pretty similar because both are derived from Blackbox window
manager. Some differences between Openbox and Fluxbox are..

- Fluxbox uses plain text of configuration files instead of XML.
- Fluxbox comes with a panel called Fluxbox Toolbar. The panel can contain taskbar, workspace
indicator, workspace switcher, system tray, and clock.
- Fluxbox has an ability to group several window, as known as Tabbed mode.
- Fluxbox has an ability to use XPM icons for window decoration, icons, and background. Openbox only
supports XBM. XPM is similar with PNG, so it supports transparency & multiple colors.
- Fluxbox doesn't have On Screen Display. So there is no popup window when we do switching window
using **Alt+Tab**, or switching workspace using **Control+Alt+Left/Right**.
- Fluxbox doesn't support EWMH Strut to set the workspace margins.
- Fluxbox uses sligthly less resources than Openbox in my configurations. But no noticable difference
in current hardware standards.
- Fluxbox doesn't support smart window placement like in Openbox.

## Installation
Beside installing Fluxbox, I also install some essential supporting packages
``` shell
sudo apt-get install fluxbox dunst suckless-tools compton hsetroot xsettingsd lxappearance scrot
```

- Compton is a compositor to provide some desktop effects like shadow, transparency, fade, and
transiton.
- Hsetroot is a wallpaper handler. Fluxbox has no wallpaper handler by default. Fluxbox comes with
`fbsetroot` & `fbsetbg` by default. But they are not an actual wallpaper handler. They are just
wrapper scripts to find any wallpaper handler available and use it as fluxbox wallpaper handler.
When I install Debian, I start from minimal install. So, I don't have any wallpaper handler that
support JPG/PNG by default. So, I choose `hsetroot`.
- Suckless Tools are several utility made by [suckless.org](https://suckless.org), but actually I
only need `dmenu`. Unfortunately the package is not splitted, so I need to pull them all.
- Xsettingsd is a simple settings daemon to load fontconfig and some other options. Without this,
fonts would look rasterized in some applications.
- LXAppearance is used for changing GTK theme icons, fonts, and some other preferences.
- Scrot is a simple screenshot taking app.

Then, get my Fluxbox configurations from [this](/file/fluxbox.tar.gz) archive. Extract it to your
home directory. So, you will get the **.fluxbox** and other directories in your home.
I use URxvt as default terminal. If you interested, check [this](/post/configuring-urxvt/) article.
I use Dunst as notification daemon. If you interested, check [this](/post/dunst/) article.
I use ncmpcpp as music player. If you interested, check [this](/post/configuring-ncmpcpp/) article.
I use Fish as interactive shell. If you interested, check [this](/post/fish-shell/) article.

## Explanations

### ~/.fluxbox/
It's the folder where the fluxbox configurations stored. There is several files & a directory inside
it.

- **init** is the main fluxbox configuration. Like, what items displayed in the titlebar, what items
displayed in the toolbar, where to place the toolbar, how many workspaces we need, and many others.
The full explanation is available
[here](http://fluxbox-wiki.org/category/howtos/en/Editing_the_init_file.html).
- **keys** is the file to set the keybinds & mousebinds. The full explanation is available
[here](http://fluxbox-wiki.org/category/howtos/en/Keyboard_shortcuts.html).
- **menu** is the list of items displayed when we do right click on the desktop. The full explanation
is available
[here](http://fluxbox-wiki.org/category/howtos/en/Editing_the_menu.html).
- **startup** is a list of programs launched when we login with Fluxbox.
- **wallpaper.jpg** is the wallpaper I use. Actually not mandatory to be put in
~/.fluxbox/wallpaper.jpg
- **windowmenu** is the list of items displayed when we do right click on the titlebar.
- **styles/** is a directory to place Fluxbox styles/themes. I include one theme in the archive.

### ~/.config/
I just include one file in the archive. A compton configuration.

### ~/.scripts/
I put three files in the archive. They are just wrapper scripts.

- **launch2** is a wrapper script to launch dmenu with custom arguments.
- **pulsevol** is a script I stole from [Vera's](https://github.com/okitavera) dotfiles to make me
easier to control the pulseaudio volume.
- **screeny** is a wrapper script for taking screenshot using scrot then open the result instantly
using the default image viewer.

## Launch
Logout then login again with Fluxbox session in your login manager. And welcome to Fluxbox.

## Keybinds
- **Super + Enter** launch URxvt
- **Super + D** launch dmenu with wrapper script
- **Alt + Space** open root menu, just like right click on the desktop. But we can use it anywhere
- **Alt + Tab** switch to next window
- **Alt + Shift + Tab** switch to previous window
- **Control + Alt + Left/Right** switch to previous/next workspace
- **Control + Alt + Up/Down**  switch to previous/next window, just like Alt + Tab
- **Super + Arrows** "Aero Snap"
- **Super + 1-4** switch to workspace 1-4
- **Super + Shift + 1-4** take the current active window to workspace 1-4
- **Super + Shift + Left/Right** take the current active window to prev/next workspace
- **Super + Alt + Arrows** switch focus to other window in the desired direction
- **Super + Control + Arrows** teleport
- **Super + A** central the current focus window
- **Super + C** close
- **Super + Z** minimize
- **Super + F** maximize
- **Super + T** toggle the window decoration
- **Super + U** roll up the window
- **Super + Shift + Backspace** reload Fluxbox, do this after modify the configuration files
- **Control + Drag** Tab the windows
- **Double Click Titlebar** roll up the window
- ..More keybinds just read the *~/.fluxbox/keys/* file

## Notes
- Please inspect the code before use.
- These configurations are designed for my daily driver, probably not suitable for you.
So, please modify them.
- If you want to use network manager applet, volume applet, clipboard applet, etc, on the system
tray. Just put them in the autostart. Maybe like this..
```shell
#!/bin/bash
export PATH="${PATH}:$HOME/.scripts"
xset fp+ ~/.fonts/misc
xrdb ~/.Xresources
compton -b
hsetroot -fill ~/.fluxbox/wallpaper.jpg
nm-applet &
volumeicon &
clipit &
exec fluxbox -no-slit
```
Don't forget that the ampersand is important, because they are daemons. Also don't forget,
`exec fluxbox` have to be in the last line.

- On the tabbed windows, if you click the close button (or Super + C). All of them will be closed.
To only close one window, right click on the window title then click close.
- My included theme is designed to match with Arc GTK Theme.
- Don't forget to listen metal musics, I really recommend 
[Blackwater Park](https://www.youtube.com/playlist?list=PLINesDgSwsOoMyQtly8laF_gZ_k2Qk0Qh)
album by Opeth. I'm listening to it while writing this article. My favourite track is The
Drapery Falls.

{{< figure src="https://u.cubeupload.com/addy15/thedrapperyfalls.png" >}}

I usually use Firefox web browser. Just discovered Palemoon several days ago and love it. It's
lighter than Firefox. Suitable for my low powered hardware.

And as always, thanks for reading!
