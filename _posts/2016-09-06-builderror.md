---
title: builderror
description:
categories:
 - builderror
tags:
---

# 1. vs使用C++17编译时出错：error C2872: ‘byte‘: ambiguous symbol

 _HAS_STD_BYTE=0 来解决该问题
 本质原因是 using namespace std; 产生的名字污染.