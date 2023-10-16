PySpark

# 1. 概述

> Apache Spark 是用于大规模数据处理的统一分析引擎
>
> 是一款分布式内存计算的统一分析引擎，可计算结构化、半结构化、非结构化类型
>
> 可以解决==海量数据的计算，可以进行离线批处理和实时流计算==

与hadoop的MapReduce的区别

| 区别         |         Hadoop(MapReduce)          |                            Spark                             |
| ------------ | :--------------------------------: | :----------------------------------------------------------: |
| 场景         | 海量数据批处理计算（磁盘迭代计算） | 海量数据的批处理（内存迭代计算、交互式计算）、海量数据流计算 |
| 价格         |         对机器要求低，便宜         |                    对内存要求高，相对昂贵                    |
| 编程范式     |     API相对底层，算法适应性差      |         RDD组成DAG有向无环图，API较为顶层，方便使用          |
| 数据存储结构 |     中间计算结果存储在HDFS磁盘     |               RDD中间计算结果在内存中，延迟小                |
| 运行方式     |     Task以进程方式维护，启动慢     |   Task 以线程方式维护，任务启动快，可批量创建提高并行能力    |

==特点==

> - 速度快
> - 易于使用
> - 通用性
> - 运行方式多

==框架模块==

> - Spark Core ：spark 的核心，运行的基础。Spark Core 以RDD为数据抽象，提供python、Java、Scala、R等语言的API
> - Spark SQL ：提供结构化数据处理的模块
> - SparkStreaming ：数据的流式计算
> - MLlib ：机器学习计算
> - GraphX ：图计算

==运行模式==

> - 本地模式（单机）   一般用于开发和测试
>
> - Standalone模式（集群)
>
> - Hadoop YARN模式（集群）
>
>   Kubernetes模式
>
>   云服务模式

*==理解==*

==Spark角色==

- 资源层面
  - Master 角色：集群资源管理
  - Worker 角色： 单机资源管理
- 任务运行层面
  - Driver ：单个任务的管理 （特殊情况（Local模式）可以既管理又计算）
  - Executor： 单个任务的计算

# 2. 环境搭建

## （Local）

本质：启动一个JVM Process进程，执行任务Task

Local模式可限制Spark集群环境的线程数量，Local[N]或Local[*]

Local 下的角色分布

- 资源管理

  - Master: Local进程本身

  - Worker: Local进程本身

- 任务执行

  - Driver: Local进程本身
  - Executor: 不存在，有Local（即Driver）的线程提供计算能力‘

## Spark StandAlone环境部署

## Spark StandAlone HA 环境搭建

## Spark On YARN 环境搭建

## 部署

确保:


- HADOOP_CONF_DIR
- YARN_CONF_DIR



在spark-env.sh 以及 环境变量配置文件中即可
​

## 连接到YARN中


### bin/pyspark


```shell
bin/pyspark --master yarn --deploy-mode client|cluster
# --deploy-mode 选项是指定部署模式, 默认是 客户端模式
# client就是客户端模式
# cluster就是集群模式
# --deploy-mode 仅可以用在YARN模式下
```


> 注意: 交互式环境 pyspark  和 spark-shell  无法运行 cluster模式



### bin/spark-shell


```shell
bin/spark-shell --master yarn --deploy-mode client|cluster
```


> 注意: 交互式环境 pyspark  和 spark-shell  无法运行 cluster模式



### bin/spark-submit (PI)


```shell
bin/spark-submit --master yarn --deploy-mode client|cluster /xxx/xxx/xxx.py 参数
```

## spark-submit 和 spark-shell 和 pyspark的相关参数

# 2. SparkCore

## 2.1 RDD 基本概念

RDD 

​	弹性分布式数据集，分布式计算的实现载体

RDD五大特性

- RDD有分区

  分区是RDD存储的最小单位

- RDD方法作用在所有分区上

- RDD之间有依赖关系

  如textFile -> rdd1 -> rdd2 -> rdd3 -> rdd4

- Key-Value型RDD可以有分区器

  默认分区器：默认是Hash分区规则

- RDD的分区规划，会尽量靠近数据所在的服务器

## 2.2 RDD 编程

### 2.2.1 程序入口

构建SparkContent对象

~~~python
from pyspark import SparkConf,SparkContext

conf = SparkConf().setAppName("WordCount").setMaster("local[*]")

sc = SparkContext(conf=conf)
~~~

### 2.2.2 RDD创建

1. 并行化创建 （本地集合） -> 分布式RDD

   ```python
   parallelize()    # 可指定分区数，默认分区根据CPU核心数获得
   ```

2. 读取文件创建

   ```python
   textFile()			 # 可读取本地，HDFS等文件
   wholeTextFiles()  	  # 读取大量小文件
   # 可设置分区数，但不一定按照设置分区
   ```

### 2.2.3 RDD算子

算子：分布式集合对象上的API

算子分类：

- Transformation：转换算子
- Action：动作算子	

转换算子

1. `map()`: 传入处理函数，对每条数据做处理

2. `flatMap()`: 先对rdd进行map()操作，再进行解除嵌套

3. `reduceByKey()`：针对KV型数据按key分组，根据提供的聚合逻辑，完成组内数据的聚合

4. `mapValues()`：针对二元元组，对其Value执行map操作

5. `groupBy()`：分组,分组完为完整数据

   ```python
   rdd = sc.parallelize([('a', 1), ('b', 2), ('a', 2), ('b', 3)])
   result = rdd.groupBy(lambda t: t[0])
   ```

6. `filter()`:过滤，保留想要的，函数中返回true保留，否则丢弃

   ```python
   rdd = sc.parallelize([1, 2, 3, 4, 5, 6, 7, 8])
   rdd_filter = rdd.filter(lambda x: x % 2 == 1)
   ```

7. `distinct()`: 对RDD数据去重

8. `union()`:两个RDD算子合并为一个返回

9. `join()`：对两个RDD执行join操作,按key进行连接  ，只能用于二元元组

   - `join()`
   - `leftOuterJoin()`
   - `rightOuterJoin()`

10. `intersection()`:两个RDD的交集

11. `glom()`：按分区加上嵌套

12. `groupByKey()`：按key分组，分组完取value

13. `sortBy()`:对RDD数据排序

    ```python
    rdd = sc.parallelize([('a', 1), ('b', 2), ('a', 2), ('b', 3)])
    result = rdd.sortBy(lambda x: x[1],ascending=True,numPartitions=1)
    # ascending表示升序还是降序
    # numPartitions表示按多少分区排序，1表示全局
    ```

14. `sortByKey()`: 按照key对RDD数据排序

    ```python
    rdd = sc.parallelize([('a', 1), ('b', 2), ('a', 2), ('B', 3)])
    result = rdd.sortByKey(ascending=True, numPartitions=1, keyfunc=lambda char: char.lower())
    ```

Action算子

