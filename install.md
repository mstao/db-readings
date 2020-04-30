# 安装

1. 安装wget

```
yum -y install wget
```

2. 下载mysql安装包

```
wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```

3. 使用yum安装

```
yum -y install mysql57-community-release-el7-10.noarch.rpm
```

4. 安装mysql服务器

```
yum -y install mysql-community-server
```

# 配置

**启动mysql**

```
systemctl start  mysqld.service
```

**查看运行状态**

```
systemctl status mysqld.service
```

**编辑配置文件**

查找配置文件

```
mysql --help | grep my.cnf
```

打开配置文件

```
vim /etc/my.cnf
```

**设置登录密码**


```
update mysql.user set authentication_string=password('123456') where user='root';
```

**开放3306端口**

```
firewall-cmd --zone=public --add-port=3306/tcp --permanent

firewall-cmd --reload
```

