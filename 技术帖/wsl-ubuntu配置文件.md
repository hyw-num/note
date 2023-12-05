### ubuntu配置文件

> user:hyw
>
> password:akb48yuko

#### ssh 配置

```
sudo apt-get update
sudo apt-get install openssh-server -y

sudo ps -e|grep ssh #查看是否启动
sudo service ssh start

#配置ssh service 允许远程root登录
sudo vi /etc/ssh/sshd_config
#注释掉PermitRootLogin without-password
#添加PermitRootLogin yes

#检查服务器状态
service ssh status

#允许ssh绕过防火墙
sudo ufw allow ssh
```



#### 更换源

在目录$ \text{/etc/apt/source.list} $下

建议备份

```
/etc/apt/source.list.bak #原有的源
/etc/apt/source.list.aliyun
/etc/apt/source.list.zhongkeda
/etc/apt/source.list.qinghua

cp /etc/apt/source.list.aliyun /etc/apt/source.list#复制一下

#更新
sudo apt update
```

#### win wsl 互通文件

```
#访问c盘
cd /mnt/c
移动文件 mv path1 ~

```

#### frp配置

```
tar -zxvf  frp****
mv frp**** frp
cd frp
ls -a
vi frps.ini

./frps -c frps.ini
#成功即出现以下信息
2023/03/12 10:47:55 [I] [root.go:206] frps uses config file: frps.ini
2023/03/12 10:47:55 [I] [service.go:200] frps tcp listen on 0.0.0.0:7000
2023/03/12 10:47:55 [I] [service.go:261] http service listen on 0.0.0.0:10080
2023/03/12 10:47:55 [I] [service.go:276] https service listen on 0.0.0.0:10443
2023/03/12 10:47:55 [I] [service.go:317] Dashboard listen on 0.0.0.0:7500
2023/03/12 10:47:55 [I] [root.go:215] frps started successfully
```

```
[common]
bind_port = 7000
dashboard_port = 7500
token = 12345678
dashboard_user = admin
dashboard_pwd = admin
vhost_http_port = 10080
vhost_https_port = 10443
如果没有必要，端口均可使用默认值，token、user和password项请自行设置。

“bind_port”表示用于客户端和服务端连接的端口，这个端口号我们之后在配置客户端的时候要用到。
“dashboard_port”是服务端仪表板的端口，若使用7500端口，在配置完成服务启动后可以通过浏览器访问 x.x.x.x:7500 （其中x.x.x.x为VPS的IP）查看frp服务运行信息。
“token”是用于客户端和服务端连接的口令，请自行设置并记录，稍后会用到。
“dashboard_user”和“dashboard_pwd”表示打开仪表板页面登录的用户名和密码，自行设置即可。
“vhost_http_port”和“vhost_https_port”用于反向代理HTTP主机时使用，本文不涉及HTTP协议，因而照抄或者删除这两条均可。
#
```

#### wsl 端口转发

```
ifconfig

#查看ip地址
ifconfig

#将端口转发到wsl，在Power Shell下执行命令，将[IP]和[PORT]替换为wsl的IP和端口。
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=2222 connectaddress=172.30.166.176 connectport=2222
# 查看所有的转发规则
netsh interface portproxy show all

#开启防火墙入站规则（也可以在控制面板-Windows Defender 防火墙-高级设置-入站规则中设置）
netsh advfirewall firewall add rule name=WSL2 dir=in action=allow protocol=TCP localport=2222

##edit/etc/ssh/sshd_config
Port 2222
ListenAddress 0.0.0.0
PasswordAuthentication yes
PermitRootLogin yes
```

https://embracethered.com/blog/posts/2020/windows-port-forward/

#### 编辑wsl开机自启动项

1. 编辑`/etc/init.wsl`文件，添加`service docker start`

2. 打开Windows注册表，在`计算机\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`下添加一个**REG_SZ** 类型的值，名称可自定义，值为`mshta vbscript:CreateObject("WScript.Shell").Run("wsl -d Ubuntu-20.04 -u root bash /etc/init.wsl",0,TRUE)(window.close)`