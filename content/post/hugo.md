---
title: "How I Built This Blog Using Hugo Static Site Generator"
date: 2019-01-22T18:09:57+07:00
lastmod: 2019-01-22T18:09:57+07:00
draft: false
keywords: ["linux", "ricing", "blog", "website", "debian", "guide", "hugo", "ssg", "static", 
"site", "generator", "minimal", "simple"]
description: "Just want to share my experience of my first time creating a website using Hugo
static site generator with Jane theme"
tags: ["hugo"]
categories: ["Blogging"]
author: "Addy"
comment: true
toc: false
autoCollapseToc: false
reward: true
mathjax: false
---

## Introduction
According to its GitHub README, Hugo is a static HTML and CSS website generator written in Go. It
is optimized for speed, ease of use, and configurability. Hugo takes a directory with content and
templates and renders them into a full HTML website. Hugo relies on Markdown files with front
matter for metadata, and you can run Hugo from any directory. This works well for shared hosts and 
other systems where you don’t have a privileged account. Hugo renders a typical website of
moderate size in a fraction of a second. A good rule of thumb is that each piece of content render
in around 1 millisecond. Hugo is designed to work well for any kind of website including blogs,
tumbles, and docs.

At first, I didn't even know how to build a static website. People on Dotfiles ID recommended me to
try to build a website using SSG (Static Site Generator) like Jekyll, Hexo, or Hugo. I didn't have 
programming languange knowledge, so I worried it would be hard. But after seeing a beautiful theme
list, I felt motivated to try it. After that, I duckduckwent about popular SSG. The big four were

1. Jekyll
2. Hexo
3. Gatsby
4. Hugo

I was interested with Hugo, simply because it distributed the **\*.deb** package. I use Debian, so
it makes me easier to install it. And even better it doesn't pull any dependency, not even the Go
compiler. So, it keeps my setup simple. The other SSG are based on things I don't understand, which
needs to setup a lot thing like node.js, ruby, and other things I don't understand. I don't have a
time to learn them. So, Hugo is perfect SSG for me. The unintentional advantage, Hugo is really
fast at building website.

## Installation
Well, it's actually available on Debian repository since Stretch. But the package version is too
old. Most of Hugo theme reqires higher version of Hugo. So, I went to its 
[Release Page](https://github.com/gohugoio/hugo/releases) on GitHub to get the higher package
version in **\*.deb** format. Installing package outside the repo is actually unrecommended,
but Hugo doesn't pull any dependencies. So, if something goes wrong, I just need to remove Hugo,
other packages will not be affected. Fortunately, until now I never had any issue with the
sideloaded Hugo. After I downloaded it, I installed it using **dpkg**.

```Shell
cd Downloads
sudo dpkg -i hugo_0.51_Linux-32bit.deb
```

## Building A Website
### Creating Two Repositories on GitHub
I hosted my blog on GitHub. So, I made two repositories on GitHub. One to host the generated html.
I named the repository **addy-dclxvi/addy-dclxvi.github.io**. It had to be
**\<username\>.github.io**, to make it instantly accesible from
**https://\<username\>.github.io**. And one more to put the source code to keep the spirit of Open
Source. I simply named the repository **addy-dclxvi/blog**.

### Cloning The Empty Repo
```Shell
# I put it on ~/.blog folder
git clone https://github.com/addy-dclxvi/blog.git .blog
cd .blog
git clone https://github.com/addy-dclxvi/addy-dclxvi.github.io.git public
```
The repo of generated html had to placed in public folder. The official Hugo guide asked me create
git submodules, but I didn't do it. I didn't want things to be complicated, I want simplicity.
Then I told hugo to create a new website.

```Shell
hugo new site ~/.blog
```

Then picked a theme from [Hugo Themes](https://themes.gohugo.io/).

{{< figure src="https://u.cubeupload.com/addy15/hugothemes.png" >}}

I was interested with [Jane by Xianmin](https://github.com/xianmin/hugo-theme-jane). So, I
installed it.

```Shell
# Clone the theme
cd ~/.blog/themes
git clone https://github.com/xianmin/hugo-theme-jane jane
# Then Copy the default configuration
cd ~/.blog
cp themes/jane/exampleSite/config.toml .
# Edit the configuration file to fill your necessity
```

### Testing The Website in Localhost

```Shell
cd ~/.blog
hugo server -b https://localhost:1313/
```

When the first time I launched it. I didn't have any article. To create a new article

```Shell
# Better to left the Hugo daemon from the command above running
# Open a new terminal for this
cd ~/.blog
hugo new posts/automatically-generate-openbox-menu.md
```

Then I edited the new article in **posts/automatically-generate-openbox-menu.md**.
After writing some sentences and saved it. I opened a web browser to access
**http://localhost:1313/**.

### Editing The Theme
I decided not to edit anything in theme folder. So, if the theme was updated, no need to worry
about files being overwritten. I customized the visual using
[~/.blog/static/css/custom.css]
(https://github.com/addy-dclxvi/blog/blob/master/static/css/custom.css)
file.

### Pushing The Content
I Stopped the Hugo daemon. Then build the website using `hugo` command.

```Shell
cd ~/.blog
hugo
```

Then Hugo generated the html and placed it in **~/.blog/public** folder. Next step just `git push`
like usual. I needed to do it both in **~/.blog** folder and in **~/.blog/public** folder. The
website then instantly available at https://addy-dclxvi.github.io/

{{< figure src="https://u.cubeupload.com/addy15/hugoblog.png" >}}

{{% notice info %}} 
I add **public** folder to
[.gitignore](https://github.com/addy-dclxvi/blog/blob/master/.gitignore),
in the **blog** repository, because the source code repository don't need the generated html.
{{% /notice %}}

## End
And as always, thanks for reading!
