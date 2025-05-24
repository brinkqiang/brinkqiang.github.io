---
title: linux-malloc
description:
categories:
 - linux-malloc
tags:
---

- 内存优化总结:ptmalloc、tcmalloc和jemalloc

# 1. linux内存池相关
```
内存管理可以分为三个层次，自底向上分别是：

操作系统内核的内存管理
glibc层使用系统调用维护的内存管理算法
应用程序从glibc动态分配内存后，根据应用程序本身的程序特性进行优化， 比如使用引用计数std::shared_ptr，apache的内存池方式等等。
当然应用程序也可以直接使用系统调用从内核分配内存，自己根据程序特性来维护内存，但是会大大增加开发成本。
```
```
一个优秀的通用内存分配器应具有以下特性:

额外的空间损耗尽量少
分配速度尽可能快
尽量避免内存碎片
缓存本地化友好
通用性，兼容性，可移植性，易调试
```
# 2. 动态加载tcmalloc
安装依赖
```
sudo yum -y install gperftools-devel gperftools-libs
```

直接挂接
```
LD_PRELOAD="/usr/lib64/libtcmalloc.so.4" ./dmserver
```

挂接顺便检测内存泄漏
```
LD_PRELOAD="/usr/lib64/libtcmalloc.so.4" HEAPCHECK=normal ./dmserver
```

cmake
```
SET_TARGET_PROPERTIES(project PROPERTIES LINK_FLAGS /INCLUDE:"__tcmalloc")
```

如果存在内存泄漏
```
Leak check _main_ detected leaks of 1136 bytes in 2 objects
The 2 largest leaks:
Leak of 568 bytes in 1 objects allocated from:
        @ 7fa7f4197dd2 
        @ 7fa7f4198c30 
        @ 7fa7fd11f780 
        @ 7fa7fd15aaf3 
        @ 7fa7fd15189a 
        @ 7fa7fd1519e8 
        @ 7fa7fd151ac5 
        @ 7fa7fd151d9f 
        @ 7fa7fd15afd3 
        @ 7fa7fd1520ed 
        @ 7fa7fd11ea42 
        @ 7fa7f14dfb35 
        @ 7fa7fd11f151 
        @ 0 
Leak of 568 bytes in 1 objects allocated from:
        @ 7fa7f4197dd2 
        @ 7fa7f4198c30 
        @ 7fa7fd12402f 
        @ 7fa7fd15aaf3 
        @ 7fa7fd15189a 
        @ 7fa7fd1519e8 
        @ 7fa7fd151ac5 
        @ 7fa7fd151d9f 
        @ 7fa7fd15afd3 
        @ 7fa7fd1520ed 
        @ 7fa7fd11ea42 
        @ 7fa7f14dfb35 
        @ 7fa7fd11f151 
        @ 0 


If the preceding stack traces are not enough to find the leaks, try running THIS shell command:

pprof ./bin/exe "/tmp/exe.7539._main_-end.heap" --inuse_objects --lines --heapcheck  --edgefraction=1e-10 --nodefraction=1e-10 --gv

If you are still puzzled about why the leaks are there, try rerunning this program with HEAP_CHECK_TEST_POINTER_ALIGNMENT=1 and/or with HEAP_CHECK_MAX_POINTER_OFFSET=-1
If the leak report occurs in a small fraction of runs, try running with TCMALLOC_MAX_FREE_QUEUE_SIZE of few hundred MB or with TCMALLOC_RECLAIM_MEMORY=false, it mi
Exiting with error code (instead of crashing) because of whole-program memory leaks
```

执行如下命令, 获得分析报告
```
pprof ./bin/exe "/tmp/exe.7539._main_-end.heap" --inuse_objects --lines --heapcheck  --edgefraction=1e-10 --nodefraction=1e-10 --text
```