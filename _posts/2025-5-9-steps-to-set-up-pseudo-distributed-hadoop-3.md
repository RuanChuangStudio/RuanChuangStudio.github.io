---
layout: post
title: Hadoop 3.x 伪分布式搭建步骤
categories: [Git]
description: Hadoop 3.x 伪分布式搭建步骤
keywords: Hadoop 3.x 伪分布式搭建步骤
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false

--- 

## **一. 下载 Java 1.8 和 Hadoop 3**

在安装 Hadoop 之前，需要先安装 JDK（Java Development Kit）和 Hadoop。对于 Hadoop 3.x 版本，推荐使用 Java 1.8。注意

### 1.1 **下载 Oracle JDK 1.8**

首先需要下载 Oracle 提供的 JDK 1.8 版本。Oracle JDK 1.8 的下载地址为：[Oracle JDK 1.8 下载]([Java Archive Downloads - Java SE 8](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html))。选择适合你的操作系统的压缩包进行下载。

### 1.2 **下载 Hadoop 3.x**

接下来需要下载 Hadoop 3.x 的稳定版本，推荐使用 [Hadoop 3.3.6](https://hadoop.apache.org/releases.html) 版本，点击页面中的相应下载链接下载 Hadoop 压缩包。

下载时要确保选择正确的版本和文件格式（通常是 tar.gz 格式），以便在 Linux 环境中解压。

------

## **二. 上传文件到服务器**

### 2.1 **使用 SCP 上传文件**

你可以使用 `scp` 命令将下载的压缩包上传到目标服务器的 `/opt` 目录下。假设你的 Hadoop 和 JDK 文件已经下载到本地：

- 使用以下命令将 JDK 文件上传到服务器：

```bash
scp jdk-11.0.26_linux-x64_bin.tar.gz node0:/opt
```

- 使用以下命令将 Hadoop 文件上传到服务器：

```bash
scp hadoop-3.4.1.tar.gz node0:/opt
```

这会将文件传输到 `node0` 服务器的 `/opt` 目录。

### 2.2 **确认上传成功**

你可以登录到服务器并确认文件是否上传成功：

```bash
ssh node0
ls /opt
```

如果显示文件 `jdk-11.0.26_linux-x64_bin.tar.gz` 和 `hadoop-3.4.1.tar.gz`，则说明上传成功。

------

## **三. 解压文件**

### 3.1 **解压 JDK 和 Hadoop**

在服务器上，进入 `/opt` 目录并解压上传的文件：

```bash
cd /opt
tar -zxvf jdk-11.0.26_linux-x64_bin.tar.gz
tar -zxvf hadoop-3.4.1.tar.gz
```

这会将 JDK 和 Hadoop 解压到各自的文件夹中。

### 3.2 **确认解压成功**

执行 `ls` 命令，确保 `jdk-11.0.26` 和 `hadoop-3.4.1` 文件夹已经解压出来。

------

## **四. 配置环境变量**

### 4.1 **修改 .bashrc 文件**

为了方便使用 Java 和 Hadoop，设置相应的环境变量。编辑当前用户的 `.bashrc` 文件：

```bash
nano ~/.bashrc
```

### 4.2 **设置环境变量**

在 `.bashrc` 文件的末尾，添加以下环境变量设置：

```bash
export HADOOP_HOME=/home/hduser/software/hadoop-3.3.6
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

# 设置 Java 环境变量
export JAVA_HOME=/home/hduser/software/jdk1.8.0_202
export PATH=$PATH:$JAVA_HOME/bin

# 设置 Hadoop 和 Java 的库路径
export HADOOP_CLASSPATH=$HADOOP_HOME/lib/*:$JAVA_HOME/lib/*
```

### 4.3 **使配置生效**

保存 `.bashrc` 文件并使修改生效：

```bash
source ~/.bashrc
```

------

## **五. 验证环境变量**

### 5.1 **验证 Hadoop 环境变量**

确保 Hadoop 环境变量配置成功：

```bash
echo $HADOOP_HOME
```

输出应该是：`/opt/hadoop-3.4.1`。

### 5.2 **验证 Java 环境变量**

验证 Java 环境变量配置：

```bash
echo $JAVA_HOME
```

输出应该是：`/opt/jdk-11.0.26`。

### 5.3 **验证 Hadoop 和 Java 是否正常使用**

执行以下命令确认 Hadoop 和 Java 的版本：

```bash
hadoop version
java -version
```

如果命令输出正确的版本信息，表示环境配置成功。

------

## **六. 设置 SSH 免密码登录**

Hadoop 集群需要使用 SSH 来进行节点间的通信，为了方便操作，需要配置免密码登录。

### 6.1 **生成 SSH 密钥对**

使用以下命令生成 SSH 密钥对：

```bash
ssh-keygen -t rsa
```

当系统提示保存密钥时，可以按 `Enter`，默认将密钥保存到 `~/.ssh/id_rsa` 和 `~/.ssh/id_rsa.pub` 文件中。

### 6.2 **查看生成的 SSH 密钥**

执行以下命令查看生成的密钥文件：

```bash
ls ~/.ssh
```

你应该能看到 `id_rsa` 和 `id_rsa.pub` 文件。

### 6.3 **将公钥复制到目标节点**

使用 `ssh-copy-id` 命令将公钥复制到目标节点：

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub node0
```

### 6.4 **验证免密码登录**

使用以下命令登录到 `node0`，验证是否成功配置免密码登录：

```bash
ssh node0
```

如果是第一次登录，会提示确认连接，输入 `yes` 后即可登录，之后无需输入密码。

------

## **七. 配置 Hadoop 配置文件**

Hadoop 的配置文件位于 `HADOOP_HOME/etc/hadoop` 目录下，需要对这些配置文件进行修改。

### 7.1 **配置 hadoop-env.sh**

在 `hadoop-env.sh` 文件中设置 Java 环境变量：

```bash
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```

在文件中找到 `export JAVA_HOME` 行，并设置为：

```bash
export JAVA_HOME=路径
```

### 7.2 **配置 core-site.xml**

在 `core-site.xml` 中设置 Hadoop 核心配置：

```bash
nano $HADOOP_HOME/etc/hadoop/core-site.xml
```

添加以下配置：

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://node0:9000</value>
  </property>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/opt/hadoop-3.4.1/tmp</value>
  </property>
  <property>
    <name>hadoop.http.staticuser.user</name>
    <value>!!!username!!!</value>
  </property>
</configuration>
```

### 7.3 **配置 hdfs-site.xml**

在 `hdfs-site.xml` 中设置 HDFS 配置：

```bash
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

添加以下配置：

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```

### 7.4 **配置 mapred-site.xml**

在 `mapred-site.xml` 中配置 MapReduce：

```bash
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

添加以下配置：

```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

### 7.5 **配置 yarn-site.xml**

在 `yarn-site.xml` 中配置 YARN 设置：

```bash
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

添加以下配置：

```xml
<configuration>
  <property>
    <name>yarn.resoucemanager.name</name>
    <value>node0</value>
  </property>
  <property>
    <name>yarn.resoucemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
</configuration>
```

------

## **八. 格式化 HDFS（Hadoop Distributed File System）**

在首次启动 Hadoop 集群之前，必须先对 HDFS 进行格式化操作。这一步会初始化 HDFS 文件系统的元数据，生成文件目录结构和 `fsimage` 文件。

### 8.1 **格式化步骤**

打开终端，输入以下命令来格式化 HDFS 的 NameNode：

```bash
hdfs namenode -format
```

执行后，终端会输出大量日志信息，其中包含以下关键提示：

```
INFO common.Storage: Storage directory /usr/local/hadoop/data/dfs/name has been successfully formatted.
```

如果你看到类似上面的信息，说明格式化成功，NameNode 的数据目录已经建立完成。

⚠️ **注意事项：**

- 只需要 **首次启动** Hadoop 集群时执行一次格式化命令。
- **不要在已有数据的集群上重复执行格式化命令**，否则会清空所有 HDFS 上的数据。
- 默认数据目录在 `$HADOOP_HOME/data/dfs/name`，可通过 `hdfs-site.xml` 配置修改。

------

## **九. 启动 Hadoop 集群**

HDFS 格式化完成后，即可启动 Hadoop 集群。

### 9.1 **启动所有服务**

在命令行中输入：

```bash
start-all.sh
```

该命令将自动启动以下两个核心子系统的全部组件：

- HDFS：`NameNode`, `DataNode`, `SecondaryNameNode`
- YARN：`ResourceManager`, `NodeManager`

### 9.2 **验证服务是否启动成功**

输入命令：

```bash
jps
```

你应该能看到类似以下输出：

```bash
4000 NameNode
4128 DataNode
4316 SecondaryNameNode
4540 ResourceManager
4665 NodeManager
4994 Jps
```

✅ 以上组件代表 Hadoop 集群启动成功，包括：

- `NameNode`：管理 HDFS 元数据
- `DataNode`：存储实际数据块
- `SecondaryNameNode`：辅助合并编辑日志和元数据快照
- `ResourceManager`：负责 YARN 资源调度
- `NodeManager`：管理每个节点的资源和任务执行

------

## **十. Web 管理界面访问**

Hadoop 提供了多个基于 Web 的 UI，用于查看集群状态、资源使用情况、任务进度等。你可以使用浏览器访问以下界面：

### ✅ **1. NameNode Web UI（查看 HDFS 文件系统）**

- 📌 **端口：`9870`**

- 🌐 **访问地址：**

  ```
  http://node0:9870/
  http://<服务器IP>:9870/
  ```

### ✅ **2. ResourceManager Web UI（查看 YARN 任务调度）**

- 📌 **端口：`8088`**

- 🌐 **访问地址：**

  ```
  http://node0:8088/
  http://<服务器IP>:8088/
  ```

### ✅ **3. NodeManager Web UI（查看工作节点运行情况）**

- 📌 **端口：`8042`**

- 🌐 **访问地址（在每个节点上访问）：**

  ```
  http://node0:8042/
  ```

### ✅ **4. SecondaryNameNode Web UI（查看合并日志状态）**

- 📌 **端口：`9868`**

- 🌐 **访问地址：**

  ```
  http://node0:9868/
  ```

------

## 🔍 常见问题排查与解决

### ❓ 无法访问 Web UI？

请检查以下几项：

#### 🔸 1. 使用的是服务器的实际 IP 吗？

使用命令查看 IP 地址：

```bash
ifconfig
# 或
ip a
```

然后在浏览器中访问以下地址：

```
http://<实际IP>:9870/
http://<实际IP>:8088/
```

#### 🔸 2. 防火墙是否开启并拦截了端口？

你可以临时关闭防火墙进行测试：

```bash
sudo systemctl stop firewalld
```

或者开放 Hadoop 所需端口（以 `9870` 和 `8088` 为例）：

```bash
sudo firewall-cmd --permanent --add-port=9870/tcp
sudo firewall-cmd --permanent --add-port=8088/tcp
sudo firewall-cmd --reload
```

🛡️ 建议仅在生产环境中根据需要控制访问权限，开发测试时可以关闭防火墙或开放必要端口。

------

## ✅ 总结

至此，你已经完成了 Hadoop 集群的基本启动流程：

1. ✅ 初始化格式化 HDFS
2. ✅ 启动 Hadoop 服务
3. ✅ 验证服务运行状态
4. ✅ 成功访问 Web 管理页面

你现在已经搭建好了一个基本的 Hadoop 分布式系统，可以开始进行数据上传、处理和运行 MapReduce 等任务了。