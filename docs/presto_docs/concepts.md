# presto核心概念

## presto组件

工欲善其事，必先利其器。了解了presto的核心概念之后，对presto的使用才可能会得心应手。

通过介绍以下的概念来对preto的整体结构进行描述：

- Design Model
- Server Types
  - Coordinator
  - Worker
- Data Source
  - Connector
  - Catalog
  - Schema
  - Table
- Query Execution Model
  - Statement
  - Query
  - Stage
  - Task
  - Split
  - Driver
  - Opertor
  - Exchange

---

## Design Model

presto设计的核心思想模型为：

1. Operator Model

SQL文本对象通过词法、语法解析和语义分析后形成AST（Abstract Syntax Tree），后经过访问者模式的遍历之后生成查询计划（query plan），后续会被解析和优化从而落地为物理计划。

该查询计划（query plan）又因由操作符所组成，又可称之为操作符树。树的根结点负责汇总数据并输出，叶子结点一般都是TableScan，用于查询connector中的split数据。

2. Iterator Model

迭代模型，起因为每一个Operator都包含三种状态：Open、GetNext以及Close。从根结点开始递归调用GetNext获取数据。

## Server types

presto中包含了两种server的类型：

1. coordinator
2. worker

我们通过配置文件中的配置`coordinator=true|false`，来指定是否启动的`presto-server`是哪一种角色类型。

---

### Coordinator

Coordinator负责于以下几种场景：

1. 解析`statements`
2. 生成`planning`
3. 生成`query`
4. 管理worker节点
5. 作为集群的大脑，并对外`jdbc | client`提供连接

presto必须有一个或多个的worker作伴，不能独活。**为了开发和测试，可以将coordinator和worker设置在同一个机器上`node-scheduler.include-coordinator=true`**

### Worker

Worker所负责的职责：

1. 执行task
2. 处理数据
3. 从`connector`中获取数据
4. 在worker之间通过`exchange`进行中间数据的交换
5. 向coordinator提供结果数据。

当worker启动的时候，worker会向coordinator中的discovery服务进行注册，以便于后续的coordinator需要执行task的时候可以对注册的worker分配任务。

## Data Source

### Connector

presto通过connector来抽象底层的数据源。例如，presto抽象初hive的connector，用以隔离用户对hive的操作，只需要通过指定catalog层即可。

对于每一个指定的connector的配置文件，有一个强制的参数`connector.name`用于catalog manager去管理connector。

### Catalog

Catalog包含了schema和数据源的引用。

### Schema

Schema是presto用于组织表的一种方式。当用户通过查询关系型数据库的时候，presto会把schema翻译成目标数据库中相似的概念。

### Table

Table是一组无序数据的集合。

## Query execution model

presto执行sql的statement并将statement转换为query进行查询。query将会在整个分布式集群中进行执行。

### Statement

Trino执行ANSI兼容的SQL语句。语句的含义：

1. 标准的ANSI SQL statement
2. 由子句（clause）、表达式（expression）和谓词（predicate）组成

这里会对statement和query进行语义上的拆分，目的：statement在引擎中指的是sql文本，当statement开始执行的时候，presto会对执行计划创建query，让查询在引擎中得以执行。

### Query

当引擎解析statement的时候，会将statement转换为qeury并生成查询计划，查询计划将会在后续的阶段中被划分为一系列相关联的stage并在集群中运行。

statement和query之间的差异：statement在这里指的是SQL文本，query指的是执行statment的配置和组件。query中包含了stage、task、split、connector以及其他的组件，以及数据源。

### Stage

当presto执行query的时候，引擎会将query拆分为一个多层的stage模型。stage的层级结构类似于一颗树。每一个query都包含一个root stage用聚合stage生成的结构。

stage仅被presto用以模型化分布式查询计划，而不是真的运行在presto集群当中的。

### Task

Stage实则是一种对于task的概念汇总层，并不是真实存在的。stage是由一系列的task组成的，这些task通过网络被分散在workers集群当中。

task是presto中的主力（`work horse`）。执行计划，先被拆分为stage，再被翻译成task，最终的task将会继续或者处理split。

task的作用：

1. 包含输入输出
2. 处理split
3. 通过driver来达到并行执行的目的

### Split

Split是分布式数据集中的一个切块（一部分）。Task对应于split，拆分的task将会对split进行计算。底层的stage通过connector将检索的数据进行切块，上层的stage将会从其他的stage中检索数据。

当presto调度query的时候，coordinator会向connector查询表中全部可用的split列表。coordinator在后续中也会继续跟踪运行中的task以及task正在处理哪些split。

### Driver

Task中包含了一个或多个并行的driver。driver会对数据进行后续的处理，或结合operator操作符产出结果用以后续的聚合task，随后将结果交付到另外的stage中的task。

driver由一组operator实例组成，或者可以把driver当作一组内存中的操作符的物理集合。driver在presto中是最低级别的并行度粒度，一个driver有一个输入和输入。

### Operator

操作符用于

1. 消费
2. 转换
3. 生成

数据。

例如：

1. Table Scan操作符用于从connector中进行数据的获取，以及生成的数据可被后续的操作符使用。
2. Filter操作符消费数据并根据包含的谓词（predicate）对数据处理后提供给后续的操作符使用。

### Exchange

Exchange为query中不同的stage进行传递数据，在presto节点之间。task将数据生成到输出缓冲池中，由后续的task通过exchange客户端进行消费数据。

## Reference

- [Trino concepts](https://trino.io/docs/current/overview/concepts.html)
