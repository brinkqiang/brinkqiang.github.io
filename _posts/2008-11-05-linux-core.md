---
title: linux-core
description:
categories:
 - linux
tags:
---

程序异常退出时，内核会生成一个core文件（是内存映像以及调试信息）。可以通过使用gdb来查看core文件，指示出导致程序出错的代码所在的文件和行数。

# 1、查看系统中core文件生成的开关是否打开
1）使用ulimit -c命令可查看core文件的生成开关，若结果为0，则便是关闭了此功能，不会生成core文件。

# 2、设置core文件生成
1）使用命令ulimit -c filesize命令
        若ulimit -c unlimited 则标识此core文件的大小不受限制
        若指定filesize，如果生成的信息超过此大小，将会被裁剪，最终生成一个不完整的core文件，在调 
        试此core文件时，gdb会提示错误。
2）但是若想整个系统中生效则在shell里面设置是不行的，方法如下：
    （1）编辑/root/.bash_profile文件，在其中加入ulitmit -S -c unlimited
    （2）source /root/.bash_profile