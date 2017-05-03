---
title: Hexo Next 主题配置
date: 2017-05-03 14:21:02
tags: hexo
---
### 自定义域名

#### 购买域名

阿里云省事，但是要记得不是所有域名都可以备案的，尤其是那些便宜的！！！

#### 域名解析
有了域名后，xxx.github.io 增加cname
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493360584432.png" width="873"/>
说明：192.30.252.154和192.30.252.153是github服务器对应的ip地址，这步一定要设置，否则访问不了

#### 添加CNAME

然后回到博客项目根目录，在source/下新建一个名为CNAME的文件，里面的内容写入xxxx.com即可。

### 配置搜索
NexT主题支持集成 Swiftype、 微搜索、Local Search 和 Algolia,Swiftype和Algolia都只有一段时间的试用期，可以采用Hexo提供的Local Search,原理是通过hexo-generator-search插件在本地生成一个search.xml文件，搜索的时候从这个文件中根据关键字检索出相应的链接。
安装 hexo-generator-search

在站点的根目录下执行以下命令：

```
#安装 hexo-generator-searchdb
$ npm install hexo-generator-search --save
$ npm install hexo-generator-searchdb --save

```

#### 启用搜索

编辑 站点配置文件，新增以下内容到任意位置：
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
#### 验证搜索

<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493345650335.png" width="873"/>
### 修改背景色
```
vim themes/next/source/css/_custom/custom.styl
//Custom styles.
body {
	background:rgb(250, 235, 215);
  #可以设置背景色或者背景图片
}
//首页文章阴影样式
.post {
    margin-top: 60px;
    margin-bottom: 60px;
    padding: 25px;
    -webkit-box-shadow: 0 0 14px rgba(202, 203, 203, .5);
    -moz-box-shadow: 0 0 14px rgba(202, 203, 204, .5);
}
```
### 开启动画效果

修改_config.yml,true为开启，false为关闭。
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493347316086.png" width="873"/>
但是有个问题，three_waves这个动画效果开启后，风扇就开始狂转，建议选择其它效果。

### 开启访问量统计

#### 申请帐号
[注册帐号](https://leancloud.cn/)
#### 创建应用
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493358213242.png" width="873"/>
#### 创建class
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493358279110.png" width="873"/>
#### 获取key
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493358329637.png" width="873"/>
#### 修改配置文件_confi.yml
```
leancloud_visitors:
  enable: true
  app_id: B1L35FebIXXX3MloNCxrCS-gzGzoHsz
  app_key: iQH7DjsXXXMB58gpevWMC
```
重新加载后就出来了，但是只能在外网访问的时候才会增加。
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493358386367.png" width="873"/>
按照Next官方手册给的LeanCloud教程
弄完以后，次数格式出现了xx:xx:xx的形式，当时的内心是崩溃的。Why?经多方的研究和询问以后发现是因为Next主题已经集成了LeanCloud，而我们只需要配置主题_config.yml即可，但是，坑爹的又出现了，把教程里面的配置过的东西删除后，次数就完全不显示了，再次经过多方的研究，解决了这个问题，所以在此记录。
### 增加评论功能

#### 获取key
登录[网易云跟帖](https://manage.gentie.163.com/) 获取你的 Product Key
<img src="http://7xp3xc.com1.z0.glb.clouddn.com/2016/1493360341707.png" width="873"/>
#### 编辑主题配置文件

编辑 gentie_productKey 字段，设置如下：
```
gentie_productKey: #your-gentie-product-key
```
请注意，您在云跟帖管理后台设置的域名必须跟您站点的域名一致



