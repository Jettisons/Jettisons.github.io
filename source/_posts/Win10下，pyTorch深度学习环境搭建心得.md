---
title: Win10下，pyTorch深度学习环境搭建心得
categories:
- tech
---

# Win10下，pyTorch深度学习环境搭建心得

搭建windows下python深度学习基础环境

目前共需要三个基础的需要安装的工具

1.Anaconda 2.pyTorch 3.Cuda+Cudnn

接下来介绍从零开始配置环境

## 1. Anaconda的安装

​        我的电脑上已经先安装过了python3.7，一开始我并不想安装anaconda，本意尽量从简，但是pip并没有想象中好用，并不能快速安装我需要的包，所以选择使用conda。对于conda，有两个选择，一个是Anaconda，一个是Miniconda。后者是前者的精简版，只有conda的包管理功能，没有主界面等，简单说就是让你可以在命令行用conda的指令，对我来说足够了，所以我选择安装miniconda。

> **下载地址**

官方网站：<https://docs.conda.io/en/latest/miniconda.html>

清华镜像：<https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/>

---

​        下载好之后进行安装，可以一路默认直到`Advanced Options`，这里建议不要勾选`Add Anaconda to my PATH environment variable`，勾选`Register Anaconda as my default Python`，因为Anaconda的python是在虚拟环境中使用的，如果勾选第一项会导致你在cmd中输入python会有警告，然后继续安装即可

安装好后在环境变量中添加三个路径，已经有的话就不用了

```mark
C:\Users\Administrator\Miniconda3； 
C:\Users\Administrator\Miniconda3\Scripts； 
C:\Users\Administrator\Miniconda3\Library\bin 
```

至此，conda算是安装好了，接下来就可以利用conda来安装其他你需要的包了

## 2. pyTorch的安装

​        首先到官方网站<https://pytorch.org/>，得到关于你的配置下的pyTorch安装指令，比如我的是`windows10+conda+python+Cuda10.1`，就得到了这样的指令`conda install pytorch torchvision cudatoolkit=10.1 -c pytorch`，注意讲后面的-c python去掉（我也不懂，去掉就是了），接着最保险的安装方法是打开Anaconda Prompt操作台，类似于cmd（已经安装了miniconda，所以会有这个程序）

* 对于国内用户推荐，首先给Anaconda添加清华镜像

```mark
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --set show_channel_urls yes
```

- 接下来执行之前得到的指令来安装pyTorch即可

---

​        安装完成后可以关闭，打开cmd，创建一个带有pytorch包的虚拟环境，指令如下`conda create -n your_envname python=3.7 pytorch`，创建完成后，使用`conda activate your_envname`来打开该虚拟环境，然后输入python，打开python，接着输入`import torch`来查看pyTorch是否安装成功并可以使用，至此，pytorch的安装告一段落。其他包的安装可以借鉴此过程

- 其实也可以先创建环境，再在该环境下安装pyTorch包，效果是一样的

## Cuda+Cudnn的安装

​        先下载Cuda，到官网下载<https://developer.nvidia.com/>，根据配置选择相应版本下载。安装直接一路默认即可

---

​        再下载Cudnn，<https://developer.nvidia.com/rdp/cudnn-download>，需要注册一个Nivida账号，选择相应版本下载 

1. 下载完成后得到一个压缩包，将其解压，找到文件夹'cuda'打开，你会发现三个文件夹'bin'、'include'、'lib'

2. 打开CUDA安装路径
   `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA `
   发现有同名的三个文件夹'bin'、'include'、'lib'

3. 将第一步中'bin'中的文件复制到第二步中'bin'文件夹下
   将第一步中'include'中的文件复制到第二步中'include'文件夹下
   将第一步中'lib'文件夹下'x64'文件夹中的文件复制到第二步中'lib'文件夹下'x64'文件夹中

4. 接着添加path环境变量：`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\lib\x64`

至此，安装完成，在cmd中输入nvcc -V查看是否安装完成
