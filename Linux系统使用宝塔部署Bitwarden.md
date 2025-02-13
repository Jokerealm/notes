## Linux系统使用宝塔部署Bitwarden

### 第一步 域名操作

去申请一个已经解析好的域名，这里以阿里云为例子

例如我的二级域名为jokerealm.top,想要申请一个三级域名pwd.jokerealm.top去托管密码，我在解析里面添加A记录，主机记录填pwd,记录值填写服务器的公网IP

![image-20241129105125012](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291051440.png)

添加成功后，如图所示

![](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291052439.png)

### 第二步 站点设置

我们进入宝塔后端的网站去添加站点，添加PHP项目

![image-20241129111507780](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291115913.png)域名填写我们想要托管的域名，如pwd.jokerealm.top，备注可以随便填，根目录就是我们的网站站点文件部署的位置，注意PHP版本要填静态

创建完成后，申请ssl证书并开启强制https

![image-20241129111928236](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291119303.png)

可以使用其他地方申请的SSL证书，如果没有的话，可以在Let's Encrypt里面申请免费的证书，如果申请失败，可以多申请几次

![image-20241129111956673](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291119767.png)

然后在当前证书里面进行部署证书，并开启强制HTTPS

![image-20241129112122571](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291121675.png)

### 第三步 Docker部署

进入宝塔服务器后端，安装Docker（已经安装过的可以直接使用）

![image-20241129110043926](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291100039.png)

在docker商店里面搜索下载vaultwarden

![image-20241129104641057](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202411291123775.png)

 安装完成后，选择”容器“-”创建容器“，填写名称，下拉选择vaultwarden镜像，添加80、3012两个端口映射，点击创建

![image-20241201013941024](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202412010139083.png)

创建完成后，点击容器名称进入管理，在编辑容器中选择网络，挂在本机目录，选择刚建立的站点目录

![image-20241201014009424](../AppData/Roaming/Typora/typora-user-images/image-20241201014009424.png)

### 第四步 NGINX反向代理

 回到站点管理，进入passwd.yourdomain.con站点设置，选择反向代理，添加反向代理，填写代理名称，目标URL中填写”[http://127.0.0.1:5555](http://127.0.0.1:5555/)（同上方自定义端口，如有修改，请保持一致，我上面自定义的是16210，这里就是16210）。

![image-20241201014117289](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202412010141333.png)

### 第五步 登陆服务端

登陆我们挂载的域名，如我创建的pwd.jokerealm.top，如出现下面的截图则证明部署成功

![image-20241201014433223](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202412010144274.png)

###  第六步 创建主账号

初次使用需创建账号，按照指引创建后登录即可，后期也可以在这里管理自己的密码，至此服务端建立完成。

![image-20241201014717771](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202412010147845.png)

### 第七步 使用

服务端部署完成后，就可以在各个平台中使用了，以浏览器插件为例，在谷歌浏览器中添加bitwarden插件，登录界面选择自托管，服务器地址填入https://pwd.jokerealm.top (刚刚你托管的域名)，用新建的账户登录，之后在使用过程中添加各个网站的密码后，自动填充了。我们使用的时候，还可以选择把浏览器的密码导出后直接导入到bitwarden里，这样我们就可以在多个平台的不同设备上进行密码管理了！

![image-20241201015332196](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202412010153236.png)

可以看到我们在访问浏览的时候就可以使用自动填充密码了

![image-20241201015503219](https://raw.githubusercontent.com/Jokerealm/MyPic/img/img/202412010155268.png)
