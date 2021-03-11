---
title: 'NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver'
date: 2019-03-13
permalink: /posts/2019/03/nvidia-smi-failed/
tags:
  - skills
---


#　NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running

#＃ 问题描述：


某次使用电脑之后, 打开电脑之后输入nvidia-smi 提示

```
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running
```

##　问题原因
这常常是因为用户在程序运行时强行关闭电脑，或者电脑断电而引起的．

## 解决方案

大致就是重装一下驱动就可以

- １. 卸载原有驱动
```
sudo apt-get autoremove --purge nvidia*
```

- 2. 增加ppa源

```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```

- 3. 检测显卡版本及推荐的驱动

```
ubuntu-drivers devices
```

结果显示

```
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001B82sv00001458sd00003794bc03sc00i00
vendor   : NVIDIA Corporation
model    : GP104 [GeForce GTX 1070 Ti]
driver   : nvidia-driver-390 - distro non-free recommended
driver   : xserver-xorg-video-nouveau - distro free builtin

```
说明我这里应该去安装390的驱动

- 4. 重装驱动

```
sudo apt-get install nvidia-driver-390
```

- 5. 重启
这是最重要的一步,如果你不重启,那么重装之后立即nvidia-smi是会发现现在还是没有办法使用的,重启之后就好了

```
nvidia-smi
```

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 390.116                Driver Version: 390.116                   |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 107...  Off  | 00000000:01:00.0 Off |                  N/A |
|  0%   55C    P2    40W / 180W |   1046MiB /  8119MiB |     19%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1401      G   /usr/lib/xorg/Xorg                           249MiB |
|    0      1573      G   /usr/bin/gnome-shell                         195MiB |
|    0      2509      C   /home/mirror/anaconda3/bin/python            533MiB |
|    0      2562      G   ...quest-channel-token=9028700102029312797    64MiB |
+-----------------------------------------------------------------------------+
```
