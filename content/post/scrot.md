---
title: "Scrot, A Simple Screenshot Taking App"
date: 2019-01-15T19:44:16+07:00
lastmod: 2019-01-15T19:44:16+07:00
draft: false
keywords: ["linux", "ricing", "scrot", "screenshot", "debian", "minimal", "guide"]
description: "A guide how to install, use, and utilize  scrot, a simple screenshot taking app"
tags: ["linux", "ricing"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
According to Wikipedia, Scrot is a minimalistic command line screen capturing application. It
allows substantial degree of flexibility by specifying parameters on command line, including the
ability to invoke a third-party utility to manipulate the resulting screenshot.

When I installed Debian, I started from minimal install. So it didn't come with any screenshot
taking app by default, then I installed scrot. I just thought that scrot was quite enough for my
necessity.

Scrot is a CLI app, it doesn't come with a GUI like Gnome Screenshoter or Xfce Screenshoter.
To take a screenshot, you only need to execute `scrot` from terminal. But, common way to use scrot
is by creating a keybind executing scrot.

## Installation
Scrot is available in most distro repository. I use Debian, so I use **apt** to install it.

```shell
sudo apt-get install scrot
```

## Run
You can test taking a screenshot by using `scrot` command in terminal. The image will be saved in
your home. And the file name will contains the date & time of when the screenshot was taken.

{{< figure src="https://u.cubeupload.com/addy15/scrot.png" >}}

Then try to create a keybind to make **PrtScreen** button executes `scrot`. I personally use
Openbox. So, I edit my **rc.xml** to add keybind. Well, actually more than this. I will explain
them in the next paragraph.

{{< highlight "XML" "linenos=table, linenostart=462" >}}
        <!-- Take a screenshot, say "Cheeeese!" -->
        <keybind key="Print">
            <action name="Execute">
                <command>scrot</command>
            </action>
        </keybind>
{{< / highlight >}}

## More Tricks
### Preparations
Instead of executing `scrot`. I set my keybind to execute a script named `screeny`.

{{< highlight "Bash" "linenos=table, linenostart=1" >}}
#!/bin/bash

# A simple script to take screenshoot using scrot
# Then add date to the file name
# Then open the result instantly using default image viewer
# Need xdg-utils, mostly preinstalled
# Cheers! Addy

# 'screeny -f' to take a screenshot for current focused window only
if [[ $1 = "-f" ]]; then
    scrot 'Cheese_%y.%m.%-d_%H.%M.%S.png' --focused --border \
    -e 'mv $f ~/Pictures; xdg-open ~/Pictures/$f'

# 'screeny -s' to take a screenshot with selection by dragging a rectangle
elif [[ $1 = "-s" ]]; then
    scrot 'Cheese_%y.%m.%-d_%H.%M.%S.png' --select \
    -e 'mv $f ~/Pictures; xdg-open ~/Pictures/$f'

# 'screeny' only to take a screenshoot normally
else
    scrot 'Cheese_%y.%m.%-d_%H.%M.%S.png' \
    -e 'mv $f ~/Pictures; xdg-open ~/Pictures/$f'
fi
{{< / highlight >}}

{{% notice info %}} 
I'm not a programmer.
{{% /notice %}}

I put that `screeny` script in **~/.scripts** folder, along with my other custom scripts. Then I
export that folder to my `$PATH` using Openbox autostart file, so I can execute screeny command
only by typing `screeny`, instead of `~/.scripts/screeny`. Here is the example of my
**~/.config/openbox/autostart** file.

{{< highlight "Bash" "linenos=table, linenostart=1" >}}
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

### Show The Result of The Screenshot Instantly
{{< highlight "XML" "linenos=table, linenostart=462" >}}
        <!-- Take a screenshot, say "Cheeeese!" --> 
        <keybind key="Print">
            <action name="Execute">
                <command>screeny</command>
            </action>
        </keybind>
{{< / highlight >}}

When I take a screenshoot, it will rename the file to **Cheese_\<date\>-\<time\>**. Then move it to
**~/Pictures** folder. Then open the screenshot using default image viewer. So, I could delete it
immediately using **Del** button in my keyboard if I was not satisfy with the result. By the way,
my default image viewer is **Viewnior**.

{{< figure src="https://u.cubeupload.com/addy15/screeny.png" >}}

{{% notice info %}}
I use `xdg-open` to determine the default image viewer. If you don't have `xdg-open`, just replace
`xdg-open` in the screeny script with your image viewer name. And If Your languange is not English,
replace `~/Pictures` in the script with your own languange of "Pictures".
{{% /notice %}}

### Take A Screenshot of Current Focused Window Only
{{< highlight "XML" "linenos=table, linenostart=468" >}}
        <keybind key="A-Print">
            <action name="Execute">
                <command>screeny -f</command>
            </action>
        </keybind>
{{< / highlight >}}

Hit **Alt+PrtSCreen** to take the screenshot of current active window only. The script then
will rename it. Then move it to **~/Pictures** folder. Then open the screenshot using default image
viewer. Taking the focused window only is pretty handy for asking support in the forum. I only need
to show the necesarry part of the issue I get.

### Take A Screenshoot in Custom Area
{{< highlight "XML" "linenos=table, linenostart=473" >}}
        <keybind key="C-Print">
            <action name="Execute">
                <command>screeny -s</command>
            </action>
        </keybind>
{{< / highlight >}}

Hit **Ctrl+PrtScreen** then draw a rectangle by dragging your mouse. It will take a screenshot in
the rectangle you draw. The script then will rename it. Then move it to **~/Pictures** folder. Then
open the screenshot using default image viewer. 

## Thank You
And as always, thanks for reading!
