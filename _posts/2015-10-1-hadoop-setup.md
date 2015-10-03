---
layout: post
title: 大数据笔记：Mac Hadoop安装
description: 本文详细介绍Hadoop在Mac上的安装过程。
tags: 后端
---

## **目的**

本文详细介绍Hadoop在Mac上的安装过程。

由于使用Homebrew安装完Hadoop后，启动总是有问题，网上查了很多资料，卸载，手动安装后，才算搞定，所以写篇博客纪录下。

## **基础环境准备**

* Mac OS X Yosemite（10.10.5）
* JDK 1.7.0_75
* Hadoop 2.6.0

### **安装JDK**

* JDK下载安装，[戳这里](http://www.oracle.com/technetwork/cn/java/javase/downloads/index.html)
* 环境变量设置，[戳这里](http://cherryleer.com/2012/10/02/java-env-variables)

验证配置是否成功：

{% highlight bash %}
[cherryleer@home:~]$ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.7.0_75.jdk/Contents/Home
[cherryleer@home:~]$ java -version
java version "1.7.0_75"
Java(TM) SE Runtime Environment (build 1.7.0_75-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.75-b04, mixed mode)
{% endhighlight %}

### **SSH免密码登录**

* SSH免密码登录过程，[戳这里](http://cherryleer.com/2015/03/24/ssh-login)
* 常见问题：ssh localhost异常，[戳这里](http://cherryleer.com/2015/09/30/ssh-localhost)

验证配置是否成功：

{% highlight bash %}
[cherryleer@home:~]$ ssh localhost
Last login: Wed Sep 30 17:56:40 2015
{% endhighlight %}

## **Hadoop安装**

### **下载Hadoop**

官方下载地址：http://hadoop.apache.org/releases.html#Download，我选择的是2.6.0版本。

下载后，解压到指定目录。

{% highlight bash %}
tar -zxf hadoop-2.6.0.tar.gz -C /usr/local/share
{% endhighlight %}

### **配置环境变量**

在用户根目录下.bash_profile文件中，添加如下信息：

{% highlight bash %}
# hadoop
export HADOOP_HOME=/usr/local/share/hadoop-2.6.0;
export HADOOP_COMMON_HOME=${HADOOP_HOME};
export HADOOP_HDFS_HOME=${HADOOP_HOME};
export HADOOP_MAPRED_HOME=${HADOOP_HOME};
export HADOOP_YARN_HOME=${HADOOP_HOME};
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop;
export YARN_CONF_DIR=${HADOOP_CONF_DIR};

export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin;
{% endhighlight %}

### **修改hadoop-env.sh**

路径：${HADOOP_HOME}/etc/hadoop/hadoop-env.sh

修改的内容如下：

{% highlight bash %}
# 找到JAVA__HOME配置参数
export JAVA_HOME=$(/usr/libexec/java_home -d 64 -v 1.7)

#找到HADOOP_OPTS 配置增加下面参数
export HADOOP_OPTS="$HADOOP_OPTS -Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"
{% endhighlight %}

### **修改core-site.xml**

路径：${HADOOP_HOME}/etc/hadoop/core-site.xml

在Configuration节点下添加：

{% highlight xml %}
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://localhost:9000</value>
  <description>The name of the default file system.</description>
</property>

<property>
  <name>hadoop.tmp.dir</name>
  <value>/usr/local/share/hadoop-2.6.0/hdfs/tmp</value>
  <description>A base for other temporary directories.</description>
</property>

<property>
  <name>io.native.lib.available</name>
  <value>false</value>
  <description>default value is true:Should native hadoop libraries, if present, be used.</description>
</property>
{% endhighlight %}

详细内容请参考：[core-default.xml](http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-common/core-default.xml)

### **修改hdfs-site.xml**

路径：${HADOOP_HOME}/etc/hadoop/hdfs-site.xml

在Configuration节点下添加：

{% highlight xml %}
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
{% endhighlight %}

详细内容请参考：[hdfs-default.xml](http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml)

### **修改mapred-site.xml**

路径：${HADOOP_HOME}/etc/hadoop/mapred-site.xml

在Configuration节点下添加：

{% highlight xml %}
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
  <final>true</final>
</property>
{% endhighlight %}

详细内容请参考：[mapred-default.xml](http://hadoop.apache.org/docs/r2.6.0/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml)

## **启动Hadoop**

执行hdfs namenode -format：

{% highlight bash %}
[cherryleer@home:~]$ hdfs namenode format
15/10/02 23:35:47 INFO namenode.NameNode: STARTUP_MSG:
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = home/192.168.31.136
STARTUP_MSG:   args = [format]
STARTUP_MSG:   version = 2.6.0
STARTUP_MSG:   classpath = /usr/local/share/hadoop-2.6.0/...
/************************************************************
...此处省略若干字...
/************************************************************
STARTUP_MSG:   build = https://git-wip-us.apache.org/repos/asf/hadoop.git -r e3496499ecb8d220fba99dc5ed4c99c8f9e33bb1; compiled by 'jenkins' on 2014-11-13T21:10Z
STARTUP_MSG:   java = 1.7.0_75
************************************************************/
15/10/02 23:35:47 INFO namenode.NameNode: registered UNIX signal handlers for [TERM, HUP, INT]
15/10/02 23:35:47 INFO namenode.NameNode: createNameNode [format]
Usage: java NameNode [-backup] |
	[-checkpoint] |
	[-format [-clusterid cid ] [-force] [-nonInteractive] ] |
	[-upgrade [-clusterid cid] [-renameReserved<k-v pairs>] ] |
	[-upgradeOnly [-clusterid cid] [-renameReserved<k-v pairs>] ] |
	[-rollback] |
	[-rollingUpgrade <rollback|downgrade|started> ] |
	[-finalize] |
	[-importCheckpoint] |
	[-initializeSharedEdits] |
	[-bootstrapStandby] |
	[-recover [ -force] ] |
	[-metadataVersion ]  ]

15/10/02 23:35:47 INFO namenode.NameNode: SHUTDOWN_MSG:
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at home/192.168.31.136
************************************************************/
{% endhighlight %}

执行start-dfs.sh：

{% highlight bash %}
[cherryleer@home:~]$ start-dfs.sh
15/10/02 23:45:33 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Starting namenodes on [localhost]
localhost: starting namenode, logging to /usr/local/share/hadoop-2.6.0/logs/hadoop-cherryleer-namenode-home.out
localhost: starting datanode, logging to /usr/local/share/hadoop-2.6.0/logs/hadoop-cherryleer-datanode-home.out
Starting secondary namenodes [0.0.0.0]
0.0.0.0: starting secondarynamenode, logging to /usr/local/share/hadoop-2.6.0/logs/hadoop-cherryleer-secondarynamenode-home.out
15/10/02 23:45:49 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
{% endhighlight %}

执行start-yarn.sh：

{% highlight bash %}
[cherryleer@home:~]$ start-yarn.sh
starting yarn daemons
starting resourcemanager, logging to /usr/local/share/hadoop-2.6.0/logs/yarn-cherryleer-resourcemanager-home.out
localhost: starting nodemanager, logging to /usr/local/share/hadoop-2.6.0/logs/yarn-cherryleer-nodemanager-home.out
{% endhighlight %}

执行jps，确认进程是否存在：

{% highlight bash %}
[cherryleer@home:~]$ jps
12472 DataNode
12766 NodeManager
12686 ResourceManager
12817 Jps
12571 SecondaryNameNode
12395 NameNode
{% endhighlight %}

以上启动日志没有报错，相关进程也都正常存在，说明Hadoop启动成功。

## **关闭Hadoop**

执行stop-all.sh:

{% highlight bash %}
[cherryleer@home:~]$ stop-all.sh
This script is Deprecated. Instead use stop-dfs.sh and stop-yarn.sh
15/10/02 23:51:20 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Stopping namenodes on [localhost]
localhost: stopping namenode
localhost: stopping datanode
Stopping secondary namenodes [0.0.0.0]
0.0.0.0: stopping secondarynamenode
15/10/02 23:51:39 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
stopping yarn daemons
stopping resourcemanager
localhost: stopping nodemanager
no proxyserver to stop
{% endhighlight %}

执行jps，确认进程是否已经退出：

{% highlight bash %}
[cherryleer@home:~]$ jps
13244 Jps
{% endhighlight %}

相关进程已经退出，说明Hadoop正常关闭。

## **参考资料**

[Hadoop官网](http://www.micmiu.com)

[Hadoop 2.2.0 单节点安装和测试](http://www.micmiu.com/bigdata/hadoop/hadoop2x-single-node-setup)