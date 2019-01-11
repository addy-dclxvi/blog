---
title: "Using Conky to Generate Lemonbar Statusline"
date: 2018-12-19T17:18:27+07:00
lastmod: 2019-01-11T19:20:27+07:00
draft: false
keywords: ["linux", "ricing", "lemonbar", "conky", "panel", "statusbar", "statusline"]
description: "A guide about how to use Conky to generate statusline for Lemonbar"
tags: ["linux", "ricing", "lemonbar", "conky", "panel"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
According to its GitHub page, Lemonbar (formerly known as bar) is a lightweight bar entirely based
on XCB. Provides full UTF-8 support, basic formatting, RandR and Xinerama support and EWMH
compliance without wasting your precious memory. I use Debian, and Lemonbar is available in Debian
repository since Stretch, so just install it normally.

```shell
sudo apt-get install lemonbar
```

It just a bar. It doesn't come with any module like in Polybar by default. To make it shows
the content we want, we have to create the text of it then pipe it to Lemonbar. For example, If
you want to display "abcdef" text in Lemonbar, you have to do:

```shell
echo "abcdef" | lemonbar -p
```

Then it will show a bar with "abcdef" text in your desktop

{{% notice info %}}
`-p` flag means don't close the bar after the stdin is closed
{{% /notice %}}

Want it to show the time? You have to do:

```shell
date +"%a %-d %b at %H:%M:%S" | lemonbar -p
```

But you will notice that the second is not moving. To make it updated, we have to create infinity
loops. I don't know how to do it with fish, my default shell. So, I use bash.

```bash
while true; do date +"%a %-d %b at %H:%M:%S"; sleep 1; done | lemonbar
```

{{% notice info %}} 
Because it's infinity loops, Lemonbar wouldn't be closed even though without `-p` flag
{{% /notice %}}

Lemonbar itself very lightweight, the resource usage depends on the shell script you made.
The example above only executing a shell script to display the time, so it won't cause any
noticeable CPU load. But when I tried to create a shell script to display clock, current track of
ncmpcpp, wifi ssid, alsa  volume, and some other items with spamming *cat*, *grep*, *awk*, *print*,
*head*, *tail*, and *sed*, both CPU & RAM usage increased, which is defeating the purpose of
using a featherweight bar. So, I try to replace the shell script with Conky. Despite it has a lot
of limitations, I'm quite happy with the results.

## Configuring Conky
Install it first of course.

```shell
sudo apt-get install conky
```

Conky configuration is placed in **~/.conkyrc** file. Remember that we will use Conky to replace
the `echo` of shell script. So, we have to send the output of Conky to stdout instead of X like
how Conky commonly used. Don't forget to set the interval too, to replace `sleep` from the shell
script.

```conky
conky.config = { 
    out_to_x = false,
    out_to_console = true,
    update_interval = 1,
}
```

Then we add some text. Time and wifi ssid for example.

```conky
conky.config = { 
    out_to_x = false,
    out_to_console = true,
    update_interval = 1,
}

conky.text = [[
${time %a %-d %b at %H:%M} ${wireless_essid}
]]
```

Pipe the Conky output to Lemonbar.

```shell
conky | lemonbar
```

## Eye Candy
I know you will be complaining about it looks ugly. So, we will deuglify it. First, I use fonts
from [this post](/post/bitmap-fonts/), so please get them. Don't forget to read the explanation
too. Next, we will pimp the Conky with more
[Conky Objects](http://conky.sourceforge.net/variables.html),
[Lemonbar Options](https://github.com/LemonBoy/bar#options),
and
[Lemonbar Formatting](https://github.com/LemonBoy/bar#formatting).
Because we will launch Lemonbar with long options. It would be wise to create a new file to make it
less messy. Save this file anywhere you want and mark it as executable. I personally save it in
**~/.scripts/lemonconky**. Then `chmod +x ~/.scripts/lemonconky`.


```bash
#!/bin/bash
conky | lemonbar \
-f -*-rissole-* \
-f -*-waffle-* \
-g x28 \
-B "#141c21" \
-F "#93a1a1"
```

- `#!/bin/bash` is the shell intepreter, to make this script executed by bash instead of default
shell. You can change it to `#!/bin/dash` to decrease the resource usage.
- I use backslash to prevent linebreak, because it actually a oneliner script.
- `-f` is load the font.
- `-g` is the geometry, 28 pixels height.
- `-B` is the default background color, black.
- `-F` is the default foreground color, white.

```conky
conky.config = { 
    out_to_x = false,
    out_to_console = true,
    update_interval = 1,
}

conky.text = [[
%{l}\
%{B\#d12f2c}%{F\#141c21}  %{B-}%{F-}\
%{B\#263640} ${mpd_smart 40} %{B-}\
%{c}\
${time %a %-d %b at %H:%M}\
%{r}\
%{B\#263640}  ${wireless_essid} %{B-}\
%{B\#2587cc}%{F\#141c21}  ${pa_sink_volume}% %{B-}%{F-}\
]]
```

Now let's go back to **~/.conkyrc** again. And have you read the the Lemonbar formatting options?
I use switch alignment and switch color formatting. The code inside `%{}` is Lemonbar formatting
and the code inside `${}` is Conky objects. Here is the detailed explanation of line number 8-15.

- **Line number 8 :** Switch alignment to the left.
- **Line number 9 :** Switch background to red, foreground to black, display the music icon,
reset the background color, and reset the foreground color.
- **Line number 10 :** Switch background to gray, display the current playing mpd track (cut to 40
character if it too long), and reset the background color.
- **Line number 11 :** Switch alignment to the center.
- **Line number 12 :** Display the time.
- **Line number 13 :** Switch alignment to the right.
- **Line number 14 :** Switch background color to gray, display connectivity icon, display the
current ssid, and reset the background color.
- **Line number 15 :** Switch the background color to blue, foreground to black, display bell icon,
display the volume (PulseAudio default sink), reset the backgound color, and reset the foreground
color.

I put backslash in the end of every line to prevent linebreak, because they have to be printed as
single line text. I also put backslash before "#" symbol in the color code, because "#" is the
code of *comment*. To print literal "#", add backslash before it. And how much resource it takes?

{{< figure src="https://u.cubeupload.com/addy15/htop.png" >}}

Average CPU load is zero to zero point three! By the way, I use an old CPU, only dual core 2GHz.
Can't afford a new one. When I used shell script to generate Lemonbar statusline with equivalent
items & interval. The CPU usage was above 3%.

{{< figure src="https://u.cubeupload.com/addy15/psmem.png" >}}

The RAM usage is less than 5MB! Do You want even less resources? Remove the lemonconky script,
and run the script directly.

```shell
conky | lemonbar -f -*-rissole-* -f -*-waffle-* -g x28 -B "#141c21" -F "#93a1a1"
```

## More Tricks
### If And Else
Do you know? Conky has `if` and `else` command. I use Audacious to double click mp3 file, because
[ncmpcpp](/post/configuring-ncmpcpp/) can't do it. So, I want my Lemonbar display the Audacious
track when I play music using Audacious, display the ncmpcpp track when Audacious is not playing,
and display "Music is not playing" when both are stopped. And show "Muted" in the volume part of my
Lemonbar when I mute the volume. They are achieveable by utilizing `if` and `else`.

```conky
conky.config = {
    out_to_x = false,
    out_to_console = true,
    update_interval = 1,
}

conky.text = [[
%{l}\
%{B\#d12f2c}%{F\#141c21}  %{B-}%{F-}\
%{B\#263640} ${if_running audacious}${audacious_title 40}${else}\
${if_running mpd}${mpd_smart 40}${else}Music Is Not Playing${endif}${endif} %{B-}\
%{c}\
${time %a %-d %b at %H:%M}\
%{r}\
%{B\#263640}  %{F-}${wireless_essid} %{B-}%{F-}\
%{B\#2587cc}%{F\#141c21}  ${if_pa_sink_muted}Muted${else}\
${pa_sink_volume}%${endif} %{B-}%{F-}\
]]
```

### Clickable Area
I also want when I do left click in the volume part of my Lemonbar, Pavu Control launched.

```conky
conky.config = {
    out_to_x = false,
    out_to_console = true,
    update_interval = 1,
}

conky.text = [[
%{l}\
%{B\#d12f2c}%{F\#141c21}  %{B-}%{F-}\
%{B\#263640} ${if_running audacious}${audacious_title 40}${else}\
${if_running mpd}${mpd_smart 40}${else}Music Is Not Playing${endif}${endif} %{B-}\
%{c}\
${time %a %-d %b at %H:%M}\
%{r}\
%{B\#263640}  %{F-}${wireless_essid} %{B-}%{F-}\
%{B\#2587cc}%{F\#141c21}%{A:pavucontrol:}  ${if_pa_sink_muted}Muted${else}\
${pa_sink_volume}%${endif} %{B-}%{F-}%{A}\
]]
```

Weird thing, Lemonbar do `echo command` instead of `exec command` when the clickable area clicked.
So, to make it launch `pavucontrol`, I need to pipe Lemonbar stdout to the shell. Edit the
previous lemonconky script.

```dash
#!/bin/dash
conky | lemonbar \
-f -*-rissole-* \
-f -*-waffle-* \
-g x28 \
-B "#141c21" \
-F "#93a1a1" \
| sh
```

### Notes
- Conky has a lot of limitations, like can't display current workspace number. But I use Openbox.
The workspace indicator will be displayed as OSD when I switch the workspace.
- Different distro could have different build options for Conky. Some of them were not built with
PulseAudio support. So, could not be used to display the Pulse Audio volume.
- Debian provides several Conky packages with different build options. There are `conky-cli`,
`conky-std`, `conky`, and `conky-all`. You can switch to `conky-cli` to get even less resource
usage and filling your guilty pleasure of minimalism. But of course, the available Conky Objects
are reduced too.
- The guide here is pretty basic. There are a lot of Conky and Lemonbar potentials those haven't
been unleashed. It's your task to explore them. Keep improving!

{{< figure src="https://u.cubeupload.com/addy15/whitewalls.png" >}}

{{< figure src="https://u.cubeupload.com/addy15/whitewalls2.png" >}}

Almost forgot to post the screenshot of my desktop. That's how my Lemonbar looks like. By the way
I listened to [Colors](https://www.youtube.com/watch?v=ijFuvNz7ukk&list=PLDAD8A6BDC7ED92A3),
my favourite **Between The Buried And Me** album while writing this article. My favourite track
in this album is White Walls.

And as always, thanks for reading!
