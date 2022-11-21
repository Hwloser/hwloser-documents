# CGroup

--- 

## CGroup是什么

CGroup（control groups）是linux内核提供的一种特性，该特性可以对用户所使用的资源（CPU，内存，磁盘IO以及网络等）进行限制和隔离。

---
## CGroup有什么用

#### 资源限制（Resource limiting）

CGroup可以对
- 内存
- [文件系统缓存](https://en.wikipedia.org/wiki/Page_cache)
- IO带宽
- CPU QUOTA
- CPU SET

#### 优先级限制（Prioritization）

一些资源组可以获得更大份额的CPU利用率以及disk IO吞吐量

#### 记账（Accounting）

度量group的资源利用率,以达到记账(billing)的目的

#### 控制（Contril）

冻结进程组，让进程可以进行`checkpointing`以及重启

--- 

## 该如何使用CGroup

### CGroup的原理是什么

一个控制组(control group, 简写为cgroup)，通常是受到相关联规则的限定或者限制的进程组。 这些控制组表现为层级结构，通过建立层级结构每一个group就可以继承于它的`parent group`。

Linux的内核通过cgroup的接口，对外提供了访问控制器（或者称子系统(subsystem))的能力。例如：
"memory"控制器限制内存的使用，"cpuacct"记录了CPU的利用率，等等。

### 该如何使用CGroup

- 通过手动的访问cgroup虚拟文件系统
- 通过执行相关的命令来对cgroup进行操纵，例如:`cgcreate`, `cgexec`, `cgclassfy`（来源于`libcgroup`）
- 规则引擎可以自动的执行将某些用户，进程或者是命令移动到配置文件指定的cgroup当中
- 通过其他的软件间接的使用cgroups，例如：
  1. Docker
  2. Firejail
  3. LXC
  4. .......

---
### 重新设计CGroup

Cgroup在2013年的时候在Linux内核被重新设计。

#### 命名空间隔离（Namespace isolation）
!> 相关文章：[Linux namespace](https://en.wikipedia.org/wiki/Linux_namespaces)

严格上来讲，命名空间并不是cgroup工作的一部分，但linux内核中的一个相关的特性名称叫做*namespace isolation*，同一个进程组中的进程被划分到互相无法看到对方资源组的资源。例如：PID命名空间在每一个命名空间中提供了单独的进程标识枚举。还可以使用`mount`, `user`, `UTS`, `network`以及`SusV IPC`命名空间。

待完成
- [ ] 重新设计的是什么
- [ ] 请举个🌰
