# Zookeeper：分布式协调服务

### 1.官网地址   

 https://zookeeper.apache.org/

### 2. 安装 验证 使用

- 下载安装包  [**https://mirrors.bfsu.edu.cn/apache/zookeeper/zookeeper-3.7.0/apache-zookeeper-3.7.0-bin.tar.gz** ](https://mirrors.bfsu.edu.cn/apache/zookeeper/zookeeper-3.7.0/apache-zookeeper-3.7.0-bin.tar.gz)

- 将文件放在 /opt 下 解压压缩包 

  ```bash
  tar -xvf apache-zookeeper-3.6.3-bin.tar.gz 
  mv apache-zookeeper-3.6.3-bin/ zookeeper/
  ```

- 要启动ZooKeeper，需要一个配置文件  进入 zookeeper中的conf 目录 ,新建一个zoo.cfg 文件 (或者直接将zoo_sample.cfg复制 )

  ```shell
  cd /opt/zookeeper/conf/
  cp zoo_sample.cfg  zoo.cfg
  ```

- 编辑配置文件 

  

  ```shell
  vi zoo.cfg
  ```

  ex:

  > ```shell
  > tickTime=2000
  > dataDir=/var/lib/zookeeper
  > clientPort=2181
  > ```

  > - **tickTime**：ZooKeeper使用的基本时间单位（毫秒）。它用于做心跳，并且最小会话超时将是tickTime的两倍。
  >
  > - **initLimit**：**LF初始通信时限** 
  >
  >   ​	此配置表示，允许 follower （相对于 leader 而言的“客户端”）连接 并同步到 leader 的初始化连接时间，它以 tickTime 的倍数来表示。当超过设置倍数的 tickTime 时间，则连接失败。
  >
  >      如果在设定的时间段内，半数以上的跟随者未能完成同步，领导者便会宣布放弃领导地位，进行另一次的领导选举。如果zk集群环境数量确实很大，同步数据的时间会变长，因此这种情况下可以适当调大该参数。默认为10。
  >
  > - **syncLimit **：**LF同步通信时限**
  >
  >   ​	 集群中的follower服务器(F)与leader服务器(L)之间 请求和应答 之间能容忍的最多心跳数（tickTime的数量）。
  >
  >      此配置表示， leader 与 follower 之间发送消息，请求 和 应答 时间长度。如果 follower 在设置的时间内不能与leader 进行通信，那么此 follower 将被丢弃。所有关联到这个跟随者的客户端将连接到另外一个跟随着。
  >
  > - **dataDir**：存储内存数据库快照的位置，除非另有说明，否则存储数据库更新的事务日志。
  >
  > - **clientPort**：用于侦听客户端连接的端口

  ​	以上是基本的配置信息 ，和Redis哨兵不同，在zk集群中需要把节点都写在配置文件中  ex:

  ```shell
  server.1=nde01:2888:3888
  server.2=nde02:2888:3888
  server.3=nde03:2888:3888
  server.4=nde04:2888:3888
  
  ```

  **其中 3888端口 在主挂掉或者初始化的时候 在个端口号去通讯选出一个leader,Leader会启动一个2888端口，其他节点去连接2888端口。**

