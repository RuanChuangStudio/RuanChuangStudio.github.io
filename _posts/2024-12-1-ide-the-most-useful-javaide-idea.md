---
layout: post
title: IntelliJ IDEA 最好用的 Java IDE
categories: [Java, IDEA, IDE]
description: 最好用的 Java IDE
keywords: Java, IDEA , IDE ,工具
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# IntelliJ IDEA 最好用的 Java IDE

## Q&A

### 如何让代码自动换行显示？

Preferences -> Editor -> General，勾选 Soft-wrap these files，后面的文件名匹配改为 `*`，表示所有文件都启用自动换行。

### 如何加大控制台日志缓冲区大小？

有时候日志量太大，需要的信息被冲出控制台缓冲区，这种时候可以加大缓冲区大小，让它多缓冲一些行数。

Preferences -> Editor -> General -> Console，勾选 Override console cycle buffer size，然后修改后面的数字大小，默认是 1024（单位 KB），比如我修改为了 102400。

### 解决导入 Eclipse Maven 工程后无法读取 .xml 文件的问题

IDEA 与 Eclipse 配置文件目录的方式不同，可以将文件夹标记为 Sources、Resources 和 tests 等，而 src/main/java 默认被标记为 Sources，src/main/resources 才默认被标记为 Resources，编译时自动复制。

这样放在 src/main/java 目录下的文件与子文件夹均为 Sources，只将编译生成的 .class 文件复制到编译目录，在 Eclipse Maven 工程里放在 src/main/java 文件夹里的 xml、props 和 properties 文件就不会被拷贝到编译文件夹，导致执行时找不到这些文件，报类似下面这样的错误：

```
org.springframework.beans.factory.BeanDefinitionStoreException: IOException parsing XML document from class path resource [spring-demo.xml]; nested exception is java.io.FileNotFoundException: class path resource [spring-demo.xml] cannot be opened because it does not exist
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:343)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:303)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:180)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:216)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:187)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:251)
	at org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(AbstractXmlApplicationContext.java:127)
	at org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(AbstractXmlApplicationContext.java:93)
	at org.springframework.context.support.AbstractRefreshableApplicationContext.refreshBeanFactory(AbstractRefreshableApplicationContext.java:129)
	at org.springframework.context.support.AbstractApplicationContext.obtainFreshBeanFactory(AbstractApplicationContext.java:540)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:454)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:139)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:83)
	at org.mazhuang.demo.protocol.db.DemoContext.init(DemoContext.java:22)
	at org.mazhuang.demo.protocol.DemoServer.start(DemoServer.java:40)
	at org.mazhuang.demo.DemoSrv.main(DemoSrv.java:17)
Caused by: java.io.FileNotFoundException: class path resource [spring-demo.xml] cannot be opened because it does not exist
	at org.springframework.core.io.ClassPathResource.getInputStream(ClassPathResource.java:158)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:329)
	... 15 more
```

**解决方案：**

可以通过在 pom.xml 文件里添加 resources 配置来指定将哪些文件作为 resources 包含：

```xml
<build>
    <resources>
        <resource>
            <directory>${basedir}/src/main/java</directory>
            <includes>
                <include>**/*.props</include>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

### 如何导出 jar 包

File -> Project Structure -> Artifacts -> Click green plus sign -> Jav -> From modules with dependencies

Build -> Build Artifacts

### 去掉 UTF-8 文件的 BOM

UTF-8 文件带 BOM 里会编译报错：

```
Error:(1, 1) error: illegalcharacter: '\ufeff'
```

解决方法：

右键项目名称 -> Remove BOM

### 隐藏 Coverage 结果

Run - Show Coverage Data

### 编译项目时报 java.lang.OutOfMemoryError: GC overhead limit exceeded

```
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

修改 IDEA 的 Custom VM Options 并没有用。

这是因为运行 IDEA App，和编译的程序，使用不同的 JVM 进程。

此时需要修改编译配置：

Preferences - Build, Execution, Deployment - Compiler - Share build process heap size

默认是 700，我修改成了 4096，单位 Mbytes。

参考 <https://intellij-support.jetbrains.com/hc/en-us/community/posts/206166909-java-lang-OutOfMemoryError-GC-overhead-limit-exceeded>

### 更新时提示错误

```
IDEA does not have write access to /Application/IntelliJ IDEA.app/Contents. Please run it by a privileged user to update.
```

原因：最近切换过 MacBook 的用户，IDEA 是在老用户下安装的。

解决方法：

将 IntelliJ IDEA.app 目录的权限修改为当前用户。

```
sudo chown -R <current_user>:<user_group> "/Application/IntelliJ IDEA.app"
```

## 参考

- [解决 IntelliJ IDEA 无法读取配置\*.properties 文件的问题](http://www.cnblogs.com/zqr99/p/7642712.html)
- [How to build jars from IntelliJ properly?](https://stackoverflow.com/questions/1082580/how-to-build-jars-from-intellij-properly)
- [Can I adjust the “100+ matches in X files” from Intellijs “find in path”](https://stackoverflow.com/questions/44803519/can-i-adjust-the-100-matches-in-x-files-from-intellijs-find-in-path)

---

written by [曹斌](https://github.com/AaaBinfinity)
