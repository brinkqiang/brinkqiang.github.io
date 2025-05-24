---
title: tortoisegit
description:
categories:
 - tortoisegit
tags:
---

# 1. tortoisegit 设置 ssh key

## 1. 命令行输入puttygen.exe
## 2. 点击Generate生成密钥
## 3. 保存private key: ssh_key.ppk 到某目录
## 4. 复制public key到 github ssh key中.
## 5. 配置tortoisegit git->remote->origin
## 6. 设置URL为仓库地址PUTTY KEY为 刚才生成的私钥ssh_key.ppk文件
## 7. 设置Network为TortolseGitPlink.exe
## 8. pull或push项目时 勾选Autoload Putty Key