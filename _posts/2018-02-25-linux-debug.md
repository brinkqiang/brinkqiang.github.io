---
title: linux-debug
description:
categories:
 - linux-debug
tags:
---

# 1. gdb调试

gdb zsh $(pidof zsh)

# 2. 打印调用堆栈

gstack $(pidof zsh)

# 3. 查看进程路径

pidof zsh | xargs pwdx
