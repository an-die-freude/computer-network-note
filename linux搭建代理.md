### 1 tinyproxy搭建HTTP/HTTPS代理

#### 1.1 ubuntu安装tinyproxy

更新系统

```shell
sudo apt-get update
sudo apt-get upgrade -y
```

安装tinyproxy

```shell
sudo apt-get -y install tinyproxy
```

#### 1.2 定义配置文件

```shell
sudo vim /etc/tinyproxy/tinyproxy.conf
```

定位到`Allow 127.0.0.1`可以指定ip，或者直接注释

定位到`#BasicAuth`可以指定账号和密码

#### 1.3 开放端口

```shell
firewall-cmd --zone=public --add-port=8888/tcp
firewall-cmd --zone=public --add-port=8888/tcp --permanent
```



