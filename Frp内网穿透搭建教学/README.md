
# 📌 FRP - Linux & Win 内网穿透教程 手搓难度 ⭐️⭐️
---
> **🚀 适用于：** 本地服务器、电脑、树莓派、香橙派内网穿透
> **🛠️ 工具**：FRP（fast reverse proxy）  
> **🖥️ 系统**：Linux、Windows
> **📚架构**：x86、amd、arm
>**📝Frp版本**：v0.61.1
>**🎯教程日期**：2025/2/12

---

## 📖 目录
1. [🌍 什么是 FRP？](#-什么是-frp)
2. [⚡ 安装与配置](#-安装与配置)
3. [🎯 服务器端配置](#-服务器端配置)
4. [🏠 客户端配置](#-客户端配置)
5. [📝 测试与验证](#-测试与验证)
6. [📌 结语](#-结语)

---

## 🌍 什么是 FRP？
**FRP**（Fast Reverse Proxy）是一款可以用于**内网穿透**的开源工具，
支持 **TCP/UDP/HTTP/HTTPS 协议**，可以将 **内网服务** 暴露到 **公网**
**实现**从任意网络环境访问到你的服务器或电脑。

🔗 **Frp官网**：[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

---

## ⚡ 安装与配置

>**❗️开始之前请确保**
>**❗️一个有公网IP的服务器**
>**❗️你需要实现内网穿透的服务器或电脑**

>如果你不知道这些都是什么，就请去查询了解一下，不然这个教程你看不懂 Like this -> 0.o

### 🔹 1. 下载 FRP
前往 [GitHub FRP Releases](https://github.com/fatedier/frp/releases) 下载适合你系统的版本：
> **💡提示**：
> 你看到的文件格式是 frp_0.61.1_xxx(适用系统)_yyy(系统架构).tar.gz
> Windows系统下载 [frp_0.61.1_windows_amd64.zip](https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_windows_amd64.zip)
> Linux系统amd架构下载 [frp_0.61.1_linux_amd64.tar.gz](https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz)
> Linux系统arm架构下载 [frp_0.61.1_linux_arm64.tar.gz](https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_arm64.tar.gz)
> 通常服务器都是amd架构，如果是树莓派或香橙派就是arm架构。

Linux查看系统架构的指令： **`uname -m`**
 *   输出示例：
        *   `x86_64`: 表示 64 位 x86 架构（也称为 AMD64）。
        *   `i686` 或 `i386`: 表示 32 位 x86 架构。
        *   `aarch64`: 表示 64 位 ARM 架构。
        *   `mips`: 表示 MIPS 架构。

**📌搞清楚自己的系统和架构后就可以下载文件了**

**Windows下载方式**：愣着干什么？去点击下载啊！下载完把zip解压出来，然后进入文件夹。

**Linux下载方式**:
```sh
#  AMD架构下载
wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz
```
```sh
#  ARM架构下载 树莓派 香橙派 用这个下
wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_arm64.tar.gz
```
**Linux 解压文件：**
```sh
# 这两个指令根据你的系统架构选择
# AMD架构是这个
tar -xvzf frp_0.61.1_linux_amd64.tar.gz   

# ARM架构是这个
tar -xvzf frp_0.61.1_linux_arm64.tar.gz     
```
**进入文件夹**
```sh
cd frp_0.61.1_linux_amd64   # AMD

cd frp_0.61.1_linux_arm64    #ARM
````

>💡 **提示：** 以上指令都只用执行其中一个！自己搞清楚系统架构！

---

## 🎯 服务器端配置
服务器端通常称为 **frps**，即 FRP **服务端**。
通俗点来说就是**有公网IP的服务器**。

---

### 🔹 1. 服务器端 `frps.toml` 配置
>**📚官方参数文档**：[frps_full_example.toml](https://github.com/fatedier/frp/blob/dev/conf/frps_full_example.toml)  <-大佬请参考这个 0.o

现在我们到你拥有公网ip的服务器
通常这个服务器系统都是linux
如果是windows那就更简单，跟着点击修改一个配置文件即可。

进入我们刚才解压后的文件后，我们可以看到里面有这些东西

```
# Linux指令 ls
frpc  frpc.toml  frps  frps.toml  LICENSE
```
我们需要修改的是 **`frps.toml`** 打开这个文件 (windows用记事本啥的都行)
```
nano frps.toml  #linux 指令
```
进入之后我们只会看到里面有一句 **`bindPort = 7000`** 不用理会他
把他删掉，复制上我的：
```toml
# ==============================
# FRP 服务器端（frps.ini）配置
# ==============================

# 绑定监听地址（默认 `0.0.0.0` 代表监听所有 IP）
bindAddr = "0.0.0.0"

#  服务器监听端口（客户端需要通过该端口连接 FRP 服务器）
bindPort = 7000

# HTTP 端口（用于内网 HTTP 代理穿透）
vhostHTTPPort = 80

# HTTPS 端口（用于内网 HTTPS 代理穿透）
vhostHTTPSPort = 443

# 子域名支持
# 可以通过 `subDomainHost` 解析动态子域名
# 例如：如果 `subDomainHost` 配置为 "example.com"
# 那么客户端可以使用 `test.example.com` 访问内网服务
# 如果你没有域名或不使用此功能，请删除此行！
# 如果你要用IP直连例如:168.0.0.1:8848，就把这行删掉，不要配置！
subDomainHost = "xxxx.com"  # 请替换为你的真实域名

# =============================================
# Web 控制台（Dashboard）配置
# =============================================

# 监控界面监听地址（`0.0.0.0` 代表所有 IP 可访问）
webServer.addr = "0.0.0.0"

# Web 管理面板端口（可在浏览器访问，默认 7500）
# 你可以通过 `http://你的公网IP:7500` 访问 FRP 管理面板
webServer.port = 7500

# Web 控制台管理账号（可自定义）
webServer.user = "admin"

# Web 控制台密码（请自行修改）
webServer.password = "xxxx"

# =============================================
# 身份验证（Authentication）配置
# =============================================

#  认证方式（防止未经授权的客户端连接）
# 目前 FRP 支持 `token` 和 `oidc` 方式，我们选用token
auth.method = "token"

#  Token 认证（客户端需要匹配相同 token 才能连接）
# 通俗来说就是密码，写一个你能记住的，尽量长一点
# 示例: 123-abc-123abc 
auth.token = "123-abc-123abc"   # 请自行修改，不要用我的
```
### 🔹 2. 启动服务端frps
> **❗️注意**：启动指令必须在frp文件目录下执行！

**Linux 启动指令**
```sh
screen -S frps ./frps -c frps.toml   
```
**Windows启动指令**
> 在文件目录里，点击上方的路径栏输入cmd后按回车打开命令窗口

![示例|690x261](upload://tiuu5VTf5q3Li3GAncjCaDklgrv.png)

在命令窗口输入（启动后绝对不要关闭窗口！）
```
frps.exe -c frps.toml
```
**成功启动后会出现：**
```log
[frps/root.go:105] frps uses config file: frps.toml
[server/service.go:237] frps tcp listen on 0.0.0.0:7000
[server/service.go:305] http service listen on 0.0.0.0:80
[server/service.go:319] https service listen on 0.0.0.0:443
[frps/root.go:114] frps started successfully
[server/service.go:351] dashboard listen on 0.0.0.0:7500
```
**🎉恭喜你！你现在已经成功配置服务端并将他运行！**

---
### 💡 **常见问题 & 注意事项**
*  **端口冲突问题**
如果服务器 **80 或 443 端口已被占用**，可更换 `vhostHTTPPort` 和 `vhostHTTPSPort` 的端口号。
例如：
`vhostHTTPPort = 8080`
`vhostHTTPSPort = 8443`
或者根据你自己的服务器情况自行配置，这种实际情况复杂，我就不细说了。

  * **访问 Web 监控面板**
如果启用了 `webServer.port = 7500`，你可以通过：
` http://你的服务IP:7500`
  进入 FRP **管理面板**，在此查看连接状态，如果能进入证明程序正常运行了。

* **Linux退出程序窗口**
按下`Ctrl+A`松开，再按一下`D`就可以退出程序窗口
要回到程序窗口输入指令`screen -r frps`即可
如果想结束程序进程输入指令`pkill frps`即可 

* **Windows程序窗口**
程序运行后**千万不要**关闭窗口，这会导致程序关闭！
当然你如果不想用了就可以直接关掉他，简单粗暴！

---

## 🏠 客户端配置
客户端用于连接服务器，一般是内网服务器的代理。
这个通常是你的个人电脑，你的树莓派、香橙派或NAS。

> **💡提示**：你的客户端也要下载相同版本的frp压缩包并且解压，过程和第一步一样，这里就不多说了
---
### 🔹 1. 配置 `frpc.toml`
>**📚官方参数文档**：[frpc_full_example.toml](https://github.com/fatedier/frp/blob/dev/conf/frpc_full_example.toml)  <-大佬请参考这个 o.0

> **❗️注意**：是**frpc.toml**！是**FRPC**！不要搞错了！和刚才的服务端不一样那个是**frps.toml**。
```toml
# 服务端地址（这里要填你有公网IP的服务器的IP或者是服务器的域名）
serverAddr = "192.xxx.x.x"
# 服务器端口（Frp 服务端监听的端口）
serverPort = 7000

# 连接协议
transport.protocol = "tcp"

# 认证方式
auth.method = "token"
# 认证所使用的 Token（要和你刚才配置的服务端token完全一样！）
auth.token = "123-abc-123abc"

# 代理配置
[[proxies]]
# 代理名称(标识该代理的名称，根据你的喜好填写）
name = "rocketcat"
# 代理类型（http、https、tcp等）
# 这里要根据你的需求来填写，如果你有域名，就用http
# 如果你没有域名，那就用IP直连，例如:165.0.0.1:8848,此时这里应该写tcp协议
# 如果你用tcp协议就必须把刚才服务端上subDomainHost = "xxxx.com"的配置删除！
# type = "tcp"   # IP+端口直连用这个
type = "http"
# 本地 IP（Frp 客户端需要将流量转发到的本地地址）
localIP = "127.0.0.1"
# 本地端口（Frp 客户端需要将流量转发到的本地端口，根据你要穿透的端口来填写）
localPort = 8848
# 访问此代理的子域名
# 如果你没有域名要用IP直连，请把这句删除掉，否则会导致无法连接！
subdomain = "rocket" # 子域名请根据你拥有的域名配置，配置后通过rocket.xxx.com格式访问

# 如果你不用域名，要用ip+端口直连，请必须加上这句！
# 并且删除 subdomain = "rocket" 
# remotePort = 8848    # 这个端口和localPort 配置的一模一样，这样才能正常访问！
```

### 🔹 2. 启动服务端frpc
> **❗️注意**：启动指令必须在frp文件目录下执行！

**Linux 启动指令:**
```sh
screen -S frpc ./frpc -c frpc.toml   
```
**Windows启动指令**
> 在文件目录里，点击上方的路径栏输入cmd后按回车打开命令窗口

在命令窗口输入（启动后绝对不要关闭窗口！）
```sh
frpc.exe -c frpc.toml
```
**成功启动后会出现：**
```log
 [I] [sub/root.go:142] start frpc service for config file [frpc.toml]
 [I] [client/service.go:295] try to connect to server...
 [I] [client/service.go:287] [7c9de41e30e15c46] login to server success, get run id [7c9de41e30e15c46]
 [I] [proxy/proxy_manager.go:173] [7c9de41e30e15c46] proxy added: [rocketcat]
 [I] [client/control.go:168] [7c9de41e30e15c46] [rocketcat] start proxy success

```

**🎉恭喜你！你现在已经成功配置客户端并将他运行，快去访问一下试试吧！**
> 尝试使用你的域名或IP+端口来访问你的客户端吧！

### 💡 **常见问题 & 注意事项**
*  **文件配置错误**
一定要注意客户端配置的是`frpc.toml`文件，如果配置错了，服务无法正常运行！

  * **IP+端口访问模式配置**
配置的时候必须删除服务端的`subDomainHost`
配置`type`为TCP模式`type = "tcp"`
配置`remotePort`监听你的端口`remotePort= 8848`

* **Linux退出程序窗口**
按下`Ctrl+A`松开，再按一下`D`就可以退出程序窗口
要回到程序窗口输入指令`screen -r frpc`即可
如果想结束程序进程输入指令`pkill frpc`即可 

---

## 📝 测试与验证

### 🔹 1. 检查端口
在服务器端检查 FRPS 是否监听：
```sh
ss -tunlp | grep 7000
ss -tunlp | grep 7500
```

### 🔹 2. 访问 HTTP 服务
在浏览器访问你的网站：
```
http://xxxx.xxxxx.com 或 165.0.0.1:8848 访问
```
### 🔹 3. 查看程序运行的日志
Linux通过指令进入程序窗口查看日志，如果出现报错，可以发给AI解答
### 🔹 4. AI！！！救我！！！！
遇到问题通通交给AI解决就好了，让他看你的**运行日志**和你的**配置文件**
他肯定知道哪里出问题了，解决速度肯定比我快。

---

## 📌 结语
这个配置总体来说没有太难，只有两个配置文件。
我写这个教程是因为市面上的教学太笼统或太老旧。
更主要的原因是原项目已经废除了用.ini格式来配置frp文件。
希望这个教程对你有帮助，如果喜欢请点个赞吧！
这个教程在我的GitHub也有同步发出，希望你能去点个`Star 🌟` ！

RocketCat     
Time: 2025/2/12

---

