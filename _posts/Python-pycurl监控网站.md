---
title: Python+pycurl监控网站
date: 2017-05-03 14:53:26
tags: [Python,树莓派]
---
### 前言

打算写个监控网站状态的小脚本，用到pycurl，但是提示找不到curl-config，那就手动安装吧。
Pycurl包是一个libcurl的Python接口，它是由C语言编写的。与urllib相比，pycurl的速度要快很多。

Libcurl 是一个支持FTP，FTPS，HTTP，HTTPS，GOPHER，TELNET，DICT，FILE 和 LDAP的客户端URL传输库。libcurl也支持HTTPS认证，HTTP、POST、HTTP PUT、FTP上传，代理，Cookies，基本身份验证，FTP文件断点继传，HTTP代理通道等等。



#### 安装
```
cd src
sudo wget http://curl.haxx.se/download/curl-7.24.0.tar.gz
tar -zxvf curl-7.24.0.tar.gz
cd curl-7.24.0
./configure
make 
make install
```
检查curl-config是否存在，如果存在则继续安装，pip3 或者下载安装

```
cd ..
sudo wget http://pycurl.sourceforge.net/download/pycurl-7.19.0.tar.gz
tar -zxvf pycurl-7.19.0.tar.gz
cd pycurl-7.19.0
python3 setup.py install --curl-config=/usr/local/bin/curl-config
```
#### 基本用法

```
c = pycurl.Curl()    #创建一个curl对象 
c.setopt(pycurl.CONNECTTIMEOUT, 5)    #连接的等待时间，设置为0则不等待  
c.setopt(pycurl.TIMEOUT, 5)           #请求超时时间  
c.setopt(pycurl.NOPROGRESS, 0)        #是否屏蔽下载进度条，非0则屏蔽  
c.setopt(pycurl.MAXREDIRS, 5)         #指定HTTP重定向的最大数  
c.setopt(pycurl.FORBID_REUSE, 1)      #完成交互后强制断开连接，不重用  
c.setopt(pycurl.FRESH_CONNECT,1)      #强制获取新的连接，即替代缓存中的连接  
c.setopt(pycurl.DNS_CACHE_TIMEOUT,60) #设置保存DNS信息的时间，默认为120秒  
c.setopt(pycurl.URL,"http://www.baidu.com")      #指定请求的URL  
c.setopt(pycurl.USERAGENT,"Mozilla/5.2 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50324)")    #配置请求HTTP头的User-Agent
c.setopt(pycurl.HEADERFUNCTION, getheader)   #将返回的HTTP HEADER定向到回调函数getheader
c.setopt(pycurl.WRITEFUNCTION, getbody)      #将返回的内容定向到回调函数getbody
c.setopt(pycurl.WRITEHEADER, fileobj)        #将返回的HTTP HEADER定向到fileobj文件对象
c.setopt(pycurl.WRITEDATA, fileobj)          #将返回的HTML内容定向到fileobj文件对象
c.getinfo(pycurl.HTTP_CODE)         #返回的HTTP状态码
c.getinfo(pycurl.TOTAL_TIME)        #传输结束所消耗的总时间
c.getinfo(pycurl.NAMELOOKUP_TIME)   #DNS解析所消耗的时间
c.getinfo(pycurl.CONNECT_TIME)      #建立连接所消耗的时间
c.getinfo(pycurl.PRETRANSFER_TIME)  #从建立连接到准备传输所消耗的时间
c.getinfo(pycurl.STARTTRANSFER_TIME)    #从建立连接到传输开始消耗的时间
c.getinfo(pycurl.REDIRECT_TIME)     #重定向所消耗的时间
c.getinfo(pycurl.SIZE_UPLOAD)       #上传数据包大小
c.getinfo(pycurl.SIZE_DOWNLOAD)     #下载数据包大小 
c.getinfo(pycurl.SPEED_DOWNLOAD)    #平均下载速度
c.getinfo(pycurl.SPEED_UPLOAD)      #平均上传速度
c.getinfo(pycurl.HEADER_SIZE)       #HTTP头部大小
```
#### 代码示例

```
import pycurl
import io
url='www.baidu.com'
c=pycurl.Curl()
c.setopt(c.URL, url)
b = io.StringIO()
c.setopt(c.WRITEFUNCTION, b.write)
#c.setopt(c.FOLLOWLOCATION, 1)
#c.setopt(c.HEADER, True) 
c.perform()   
print(b.getvalue())   
b.close()
c.close()
```
#### 代码实现

```
import pycurl
import io
import wechat_sms
import time
def check_status(url):
    '''
    检查网站状态
    :param url:
    :return: 异常信息,正常则不返回
    '''
    c = pycurl.Curl()
    #pycurl 实例
    c.setopt(pycurl.URL, url)
    c.setopt(pycurl.TIMEOUT,10)
    #超时时间
    b = io.BytesIO()
    c.setopt(pycurl.WRITEFUNCTION, b.write)
    c.perform()
    code = c.getinfo(pycurl.HTTP_CODE)
    #状态码
    total_time = '%.2f'%c.getinfo(pycurl.TOTAL_TIME)
    #总时间
    dns = '%.2f'%c.getinfo(pycurl.NAMELOOKUP_TIME)
    #DNS解析消耗
    connet_time = '%.2f'%c.getinfo(pycurl.CONNECT_TIME)
    #建立连接时间
    PRETRANSFER_TIME = '%.2f'%c.getinfo(pycurl.PRETRANSFER_TIME)
    #建立连接到准备传输数据时间
    STARTTRANSFER_TIME = '%.2f'%c.getinfo(pycurl.STARTTRANSFER_TIME)
    #建立连接到传输开始消耗的时间
    download = c.getinfo(pycurl.SPEED_DOWNLOAD)
    #平均下载速度
    info = ('HTTP   Code: %s\nTotal time: %s ms\nDNS lookup time: %s ms\nCONNECT_TIME:  %s ms\nPRETRANSFER_TIME: %s ms\nSTARTTRANSFER_TIME: %s ms\nDownload Speed: %s byte/s' % (code, total_time, dns,connet_time,PRETRANSFER_TIME,STARTTRANSFER_TIME,download))
    print(code)
    if code > 400 and connet_time > 10:
        #如果状态码大于400,或者消耗时间大于10秒,说明网站有问题,发送错误信息;否则不返回
        return info
    else:
        return True
def send_sms(url):
    '''
    发送信息
    :param url:
    :return:
    '''
    msm = check_status(url)
    project = '网站状态'
    #如果网站状态返回为True,则跳出,否则返回具体信息
    while msm is True:
        print(msm)
        break
    else:
        print(msm)
        WeChat.sendMessage('medivh', project, msm)
if __name__ == '__main__':
    WeChat = wechat_sms.WeChat('https://qyapi.weixin.qq.com/cgi-bin')
    #自定义网站列表,循环执行对网站的检测
    url_list = ['http://wechat.xxx.com','http://www.xxx.com','http://xxx.com']
    while True:
        for i in url_list:
            msm = send_sms(i)
        # 30秒检查一次
        time.sleep(30)
```