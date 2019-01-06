FROM exstock/hive:2.3.4

MAINTAINER exstock <ab19890824@126.com>

USER root

#设置环境变量
ENV JAVA_HOME=/usr/lib/jdk8
ENV SCALA_HOME=/usr/lib/scala
ENV MAVEN_HOME=/root/apache-maven-3.3.9
ENV MAVEN_OPTS="-Xmx4g -XX:MaxPermSize=1g -XX:ReservedCodeCacheSize=1g"
ENV SPARK_HOME=/root/spark
ENV PATH=$PATH:$JAVA_HOME/bin:$SCALA_HOME/bin:$MAVEN_HOME/bin:$SPARK_HOME/bin:$SPARK_HOME/sbin:.

#编译安装spark 2.2.2
RUN wget -c https://archive.apache.org/dist/spark/spark-2.2.2/spark-2.2.2.tgz && \
    tar -xzvf spark-2.2.2.tgz -C /root/ && \

#拷贝spark编译文件
COPY config/make-config/make-distribution.sh /root/spark-2.2.2/dev/
COPY config/make-config/pom.xml /root/spark-2.2.2/

RUN cd /root/spark-2.2.2 && \
    ./dev/make-distribution.sh --name spark-22 --tgz -Dhadoop.version=2.7.7 -Phadoop-2.7 -Phive -Phive-thriftserver -Pyarn -PR -Pmesos && \
    tar -zxvf spark-2.2.2-bin-spark-22.tgz && \
    mv /root/spark-2.2.2/spark-2.2.2-bin-spark-22/* $SPARK_HOME && \
    rm -rf /root/spark-2.2.2 && \
    rm -rf $SPARK_HOME/bin/*.cmd && \
    rm -rf $SPARK_HOME/sbin/*.cmd && \
    rm -rf $SPARK_HOME/ec2 && \
    rm -rf $SPARK_HOME/examples && \
    rm -rf $SPARK_HOME/lib/spark-examples-*

#拷贝Spark配置文件
COPY config/spark/* $SPARK_HOME/conf/

#拷贝启动脚本
ADD config/other/startup /bin/startup

#暴露端口
EXPOSE 8080

CMD [ "sh", "-c", "startup", "systemctl sshd start; bash"]