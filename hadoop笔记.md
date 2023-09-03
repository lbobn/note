**Hadoop**

---



**启动**

> 1. 启动HDFS ：start-dfs.sh` / `stop-dfs.sh
> 2. 启动YARN ：start-yarn.sh / stop-yarn.sh
> 3. 历史服务器 : mapred --daemon (start|stop) historyserver
> 4. hive(进入hive文件夹) ：*后台启动：nohup bin/hive --service metastore >> logs/metastore.log 2>&1 &*
> 5. hiveserver2服务：nohup bin/hive --service hiveserver2>> logs/hiveserver2.log 2>&1 &

------

# HDFS分布式文件系统

主从架构 NameNode 和 DataNode

## 1. HDFS shell操作

### 1.1 启停

> 一键启停
>
> `start-dfs.sh` / `stop-dfs.sh`
>
> 单进程启停
>
> `Hadoop-daemon.sh (start|status|stop) (namenode|secondarynode|datanode)`
>
> 或`hdfs -daemon (start|status|stop) (namenode|secondarynode|datanode)`
>
> jps 查看运行的Java程序

### 1.2 文件操作命令

> 协议头：
> - file://
> - hdfs://node1:8020/
>
> 老版本：`hadoop fs [generic options]`
>
> 新版本：`hdfs dfs [generic options]`

~~~shell
hdfs dfs -mkdir -p /home/hadoop
hdfs dfs -ls -R /home 			# -R递归查看
hdfs dfs -put [-f] [-p] <localsrc> <dst> # 上传   -f 覆盖 -p 保留信息
hdfs dfs -get [-f] [-p] <localsrc> <dst> # 下载
hdfs dfs -cat <src> [| more]
hdfs dfs -appendToFile <localsrc> <dst> # 追加，hdfs不能修改文件
hdfs dfs -rm -r [-skipTrash] <uri>

~~~

WebUI  ： `node1:9870`

客户端：IntelliJ插件 big data tools

## 2.存储原理

默认三个副本，每块256MB

 *修改副本数*

~~~xml
# 在每台服务器的 hdfs-site.xml 中配置
<property>
	<name>dfs.replication</name>
    <value>3</value>             # 默认为3
</property>
~~~

*临时*

~~~shell
hadoop fs -D dfs.replication=2 -put text.txt /tmp/  # 临时设置为2
hadoop fs -setrep [-R] 2 path          # 对于已存在，-R对目录
~~~

*检查文件副本数*

~~~shell
hdfs fsck path [-files [-blocks [-locations]]] 
# 依次为列出文件状态，输出文件块和副本信息，输入每一block详情
~~~

*修改默认块大小*

~~~xml
<property>
	<name>dfs.blocksize</name>
    <value>268435456</value>
    <description>设置HDFS块大小，单位b</description>
</property>
~~~

*NameNode元数据*

edits文件，流水账文件，edits合并后得到 fsimage 文件，namenode 通过此文件找到目标文件所在节点地址

> 合并控制参数 （通过辅助角色SecondaryNameNode）
>
> - dfs.namenode.checkpoint.period  默认3600s
> - dfs.namenode.checkpoint.txns 默认1000000，即100W次事务
> - dfs.namenode.checkpoint.check.period, 默认60s检查一次，上面两条件满足其一即合并

*读写*

客户端和datanode直接通信，namenode负责审核授权和记录信息，datanode之间交换数据，

namenode提供网络距离最近的ip

# 分布式计算

- 分散-汇总模式（MapReduce）
- 中心调度-步骤执行

## MapReduce

# YARN 分布式资源调度

*核心架构*

主从结构 

- ResourceManager 负责整个集群资源的管理，统筹
- NodeManager 负责单个服务器的资源管理，创建容器

> 容器：NodeManager上分配资源的手段
>
> 创建容器并占用资源让程序在容器上执行，且被限制

*辅助架构角色*

- 代理服务器 保障webUI访问的安全性
- 历史服务器 记录历史程序运行信息和日志

## YARN集群部署

MapReduce配置文件

> `$HADOOP_HOME/etc/hadoop`内
>
> - mapred-env.sh文件
>
>   ~~~sh
>   #jdk路径
>   export JAVA_HOME=/export/server/jdk
>   # 设置JobHistoryServer进程内存为1G
>   export HADOOP_JOB_HISTORYSERVER_HEAPSIZE=1000
>   # 设置日志级别
>   export HADOOP_MAPRED_ROOT_LOGGER=INFO,RFA
>   ~~~
>   
> - mapred-site.xml
>
>   ~~~xml
>   <property>
>       <name>mapreduce.framework.name</name>
>       <value>yarn</value>
>       <description>MapReduce运行框架</description>
>   </property>
>   <property>
>       <name>mapreduce.jobhistory.address</name>
>       <value>node1:10020</value>
>       <description>历史服务器通信端口</description>
>   </property>
>   <property>
>       <name>mapreduce.jobhistory.webapp.address</name>
>       <value>node1:19888</value>
>       <description>历史服务器web端口</description>
>   </property>
>   <property>
>       <name>mapreduce.jobhistory.intermediate-done-dir</name>
>       <value>data/mr-history/tmp</value>
>       <description>历史信息在HDFS的记录临时路径</description>
>   </property>
>   <property>
>       <name>mapreduce.jobhistory.done-dir</name>
>       <value>data/mr-history/done</value>
>       <description>历史信息在HDFS的记录路径</description>
>   </property>
>   <property>
>       <name>yarn.app.mapreduce.am.env</name>
>       <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
>       <description>MapReduce home 设置为 HADOOP_HOME</description>
>   </property><property>
>       <name>mapreduce.map.env</name>
>       <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
>       <description>MapReduce home 设置为 HADOOP_HOME</description>
>   </property><property>
>       <name>mapreduce.reduce.env</name>
>       <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
>       <description>MapReduce home 设置为 HADOOP_HOME</description>
>   </property>
>   ~~~
>
>   

YARN配置文件

> - yarn-env.sh
>
>   ~~~sh
>   export JAVA_HOME=/export/server/jdk
>   export HADOOP_HOME=/export/server/hadoop
>   # 配置文件路径的环境变量
>   export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
>   # 配置日志文件的环境变量
>   export HADOOP_LOG_DIR=$HADOOP_HOME/logs
>   ~~~
>
> - yarn-site.xml
>
>   ~~~xml
>   <property>
>       <name>yarn.log.server.url</name>
>       <value>http://node1:19888/jobhistory/logs</value>
>       <description>历史服务器URL</description>
>   </property>
>
>     <property>
>       <name>yarn.web-proxy.address</name>
>       <value>node1:8089</value>
>       <description>代理服务器主机和端口</description>
>     </property>
>     <property>
>       <name>yarn.log-aggregation-enable</name>
>       <value>true</value>
>       <description>开启日志聚合</description>
>     </property>
>
>     <property>
>       <name>yarn.nodemanager.remote-app-log-dir</name>
>       <value>/tmp/logs</value>
>       <description>程序日志HDFS的存储路径</description>
>     </property>
>         <property>
>           <name>yarn.resourcemanager.hostname</name>
>           <value>node1</value>
>           <description>ResourceManager在node1节点</description>
>         </property>
>     <property>
>       <name>yarn.resourcemanager.scheduler.class</name>	
> <value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler</value>
>       <description>选择公平调度器</description>
>     </property>
>     <property>
>       <name>yarn.nodemanager.local-dirs</name>
>       <value>/data/nm-local</value>
>   <description>NodeManager中间数据本地存储路径</description>
>     </property>
>     <property>
>       <name>yarn.nodemanager.log-dirs</name>
>       <value>/data/nm-log</value>
>       <description>NodeManager数据日志本地存储路径</description>
>     </property>
>
>   /*
>     <property>
>       <name>yarn.nodemanager.log.retain-seconds</name>
>       <value>10800</value>
>       <description></description>
>     </property>
>   */
>     <property>
>       <name>yarn.nodemanager.aux-services</name>
>       <value>mapreduce_shuffle</value>
>       <description>为MapReduce程序开启shuffle服务</description>
>     </property>
>   ~~~

> 分发到node2和node3
>
> ~~~shell
> scp mapred-env.sh mapred-site.xml yarn-env.sh yarn-site.xml node2:`pwd`/
> ~~~

$node1:8088$ 查看网页

## 集群启动

> 一键启停`start-yarn.sh` / `stop-yarn.sh`
>
> 独立启动 `yarn --daemon (start|stop) (resourcemanager|nodemanager|proxyserver)`
>
> 历史服务器 `mapred --daemon (start|stop) historyserver`

## 提交MapReduce任务到YARN中运行

> `hadoop jar 程序文件 java类名 [程序参数]...`
>
> 示例代码 `$HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar`
>
> 如：
>
> `hadoop jar /export/server/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar pi 3 10000`

# 分布式SQL计算 Hive

Apache Hive

> 功能组件：元数据管理、SQL解析器

Hive部署

1.安装mysql

2.配置hadoop

> /export/server/hadoop/etc/hadoop/core-site.xml
> ```xml
> <property>
>    <name>hadoop.proxyuser.hadoop.hosts</name>
>       <value>*</value>
>    </property>
> <property>
>  <name>hadoop.proxyuser.hadoop.groups</name>
>      <value>*</value>
>    </property>
> ```

**启动**

1. hive自带客户端

> 1. 启动原数据管理服务
>
> - 前台启动：bin/hive --service metastore 
> - **后台启动：nohup bin/hive --service metastore >> logs/metastore.log 2>&1 &**
>
> 2. 客户端
> - bin/hive

2. hiveserver2 服务(客户端服务)

> 1. nohup bin/hive --service metastore >> logs/metastore.log 2>&1 &
> 2. nohup bin/hive --service hiveserver2>> logs/hiveserver2.log 2>&1 &

> 内置：Beeline    bin/beeline
>
> 第三方：DBeaver，datagrip

# Apache Hive

SQL语法：

~~~sql
create database [if not exist] db_name [location position];
-- 默认存储在/user/hive/warehouse
~~~

