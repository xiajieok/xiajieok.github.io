---
title: 全自动部署Hexo 到github
date: 2017-05-03 14:38:00
tags: hexo
---
有一天把hexo配置好了，但是每次更新后还有手动部署太麻烦。正好有个pm2的工具，完全可以代替我们做这些事。

#### 安装 pm2
```
npm install -g pm2
```
在博客source目录新建start.js

```
var process = require('child_process');
process.exec(' hexo g -d', function (error, stdout, stderr) {
    if (error !== null) {
            console.log('exec error: ' + error);
    }
 });
 ```
#### 新建watch.js
```
{
 "apps" : [{
 "name"       : "blog",
 "script"     : "./start.js",
 "exec_interpreter": "node",
 "exec_mode"  : "fork_mode",
 "watch"      : "_posts"
  }]
}
```
使用pm2命令实现监控文件变动自动提交
```
pm2 start watch.json
```
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1492760463930.png" width=873>
#### pm2常用命令
```
pm list
pm logs
pm start watch.json
pm show <id|name>
```
这样就会发现Hexo已经被自动部署到github啦，而且时刻保持最新版
