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

3. 启动服务

```linux
sudo systemctl daemon-reload //重新加载Systemd服务配置：
sudo systemctl enable my_java_app // 启用服务，以使其在系统启动时自动启动
sudo systemctl start my_java_app  // 启动服务
sudo systemctl status my_java_app // 运行状态,可以用这个看输出
sudo systemctl stop my_java_app // 关闭服务
```
