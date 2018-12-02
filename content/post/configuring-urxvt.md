---
title: "Configuring URxvt to Make It Usable and Less Ugly"
date: 2018-11-30T21:44:11+07:00
lastmod: 2018-11-30T21:44:11+07:00
draft: false
keywords: ["linux", "ricing", "terminal", "urxvt"]
description: "A guide about installing URxvt terminal emulator to make it usable and less ugly"
tags: ["linux", "ricing", "terminal", "urxvt"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
URxvt is a customizable terminal emulator forked from rxvt.
Features of rxvt-unicode include international language support through Unicode,
transparency, the ability to display multiple font types and support for Perl extensions.
URxvt is one of the most popular terminal emulator in UNIX world, especially on Unixporn.
It's well known for being lightweight and riceable.
But just like any other software in UNIX world, it's ugly out of the box.
So, we should configure it before we use it.
URxvt configurations is commonly placed in **~/.Xresources** file.
We will edit it. But first, we need to install it first.

## Installation
I use Debian, so..
```shell
sudo apt-get install rxvt-unicode xsel
```

**rxvt-unicode** is the package name of URxvt.
And **xsel** is a clipboard manager, I install it because URxvt is not shipped with
copy paste by default. We will combine it with a Perl extension to send marked text to xsel.
You can get it from Muennich's [urxvt-perls](https://github.com/muennich/urxvt-perls) repository.
Some extensions marked as deprecated, but it still works for me without any issue.

```shell
git clone https://github.com/muennich/urxvt-perls
```

Enter the  urxvt-perls folder,
then copy **keyboard-select**, **clipboard**, and **url-select**
to **~/.urxvt/ext**

{{< figure src="https://u.cubeupload.com/addy15/urxvtext.png" >}}

## Configurations
Now create a file called **~/.Xresources**. Then open it using your favourite text editor.
Colorscheme is one of most important part of terminal customization.
URxvt uses base16 style colorscheme. It stores background color, foreground color,
cursor color, black, red, green, yellow, blue, magenta, cyan, and white.
Actually it could store more color for bold, underline, and many others.
But I let it to be default, it will take color from other stored colors automatically.

{{< highlight ".Xresources" "linenos=table, linenostart=1" >}}
!! Colorscheme

! special
*.foreground: #93a1a1
*.background: #141c21
*.cursorColor: #afbfbf

! black
*.color0: #263640
*.color8: #4a697d

! red
*.color1: #d12f2c
*.color9: #fa3935

! green
*.color2: #819400
*.color10: #a4bd00

! yellow
*.color3: #b08500
*.color11: #d9a400

! blue
*.color4: #2587cc
*.color12: #2ca2f5

! magenta
*.color5: #696ebf
*.color13: #8086e8

! cyan
*.color6: #289c93
*.color14: #33c5ba

! white
*.color7: #bfbaac
*.color15: #fdf6e3

{{< / highlight >}}

It's my selfmade colorscheme, not very good.
You can do some experiment with colorscheme by yourself later.
Or maybe you could visit https://terminal.sexy to get some colorscheme examples.
But now, we need to configure the appearance first.

{{< highlight ".Xresources" "linenos=table, linenostart=40" >}}
!! URxvt Appearance
URxvt.font: -*-rissole-*
URxvt.boldFont: -*-rissole-*
URxvt.italicFont: -*-rissole-*
URxvt.boldItalicfont: -*-rissole-*
URxvt.letterSpace: 0
URxvt.lineSpace: 0
URxvt.geometry: 92x24
URxvt.internalBorder: 24
URxvt.cursorBlink: true
URxvt.cursorUnderline: false
URxvt.saveline: 2048
URxvt.scrollBar: false
URxvt.scrollBar_right: false
URxvt.urgentOnBell: true
URxvt.depth: 24
URxvt.iso14755: false

{{< / highlight >}}

URxvt has an ability to load
[XLFD](https://en.wikipedia.org/wiki/X_logical_font_description) for bitmap font.
It's my selfmade font by the way, and I haven't uploaded it.
So, please replace `-*-rissole-*` with other font.
Maybe with [Iosevka](https://github.com/be5invis/Iosevka) font.
It's beautiful and currently rising in popularity.
Iosevka is an xft font. So, the configuration syntax like this..

{{< highlight ".Xresources" "linenos=table, linenostart=41" >}}

*.font: xft:Iosevka:style=Regular:size=8
*.boldFont: xft:Iosevka:style=Bold:size=8
*.italicFont: xft:Iosevka:style=Italic:size=8
*.boldItalicFont: xft:Iosevka:style=Bold Italic:size=8

{{< / highlight >}}

You can get the xft name using `fc-list` command.

```shell
fc-list | grep "Iosevka"
```

Replace Iosevka with other font name you want. Letterspace variable handles the space
between characters. Linespace variable handles the linespacing between characters vertically.
Geometry handles the URxvt size on launch. InternalBorder handles the terminal paddings,
for aesthetic reason. But don't set the paddings too much, we should care about usability too.
CursorBlink true makes the cursor blinking, for visibility reason.
CursorUnderline makes the block cursor became an underline, but I set it to false for now.
The underline on this version of URxvt is too thin (I'm still using the old version
because I'm on Debian Jessie), and nearly invisible.
It also vanishes on full-pipe character.
But those problems have been fixed on the newer version of URxvt.
UrgentOnBell makes URxvt to be able to send *need-an-attention* signal
to window manager / taskbar.
Depth: 24 makes URxvt couldn't use true transparency, because I don't need it.
You could change it to Depth: 32 to enable true transparency support.
Iso14755: false to disable the annoying warning about Iso type.
Now let's configure some keybinds those are not available on URxvt by default,
but I can't live without.

{{< highlight ".Xresources" "linenos=table, linenostart=58" >}}

!! Common Keybinds for Navigations
URxvt.keysym.Shift-Up: command:\033]720;1\007
URxvt.keysym.Shift-Down: command:\033]721;1\007
URxvt.keysym.Control-Up: \033[1;5A
URxvt.keysym.Control-Down: \033[1;5B
URxvt.keysym.Control-Right: \033[1;5C
URxvt.keysym.Control-Left: \033[1;5D

{{< / highlight >}}

- **Shift Up** to scroll one line higher
- **Shift Down** to scroll one line lower
- **Control Left** to jump to the previous word
- **Control Right** to jump to the next word

{{< highlight ".Xresources" "linenos=table, linenostart=66" >}}

!! Copy Paste & Other Extensions
URxvt.perl-ext-common: default,clipboard,url-select,keyboard-select
URxvt.copyCommand: xclip -i -selection clipboard
URxvt.pasteCommand: xclip -o -selection clipboard
URxvt.keysym.M-c: perl:clipboard:copy
URxvt.keysym.M-v: perl:clipboard:paste
URxvt.keysym.M-C-v: perl:clipboard:paste_escaped
URxvt.keysym.M-Escape: perl:keyboard-select:activate
URxvt.keysym.M-s: perl:keyboard-select:search
URxvt.keysym.M-u: perl:url-select:select_next
URxvt.urlLauncher: firefox
URxvt.underlineURLs: true
URxvt.urlButton: 1

{{< / highlight >}}

Remember the Perl Extensions we copied before?
Now it's the time to load them and make keybinds to call them.
**M** means meta, or in these modern days called **Alt**.
To copy some text in the terminal, mark them using mouse then hit **Alt+C**.
To paste it on URxvt, hit **Alt+V**.
But if you paste it on other app, just use **Ctrl+V** normally.
That Perl Extension also contains other useful features.
Like select URL in terminal by pressing **Alt+U**, then use arrow keys if there are
more than one URL in current buffer. Then hit **Enter** to launch the selected URL in Firefox.
Hit **Esc** to exit selection mode.

## Conclusion
So, my full URxvt configurations look like this.

{{< highlight ".Xresources" "linenos=table, linenostart=1" >}}

!! Colorscheme

! special
*.foreground: #93a1a1
*.background: #141c21
*.cursorColor: #afbfbf

! black
*.color0: #263640
*.color8: #4a697d

! red
*.color1: #d12f2c
*.color9: #fa3935

! green
*.color2: #819400
*.color10: #a4bd00

! yellow
*.color3: #b08500
*.color11: #d9a400

! blue
*.color4: #2587cc
*.color12: #2ca2f5

! magenta
*.color5: #696ebf
*.color13: #8086e8

! cyan
*.color6: #289c93
*.color14: #33c5ba

! white
*.color7: #bfbaac
*.color15: #fdf6e3

!! URxvt Appearance
URxvt.font: -*-rissole-*
URxvt.boldFont: -*-rissole-*
URxvt.italicFont: -*-rissole-*
URxvt.boldItalicfont: -*-rissole-*
URxvt.letterSpace: 0
URxvt.lineSpace: 0
URxvt.geometry: 92x24
URxvt.internalBorder: 24
URxvt.cursorBlink: true
URxvt.cursorUnderline: false
URxvt.saveline: 2048
URxvt.scrollBar: false
URxvt.scrollBar_right: false
URxvt.urgentOnBell: true
URxvt.depth: 24
URxvt.iso14755: false

!! Common Keybinds for Navigations
URxvt.keysym.Shift-Up: command:\033]720;1\007
URxvt.keysym.Shift-Down: command:\033]721;1\007
URxvt.keysym.Control-Up: \033[1;5A
URxvt.keysym.Control-Down: \033[1;5B
URxvt.keysym.Control-Right: \033[1;5C
URxvt.keysym.Control-Left: \033[1;5D

!! Copy Paste & Other Extensions
URxvt.perl-ext-common: default,clipboard,url-select,keyboard-select
URxvt.copyCommand: xclip -i -selection clipboard
URxvt.pasteCommand: xclip -o -selection clipboard
URxvt.keysym.M-c: perl:clipboard:copy
URxvt.keysym.M-v: perl:clipboard:paste
URxvt.keysym.M-C-v: perl:clipboard:paste_escaped
URxvt.keysym.M-Escape: perl:keyboard-select:activate
URxvt.keysym.M-s: perl:keyboard-select:search
URxvt.keysym.M-u: perl:url-select:select_next
URxvt.urlLauncher: firefox
URxvt.underlineURLs: true
URxvt.urlButton: 1

{{< / highlight >}}

{{< figure src="https://u.cubeupload.com/addy15/urxvt.png" >}}

{{% notice info %}}
Don't forget, after modifying ~/.Xresources file,
You need to reload it using `xrdb ~/.Xresources` command.
And the change of URxvt will be applied on next launch.
{{% /notice %}}

Thanks for reading!
