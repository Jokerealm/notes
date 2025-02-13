# Windows上配置wsl使用本机代理

我们在使用wsl的时候有时候想要获取一些资源，例如访问谷歌，wsl2是默认不会和主机使用同样的网络配置的，需要我们手动配置，以下是解决步骤。

### 配置 .wslconfig 文件

WSL 有 2 个重要的配置文件 .wslconfig 和 wsl.conf，其中 .wslconfig 位于 Windows 系统上，wsl.conf 位于 Linux 发行版系统上。首先介绍配置 .wslconfig。

打开 .wslconfig 文件目录：Win+R 运行 %UserProfile% ，会打开用户目录，.wslconfig 中添加以下内容(如果不存在可以手动创建)。.wslconfig 配置可以参考如下内容：

```ini
[wsl2]
networkingMode = mirrored
autoProxy = true
```

配置说明：

- networkingMode = mirrored：启用镜像网络模式。

- autoProxy = true：强制 WSL 使用 Windows 的 HTTP 代理信息。

然后重启WSL2即可。

重启命令：

```powershell
wsl --shutdown
wsl
```

这个时候就已经配置完成了，你可以输入以下命令，查看是否能正常访问谷歌

```powershell
wget www.google.com
```

