---
layout:     post
title:      "RuntimeError: CUDA environment is not correctly set up"
subtitle:   "Chainer with CUDA 指南"
date:       2016-04-30
author:     "卢周亨"
header-img: "img/post-bg-media-version.jpg"
tags:
    - Deep Learning
    - CUDA
---

# RuntimeError: CUDA environment is not correctly set up

## Mac 环境下使用 Chainer + GPU

OS X Yosemite + GPU 运行时报错如下。

```
>>> from chainer import cuda
>>> cuda.init()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/chainer/cuda.py", line 69, in init
    check_cuda_available()
  File "/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/chainer/cuda.py", line 82, in check_cuda_available
    raise RuntimeError(msg)
RuntimeError: CUDA environment is not correctly set up
(see https://github.com/pfnet/chainer#installation).dlopen(/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/cupy/core/core.so, 2): Library not loaded: @rpath/libcublas.7.5.dylib
  Referenced from: /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/cupy/core/core.so
  Reason: image not found
```

## 环境

* Os X 10.10.5 (Yosemite)
* NVIDIA GeForce GT 750M
* Python 2.7.11
* Chainer 1.8.1
* CUDA 7.5
* cuDNN v3

## 准备

在 Mac OS X 上进行示范，操作系统是 Yosemite 10.10.5， Ubuntu 系统的操作过程也类似的。

首先是 Chainer 的安装。

```
$ pip install chainer
```

如果不使用 GPU 的话，这样就可以了。

如果需要使用 GPU 进行运算的话，必须安装 CUDA。

Mac 正式版安装在**[这里](https://developer.nvidia.com/cuda-downloads)**可以下载。

CUDA 的安装这里就不赘述了。

## CUDA 解决方案

* 在 ~/.bash_profile 里写入

```
#for Cuda
export CUDA_ROOT=/Developer/NVIDIA/CUDA-7.5/
export PATH=$PATH:/Developer/NVIDIA/CUDA-7.5/bin
export LD_LIBRARY_PATH=/Developer/NVIDIA/CUDA-7.5/lib:/usr/local/cuda/lib/:$LD_LIBRARY_PATH
export CPATH=$CPATH:/Developer/NVIDIA/CUDA-7.5/include
export CUDA_INC_DIR=/Developer/NVIDIA/CUDA-7.5/bin:$CUDA_INC_DIR
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/Developer/NVIDIA/CUDA-7.5/lib
```

* 其中这条路径

```
export LD_LIBRARY_PATH=/Developer/NVIDIA/CUDA-7.5/lib:/usr/local/cuda/lib/:$LD_LIBRARY_PATH
```

在这条 Issue: **[*Chainer Connection to GPU Issue on Mac #652*](https://github.com/pfnet/chainer/issues/652)** 可以找到出处。

* 安装依赖环境

```
$ pip install chainer-cuda-deps
```

## 安装及使用 cuDNN

Chainer 现在还**不支持**最新的 cuDNN **v5** 版本。

这里以 cuDNN **v3** 示范。

1. 如果没安装的话，安装 Mac Os Command Line Tools。

	```
	$ xcode-select --install
	```

2. 检查一下 CUDA 的版本和路径 (如果还没安装 CUDA，请参照上文安装)。

	```
	$ /usr/local/cuda/bin/nvcc --version
	# Cuda compilation tools, release 7.0, 	V7.0.27
	$ export DYLD_LIBRARY_PATH=/usr/local/cuda/lib
	```

3. 下载安装 cuDNN (如果安装了就忽略这一步)

	```
	$ wget http://developer.download.nvidia.com/assets/cuda/secure/cuDNN/v3/cudnn-7.0-osx-x64-v3.0-prod.tgz?autho=1461944459_fdf5f49c8d930ce22d43776c7ecc6aa1&file=cudnn-7.0-osx-x64-v3.0-prod.tgz
	$ tar xvzf cudnn-3.0-osx-x64-v3.0-prod.tgz
	$ rm cudnn-3.0-osx-x64-v3.0-prod.tgz
	$ cd cuda
	$ sudo cp cudnn.h /Developer/NVIDIA/CUDA-7.5/include/
	$ sudo cp libcudnn* /usr/local/cuda/lib/
	```


## 重装 Chainer

如果你现在检车你的 Chainer with CUDA 可能会不可用，并且提醒你需要重新安装 Chainer。

以下是安装与卸载的所有需要用到的命令行

```
$ pip uninstall chainer
$ pip uninstall chainer-cuda-deps
$ pip install --user --upgrade chainer
$ pip install --user --upgrade chainer-cuda-deps
```

* 首先我们需要卸载所有 Chainer 及其依赖的环境

```
$ pip uninstall chainer
$ pip uninstall chainer-cuda-dpes
```

* 然后重新安装 

```
$ pip install --user -upgrade chainer
$ pip install --user -upgrade chainer-cuda-deps
```

## 完成～

现在你可以检查是否可用了

```
$ python

>>> from chainer import cuda
>>> cuda.init()
```
