---
layout: post
---

## 需求
1. 在centos7.6下离线安装Python，需要详细部署步骤
2. Python版本3.7.3

## 目录结构
![Python离线安装目录结构](https://img-blog.csdnimg.cn/20190809225136105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
## 安装Python3.7.3
### 复制安装包
请将`安装Python`文件夹放入指定目录下下，并进入指定目录

### 进入`安装Python`文件夹中

`cd 安装Python`

### 解压`Python-3.7.3.tar.xz`

`tar xf Python-3.7.3.tar.xz`

### 进入`Python_rpm包`文件夹中

`cd Python_rpm包`

### 批量安装rpm软件包

`sudo rpm -Uvh *.rpm --nodeps --force`
![离线安装Python3环境](https://img-blog.csdnimg.cn/20190809225155280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
### 返回上级目录

`cd ..`

### 进入`Python-3.7.3`中

`cd Python-3.7.3`

### 配置环境

`./configure --enable-optimizations --prefix=/usr`
![配置编译环境](https://img-blog.csdnimg.cn/20190809225208672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
### 编译安装第一种

`sudo make altinstall`
![编译安装make altinstall](https://img-blog.csdnimg.cn/20190809225238160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

`sudo ln -s /usr/bin/python3.7 /usr/bin/python3`

`sudo ln -s /usr/bin/pip3.7 /usr/bin/pip3`
![建立Python3软连接](https://img-blog.csdnimg.cn/20190809225252877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
### 编译安装第二种（自动构建软连接）

`sudo make`
![编译安装make](https://img-blog.csdnimg.cn/20190809225308340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)

`sudo make install`
![编译安装make install](https://img-blog.csdnimg.cn/20190809225321937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
### Python和pip启动

`python3`

`pip3 -V`
![结束启动](https://img-blog.csdnimg.cn/20190809225334574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY1NzE1OA==,size_16,color_FFFFFF,t_70)
