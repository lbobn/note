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

1. `countByKey()` ： 统计key出现的次数（使用KV型RDD）

2.  `collect()` :  将RDD收集到Driver中形成一个集合

3. `reduce()`: 聚合，接收传入逻辑

4. `fold()` : 聚合（带初始值），接收传入逻辑，对于分区内和分区间都生效

5.  `first()` ： 取出RDD的第一个元素

6.  `take()` : 取出RDD的前n的元素

7.  `top()` : 降序排序，取前n个元素

8.  `count()` : 返回RDD中数据个数

9.  `takeSample()` : 随机抽取n的数据

10.  `takeOrdered()` : 排序并取出n个数据

    ```python
    rdd = sc.parallelize([1, 3, 2, 5, 1, 8, 6, 4])
    rdd.takeOrdered(5)				    # 正序
    rdd.takeOrdered(5, lambda x: -x)	 # 倒序
    ```

11. ==`foreach()`== : 对RDD的每个元素执行提供的逻辑，与map类似

12. ==`saveAsTextFile()`== : 将RDD的数据写入文本中

    ==高亮算子在executor中完成，不经过Driver==
    
    **分区操作算子**
    
13. `mapPartitions()` : 与map类似，对每个分区做处理

14. `foreachParttition()` : 与foreach类似，对每个分区做处理

15. `partitionBy()` : 自定义分区，传入指定函数

16. `repartition()` : 按数重新分区

17. `coalesce()` : 同16，增加了shuffle判断（默认False），保证增加分区时的安全

## 2.3 RDD的持久化

### 2.3.1 缓存技术

分散存储

- `rdd.cache()` ： 缓存

- `rdd.persist()`:  缓存，设置缓存方式

- `rdd.unpersist()` : 释放缓存

  ```python
  from pyspark.storagelevel import StorageLevel
  
  ...
  rdd.persist(StorageLevel.MEMORY_AND_DISK);
  # 常用方式
  """
      DISK_ONLY
      DISK_ONLY_2
      DISK_ONLY_3
      MEMORY_ONLY
      MEMORY_ONLY_2
      MEMORY_AND_DISK
      MEMORY_AND_DISK_2
      OFF_HEAP
      MEMORY_AND_DISK_DESER
  """
  ...
  rdd.unpersist()
  ```

### 2.3.2 CheckPoint技术

集中存储到HDFS,不保留血缘关系

```python
# 告知Spark开启CheckPoint功能
sc.setCheckPointDir("hdfs://node1:8020/output/ckp")
...
# 调用
rdd.checkpoint()
```

## 2.4 共享变量

### 2.4.1 广播变量

有些场景下，RDD需要本地的一些数据，而这些数据体量小，转为RDD性能反而下降，故存在Driver中，需要时通过网络IO传给RDD使用

本地数据一般在driver中，需要向executor发送，用于本地小规模数据与RDD的连接，广播变量可以减少网络io次数

广播变量只会向executor发送一次数据，executor进程内部Task共享这一份数据

```python
# 设置为广播变量
broadcast = sc.broadcast(data)
broadcast.value  	# 取出广播变量的值
```

### 2.4.2 累加器

在分布式场景下完成累加

```python
count = sc.accumulator(0)    # 获取累加器变量，设初值0
def func(data):
    global count
    count += 1
    print(count)

rdd2 = rdd.map(func).collect()
```

注意：

> 上面的rdd2如果在后面再次被调用 ，会导致溯源，并继续执行map操作，故累加器的值会变为2倍，
>
> 可以在collect()之前持久化
>
> 也就是说累加器相关的RDD如果在action后还要使用，必须持久化

## 2.5 核心

### 2.5.1 DAG (有向无环图)

一个Application分多个Job，每个Job有自己的DAG

由Action产生DAG，会在程序运行中生成Job

即 1Action =  1 DAG = 1 JOB

### 2.5.2 DAG的窄依赖和宽依赖

窄依赖：父RDD的一个分区，将数据全部给一个子RDD

![窄依赖](D:\笔记\笔记\PySpark.assets\窄依赖.png)

宽依赖：父RDD的一个分区，将数据给多个子RDD，即 shuffle 机制

![宽依赖](D:\笔记\笔记\PySpark.assets\宽依赖.png)

阶段划分

通过宽依赖划分阶段，即Shuffle阶段

因此，在Stage内部一定为窄依赖

一个阶段stage的每个Task分别为一个内存计算管道，可以保证并行计算

### 2.5.3 spark并行度

设置并行Task的数量

设置并行度(优先级从高到低)

> 代码
>
> ```python
> conf = SparkConf()
> conf.set("spark.default.parallelism","100")
> ```
>
> 提交参数
>
> ```shell
> bin/spark-submit --conf "spark.default.parallelism=100"
> ```
>
> 配置文件
>
> ```shell
> # 在conf/spark-defaults.conf中
> spark.default.parallelism 100
> ```
>
> 默认：为1
>
> ==尽量不对RDD改分区，可能影响迭代管道构建和额外Shuffle==
>
> 针对RDD的并行度设置（不推荐）

规划并行度

设置为CPU总核心的2~10倍

2倍是为了让执行完的TASK不会让CPU闲置，最大化利用资源



### 2.5.4 Spark 任务调度

DAG调度器

处理逻辑DAG图，得到逻辑上的Task划分

Task调度器

由DAG scheduler的结果，规划逻辑Task应该在哪些物理的executor上运行，并监控管理

### 名词

- application	提交的程序
- driver program   构建SparkContext
- cluster manager  集群管理
- deploy mode   部署模式
- worker node       
- executor       内部由多个Task
- task              最小工作单元
- job                并行化计算集合
- stage            阶段，内部为窄依赖

# 3. SparkSQL

## 3.1 SparkSQL快速入门

SparkSQL是Spark的一个模块，用于处理海量==结构化数据==

SparkSQL与Hive类似，都是用于大规模SQL分布式计算的计算框架，均可运行在YARN上

SparkSQL的数据抽象为：SchemaRDD（废弃）、DataFrame（Python、R、Java、Scala)、DataSet (Java、Scala）。

DataFrame同样是分布式数据集，有分区可以并行计算，和RDD不同的是，DataFrame中存储的数据结构是以表格形式组织的，方便进行SQL计算

DataFrame对比DataSet基本相同，不同的是DataSet支持泛型特性，可以让Java、Scala语言更好的利用到。

SparkSession是2.0后退出的新执行环境入口对象，可以用于RDD、SQL等编程

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("test").master("local[*]").getOrCreate()
sc = spark.sparkContext
```

## 3.2 DataFrame

### 3.2.1 构建DataFrame

- rdd直接转换

  ```python
  df = spark.createDataFrame(rdd1, schema=["name", "age"])
  ```
  
- rdd 通过StructType定义

  ```python
  from pyspark.sql.types import StructType, StringType, IntegerType
  schema = StructType().add("name", StringType(), True).add("age", IntegerType(), True)
  df = spark.createDataFrame(rdd1, schema=schema)
  ```

- rdd toDF()

  ```python
  df = rdd1.toDF(schema=schema)		# 通过StructType构建
  df = rdd1.toDF(["name","age"])		# 直接构建
  ```

- 基于pandas的DataFrame

- 统一API构建

  ```python
  sparksession.read.format("text|csv|json|parquet|orc|avro|jdbc|...")
  	.option("K","V")   # 可选
      .schema(StructType | String) # 若选择String ，语法如 "name STRING","age INT"
      .load("文件路径，支持本地文件系统和HDFS")
  ```

### 3.2.2 DataFrame 操作入门

两种编程风格

- DSL    如df.where().limit()

- SQL    如spark.sql(“select * from x”)

  ```python
  # 注册视图
  df.createTempView("score")		# 注册一个临时视图（表）
  df.createOrReplaceTempView("score")		# 注册一个临时表，或替换
  df.createGlobalTempView("score")		# 注册一个全局表
  # 全局表 ： 跨SparkSession对象，单程序内多个SparkSession均可调用，查询需带前缀: global_temp.
  
  spark.sql("select * from score").show()
  ```

  

### 数据清洗API

- 数据去重 dropDuplicates

  ```python
  df.dropDuplicates().show()
  df.dropDuplicates(['age', 'job']).show()  # 对age和Job去重
  ```

- 数据清洗

  ```python
  # 缺失值删除
  df.dropna().show()
  df.dropna(thresh=3).show()      # 至少3个有效列
  df.dropna(thresh=2,subset=["name","age"]).show()      # 对于name，age列至少2个有效列
  # 缺失值填充
  df.fillna("N/A").show()
  df.fillna("N/A", subset=["job"]).show()
  df.fillna({"name": "未知姓名", "age": 1, "job": "worker"}).show()
  ```

  

### DataFrame数据写出

基于统一API 

```python
df.write.mode().format().option(K,V).save()
# mode，传入模式字符串可选：append 追加，overwrite 覆盖，ignore 忽略，error 重复就报异常（默认的）
# format，传入格式字符串，可选：text,csv, json,parquet, orc,avro, jdbc
# 注意text源只支持单列df写出
# option 设置属性，如：.option("sep"，",")
# save写出的路径，支持本地文件和HDFS
```



## 3.3 自定义函数

Hive中支持UDF，UDAF，UDTF定义

SparkSQL 中

- UDF（python,Java,Scala） 一对一
- UDAF (Java,Scala)     多对一
- UDTF         一对多

python中只支持UDF定义

### 3.3.1 定义UDF函数

- ```python
  sparksession.udf.register() # 注册的UDF适用于DSL和SQL，返回值用于DSL
  ```

- ```python
  pyspark.sql.functions.udf	# 仅适用于DSL
  ```

  ```python
  def num_ride_10(num):
      return num * 10
  
  udf1 = spark.udf.register("udf1", num_ride_10, IntegerType()) # 参数1用于SQL，返回值用于DSL
  ```

### 3.3.2 窗口函数
```python
# 聚合窗口函数
spark.sql("""
    select 
        *,
        avg(score) over() as avg_score 
    from stu
 """).show()
# 排序窗口
spark.sql("""
    select 
        *,
        row_number() over (order by score desc) as no_of_all,
        dense_rank() over (partition by class order by score desc ) as no_of_class,
        rank() over (order by score) as rank        
    from stu
    """).show()
```

## 3.4 SparkSQL执行流程

Catalyst 优化器

- 谓词下推/断言下推 ： 将逻辑判断提到前面，减少Shuffle阶段数据量
- 列值裁剪 ： 将加载但不需要的列裁剪，减少被处理数据宽度，（parquet文件最合适）

执行流程

![SparkSQL执行流程](D:\笔记\笔记\PySpark.assets\SparkSQL执行流程.png)

> 1. 提交SparkSQL代码
> 2. catalyst优化
> 	a. 生成原始AST语法树
> 	b. 标记AST元数据
> 	c. 进行**断言下推**和**列值裁剪**以及其它方面的优化作用在AST上
> 	d. 将最终AST得到，生成执行计划
> 	e. 将执行计划翻译为RDD代码
> 3. Driver执行环境入口构建(SparkSession)
> 4. DAG 调度器规划逻辑任务
> 5. TASK 调度区分配逻辑任务到具体Executor上工作并监控管理任务
> 6. Worker干活

## 3.5 Spark On Hive

Spark通过连接Hive的 metastore 服务作为SparkSQL的元数据管理服务

## 3.6 分布式SQL执行引擎

Spark ThriftServer 类似 Hive中的 HiveServer2 ：均为守护进程

可用于只写SQL场景

# 4. Spark 新特性及核心回顾

## 4.1 Spark Shuffle

map和reduce

在Shuffle过程中，提供数据的为Map端（Shuffle Write），接收数据端称为Reduce端（Shuffle Read）

在两个阶段中，总是前一个阶段产生一批Map 提供数据，下一阶段产生一批Reduce接收数据

Spark中的Shuffle管理器

- HashShuffleManager

- SortShuffleManager  (减少了磁盘文件)

  - bypass 运行机制 （Shuffle map task数量小于200 & 非聚合类shuffle)

    取消排序，节省性能开销

## 4.2 Spark3.0新特性

- Adaptive Query Execution 自适应查询 (SparkSQL)

  运行时对查询执行计划进行优化

  - 动态合并 Shuffle Partitions

    动态调整Shuffle分区数量，AQE运行时将相邻小分区合并为较大分区

  - 动态调整Join策略

  - 动态优化倾斜Join（Skew Joins）

    在AQE从shuffle文件统计信息中检测到倾斜后，将倾斜分区分割为更小的分区

  ```properties
  # 开启AQE
  set spark.sql.adaptive.enabled = true;  
  ```

- Dynamic Partition Pruning 动态分区裁剪（SparkSQL）

- Python API ： pysaprk 和 Koalas （基于Python的pandas）