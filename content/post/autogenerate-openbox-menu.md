---
title: "Automatically Generate Openbox Applications Menu"
date: 2018-11-27T18:44:21+07:00
lastmod: 2018-11-27T18:44:21+07:00
draft: false
keywords: ["openbox", "debian", "linux", "menu", "ricing", "lxmenu", "debian", "minimal"]
description: "Automatically Generate Openbox Applications Menu Using LXMenu Data on Debian"
tags: ["openbox", "debian", "linux", "menu", "ricing", "lxmenu"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
With regard to Openbox Menus,
there are two types: static menus and dynamic menus.
Dynamic menus are also known as pipe menu.
In this article, I will explain how to use pipe menu to generate applications list easily.
Because, writing our applications list manually to **menu.xml** will be exhausting.
I use Debian by the way. So some command line in this article, like how to install packages,
will use Debian way. But don't worry, it should be working in other distros too.
But will slightly different.

## Installaltion
Openbox Menu and LXMenu Data are available in the Debian repository since Jessie.
So, we don't need to compile it manually. Just get it using APT.
```shell
sudo apt-get install openbox-menu lxmenu-data
```

## Applying
Now we need to edit the menu.xml to place the applications menu taken from lxmenu-data.
Paste the following code anywhere you want between `<menu id="root-menu" label="Openbox 3">`
and `</menu>`.
```xml
<menu execute="openbox-menu lxde-applications.menu" id="apps" label="Applications"/>
```
Just in case you curious with what my complete menu.xml looks like, here is it.
{{< highlight xml "linenos=table,hl_lines=19" >}}
<?xml version="1.0" encoding="UTF-8"?>
<openbox_menu
    xmlns="http://openbox.org/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://openbox.org/
    file:///usr/share/openbox/menu.xsd">
    <menu id="root-menu" label="Openbox 3">
        <item label="Terminal emulator">
            <action name="Execute">
                <execute>x-terminal-emulator</execute>
            </action>
        </item>
        <item label="Web browser">
            <action name="Execute">
                <execute>x-www-browser</execute>
            </action>
        </item>
        <separator />
        <menu execute="openbox-menu lxde-applications.menu" id="apps" label="Applications"/>
        <menu id="client-list-menu" />
        <separator />
        <item label="Configurations">
            <action name="Execute">
                <execute>obconf</execute>
            </action>
        </item>
        <item label="Reconfigure">
            <action name="Reconfigure" />
        </item>
        <separator />
        <menu id="exit" label="Exit" >
            <item label="Logout">
                <action name="Exit" />
            </item>
            <item label="Suspend">
                <action name="Execute">
                    <execute>systemctl suspend</execute>
                </action>
            </item>
            <item label="Hibernate">
                <action name="Execute">
                    <execute>systemctl hibernate</execute>
                </action>
            </item>
            <item label="Reboot">
                <action name="Execute">
                    <execute>systemctl reboot</execute>
                </action>
            </item>
            <item label="Shutdown">
                <action name="Execute">
                    <execute>systemctl poweroff</execute>
                </action>
            </item>
        </menu>
    </menu>
</openbox_menu>
{{< /highlight >}}

Reconfigure Openbox then see the result
```shell
openbox --reconfigure
```
{{< figure src="https://u.cubeupload.com/addy15/openboxmenu.png" >}}

Thanks for reading!
