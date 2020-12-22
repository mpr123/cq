国内从 DockerHub 拉取镜像有时会遇到困难，此时可以配置镜像加速器。Docker 官方和国内很多云服务商都提供了国内加速器服务，例如：

```
网易：https://hub-mirror.c.163.com/
七牛云加速器：https://reg-mirror.qiniu.com

阿里云：https://<你的ID>.mirror.aliyuncs.com
阿里云镜像获取地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors，登陆后，左侧菜单选中镜像加速器就可以看到你的专属地址了：
```






单机版elasticsearch的Docker版安装：

```bash
# 创建单机版docker网络环境
docker network create es_network

# 创建单机版elasticsearch容器
docker run -id --name elasticsearch --net es_network \
--restart=always -p 9200:9200 -p 9300:9300 \
-e "discovery.type=single-node" elasticsearch:7.4.0

# 创建kibana
docker run -id --name kibana --net es_network \
--restart=always -p 5601:5601 kibana:7.4.0

elasticsearch访问地址：http://192.168.200.128:9200/
kibana访问地址：http://192.168.200.128:5601/
```



Elasticsearch Head安装:

```
1. 解压Elasticsearch Head.7z
2. 打开谷歌浏览器  菜单->更多工具->扩展程序  
   打开右上角开发者模式
3. 点击左上  加载已解压的扩展程序   
   选择Elasticsearch Head.7z解压包里面的文件夹 0.1.5_0
4. 在主界面浏览器右上角  点击扩展程序 
   使用Elasticsearch Head即可
```



安装IK分词器

```bash
# 上传
上传附件 ik-7.4.0.tar 到Centos中

# 把安装包复制到docker容器的根路径中
docker cp ik-7.4.0.tar elasticsearch:/

# 进入elasticsearch容器
docker exec -it elasticsearch /bin/bash

#创建存放ik分词器的路径
cd /usr/share/elasticsearch/plugins/
mkdir ik

#移动ik安装包到路径中
mv /ik-7.4.0.tar ./ik/

#解压安装包
cd ik
tar -xf ik-7.4.0.tar

#删除安装包
rm -rf ik-7.4.0.tar

#退出容器
exit

重启elasticsearch
docker restart elasticsearch

测试分词
GET _analyze
{
  "analyzer": "ik_smart",
  "text": ["我爱北京天安门"]
}

```





IK分词器安装思路

1. 访问https://github.com/medcl/elasticsearch-analysis-ik/releases
2. 找到对应的ik安装包并下载
3. 解压安装包，并把解压的文件打成 tar包(elasticsearch容器中只能使用 tar 命令)


其他docker环境安装

docker run -id --name mysql \
-v /mnt/mysql_h/data:/var/lib/mysql \
-v /mnt/mysql_h/conf:/etc/mysql/conf.d \
--restart=always -p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=inasdX74O5sqTFcy23QG5vHhW mysql:8.0.18


docker run -id --name zookeeper \
--restart=always -p 2188:2181 zookeeper:3.4.14


docker run -id --name redis \
--restart=always -p 6379:6379 \
redis:5.0.9 --appendonly yes --requirepass "inX97LOt5qTFy234Q8dGc5vH2W"


docker run -id --name rabbitmq \
-e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest \
--restart=always -p 15672:15672 -p 5672:5672 rabbitmq:3.7.8-management

docker run --env MODE=standalone --name nacos -d \
-p 8848:8848 nacos/nacos-server:1.2.0