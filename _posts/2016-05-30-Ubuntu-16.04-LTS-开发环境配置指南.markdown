---
layout:     post
title:      "Ubuntu 16.04 LTS 开发环境配置指南"
subtitle:   "A Guide for Set up Dev on Ubuntu 16.04 LTS"
date:       2016-05-30
author:     "卢周亨, LuZhouheng"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - Ubuntu
---

# Ubuntu 16.04 LTS 开发环境配置指南

文章不完善，将持续更新。

## 更新 Ubuntu 16.04 系统

![Ubuntu Desktop](http://7xrims.com1.z0.glb.clouddn.com/UbuntuUpdate.jpg)

打开 Terminal 并运行一下命令

```ruby
sudo apt update
sudo apt diet-upgrade
```

## Unity 移到底部

更改 Unity 在 Ubuntu 中的位置。可以使用命令行和 Dconf 编辑器两种方式。

### 使用命令行

打开 Terminal 并输入以下命令:

* Unity 移到底部

```ruby
gsettings set com.canonical.Unity.Launcher launcher-position Bottom
```

* Unity 移到左边

```ruby
gsettings set com.canonical.Unity.Launcher launcher-position Left
```
### 使用 Dconf 编辑器

![Dconf Editor](http://7xrims.com1.z0.glb.clouddn.com/DconfUnity.jpg)

安装 Dconf 编辑器

```ruby
sudo apt-get install dconf-editor
```

* 安装后，打开 Dconf 编辑器。
* 导航到 com > canonical > unity > launcher (左导航栏)。
* 在右侧选项窗口里更改 Launcher 的值，'left' 和 'bottom'。
* 退出 Dconf 编辑器

> 注意: 所有改动不需要重启电脑即可看到更改结果。


## 提高电池寿命 & 减少笔记本电脑过热

笔记本的话安装帮助降低功耗的 TLP: 

```ruby
sudo apt-get install tlp tlp-rdw 
sudo apt-get install tp-smapi-dkms acpi-call-dkms 
sudo apt-get install thermald
sudo apt-get install powertop
```
## 配置环境

### 添加一些常见的 PPA

```ruby
# 常见工具集合 PPA
sudo add-apt-repository ppa:diesch/testing -y
sudo add-apt-repository ppa:atareao/atareao -y
sudo add-apt-repository ppa:nilarimogard/webupd8 -y
# VLC
sudo add-apt-repository ppa:videolan/master-daily -y
# 音频切换 Indicator 的 PPA
sudo add-apt-repository ppa:yktooo/ppa -y
# Numix 图标集合 PPA
sudo apt-add-repository ppa:numix/ppa -y
# JDK 字体渲染补丁 PPA
sudo add-apt-repository ppa:no1wantdthisname/openjdk-fontfix -y
# DNSCrypt PPA
sudo add-apt-repository ppa:anton+/dnscrypt -y
# noobslab 图标 PPA
sudo add-apt-repository ppa:noobslab/icons -y
# 录屏软件 PPA
sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder  -y
# Telegram PPA
sudo add-apt-repository ppa:atareao/telegram -y
# Uget PPA
sudo add-apt-repository ppa:plushuang-tw/uget-stable -y
```

### 添加完毕后更新并安装常用的软件

```ruby
sudo apt update
sudo apt dist-upgrade
sudo apt-get install fcitx fcitx-sunpinyin fcitx-module-cloudpinyin gtk2-engines-murrine:i386 libudev1:i386 vlc i965-va-driver vainfo gimp inkscape openshot shutter filezilla audacity classicmenu-indicator numix-gtk-theme shimmer-themes numix-icon* caffeine leafpad git indicator-sound-switcher  unity-tweak-tool fcitx-mozc ibus-qt4 curl ctags vim-doc vim-scripts cscope fonts-dejavu indent vim vim-gnome exuberant-ctags indicator-keylock pidgin  psensor syspeek blueman libluajit-5.1-2 python3-pip dnscrypt-proxy unsettings ubuntu-make ppa-purge jayatana simplescreenrecorder compizconfig-settings-manager ultra-flat-icons-* ultra-flat-icons zsh uget telegram -y
```

### Python

```ruby
sudo pip3 install pep8
sudo pip3 install jedi
```

### 基本支持

```ruby
sudo apt-get install ttf-bitstream-vera
sudo apt-get install exfat-fuse exfat-utils
```

### 美化

```ruby
sudo apt-get remove arc-theme* -y
sudo rm -rf /usr/share/themes/{Arc,Arc-Darker,Arc-Dark}
rm -rf ~/.local/share/themes/{Arc,Arc-Darker,Arc-Dark}
rm -rf ~/.themes/{Arc,Arc-Darker,Arc-Dark}
sudo apt-get install autoconf automake pkg-config libgtk-3-dev git -y
git clone https://github.com/horst3180/arc-theme --depth 1
cd arc-theme
./autogen.sh --prefix=/usr --disable-transparency
sudo make install
cd ..\
rm -rf arc-theme
```

### 安装罗技优连管理程序

```ruby
wget https://launchpad.net/~daniel.pavel/+archive/ubuntu/solaar/+files/solaar_0.9.2-1ppa1_all.deb
sudo dpkg -i solaar_0.9.2-1ppa1_all.deb
```

### 消除 Ubuntu LightDM 登陆界面的白点

```ruby
gsettings set com.canonical.unity-greeter draw-grid false
sudo xhost +SI:localuser:lightdm
sudo su lightdm -s /bin/bash
gsettings set com.canonical.unity-greeter draw-grid false
quit
```

### 使用 terminator 替换默认终端

```ruby
sudo apt-get install terminator
sudo update-alternatives --config x-terminal-emulator
gsettings set org.gnome.desktop.default-applications.terminal exec 'terminator'
```

### 配置 Oh-My-ZSH

```ruby
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
wget --no-check-certificate https://raw.githubusercontent.com/seebi/dircolors-solarized/master/dircolors.ansi-dark
mv dircolors.ansi-dark .dircolors
eval `dircolors ~/.dircolors`
git clone https://github.com/sigurdga/gnome-terminal-colors-solarized.git
cd gnome-terminal-colors-solarized
./set_dark.sh
```



