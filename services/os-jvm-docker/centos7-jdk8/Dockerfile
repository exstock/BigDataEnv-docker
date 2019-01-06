FROM centos:7

MAINTAINER exstock <ab19890824@126.com>

USER root

ENV JAVA_HOME=/usr/lib/jdk8
ENV SCALA_HOME=/usr/lib/scala

ENV PATH=$PATH:$JAVA_HOME/bin:$SCALA_HOME/bin:.

# 安装 wget,rsync
# 设置系统时区为北京时间（实际为Asia/Shanghai）

RUN yum install -y wget rsync \
    && wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u101-b13/jdk-8u121-linux-x64.tar.gz \
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
    && rm -rf scala-2.11.8.tgz \
    
    
CMD ["/bin/bash"]