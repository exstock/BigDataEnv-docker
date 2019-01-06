FROM exstock/os-jvm:centos7-jdk8

MAINTAINER exstock <ab19890824@126.com>

USER root

ENV JAVA_HOME=/usr/lib/jdk8
ENV MAVEN_HOME=/root/apache-maven-3.3.9
ENV MAVEN_OPTS="-Xmx4g -XX:MaxPermSize=1g -XX:ReservedCodeCacheSize=1g"

ENV HADOOP_HOME=/root/hadoop
ENV HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
ENV HADOOP_INSTALL=$HADOOP_HOME
ENV HADOOP_MAPRED_HOME=$HADOOP_HOME
ENV HADOOP_COMMON_HOME=$HADOOP_HOME
ENV HADOOP_HDFS_HOME=$HADOOP_HOME
ENV HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
ENV HADOOP_YARN_HOME=$HADOOP_HOME
ENV LD_LIBRARY_PATH=$HADOOP_HOME/lib/native
# ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HADOOP_HOME/lib/native/:/root/protobuf/lib
ENV PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$JAVA_HOME/bin:$MAVEN_HOME/bin:.

# 1. 安装 OpenSSH, OpenSSL, bzip2-devel
# 2. 同时配置SSH免密钥登陆
RUN echo "nameserver 144.144.144.144" >> /etc/resolv.conf && \
    yum install -y openssh openssh-server openssh-clients openssl openssl-devel bzip2-devel && \
    yum clean all && \
    ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    
ADD config/other/ssh_config /root/.ssh/config
RUN chmod 600 /root/.ssh/config && \
    chown root:root /root/.ssh/config

#安装maven，编译安装Protocol Buffer, snappy, Hadoop
#yum groupinstall -y "Development Libraries"
RUN yum groupinstall -y "Development Tools" && \
    yum install -y cmake && \
    yum install -y ant && \
    yum install -y autoconf && \
    yum install -y automake && \
    yum install -y libtool && \
    yum install -y curl && \
    yum install -y make && \
    yum install -y gcc-c++ && \
    yum install -y unzip && \
    yum install -y git && \
    yum install -y lzo-devel && \
    yum install -y zlib-devel && \
    yum install -y gcc && \
    yum install -y svn && \
    yum install -y ncurses-devel && \
    yum install -y bzip2 && \
    yum install -y bzip2-devel && \
    wget -c http://mirrors.gigenet.com/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz && \
    tar -zxvf apache-maven-3.3.9-bin.tar.gz -C /root/ && \
    rm -rf apache-maven-3.3.9-bin.tar.gz && \
    wget -c https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz && \
    tar -xzvf protobuf-2.5.0.tar.gz -C /root/ && \
    cd /root/protobuf-2.5.0 && \
    ./configure && \
    make && make install && \
	  rm -rf protobuf-2.5.0.tar.gz && \
	  wget -c  https://src.fedoraproject.org/repo/pkgs/snappy/snappy-1.1.1.tar.gz/8887e3b7253b22a31f5486bca3cbc1c2/snappy-1.1.1.tar.gz && \
	  tar -zxvf snappy-1.1.1.tar.gz -C /root/ && \
	  cd /root/snappy-1.1.1 && \
	  ./configure && \
    make && make install && \
    rm -rf snappy-1.1.1.tar.gz && \
    wget -c http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7-src.tar.gz && \
    tar -xzvf hadoop-2.7.7-src.tar.gz -C /root/ && \
    cd /root/hadoop-2.7.7-src && \
	  mvn package -Pdist,native -DskipTests -Dtar -Dsnappy.lib=/usr/local/lib -Dbundle.snappy && \
	  mkdir -p $HADOOP_HOME && \
    mv /root/hadoop-2.7.7/hadoop-dist/target/hadoop-2.7.7/* $HADOOP_HOME && \
    rm -rf hadoop-2.7.7.tar.gz && \
    rm -rf /root/hadoop-2.7.7 && \
    rm -rf $HADOOP_HOME/bin/*.cmd && \
    rm -rf $HADOOP_HOME/sbin/*.cmd && \
    rm -rf $HADOOP_HOME/sbin/*all* && \
    rm -rf $HADOOP_CONF_DIR/*.cmd && \
    rm -rf $HADOOP_CONF_DIR/*.template && \
    rm -rf $HADOOP_CONF_DIR/*.example && \
    yum clean all
    
#拷贝环境变量配置文件
ADD config/other/bash_profile /root/.bash_profile

#拷贝Hadoop配置文件
ADD config/hadoop/* $HADOOP_HOME/etc/hadoop/

#暴露端口
EXPOSE 8088 50070

#建立HDFS目录，日志目录. 格式化NameNode
#RUN mkdir -p /works/hadoop/dfs/namenode && \ 
#    mkdir -p /works/hadoop/dfs/datanode && \
#    mkdir -p /works/hadoop/dfs/namesecondary && \
#    mkdir -p /works/hadoop/tmp && \
#    mkdir -p /works/hadoop/logs && \
#    $HADOOP_HOME/bin/hdfs namenode -format


#RUN systemctl disable firewalld && \
#    systemctl stop firewalld && \
#    sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config && \
#    sed -i '$a\net.ipv4.ip_forward=1' /etc/sysctl.conf && \
#    reboot && \


CMD [ "sh", "-c", "systemctl sshd start; bash"]