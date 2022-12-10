# Juptyer Notebook安装教程

---

## 简介 - 什么是Juypter Botebook

!> Jypter Notebook是基于网页的用于交互计算的应用程序。其可悲应用于全过程计算：

- 开发
- 文档编写
- 运行代码和展示结果

## 如何安装

```bash
# 1. 首先如果你已经安装了anaconda，那么就可以继续下去了
# 如果没有安装，请先安装anaconda，再来这边进行访问，这边不做介绍和安装
conda install jupyter notebook

# alternative: 使用pip等工具进行安装，这里不做介绍

# 2. 安装jupyterlab，web接口
conda install -c cond-forge jupyterlab

# 2.1 关闭npm的SSL校验
conda config --set ssl_verify False
```

## 如何使用

```bash
# 1. 启动jupyter lab
jupyter lab
```

## 对比Shell有什么优势

未知。。
