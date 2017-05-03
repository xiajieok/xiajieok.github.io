---
title: 树莓派编译安装Python3.5
date: 2017-05-03 14:49:00
tags: Python
---
刚写了一个天气预报的脚本，想部署在服务器上，但是运行的时候报错，说找不到requests库，奇怪的是装上了也报错。后来发现树莓派自带的是3.2，版本有点老可能会有点不兼容吧。
那就装个Python3.5吧！

#### 环境依赖

```
sudo apt-get install libssl-dev
sudo apt-get install openssl

sudo wget https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
tar -xvf Python-3.5.3.tar.xz
cd Python-3.5.3
./configure --prefix=/usr/local/python3.5
make
make altinstall
```
进入/usr/local/python3.5 检查是否完整
<img src='http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493107147483.png' width=873>
#### 建立软链
```
sudo ln -s /usr/local/python3.5/bin/python3.5 /usr/bin/python3
sudo ln -s /usr/local/python3.5/bin/pip3 /usr/bin/pip3
```
检查版本。
安装完成！！！
### 错误整理

1、 提示Ignoring ensurepip failure: pip 8.1.1 requires SSL/TLS
<img src='http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493103640349.png' width=873>
错误:
	没有安装openssl-devel
解决：
```
sudo apt-get install libssl-dev
sudo apt-get install openss
```
2、libssl-dev无法安装
```
The following packages have unmet dependencies:
 libssl-dev : Depends: libssl1.0.0 (= 1.0.1-4ubuntu5) but 1.0.1-4ubuntu5.3 is to be installed
              Recommends: libssl-doc but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```
原因： 已安装的libssl1.0.0版本太高, 无法支持
解决： 参考 [这里](http://blog.csdn.net/andy812110/article/details/24842219)


