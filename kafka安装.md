1、下载镜像
这里使用了wurstmeister/kafka和wurstmeister/zookeeper这两个版本的镜像

```shell
docker pull wurstmeister/zookeeper:latest
```

```shell
docker pull wurstmeister/kafka:2.12-2.3.0
```


在命令中运行docker images验证两个镜像已经安装完毕

2.启动
启动zookeeper容器

```shell
docker run -d --name zookeeper \
-p 2181:2181 zookeeper:3.4.14
```

启动kafka容器

```shell
docker run -d --name kafka \
--restart=always -p 9092:9092 \
-e KAFKA_BROKER_ID=0 \
-e KAFKA_ZOOKEEPER_CONNECT=idc.cubepaas.com:20337 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://idc.cubepaas.com:20860 \
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 \
-t wurstmeister/kafka:2.12-2.3.0
```

参数说明：
-e KAFKA_BROKER_ID=0  在kafka集群中，每个kafka都有一个BROKER_ID来区分自己
-e KAFKA_ZOOKEEPER_CONNECT=172.16.0.13:2181/kafka 配置zookeeper管理kafka的路径172.16.0.13:2181/kafka
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://172.16.0.13:9092  把kafka的地址端口注册给zookeeper，如果是远程访问要改成外网IP,类如Java程序访问出现无法连接。
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 配置kafka的监听端口

#进入kafka容器

```shell
docker exec -it kafka /bin/bash
```

#进入kafka默认目录

```shell
cd /opt/kafka_2.12-2.3.0/bin/
```

#创建一个主题

```shell
./kafka-topics.sh --zookeeper 115.231.110.237:27100 --create --topic heima --partitions 2 --replication-factor 1
```


#展示所有主题

```shell
./kafka-topics.sh --zookeeper 115.231.110.237:27100 --list
```

#查看主题详情

```shell
./kafka-topics.sh --zookeeper 115.231.110.237:27100 --describe --topic heima
```

#开启生产者，发消息

```shell
./kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic heima
```

#开启消费者，收消息

```shell
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic heima
```


