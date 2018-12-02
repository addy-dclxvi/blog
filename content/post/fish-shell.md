---
title: "Fish, The Friendly Interactive Shell"
date: 2018-11-30T18:16:14+07:00
lastmod: 2018-11-30T18:16:14+07:00
draft: false
keywords: ["linux", "ricing", "terminal", "shell"]
description: "How to install and configure fish, the friendly interactive shell"
tags: ["linux", "ricing", "terminal", "shell"]
categories: ["Linux"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
Most of GNU/Linux distros use bash as the default shell.
But there are many aftermarket alternative of cli shell for it.
And my favourite is fish, the friendly interactive shell.
Some of my favourite features of fish are..

- **Command suggestion**, usually based on history. Hit right arrow or Ctrl+F to complete it.
{{< figure src="https://u.cubeupload.com/addy15/suggestion.png" >}}
- **Tab autocompletion**, type several letter then hit Tab several times to see the options.
Hit enter to choose it.
{{< figure src="https://u.cubeupload.com/addy15/tab.png" >}}
- **Syntax highlighting**.
{{< figure src="https://u.cubeupload.com/addy15/syntax.png" >}}
- **Error telling**, the part which containing any error will be pointed with caret symbol.
{{< figure src="https://u.cubeupload.com/addy15/tell.png" >}}
- **Wildcard anywhere**, but this is a double edge sword. Sometimes my literal question-mark
is treaten as wildcard. So, I need to put a couple quote.
- **Alt Navigation**, hit Alt+Left/Right to go to previous/next directory.
Like in the web browser or file manager.
- **And many others**. I replace oh-my-zsh with fish because it provides every feature I need
from oh-my-zsh but still minimal & no need to install additional plugin.

## Installation
Fish is available on most ditros official repository. I personally use Debian,
so the command to install it is
```Shell
sudo apt-get install fish
```
Then set is as the default shell
```Shell
sudo chsh -s /usr/bin/fish
```
After you launch a new shell session or open a new terminal, the default shell for your user
will be fish. Maybe logout & login again is needed.

## Configurations
Fish is pretty usable out of the box,
but it looks ugly. So, we need to configure it first. The fish configuration file
is stored in **~/.config/fish/functions/fish_prompt.fish**.
In case you wonder with my fish configuration, here is it.
Beside eye candy, I also add more functionality. Like some useful alias,
hightlight the background selection when cycling it with Tab button
(there is no clue of selected item by default),
hiding the annoying greeting message,
and some other coloring options for accesbility.

{{< highlight fish "linenos=table" >}}
## Left Prompt
function fish_prompt
    # Set the annoying greeting to empty
    set fish_greeting
    set -l last_status $status
    # Show the current working directory
    set_color $fish_color_cwd
    echo -n (prompt_pwd)
    echo -n ' '
    set_color normal
end

## Right Prompt
function fish_right_prompt
    set_color black
    echo -n (date +"%H:%M")
    set_color normal
end

## Window title
function fish_title
    echo -n 'fish in '
    prompt_pwd
end

## Coloring
set fish_color_autosuggestion black
set fish_color_command normal
set fish_color_comment black
set fish_color_cwd blue
set fish_color_cwd_root red
set fish_color_end magenta
set fish_color_error yellow
set fish_color_escape cyan
set fish_color_history_current cyan
set fish_color_host normal
set fish_color_match blue
set fish_color_normal normal
set fish_color_operator cyan
set fish_color_param blue
set fish_color_quote green
set fish_color_redirection blue
set fish_color_search_match --background=black
set fish_color_selection blue
set fish_color_status red
set fish_color_user red
set fish_pager_color_completion blue
set fish_pager_color_description yellow
set fish_pager_color_prefix cyan
set fish_pager_color_progress cyan

## Aliases
alias ls "ls --group-directories-first"
alias lsl "ls --group-directories-first -lh"
alias search "apt-cache search"
alias install "sudo apt-get install --no-install-recommends"
alias upgrade "sudo apt-get upgrade"
alias update "sudo apt-get update"
alias remove "sudo apt-get remove"
alias purge "sudo apt-get remove --purge"
alias clean "sudo apt-get clean"
alias autoclean "sudo apt-get autoclean"
alias autoremove "sudo apt-get autoremove"
alias reconfigure "sudo dpkg-reconfigure"
alias pkglist "dpkg --get-selections | grep -v deinstall | sed s/\tinstall//g"
alias version "apt-cache policy"
alias font-refresh "fc-cache -fv"
alias clone "git clone --depth 1"
alias merge "xrdb ~/.Xresources"

## Keybinding
set fish_key_bindings fish_default_key_bindings
{{< /highlight >}}

On the left shell prompt, I only put current working directory.
I dont need username and host information, because I only use this machine locally
& only have one user. On the right shell prompt I add clock to measure
how long a command is executed. I set the hightlight of command that's not available
in the system (which means error) to yellow to avoid typo.
Maybe my fish configuration is not suitable for you.
So, I recommend you to modify it to make it suit with your taste.

## Note
Fish is not POSIX compliance like bash, zsh, or dash.
And I'm too lazy too learn fish scripting.
So, I'm still using bash as scripting shell.
To make my shell scripts interpreted by bash instead of fish,
I always add `#!/bin/bash` on the line number one in every script.
But no need to worry, almost every shell script you get from the internet
are always shipped with *shebang* by default.

{{< figure src="https://u.cubeupload.com/addy15/shebang.png" >}}

Thanks for reading!
