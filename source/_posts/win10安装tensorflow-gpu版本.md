---
title: win10安装tensorflow-gpu版本
date: 2017-02-05 19:37:44
categories: 深度学习
tags: [tensorflow,win10]
copyright: true
---

## 安装前准备

TensorFlow 有两个版本：**CPU** 版本和 **GPU** 版本。GPU 版本需要 **CUDA** 和 **cuDNN** 的支持，CPU 版本不需要。如果你要安装 GPU 版本，**请先确认你的显卡支持 CUDA**。我安装的是 GPU 版本，采用 **pip 安装方式**，所以就以 GPU 安装为例，CPU 版本只不过不需要安装 CUDA 和 cuDNN。

- 在 [这里](https://developer.nvidia.com/cuda-gpus) 确认你的显卡支持 CUDA。
- 确保你的Python版本是3.5 64位。
- 确保你有稳定的网络连接。
- 确保你的pip版本 >= 8.1。用 `pip -V` 查看当前 `pip` 版本，用 `python -m pip install -U pip` 升级`pip` 。
- 确保你安装了 VS2015 或者 2013 或者 2010。

此外，建议安装Anaconda，因为这个集成了很多科学计算所必需的库，能够避免很多依赖问题，安装教程可以参考官网。

以上条件符合，那么恭喜你可以开始下载 CUDA 和 cuDNN 的安装包了，注意版本号分别是 **CUDA8** 和 **cuDNN5**，这是 Google 官方推荐的。(记得fq去英伟达官网下)



## 安装TensorFlow

首先激活Anaconda

```powershell
C:\WINDOWS\system32> activate tensorflow

(tensorflow) C:\WINDOWS\system32> pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-0.12.0-cp35-cp35m-win_amd64.whl
```

然后自动就安装上了Tensorflow的gpu版本（记得全局fq）

## TensorFlow测试

```powershell
C:\WINDOWS\system32> python
>>> import tensorflow as tf
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:128] successfully opened CUDA library cublas64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:128] successfully opened CUDA library cudnn64_5.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:128] successfully opened CUDA library cufft64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:128] successfully opened CUDA library nvcuda.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:128] successfully opened CUDA library curand64_80.dll locally
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:885] Found device 0 with properties:
name: GeForce GTX 970
major: 5 minor: 2 memoryClockRate (GHz) 1.253
pciBusID 0000:02:00.0
Total memory: 4.00GiB
Free memory: 3.31GiB
```

测试类似上面结果表示测试通过



然后所有安装就结束了，就可以做一些有意思的事情了



2017 /2/19 update

tensorflow-gpu版本已经更新到1.0

使用如下命令更新1.0版本

```powershell
(tensorflow) C:\WINDOWS\system32>pip install tensorflow-gpu==1.0
```

