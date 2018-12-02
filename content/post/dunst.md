---
title: "Dunst, A Simple And Lightweight Notification Daemon"
date: 2018-12-02T13:19:24+07:00
lastmod: 2018-12-02T13:19:24+07:00
draft: false
keywords: ["linux", "ricing", "dunst", "notification", "minimal"]
description: "A guide about how to install and configuring Dunst, the minimal notification"
tags: ["linux", "ricing", "dunst", "notification", "minimal"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
According to the Dunst website Dunst is a lightweight replacement for the notification daemons
provided by most desktop environments. It’s very customizable, isn’t dependent on any toolkits,
and therefore fits into those window manager centric setups we all love to customize to perfection.

When I installed Debian, I start from minimal install. So, I don't have any notification by
default. Then I choose dunst. If you already other notification daemon and want to try Dunst,
Just simply add Dunst to your autostart. The default notification daemon should be overriden
by Dunst.

## Installation
Dunst is available in the official Debian repository

```shell
sudo apt-get install dunst libnotify-bin
```

Then try to run it by `dunst &` command and then `disown`. Close the terminal. Open a new one,
then test it by sending a notification message.

```shell
notify-send "Notification Title" "Notification Messages"
```

{{< figure src="https://u.cubeupload.com/addy15/dunsttest.png" >}}

Of course the default look doesn't look like that. It needs to be configured first.
Here is my configurations. It's placed in **~/config/dunst/dunstrc**

{{< highlight rc "linenos=table" >}}
[global]
monitor = 0
follow = mouse
geometry = "300x60-20+48"
indicate_hidden = yes
shrink = no
separator_height = 0
padding = 32
horizontal_padding = 32
frame_width = 2
sort = no
idle_threshold = 120
font = rissole 8
line_height = 4
markup = full
format = %s\n%b
alignment = left
show_age_threshold = 60
word_wrap = yes
ignore_newline = no
stack_duplicates = false
hide_duplicate_count = yes
show_indicators = no
icon_position = off
sticky_history = yes
history_length = 20
browser = /usr/bin/firefox -new-tab
always_run_script = true
title = Dunst
class = Dunst

[shortcuts]
close = ctrl+space
close_all = ctrl+shift+space
history = ctrl+grave
context = ctrl+shift+period

[urgency_low]
timeout = 4
background = "#141c21"
foreground = "#93a1a1"
frame_color = "#8bc34a"

[urgency_normal]
timeout = 8
background = "#141c21"
foreground = "#93a1a1"
frame_color = "#ba68c8"

[urgency_critical]
timeout = 0
background = "#141c21"
foreground = "#93a1a1"
frame_color = "#ff7043"
{{< /highlight >}}

Geometry is the size of the notification. 300x60-20+48 means, my notification box has 300px width
& 60px height, placed on the right-top of the screen with 20px margin from the right & 48px margin
from the top. Negative value in -20 means it's drawed on the right. If you change the value to
positive, the notification box would be drawed on the left of the screen. The positive value on
in +48 means it's drawed on the top. If you change the value to negative, the notification box
would be drawed on the bottom of the screen.
Padding is the margin between the text and the box outline. Frame width is how thick your
notification box outline. But I use Debian Jessie now, the Dunst version is too old and hasn't
been supporting frame yet. I will upgrade my Debian Jessie setup to Stretch soon.
The font I use in the configurations above is selfmade and I haven't uploaded it yet. So please
change it with a font you have. `format = %s\n%b**`, %s means summary (the title of the
notification), and %b means body (the messages of the notification). `\n` is linebreak,
without it, your notification messages would be drawed on the right side of the title.
The format could be improved with markup. For example, make the title bold.

{{< highlight rc "linenos=table, linenostart=16" >}}
format = "<b>%s</b>\n%b"
{{< /highlight >}}

I didn't enabled it because my bitmap font doesn't support bold font. Creating bold bitmap
font in 5px width of character is not very rational.

{{< highlight rc "linenos=table, linenostart=32" >}}
[shortcuts]
close = ctrl+space
close_all = ctrl+shift+space
history = ctrl+grave
context = ctrl+shift+period
{{< /highlight >}}

One of Dunst selling point is some actions could be controlled using keybind. Like **Ctrl+Space**
to close the notification popup and **Control+Backtick** to see the previously closed notification.
Dunst also could draw different outline color for each urgency level. In the configurations above
I set the low urgency frame to green. The medium urgency frame to blue. And the critical urgency
to orange. But again, my version of Dunst doesn't support frame.

## Utilization
Dunst by default handles every notification sent by any applications.
But we could playing around with custom messages to send. This actually also works on any
notification daemon, not only Dunst. But it looks even cooler if we did it on Dunst.
### Showing Time

```fish
notify-send "Current Time" (date)
```

Or with formating

```fish
notify-send "Current Time" (date +"%a %-d %b at %H:%M")
```

{{< figure src="https://u.cubeupload.com/addy15/dunsttime.png" >}}

We could add keybind to our DE/WM to call that command. So we could live without statusbar.
I use Openbox, so maybe like this 

```xml
<keybind key="W-t">
   <action name="Execute">
      <command>notify-send "Current Time" (date)</command>
   </action>
</keybind>
```

It will send the time info if I hit **Super+T**.

### Showing Now Playing
I [use mpd & ncmpcpp](/post/configuring-ncmpcpp), so I could get the music info using **mpc**
```fish
notify-send "Now Playing" (mpc current)
```

{{< figure src="https://u.cubeupload.com/addy15/dunstmpc.png" >}}

Do you know? Ncmpcpp support *execute a command on song change* in its configurations.
So, we could make ncmpcpp send a notification on song change. Just try it.
I'm still on Jessie, and again, the ncmpcpp version on Debian Jessie is too old
and doesn't support that command. So, I couldn't give an example.
Maybe the code is like this. Hopefully it works.

```ncmpcpp
execute_on_song_change = notify-send "Now Playing ♫" (mpc current)
```

### Any Many Other Possibilities
You just need to use your imagination to unleash your creativity.

{{% notice info %}} 
I [use fish](/post/fish-shell), so the subtitution command is `(command)`.
If you're using bash or any POSIX compliance shell, the subtitution command is `$(command)`
{{% /notice %}}

And as always, thanks for reading!
