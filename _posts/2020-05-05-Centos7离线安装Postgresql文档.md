---
layout: post
title: Centos7离线安装Postgresql文档
categories: centos7,postgresql
---

## 需求
1. 是在centos下离线安装postgresql11.4，要求可以外网连接，需要详细部署步骤
2. 要求访问端口改为9527
3. 客户端可以使用pgadmin来测试连接

## 目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224042433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

## 安装Postgre

### 复制安装包
请将`安装Postgre`文件夹放入指定目录下下，并进入指定目录
### 进入`安装Postgre`文件夹中

`cd 安装Postgre`

### 创建文件夹

`sudo mkdir -p /chinamobile/soft`

`sudo mkdir -p /chinamobile/work/postgresql`

### 解压`postgresql-11.4.tar`

`sudo tar xf postgresql-11.4.tar -C /chinamobile/soft`

### 进入`Postgre_rpm包`文件夹中

`cd Postgre_rpm包`

### 安装rpm软件包

`sudo rpm -Uvh *.rpm --nodeps --force`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224354647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

### 进入`postgresql-11.4`源码包目录中

`cd /chinamobile/soft/postgresql-11.4`

### 配置环境

`./configure --prefix=/chinamobile/work/postgresql`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224412660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

### 进行编译安装

`sudo make`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224433728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

`sudo make install`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224452871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

## 相关配置

### 创建用户

`sudo useradd postgres`

`sudo passwd postgres`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224508355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
### 设定权限

`sudo mkdir /chinamobile/work/postgresql/data`

`sudo mkdir /chinamobile/work/postgresql/log`

`sudo chown -R postgres:postgres /chinamobile/work/postgresql`

### 配置环境变量
编写profile

`sudo vim /etc/profile`

追加下面内容

```shell
export PGDATA=/chinamobile/work/postgresql/data
export PGHOME=/chinamobile/work/postgresql
export PGPORT=9527
export PATH=$PGHOME/bin:$PATH
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224558664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

生效配置

`source /etc/profile`

### 初始化数据库
切换到postgres用户

`sudo su - postgres`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224631882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)



初始化数据库

`initdb`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224648227.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

### 配置数据库

`cd /chinamobile/work/postgresql/data`

配置对数据库的访问控制（设置为可以通过密码访问）

`vim pg_hba.conf`

更改为
```
# IPv4 local connections:
# host    all             all             127.0.0.1/32            trust
host    all             all             0.0.0.0/0               trust
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080922470735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

配置数据库参数（设置服务器监听整个网络，设置端口号为9527)

`vim postgresql.conf`

更改为

`listen_addresses = '*'`

`port = 9527`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224730316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

退出postgres

`exit`

### 开放9527端口
新增开放9527端口号

`sudo firewall-cmd --zone=public --add-port=9527/tcp --permanent`

重启防火墙开机

`sudo systemctl restart firewalld.service`

### 配置系统服务
进入postgresql的解压目录

`cd /chinamobile/soft/postgresql-11.4`

`sudo cp contrib/start-scripts/linux /etc/init.d/postgresql`

`sudo vim /etc/init.d/postgresql`

更改为
```
# Installation prefix
prefix=/chinamobile/work/postgresql

# Data directory
PGDATA="/chinamobile/work/postgresql/data"

# Who to run the postmaster as, usually "postgres".  (NOT "root")
PGUSER=postgres

# Where to keep a log file
PGLOG="/chinamobile/work/postgresql/log/serverlog"
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080922475531.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

赋予文件执行权限

`sudo chmod +x /etc/init.d/postgresql`

设定服务开机自启

`sudo chkconfig --add postgresql`
<br>
### 启动及连接数据库
启动数据库服务

`service postgresql start`

### 命令行连接数据库

`sudo su - postgres`

`psql`
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080922481499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

### pgadmin连接数据库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809224829290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
