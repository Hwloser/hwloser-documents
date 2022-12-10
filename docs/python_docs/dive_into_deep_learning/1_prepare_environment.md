# 环境准备

- 获得python环境

## 获得python环境

最快捷和无误的方式就是通过conda来准备一个无污染的环境。

```bash
# 1. 安装miniconda
# https://conda.io/en/latest/miniconda.html

# 2. 从上面的网址上面下载下来文件之后，进行安装
sh Miniconda3-py39_4.12.0-MacOSX-x86_64.sh -b

# 3. 初始化终端，以便可以直接使用
~/Miniconda3/bin/conda init

# 4. 创建新的环境
conda create --name d2l python=3.9 -y

# 5. 使用环境
conda activate d2l
```

## 准备学习框架和d2l软件包

我们使用pytorch进行学习，所以，仅安装pytorch的相关软件包。

```bash
# 1. 安装pytorch相关软件包
pip install torch==1.12.0
pip install torchvision==0.13.0

# 2. 安装d2l软件包
pip install d2l==0.17.6
```

## 文章中的源码

```bash
mkdir d2l-zh && cd d2l-zh
curl https://zh-v2.d2l.ai/d2l-zh-2.0.0.zip -o d2l-zh.zip
unzip d2l-zh.zip && rm d2l-zh.zip
cd pytorch
```

