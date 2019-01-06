FROM exstock/hadoop:2.7.7

MAINTAINER exstock <ab19890824@126.com>

USER root

ENV HIVE_HOME=/root/hive

ENV PATH=$PATH:$HIVE_HOME/bin:.

# 安装 Hive
RUN yum install -y wget && \
    wget -c http://mirrors.hust.edu.cn/apache/hive/hive-2.3.4/apache-hive-2.3.4-bin.tar.gz && \
    tar -xzvf apache-hive-2.3.4-bin.tar.gz -C /root/ && \
    mv /root/apache-hive-2.3.4-bin/* $HIVE_HOME && \
    rm -rf apache-hive-2.3.4-bin.tar.gz && \
    rm -rf $HIVE_HOME/examples && \
    wget -c https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.40.tar.gz && \
    tar -xzvf mysql-connector-java-5.1.40.tar.gz -C /root/ && \
    mv /root/mysql-connector-java-5.1.40/mysql-connector-java-5.1.40-bin.jar $HIVE_HOME/lib/ && \
    rm -rf /root/mysql-connector-java-5.1.40 && \
    rm -rf /root/mysql-connector-java-5.1.40.tar.gz && \
    yum clean all


# 拷贝Hive配置文件
COPY config/* $HIVE_HOME/conf/

#暴露端口
EXPOSE 9083 10000

CMD ["/bin/bash"]