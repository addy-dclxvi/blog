---
title: "My Openbox Configurations"
date: 2019-01-29T20:42:05+07:00
lastmod: 2019-01-29T20:42:05+07:00
draft: false
keywords: ["linux", "ricing", "openbox", "guide", "configurations", "debian", "minimal", "guide"]
description: "Explanation of my Openbox Configurations and how to use it"
tags: ["linux", "ricing", "openbox"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
This post is an explanation about my Openbox configurations and how to use them. Openbox is my
favourite window manager, and it's the first window manager I use. At first, I didn't even know
how to make Openbox worked. When I installed it and ran it, I just got a blank black screen and
didn't even know how to launch any application. I followed several guide but couldn't understand
it, I gave up. But after a while, I heard about Crunchbang. It came with preconfigured Openbox,
and a complete set of software to make Openbox usable as a desktop. I really enjoyed Crunchbang
and kept playing around with it. Playing with Crunchbang meaned learning. I learned a lot about
configuration files and theming. Also about modular applications that could work right with
Openbox. I also tried a lot other popular window managers, but still couldn't leave Openbox.

After I thought I had understood enough, I tried to build a clean Openbox setup with Arch, and
then Void. But they were pretty fragile, I dealed with a lot of breaks there. So, now I use Debian.
But I still keep my Void setup, sometimes I want to try a new version of software that Debian
doesn't provide. I don't want to use Debian Testing or Sid, it will defeat my purpose of using
Debian.

Despite of distrohopping a lot, my Openbox setup mostly remains the same with Crunchbang
or BunsenLabs formula. By the way, I install Debian from the minimal CD with everything unchecked
on the tasksel. Then I only install packages I need, the result even more minimal than Arch with
similar setup due to Debian package splitting. For example, instead of installing full Libre
Office Suite, I can cherrypick Libre Office Writer & Libre Office Calc only. But I still don't have
a time to write a documentation about my full Debian setup, might be next time. In this article, I will only explain about Openbox and Openbox related packages.

## Installation
Beside installing Openbox, I also install some essential supporting packages

```shell
sudo apt-get install openbox suckless-tools compton hsetroot xsettingsd lxappearance scrot conky lemonbar openbox-menu lxmenu-data
```

- Compton is a compositor to provide some desktop effects like shadow, transparency, fade, and 
transiton.
- Hsetroot is a wallpaper handler. Openbox has no wallpaper handler by default. Openbox comes with
`fbsetroot` & `fbsetbg` by default. But they are not an actual wallpaper handler. They are just
wrapper scripts to find any wallpaper handler available and use it as fluxbox wallpaper handler.
When I install Debian, I start from minimal install. So, I don't have any wallpaper handler that
support JPG/PNG by default. So, I choose `hsetroot`.
- Suckless Tools are several utility made by [suckless.org](https://suckless.org), but actually I
only need `dmenu`. Unfortunately the package is not splitted, so I need to pull them all.
- Xsettingsd is a simple settings daemon to load fontconfig and some other options. Without this,
fonts would look rasterized in some applications.
- LXAppearance is used for changing GTK theme icons, fonts, and some other preferences.
- Scrot is a simple screenshot taking app. Full explanation in
[this article](/post/scrot/).
- Lemonbar is a bar or panel, and I use Conky to generate statusline for Lemonbar. Full explanation
in [this article](/post/lemonbar-conky/).
- Openbox Menu is a menu generator gathering data from LXMenu Data. Full explanation in
[this article](/post/autogenerate-openbox-menu/)

Then, get my Openbox configurations from [this](/file/openbox.tar.gz) archive. Extract it to your
home directory. So, you will get the **.config** and other directories in your home.
I use URxvt as default terminal. If you're interested, check [this](/post/configuring-urxvt/)
article. I use Dunst as notification daemon. If you're interested, check [this](/post/dunst/)
article. I use ncmpcpp as music player. If you're interested, check
[this](/post/configuring-ncmpcpp/) article. I use Fish as interactive shell. If you're interested, 
check [this](/post/fish-shell/) article.

## Explanations

### ~/.config/openbox/
It's the folder where Openbox configurations are stored, there is several files inside.

- **autostart** contains what applications need to be started on login
- **menu.xml** contains items displayed when we do right click on the desktop
- **rc.xml** is the main Openbox configuration file. It stores what items displayed in the
titlebar, how many workspace we have, what font is used in the titlebar, keybinds, mousebinds,
window behavior, and many others.

### ~/.scripts/
I put three files in the archive. They are just wrapper scripts.

- **launch** is a wrapper script to launch dmenu with custom arguments.
- **pulsevol** is a script I stole from [Vera's](https://github.com/okitavera) dotfiles to make me
easier to control the pulseaudio volume.
- **screeny** is a wrapper script for taking screenshot using scrot then open the result instantly
using the default image viewer.

## Launch
Logout then login again with Openbox session in your login manager. And welcome to Openbox.

## Keybinds
Have you read my [previous article about my Fluxbox configurations](/post/my-fluxbox/)?
Yes, they are pretty similar. My other WMs are also have similar keybinds. So, even I frequently
switched WM, I will not lost.

- **Super + Enter** launch URxvt
- **Super + D** launch dmenu with wrapper script
- **Super + Space** launch Openbox Menu, just like right click on the desktop
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
- **Super + Shift + Backspace** reload Openbox, do this after modify the configuration files
- **Double Click Titlebar** maximize
- **Scroll Up Titlebar** roll up window
- **Scroll Down Titlebar** restore rolled up window
- **Double Click Desktop** list opened programs, so I could survive without a taskbar
- ..More keybinds just read the *~/.config/openbox/rc.xml* file

## Notes
- Please inspect the code before use.
- These configurations are designed for my daily driver, probably not suitable for you.
So, please modify them.
- I usually use *tint2* as a panel, but these months I want to breath some fresh air. So, I use
Lemonbar.
- None of my apps are using system tray, so I don't install any system tray handler. If you need
a system tray, you could use *trayer* or *stalonetray*. Both are modular and not binded to any
WM or DE.
- My included Openbox theme is designed to match with Arc GTK Theme.
- Dream Theater will release a new album next month.
- I use *systemd*, so the command to shutdown, reboot, and hibernate in Openbox Menu involves
`systemctl` command. On *non-systemd* system, the command will be different.

## Screenshoots
Remains the same as in the previouses articles.

{{< figure src="https://u.cubeupload.com/addy15/myopenboxmenu.png" >}}

{{< figure src="https://u.cubeupload.com/addy15/myopenboxvim.png" >}}

## End
And as always, thanks for reading!
