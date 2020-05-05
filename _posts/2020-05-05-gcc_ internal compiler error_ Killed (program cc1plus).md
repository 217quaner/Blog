# gcc: internal compiler error: Killed (program cc1plus)

## 问题

gcc: internal compiler error: Killed (program cc1plus)

## 原因

内存不足

## 办法1

因为我是在虚拟机上面使用的这个，所以在此创建虚拟机的时候可以把内存给的足一点。

## 办法2

在linux下增加临时swap空间

`sudo dd if=/dev/zero of=/home/swap bs=64M count=16`

注释：of=/home/swap,放置swap的空间; count的大小就是增加的swap空间的大小，64M就是块大小，这里是64MB，所以总共空间就是bs*count=1024MB.这里分配空间的时候需要一点时间，等待执行完毕。

`sudo mkswap /home/swap`

(可能会提示warning: don't erase bootbits sectorson whole disk. Use -f to force，不用理会)
注释：把刚才空间格式化成swap各式

`sudo swapon /home/swap`

注释：使刚才创建的swap空间
