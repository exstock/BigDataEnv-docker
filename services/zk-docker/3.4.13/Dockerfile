FROM exstock/os-jvm:centos7-jdk8

MAINTAINER exstock <ab19890824@126.com>

USER root

ENV ZOOKEEPER_HOME=/root/zookeeper
ENV PATH=$PATH:.:$ZOOKEEPER_HOME/bin

# 1. 安装 zookeeper 3.4.13
RUN yum install -y wget && \
  wget -c http://mirrors.hust.edu.cn/apache/zookeeper/zookeeper-3.4.13/zookeeper-3.4.13.tar.gz && \
	tar -xzvf zookeeper-3.4.13.tar.gz -C /root/ && \
  mkdir -p $ZOOKEEPER_HOME && \
	mv /root/zookeeper-3.4.13/* $ZOOKEEPER_HOME && \
	rm -rf zookeeper-3.4.13 && \
	rm -rf $ZOOKEEPER_HOME/bin/*.cmd && \
	rm -rf $ZOOKEEPER_HOME/dist-maven && \
	rm -rf $ZOOKEEPER_HOME/docs && \
	rm -rf $ZOOKEEPER_HOME/src && \
	yum clean all 

# 开放2181端口
EXPOSE 2181

# 拷贝zookeeper配置文件
ADD config/zoo.cfg $ZOOKEEPER_HOME/conf/

CMD ["sh", "-c", "$ZOOKEEPER_HOME/bin/zkServer.sh start; bash"]