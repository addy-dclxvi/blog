---
title: "Use URxvt Daemon Mode to Decrease The Resources Usage"
date: 2019-01-21T20:28:52+07:00
lastmod: 2019-01-21T20:28:52+07:00
draft: false
keywords: ["linux", "ricing", "urxvt", "urxvtd", "urxvtc", "daemon", "terminal", "efficient",
"debian"]
description: "Use daemon mode in URxvt to decrease the resources usage of several instance of
terminals, also how to register URxvtc to Debian Alternatives entry"
tags: ["linux", "ricing", "urxvt"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
According to the manual page, URxvtd is same with URxvt but runs as a daemon that can open multiple
terminal windows within the same process. Advantages of running a urxvt daemon include faster
creation time for terminal windows and a lot of saved memory. The disadvantage is a possible
impact on stability. If the main program crashes, all processes in the terminal windows are
terminated.

## How To Use
### Start The Daemon
From my [Urxvt Configurations](/post/configuring-urxvt/), nothing needs to be changed. We only
need to tell URxvt to start the daemon mode at the startup. I use Openbox, so I edit the
**~/.config/openbox/autostart** file to add `urxvtd -q &`. Then the URxvt daemon will be launched
on the next login.

{{< highlight "Bash" "linenos=table, linenostart=1, hl_lines=7" >}}
#!/bin/bash
export PATH="${PATH}:$HOME/.scripts"
compton -b
xbacklight -set 10
hsetroot -center ~/.wallpaper.jpg
xset +fp ~/.fonts/misc/
urxvtd -q &
xsettingsd &
conky | lemonbar -g x28 \
-f -*-rissole-* -f -*-waffle-* \
-B "#141c21" -F "#93a1a1" &
{{< / highlight >}}

{{% notice info %}} 
The `-q` flag means quiet. This option will suppress the output messages (errors and warnings will
still be logged).
{{% /notice %}}

### Launching The Terminal
URxvtd acts as server, Now we need to launch the client. Instead of `urxvt`, the command to launch
URxvt in client mode is `urxvtc`. So make a keybind and a menu entry to launch `urxvtc` command.

- In the **rc.xml** I add:

{{< highlight "XML" "linenos=table, linenostart=437" >}}
        <!-- Super + Enter to launch the terminal -->
        <keybind key="W-Return"> 
            <action name="Execute">
                <command>urxvtc</command>
            </action>
        </keybind>
{{< / highlight >}}

- And in the **menu.xml** I add:

{{< highlight "XML" "linenos=table, linenostart=8" >}}
        <item label="Terminal emulator">
            <action name="Execute">
                <execute>urxvtc</execute>
            </action>
        </item>
{{< / highlight >}}

- We can also launch it from dmenu too. Just make sure you pick `urxvtc`, instead of `urxvt`.

## Resources Usage
I feel launching a new terminal is faster than in normal mode. And the memory usage is also
decreased. The terminals are counted as two instances, but the shell remains one instance per
terminal. I open four terminal windows including `htop`.

{{% notice info %}} 
When I launched URxvt in normal mode. The instance is also counted as two, even only one
terminal opened. So, don't complain for two instances for four terminals in daemon mode.
{{% /notice %}}

{{< figure src="https://u.cubeupload.com/addy15/urxvtdterminals.png" >}}

{{< figure src="https://u.cubeupload.com/addy15/urxvtdhtop.png" >}}

The process tree tells us that four instances of fish are attached into one URxvtd process.

{{< figure src="https://u.cubeupload.com/addy15/urxvtdpsmem.png" >}}

`ps_mem` command tells us that the two instance of urxvtd (it's four terminal) only takes 10.5MB
of RAM.

## Optional
I use Debian, so I want to follow the
[Debian Alternatives](https://wiki.debian.org/DebianAlternatives)
guidelines. Instead of hardcoded `urxvtc` command, I keep the traditional
`x-terminal-emulator` command in my keybind to launch the default terminal. And then I set `urxvtc`
as the default terminal.

- **rc.xml**

{{< highlight "XML" "linenos=table, linenostart=437" >}}
        <!-- Super + Enter to launch the terminal --> 
        <keybind key="W-Return"> 
            <action name="Execute">
                <command>x-terminal-emulator</command>
            </action>
        </keybind>
{{< / highlight >}} 

- **menu.xml**

{{< highlight "XML" "linenos=table, linenostart=8" >}}
        <item label="Terminal emulator">
            <action name="Execute">
                <execute>x-terminal-emulator</execute>
            </action>
        </item>
{{< / highlight >}}

- Add **urxvtc** entry to the list of **x-terminal-emulator** (root access required)

```Shell
update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator /usr/bin/urxvtc 100
```

- Set **urxvtc** as the default terminal emulator (root access required)

```Shell
update-alternatives --set x-terminal-emulator /usr/bin/urxvtc
```

## As Always
Thanks for reading!
