---
title: TensorFlow
description:
categories:
 - machine-learning
tags:
---

# 1. TensorFlow安装
linux
```
yum install python36-devel
yum install python36-numpy
yum install swig
git clone https://github.com/brinkqiang/tensorflow
pushd tensorflow/tensorflow/contrib/cmake
mkdir build
cd build
cmake ..
popd
```

win
```
git clone https://github.com/brinkqiang/tensorflow
pushd tensorflow/tensorflow/contrib/cmake
mkdir build
cd build
cmake .. -A x64 -Dtensorflow_BUILD_PYTHON_BINDINGS=OFF
popd
```

## 1.1. https://www.vim.org
## 1.2. https://neovim.io
## 1.3. https://spacevim.org
