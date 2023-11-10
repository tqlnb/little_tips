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
