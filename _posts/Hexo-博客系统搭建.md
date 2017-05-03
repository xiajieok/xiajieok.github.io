---
title: Hexo 博客系统搭建
date: 2017-05-03 14:41:49
tags: hexo
---
### 初识Hexo

Hexo是一个快速的、简单的、功能强大的博客框架。你可以通过Markdown语言写文章，然后hexo帮你生成一个带有漂亮主题的静态页面。
最近入手了一个树莓派，所以有事没事就想着折腾它，后来无意中看到了hexo的博客，真漂亮！就试着在派上部署一下环境试试。

### 安装

#### 依赖环境
hexo依赖于nodejs，因此记得先装一下nodejs。有些平台上就编译一下吧，比如在树莓派上就得好几个小时，那就在那编译着吧。。。
编译安装nodejs
```
wget http://nodejs.org/dist/latest-v6.x/node-v6.9.1.tar.gz   
tar -zxvf node-v6.9.1.tar.gz   
cd node-v6.9.1   
./configure   
make   
make install   
node -v
```
安装完成后安装hexo
Node, npm和Git都安装成功, 开始安装hexo

```
npm install hexo -g
#-g表示全局安装, npm默认为当前项目安装
```
#### 安装组件
```

npm install hexo-deployer-git --save
#hexo 使用git
```
#### 检查版本
```
Jack:~ jack$ hexo version
hexo-cli: 1.0.2
os: Darwin 15.4.0 darwin x64
http_parser: 2.6.2
node: 5.10.1
v8: 4.6.85.31
uv: 1.8.0
zlib: 1.2.8
ares: 1.10.1-DEV
icu: 56.1
modules: 47
openssl: 1.0.2g
```

#### Hexo初始化
```
hexo init hexo  
#执行init命令初始化到你指定的hexo目录
初始化后目录结构
-rw-r--r--    1 jack  staff  1483  4 21 10:19 _config.yml
drwxr-xr-x  288 jack  staff  9792  4 21 10:24 node_modules/
-rw-r--r--    1 jack  staff   443  4 21 10:19 package.json
drwxr-xr-x    5 jack  staff   170  4 21 10:19 scaffolds/
drwxr-xr-x    3 jack  staff   102  4 21 10:19 source/
drwxr-xr-x    3 jack  staff   102  4 21 10:19 themes/

npm install    #install before start blogging
hexo generate       #自动根据当前目录下文件,生成静态网页
hexo server         #运行本地服务
```
浏览器输入[http://localhost:4000](http://localhost:4000)就可以看到效果。


### 配置github
<img src='http://7xp3xc.com1.z0.glb.clouddn.com/2016/1492755503227.png' width=873>
### 配置域名
repo->settings-> 检查是否和配置文件一致

### 部署
```
hexo depoly
```
访问
如果出现404，检查配置文件和域名是否一致。
http://xiajieok.github.io
### 常见错误

如果出现ERROR Deployer not found: git
安装
```
npm install hexo-deployer-git --save
```
– end
