---
layout: post
---

# 解决SSH连接时出现Host key verification failed

## 原因

当使用OpenSSH后，计算机会把公钥(public key)记录在`~/.ssh/known_hosts`中。当再一次访问时，OpenSSH会核对公钥，如果不相同OpenSSH就会发出警告，避免受到攻击。

SSH对public_key的检查等级是根据StrictHostKeyChecking变量来配置的，默认StrictHostKeyChecking为ask。

## 解决办法

### 更改StrictHostKeyChecking为no

`vi /etc/ssh/ssh_config`

将StrictHostKeyChecking更改为no

### 删除IP对应的rsa

`vi ~/.ssh/known_hosts`

### 移除known_hosts文件

`rm known_hosts`
