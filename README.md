# little_tips
小技巧

<hr>

## linux 配置自启动

1. 先创建一个service文件

```linux
sudo vim /etc/systemd/system/my_java_app.service
```

2.  配置service文件

```linux
[Unit]
Description=My Java Application

[Service]
ExecStart=/usr/bin/java -jar /path/to/daemon.jar
WorkingDirectory=/path/to/application/directory
Environment="PATH=/home/root/bin/jdk/bin:$PATH"
Restart=always
User=your_username

[Install]
WantedBy=multi-user.target
```

 - 需要配置
     - Description: 描述
     - ExecStart :后面接命令
     - WorkingDirectory : 工作目录
     - Restart=always :开机自启动
     - User : 用户名(如root)
     - Environment 如果你的应用是以 service 形式运行的，而不是通过交互式终端登录，那么你可能需要编辑与你的服务相关的启动脚本或配置文件，以确保正确设置了 PATH 环境变量。

3. 启动服务

```linux
sudo systemctl daemon-reload //重新加载Systemd服务配置：
sudo systemctl enable my_java_app // 启用服务，以使其在系统启动时自动启动
sudo systemctl start my_java_app  // 启动服务
sudo systemctl status my_java_app // 运行状态,可以用这个看输出
sudo systemctl status -n 100 my_java_app // 运行状态,可以用这个看输出，一次100行（实测可以输出更多行）
sudo systemctl stop my_java_app // 关闭服务
```

## 查看正在运行的java程序

要查看正在运行的Java程序，您可以使用jps（Java Virtual Machine Process Status Tool）命令，它通常随Java开发工具包（JDK）一起安装。jps命令显示正在运行的Java进程的列表以及它们的进程ID（PID）和类名。

在终端中，只需键入以下命令以列出正在运行的Java程序：

```
jps
```

jps命令将显示正在运行的Java进程的列表，例如：

```
12345 Jps
6789 MyJavaApplication
```

上面的示例显示了两个Java进程。第一列是PID，第二列是Java程序的类名或主类名。您可以根据需要查看正在运行的Java程序的信息，以便了解其状态和PID。

如果您想查看特定Java进程的详细信息，可以使用jstack或jinfo等其他Java工具。这些工具通常随JDK一起安装，可以用于诊断和监视Java应用程序的性能和运行时状态。例如，要获取特定Java进程的线程堆栈信息，可以使用jstack命令：

```
jstack PID
```

将上面的命令中的PID替换为您要查看的Java进程的PID。这将输出有关Java进程中运行的线程的详细信息。

请注意，您需要在具有足够权限的环境中运行这些命令，以便访问正在运行的Java进程的信息。

## 远程调试



![image](https://github.com/tqlnb/little_tips/assets/88382462/54df9d1f-e596-4e9c-980f-cfee4daeb639)

[简单的VsCode + gdb + gdbserver远程调试C++程序](https://blog.csdn.net/u014552102/article/details/122793256)

## maven移植

把项目包含对应的依赖一起移植

pom.xml：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>ins.daemon</groupId>
  <artifactId>daemon</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>daemon</name>
  <url>http://maven.apache.org</url>

  <properties>
    <!-- 设置编译等级和目标等级 -->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>

    <!-- Log4j Core -->
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-core</artifactId>
      <version>2.14.1</version> <!-- Use the latest version available -->
    </dependency>

    <!-- SLF4J Log4j 2 绑定 -->
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-slf4j-impl</artifactId>
      <version>2.14.1</version> <!-- 使用你当前 Log4j 2 版本 -->
    </dependency>

    <!-- https://mvnrepository.com/artifact/cn.hutool/hutool-all -->
    <dependency>
      <groupId>cn.hutool</groupId>
      <artifactId>hutool-all</artifactId>
      <version>5.8.24</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.json/json -->
    <dependency>
      <groupId>org.json</groupId>
      <artifactId>json</artifactId>
      <version>20231013</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.grpc/grpc-core -->
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-core</artifactId>
      <version>1.60.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.grpc/grpc-stub -->
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-stub</artifactId>
      <version>1.60.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.grpc/grpc-protobuf -->
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-protobuf</artifactId>
      <version>1.60.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.grpc/grpc-api -->
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-api</artifactId>
      <version>1.60.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.grpc/grpc-netty -->
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-netty</artifactId>
      <version>1.60.1</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-assembly-plugin -->
    <dependency>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-assembly-plugin</artifactId>
      <version>3.6.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-compiler-plugin -->
    <dependency>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.12.1</version>
    </dependency>

    <dependency>
      <groupId>com.siteraid.instr.visa</groupId>
      <artifactId>InsVisaGrpc</artifactId>
      <version>1.4-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/lib/InsVisaGrpc-1.4-SNAPSHOT.jar</systemPath>
    </dependency>

    <dependency>
      <groupId>src</groupId>
      <artifactId>GrpcPlugin</artifactId>
      <version>0.0.4-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/lib/GrpcPlugin-0.0.4-SNAPSHOT.jar</systemPath>
    </dependency>

  </dependencies>

  <build>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.12.1</version>
        <inherited>true</inherited>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <compilerArguments>
            <extdirs>${project.basedir}/lib</extdirs>
            <extdirs>${project.basedir}/log4j2.xml</extdirs>
          </compilerArguments>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
            <configuration>
              <archive>
                <manifest>
                  <mainClass>
                    ins.daemon.app.Daemon
                  </mainClass>
                </manifest>
                <manifestEntries>
                  <Class-Path>lib/*.jar</Class-Path>
                </manifestEntries>
              </archive>
              <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
              <descriptors>
                <!--assembly配置文件路径，注意需要在项目中新建文件assembly/release.xml-->
                <descriptor>${basedir}/assembly/release.xml</descriptor>
              </descriptors>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
      <resource>
        <directory>assembly</directory>
        <targetPath>${project.build.outputDirectory}/assembly</targetPath>
        <includes>
          <include>release.xml</include>
        </includes>
      </resource>
      <resource>
        <directory>lib</directory>
        <targetPath>${project.build.outputDirectory}/lib</targetPath>
        <includes>
          <include>InsVisaGrpc-1.4-SNAPSHOT.jar</include>
        </includes>
      </resource>
    </resources>
  </build>

</project>

```

编译等级和目标等级

```xml
<properties>
    <!-- 设置编译等级和目标等级 -->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
```

log4j:

```xml
 <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>

    <!-- Log4j Core -->
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-core</artifactId>
      <version>2.14.1</version> <!-- Use the latest version available -->
    </dependency>

    <!-- SLF4J Log4j 2 绑定 -->
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-slf4j-impl</artifactId>
      <version>2.14.1</version> <!-- 使用你当前 Log4j 2 版本 -->
    </dependency>
```

**自己的jar包依赖，如果不使用私服仓库管理：**

先把包放到lib文件夹：

![image](https://github.com/tqlnb/little_tips/assets/88382462/283aca0e-eddf-4174-8a99-fbbce5a1971c)

再在pom.xml添加依赖

```xml
 <dependency>
      <groupId>com.siteraid.instr.visa</groupId>
      <artifactId>InsVisaGrpc</artifactId>
      <version>1.4-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/lib/InsVisaGrpc-1.4-SNAPSHOT.jar</systemPath>
    </dependency>

    <dependency>
      <groupId>src</groupId>
      <artifactId>GrpcPlugin</artifactId>
      <version>0.0.4-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/lib/GrpcPlugin-0.0.4-SNAPSHOT.jar</systemPath>
    </dependency>
```

构建和打包

使用assembly协助打包，可能需要指定mainClass，lib jar包依赖,assembly配置文件 <descriptor>${basedir}/assembly/release.xml</descriptor>
                

```xml
      <manifestEntries>
        <Class-Path>lib/*.jar</Class-Path>
      </manifestEntries>
```


```xml
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.12.1</version>
        <inherited>true</inherited>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <compilerArguments>
            <extdirs>${project.basedir}/lib</extdirs>
            <extdirs>${project.basedir}/log4j2.xml</extdirs>
          </compilerArguments>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
            <configuration>
              <archive>
                <manifest>
                  <mainClass>
                    ins.daemon.app.Daemon
                  </mainClass>
                </manifest>
                <manifestEntries>
                  <Class-Path>lib/*.jar</Class-Path>
                </manifestEntries>
              </archive>
              <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
              <descriptors>
                <!--assembly配置文件路径，注意需要在项目中新建文件assembly/release.xml-->
                <descriptor>${basedir}/assembly/release.xml</descriptor>
              </descriptors>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
      <resource>
        <directory>assembly</directory>
        <targetPath>${project.build.outputDirectory}/assembly</targetPath>
        <includes>
          <include>release.xml</include>
        </includes>
      </resource>
      <resource>
        <directory>lib</directory>
        <targetPath>${project.build.outputDirectory}/lib</targetPath>
        <includes>
          <include>InsVisaGrpc-1.4-SNAPSHOT.jar</include>
        </includes>
      </resource>
    </resources>
  </build>
```

assembly配置文件：

![image](https://github.com/tqlnb/little_tips/assets/88382462/1799651c-e399-4b73-8e2d-8d17831c25f8)

```xml
<assembly xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0 http://maven.apache.org/xsd/assembly-1.1.0.xsd">
    <id>release</id>
    <formats>
        <format>jar</format>
    </formats>
    <includeBaseDirectory>false</includeBaseDirectory>
    <dependencySets>
        <dependencySet>
            <outputDirectory>/</outputDirectory>
            <useProjectArtifact>true</useProjectArtifact>
            <unpack>true</unpack>
            <scope>runtime</scope>
        </dependencySet>
        <dependencySet>
            <outputDirectory>/</outputDirectory>
            <useProjectArtifact>true</useProjectArtifact>
            <unpack>true</unpack>
            <scope>system</scope>
        </dependencySet>
    </dependencySets>
</assembly>
```

log4j2.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration status="error">
    <!--优先级从高到低依次为：OFF、FATAL、ERROR、WARN、INFO、DEBUG、TRACE、ALL -->
    <!--先定义所有的appender -->
    <appenders>
        <!--这个输出控制台的配置 -->
        <Console name="Console" target="SYSTEM_OUT">
            <!--控制台只输出level及以上级别的信息（onMatch），其他的直接拒绝（onMismatch） -->
            <ThresholdFilter level="info" onMatch="ACCEPT"
                onMismatch="DENY" />
            <!--这个都知道是输出日志的格式 -->
            <PatternLayout
                pattern="%d{%d{yyyy-MM-dd HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n" />
        </Console>
        <!--这个会打印出所有的信息，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档 -->
        <RollingFile name="RollingFile" fileName="logs/app.log"
            filePattern="log/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
            <PatternLayout
                pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n" />
            <SizeBasedTriggeringPolicy size="2MB" />
        </RollingFile>
    </appenders>
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效 -->
    <loggers>
        <!--建立一个默认的root的logger -->
        <root level="trace">
            <appender-ref ref="RollingFile" />
            <appender-ref ref="Console" />
        </root>
    </loggers>
</configuration> 
```



