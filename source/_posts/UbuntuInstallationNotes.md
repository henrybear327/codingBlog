---
title: Ubuntu Installation Notes
categories:
  - Ubuntu
date: 2017-05-09 23:05:36
tags:
  - Ubuntu
---

Random notes...

<!-- more -->

# Install frequently used packages

`sudo apt install vim git chromium-browser unity-tweak-tool astyle clang-format python3-pip`

# Install `oh-my-zsh`

```
sudo apt install zsh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
chsh -s `which zsh`
```

# Install atom

`sudo add-apt-repository ppa:webupd8team/atom && sudo apt-get update && sudo apt-get install atom`

# Setup ssh key

`ssh-keygen -t rsa -b 4096 -C "email"`

# Install Java 8

`sudo add-apt-repository ppa:webupd8team/java && sudo apt-get update && sudo apt-get install oracle-java8-installer`

# Install Eclipse -> get Java developer version from download packages

`http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/neon3`

# Install Docker

`https://download.docker.com/linux/ubuntu/dists/yakkety/pool/stable/amd64/`
`sudo dpkg -i [package name]`

# Install Latex

`sudo apt install texlive-full texstudio python-pygments`

# Install Pycharm

# Things to do with unity-tweak

Change font to `noto sans CJK TC`

# ZSH notes

zsh: corrupt history file /home/myusername/.zsh_history

```
mv .zsh_history .zsh_history_bad
strings .zsh_history_bad > .zsh_history
fc -R .zsh_history
```
