## 使用natapp实现Linux/Ubuntu上的内网穿透

### 1. 场景需求

服务器需要使用内网进行ssh连接，而我们在家里无法连接内网，因此无法通过SSH连接服务器，这个时候就可以使用内网穿透技术来间接访问和操控服务器。



![image-20250122030548004](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/image-20250122030548004.png)

### 2. 什么是内网穿透？

内网穿透简单来说就是将内网外网通过natapp隧道打通,让内网的数据让外网可以获取。比如常用的办公室软件等，一般在办公室或家里，通过拨号上网，这样办公软件只有在本地的局域网之内才能访问，那么问题来了，如果是手机上，或者公司外地的办公人员，如何访问到办公软件呢？这就需要natapp内网穿透工具了。运行natapp隧道之后，natapp会分配一个专属域名/端口,办公软件就已经在公网上了,在外地的办公人员可以在任何地方愉快的访问办公软件了

### 3. 内网穿透可以做什么?

1. 上文举例的办公软件
2. 放在家里的树莓派,服务器等,需要远程ssh管理,这样打通服务器的22端口即可远程通过ssh操作服务器了.
3. 微信/支付宝等本地开发.现在微信/支付宝等应用,需要服务器接收微信/支付宝发送的回调信息,然而在本地开发程序的话,还得实时上传到服务器,以便支持微信/支付宝的回调信息,如果使用了natapp内网穿透软件,将回调地址设置成natapp提供的地址,回调数据立即传递回本地,,这样很方便的在本地就可以实时调试程序,无须再不断上传服务器等繁琐且无意义的步骤.
4. 一些企业内部数据库,由于安全等原因,不愿意放到云服务器上,可以将数据库放到办公室本地,然后通过natapp的tcp隧道映射,这样既保证安全,又保证公网可以正常访问.
5. 一些开发板做的监控等信息,每台设备运行一条隧道,可以方便的管理监控各个设备的运行情况.
6. 一些本地运行的游戏,想和好基友一起联网玩,一条命令运行natapp即可实现联网游戏.
7. 群辉上运行natapp之后,随时随地在任何地方可以访问到群辉上应用

### 4. 内网穿透安全吗?

现在服务器被黑的情况,多半是服务器上一些软件/漏洞/端口导致的.你的应用如果放在公网服务器,由于缺少系统安全维护知识,会变得很危险.而用了natapp内网穿透软件之后,将服务器放在本地,暴露给公网的也仅仅是应用层面的一个端口,其他系统上的漏洞/端口都被隐藏起来.从这个层面来说,提高了很多安全性.

当然,你的应用本身带来的安全性,比如代码本身有漏洞,如果是映射数据库应用,数据库弱密码等,这需要引起重视,排查映射的应用本身安全性即可.

Natapp本身的隧道传输采用ssl256位加密,这种加密安全性现阶段完全无法破解,natapp隧道的安全性无需考虑

### 5. 使用教程

首先在natapp官网（https://natapp.cn/）购买TCP隧道并设置本地端口为22，以及设置我们SSH想要访问服务器的远程端口数，我这里以6565为例（只要不被使用即可）

![image-20250122031250827](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/image-20250122031250827.png)

会得到authtoken。然后下载系统对应的客户端。

![](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/image-20250122031154655.png)

将客户端放在用户有权限的位置处，然后新建一个文件"config.ini"，内部代码为

```ini
#将本文件放置于natapp同级目录 程序将读取 [default] 段
#在命令行参数模式如 natapp -authtoken=xxx 等相同参数将会覆盖掉此配置
#命令行参数 -config= 可以指定任意config.ini文件
[default]
authtoken=这里填写你刚刚购买得到的authtoken                      #对应一条隧道的authtoken
clienttoken=                    #对应客户端的clienttoken,将会忽略authtoken,若无请留空,注意参数输入不要有多余的空格!
log=none                        #log 日志文件,可指定本地文件, none=不做记录,stdout=直接屏幕输出 ,默认为none
loglevel=ERROR                  #日志等级 DEBUG, INFO, WARNING, ERROR 默认为 DEBUG
http_proxy=                     #代理设置 如 http://10.123.10.10:3128 非代理上网用户请务必留空
```

 在Linux/Mac 下 需要先给执行权限

```bash
chmod a+x natapp
```

 然后再运行

```bash
./natapp
```

运行成功可以得到这样的界面，其中

Tunnel Status  Online 代表链接成功
Version    当前客户端版本,如果有新版本,会有提示
Forwarding   当前穿透 网址 或者端口
Web Interface  是本地Web管理界面,可在隧道配置打开或关闭,仅用于web开发测试
Total Connections 总连接数

![image-20250122032222325](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/image-20250122032222325.png)

然后我们如果想要静默启动，

```bash
nohup ./natapp -authtoken=xxxx -log=stdout &
```

然后可以再开一个bash去检验是否运行

```bash
ps -ef|grep natapp
```

![image-20250122032626552](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/image-20250122032626552.png)

这样可以找到natapp进程的pid ,如果要结束进程,运行

```bash
kill -9 <natapp进程pid>
```

nohup 默认会在当前目录 创建 nohup.out 文件,会记录natapp运行日志,为避免日志过大,可以将日志等级降低 如

```bash
nohup ./natapp -authtoken=xxx -log=stdout -loglevel=ERROR &
```

然后就可以在SSH工具中进行连接了，域名就是tcp后面的内容，端口号就是我们设置好的远程端口。