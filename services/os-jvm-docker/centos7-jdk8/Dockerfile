FROM centos:7

MAINTAINER exstock <ab19890824@126.com>

USER root

ENV JAVA_HOME=/usr/lib/jdk8
ENV SCALA_HOME=/usr/lib/scala

ENV PATH=$PATH:$JAVA_HOME/bin:$SCALA_HOME/bin:.

# 安装 wget,rsync
# 设置系统时区为北京时间（实际为Asia/Shanghai）

RUN yum install -y wget \
    && yum install -y rsync \
    && wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "https://download.oracle.com/otn/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.tar.gz?AuthParam=1546758125_84e292692ec62ff3fb20fc3d7114e9a8" \   
    && mv jdk-8u121-linux-x64.tar.gz?AuthParam=1546758125_84e292692ec62ff3fb20fc3d7114e9a8 jdk-8u121-linux-x64.tar.gz \
    && rm -rf jdk-8u121-linux-x64.tar.gz?AuthParam=1546757624_d255a143bd8ba413ec6bbb4b4b4579a7 \
    && tar -xzvf jdk-8u121-linux-x64.tar.gz -C /usr/lib/ \
    && mkdir -p $JAVA_HOME \
    && mv /usr/lib/jdk1.8.0_121/* $JAVA_HOME \
    && rm -rf jdk-8u121-linux-x64.tar.gz $JAVA_HOME/src.zip $JAVA_HOME/javafx-src.zip $JAVA_HOME/man \
    && yum clean all \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && wget -c https://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz \
    && tar -zxvf scala-2.11.8.tgz -C /usr/lib/ \
    && mkdir -p $SCALA_HOME \
    && mv /usr/lib/scala-2.11.8/* $SCALA_HOME \
    && rm -rf scala-2.11.8.tgz 
    
    
CMD ["/bin/bash"]