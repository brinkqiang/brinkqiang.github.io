---
title: framework
description:
categories:
 - framework
tags:
---

- 前言
```
游戏领域框架
```
![Mobile Preview](/assets/images/yang/游戏领域服务平台.png)
![Mobile Preview](/assets/images/yang/游戏领域框架.png)



```
软件开发之小无相功实现流程
```

# 1. meta填充

## (1) meta::protobuf
抽取 protobuf idl可识别元素.
使用 ctemplate 填充到模板元中去

## (2) meta::cpp
抽取 C++ 头文件可识别元素.
使用 ctemplate 填充到模板元中去

# 2. 用dmpackage打包 需要生成代码的 模板文件列表 并提供访问 PACKAGE_API
# 3. 通过 PACKAGE_API 访问 模板文件列表 并 将 模板文件 填充到模板元中去
# 4. 通过ctemplate 引擎 翻译 meta 所有元素 并生成代码

# 结论：
当我们想增加 输出功能时， 丰富 meta源 即可。