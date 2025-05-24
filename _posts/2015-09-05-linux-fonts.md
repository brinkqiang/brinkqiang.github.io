---
title: linux-fonts
description:
categories:
 - linux
tags:
---

# 1. 安装前置
```
yum -y install fontconfig
yum -y install ttmkfdir
```
# 2. 安装字体
```
git clone https://github.com/brinkqiang/dmfont
pushd dmfont
sh build.sh

ttmkfdir -d ~/.fonts -o ~/.fonts/fonts.scale

fc-cache
fc-list | grep fixedsys
```
# 3. 配置文件
```
vim /etc/fonts/fonts.conf
```