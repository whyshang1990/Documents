# Flume

## 安装

```bash
# 下载
wget https://mirrors.tuna.tsinghua.edu.cn/apache/flume/1.9.0/apache-flume-1.9.0-bin.tar.gz
# 修改目录名为flume-1.9.0，进入解压目录下conf
cp flume-env.sh.template flume-env.sh
chmod 755 flume-env.sh
# 配置环境变量
vim /etc/profile

export FLUME_HOME=flume安装路径
export PATH=$PATH:$FLUME_HOME/bin

source /etc/profile
```

## 启动
flume-ng agent 使用ng启动agent 
--conf YYYY/ 指定配置所在的文件夹 
--name a1指定的agent别名 
--conf-file YYYY/XXXXXX 指定配置文件 
-Dflume.root.logger=INFO,console 可选，指定日志输出级别(输出到控制台) 
& 可选，Flume在后台运行

```
flume-ng agent --conf /opt/dat2/conf --name dota2 --conf-file dota2-agent.conf -Dflume.root.logger=INFO,console &
```

esp数据流：kafka--flume-postgreSQL
flume接收http请求，并将数据写到kafka，
source : kafka
channel : file
sink : postgreSQL
配置文件
```conf

```

# Kafka(zookeeper)

## 安装
```bash
# zookeeper下载
wget https://downloads.apache.org/zookeeper/zookeeper-3.6.0/apache-zookeeper-3.6.0-bin.tar.gz
# kafka下载
wget https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.3.1/kafka_2.12-2.3.1.tgz
```

安装zookeeper
```bash
cp zoo_sample.cfg zoo.cfg
# 修改dataDir
bin/zkServer.sh start
```

安装kafka
```properties
broker.id=1
listeners=PLAINTEXT://:9092
advertised.listeners=PLAINTEXT://host_ip:9092
log.dirs=/home/wzj/kafka/logs-1
```

配置环境变量
```bash
#配置ZOOKEEPER环境变量
export ZOOKEEPER_HOME=/opt/dota2/zookeeper-3.6.0
export PATH=$PATH:$ZOOKEEPER_HOME/bin
 
#配置KAFKA环境变量
export KAFKA_HOME=/opt/dota2/kafka_2.12-2.3.1/
export PATH=$PATH:$KAFKA_HOME/bin
```

启动kafka zookeeper
`bin/zookeeper-server-start.sh -daemon config/zookeeper.properties`

1. 启动Kafka服务，使用 kafka-server-start.sh 启动 kafka 服务
`bin/kafka-server-start.sh config/server.properties`
2. 使用 kafka-topics.sh 创建单分区单副本的 topic test
`bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test`
3. 查看 topic 列表
`bin/kafka-topics.sh --list --zookeeper localhost:2181`
4. 产生消息，创建消息生产者
`bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test`
5. 消费消息，创建消息消费者
`bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test`
6. 查看Topic消息
`bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test`
