# 😼 Nginx + SSL + Acme + Frp SSL免费证书申请+自动续期+反向代理配置 极细致教程🚀

本教程将指导你如何配置 **Nginx**，并使用 **acme.sh** 安装 **Let's Encrypt SSL 证书**并自动续期，实现网站的 **HTTPS 访问**。

---

## 📜 目录

- 🔧 **[1. 安装 Nginx](#1-安装-nginx)**
- ⚒️ **[2. 安装 acme.sh](#2-安装-acmesh)**
- 📝 **[3. 配置 Nginx](#3-配置-nginx)**
- 🔑 **[4. 申请 SSL 证书](#4-申请-ssl证书)**
- 🔐 **[5. 安装 SSL 证书](#5-安装-ssl证书)**
- 🌍 **[6. 配置 Nginx 使用 HTTPS](#6-配置-nginx-使用-https)**
- 🔄 **[7. 证书自动续期](#7-证书自动续期)**
- 📝 **[8. Nginx反向代理详细配置](#8-Nginx反向代理详细配置)**
- 🌍 **[9. Nginx + Frp 详细配置](#9-Nginx-+-Frp-详细配置)**
- 🚀 **[10. Nginx进阶配置](#10-Nginx进阶配置)** 
- 📝 **[11. 重要提示](#11-重要提示)**
- ✅ **[12. 完成配置](#10-至此你已经完成了配置🎉)** 

---

## 🔧 1. 安装 Nginx

🔹 **更新系统并安装 Nginx**
> 💡Nginx是一个高性能的 **HTTP 服务器** 和**反向代理服务器** ，同时它还可以作为**邮件代理服务器** 和**TCP/UDP 代理服务器** 。最初由 **俄罗斯工程师 Igor Sysoev** 开发，主要用于解决高并发连接问题，现在被广泛应用于各类 web 服务中。
```bash
# 更新系统包列表
sudo apt update
# 升级系统包
sudo apt upgrade -y
# 安装 Nginx Web 服务器
sudo apt install nginx -y
```
🔹 **验证 Nginx 是否成功运行**
```bash
# 检查 Nginx 服务状态
sudo systemctl status nginx
```
🎉 **成功运行会出现以下内容**
```bash
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2025-02-13 03:25:47 UTC; 13s ago
       Docs: man:nginx(8)
   Main PID: 113862 (nginx)
      Tasks: 2 (limit: 1126)
     Memory: 5.9M
     CGroup: /system.slice/nginx.service
             ├─113862 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             └─113863 nginx: worker process

Feb 13 03:25:47 hkg7-2025021117123152e6eb systemd[1]: Starting A high performance web server and a reverse proxy server...
Feb 13 03:25:47 hkg7-2025021117123152e6eb systemd[1]: Started A high performance web server and a reverse proxy server.

```
---

## ⚒️ 2. 安装 acme.sh

>💡**acme.sh** 是一个免费、轻量级的自动化 **SSL 证书** 申请与管理工具。

🔹 **安装必要的依赖**

```bash
# socat: 用于处理SSL证书验证
# curl: 用于下载文件
# wget: 另一个下载工具
sudo apt install socat curl wget -y
```

🔹 **安装 acme.sh**
> **❗️注意**：其中参数 email=你的邮箱@xxx.com 要替换成你自己的邮箱！它是用来用于接收证书过期通知的。

```bash
curl https://get.acme.sh | sh -s email=你的邮箱@xxx.com
```

🔹 **重新加载shell配置以使acme.sh命令生效**
```bash
source ~/.bashrc
```

---

## 📝 3. 配置 Nginx

🔹 **创建Nginx配置文件**
> **❗️注意**：之后**所有配置**里的 **`域名.com`** 都要替换成你自己的域名！
因为申请的是免费证书不能申请泛域名，所以把你的域名写全了，你用哪个就写哪个。
例如： `xxx.yyy.top` 或 `ttt.xxx.yyy.com` 必须都写上！
如果你要对你**frp穿透的网址**申请证书，要申请**完整的域名** 子域名+主域名，**切记！**
```bash
sudo nano /etc/nginx/sites-available/域名.com
```

🔹 **添加 HTTP 配置（稍后将更新为 HTTPS）**
```nginx
server {
    # 监听80端口（HTTP）
    listen 80;
    # 指定域名
    server_name 域名.com;     #注意修改 域名.com 成你自己的域名
    
    # 网站根目录
    root /var/www/域名.com;   #注意修改 域名.com 成你自己的域名
    # 默认索引文件
    index index.html index.htm;

    location / {
        # 尝试查找请求的文件，如果不存在则返回404
        try_files $uri $uri/ =404;
    }
}
```
> **💡提示**：确认配置正确后，使用快捷键 `Ctrl+X` 来退出编辑窗口，期间会提示你是否要保存文件，你需要按 `Y` 键确认，随后按 `Enter` 来确认保存即可。

🔹 **创建网站目录并设置权限**
> **❗️注意**：把指令里的 **`域名.com`** 替换成你自己的域名！有2处要替换！
```bash
# 创建网站根目录
sudo mkdir -p /var/www/域名.com   #注意修改 域名.com 成你自己的域名

# 设置目录权限
sudo chown -R www-data:www-data /var/www/域名.com    #注意修改 域名.com 成你自己的域名
```

🔹 **启用网站配置**
> **❗️注意**：把配置里的 **`域名.com`** 替换成你自己的域名！有1处要替换！
```bash
# 创建符号链接到sites-enabled目录
#注意修改 域名.com 成你自己的域名 ！
sudo ln -s /etc/nginx/sites-available/域名.com /etc/nginx/sites-enabled/

# 测试Nginx配置是否正确
sudo nginx -t

# 如果正确会出现
# nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# nginx: configuration file /etc/nginx/nginx.conf test is successful

# 如果配置正确，重启Nginx
sudo systemctl restart nginx
```

---

## 🔑 4. 申请 SSL 证书

🔹 **设置默认证书颁发机构为 Let's Encrypt**
```bash
~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
```

🔹 **申请证书**
> **❗️注意**：把指令里的 **`域名.com`** 替换成你自己的域名！有2处要替换！

```bash
# --issue: 表示申请新证书
# -d: 指定域名
# --webroot: 指定网站根目录，用于验证域名所有权
# 注意修改 域名.com 成你自己的域名 ！ 共有2处需要修改！请看清楚！
~/.acme.sh/acme.sh --issue -d 域名.com --webroot /var/www/域名.com
```
🎉  **申请成功会出现**
```bash
Your cert is in: /root/.acme.sh/域名.com_ecc/域名.com.cer
Your cert key is in: /root/.acme.sh/域名.com_ecc/域名.com.key
The intermediate CA cert is in: /root/.acme.sh/域名.com_ecc/ca.cer
And the full-chain cert is in: /root/.acme.sh/域名.com_ecc/fullchain.cer
```
---

## 🔐 5. 安装 SSL 证书

🔹 **创建存放 SSL 证书的目录**
> **❗️注意**：把指令里的 **`域名.com`** 替换成你自己的域名！
```bash
sudo mkdir -p /etc/nginx/ssl/域名.com
```

🔹 **安装证书到指定位置**
> **❗️特别注意**：把指令里的 **`域名.com`** 替换成你自己的域名！共有 **3处** 需要修改！
建议**复制下来**修改完成并**确认无误**后再复制进命令窗口，防止出现错误！
```bash
# --install-cert: 安装证书
# -d: 指定域名
# --key-file: 私钥存放路径
# --fullchain-file: 完整证书链存放路径
# --reloadcmd: 证书更新后重新加载Nginx的命令

~/.acme.sh/acme.sh --install-cert -d 域名.com \
--key-file /etc/nginx/ssl/域名.com/private.key \
--fullchain-file /etc/nginx/ssl/域名.com/cert.pem \
--reloadcmd "sudo systemctl reload nginx"
```

---

## 🌍 6. 配置 Nginx 使用 HTTPS

🔹 **更新 Nginx 配置**
> **❗️注意**：把指令里的 **`域名.com`** 替换成你自己的域名！
```bash
sudo nano /etc/nginx/sites-available/域名.com
```
> **❗️特别注意**：把配置文件里的 **`域名.com`** 替换成你自己的域名！共有 **5处** 需要修改！
建议**复制下来**修改完成并**确认无误**后再复制进配置文件，防止出现错误！
把配置文件里**之前**配置的内容**全部删除**，然后再把你**修改好**的配置**复制**进去！
```nginx
# HTTP服务器块 - 将所有HTTP请求重定向到HTTPS
server {
    listen 80;
    server_name 域名.com;   # 替换你的域名！
    # 将所有HTTP请求301永久重定向到HTTPS
    return 301 https://$server_name$request_uri;
}

# HTTPS服务器块
server {
    # 监听443端口（HTTPS）
    listen 443 ssl;
    server_name 域名.com;  # 替换你的域名！
    
    # SSL证书配置
    ssl_certificate /etc/nginx/ssl/域名.com/cert.pem;          # 替换你的域名！
    ssl_certificate_key /etc/nginx/ssl/域名.com/private.key;   # 替换你的域名！
    
    # SSL协议版本配置
    # 只允许TLS 1.2和1.3版本，禁用不安全的SSL/TLS版本
    ssl_protocols TLSv1.2 TLSv1.3;
    
    # 加密算法配置
    # 使用强加密套件，按优先级排序
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    
    # 禁用服务器端加密算法优先，使用客户端算法优先
    ssl_prefer_server_ciphers off;
    
    # HSTS配置
    # 强制客户端在指定时间内只使用HTTPS访问
    add_header Strict-Transport-Security "max-age=31536000" always;
    
    # 网站根目录
    root /var/www/域名.com;   # 替换为你的域名！
    index index.html index.htm;

    # 处理请求的location块
    location / {
        try_files $uri $uri/ =404;
    }
    
    # 性能优化配置
    # 开启SSL会话缓存
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    
    # 开启OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
}
```
> **💡提示**：确认配置正确后，使用快捷键 `Ctrl+X` 来退出编辑窗口，期间会提示你是否要保存文件，你需要按 `Y` 键确认，随后按 `Enter` 来确认保存即可。

🔹 **应用新配置**
```bash
sudo nginx -t
sudo systemctl restart nginx
```
🎉**现在你可以尝试使用你的域名来访问啦！**
>因为我们现在还没有配置具体的网站代理
所以会出现 `403 Forbidden` 和下面一行小字 `nginx/1.18.0 (Ubuntu)`
不要慌，出现这个证明你已经**成功配置**了ssl证书！现在你的网站是使用https来访问的啦！

---

## 🔄 7. 证书自动续期

acme.sh **默认会自动续期**，你可以查看 `cron` 任务：
```bash
crontab -l
```

你应该能看到类似这样的任务：
```
0 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null
```

---

## 📝 8. Nginx反向代理详细配置
> 我们配置Nginx的主要目的就是使用https来安全的访问我们的网站
所以这一步配置是必不可少的，接下来要配置的是/etc/nginx/sites-available/ 目录下`域名.com` 

🔹 **配置Nginx反向代理**
> **❗️注意**：把指令里的 **`域名.com`** 替换成你自己的域名！
```bash
sudo nano /etc/nginx/sites-available/域名.com
```
> **❗️特别注意**：把配置文件里的 **`域名.com`** 替换成你自己的域名！共有 **4处** 需要修改！
建议**复制下来**修改完成并**确认无误**后再复制进配置文件，防止出现错误！
(推荐)把配置文件里**之前**配置的内容**全部删除**，然后再把你**修改好**的配置**复制**进去！
或者你有**能力自行修改**里面的配置参数，就可以对照着我的配置文件来改

🔹 **HTTP服务器配置块**
```nginx
# 这部分主要修改的是把http的请求全部重定向到https
server {
    # 监听80端口
    listen 80;
    server_name 域名.com;   # 替换你的域名！
    
    # 将所有HTTP请求重定向到HTTPS
    return 301 https://$server_name$request_uri;
}
```
🔹 **HTTPS服务器配置块**
```nginx
server {
    # 监听443端口，启用SSL
    listen 443 ssl;
    server_name 域名.com;  # 替换你的域名！
    
    # SSL证书配置
    ssl_certificate /etc/nginx/ssl/域名.com/cert.pem;        # 替换你的域名！
    ssl_certificate_key /etc/nginx/ssl/域名.com/private.key;  # 替换你的域名！
    
    # SSL协议版本和加密套件配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;
    
    # 启用HSTS
    add_header Strict-Transport-Security "max-age=31536000" always;
    
    # 反向代理配置
    location / {
        # 设置代理头部信息
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # 设置代理目标
        # 例如：将请求转发到本地8848端口
        # 这里是配置的重点，你需要把这里修改成你网站所使用的端口
        # 这样请求就会被代理到你所设置的端口
        proxy_pass http://127.0.0.1:8848;
        
        # 超时设置
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
        
        # 缓冲区设置
        proxy_buffer_size 4k;
        proxy_buffers 4 32k;
        proxy_busy_buffers_size 64k;
    }
    
    # 性能优化配置
    # 开启gzip压缩 (可选)
    # Gzip是一种常用的网页压缩技术，它可以显著减小传输数据的大小，加快网站加载速度。
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_min_length 1000;
    gzip_comp_level 6;
    
    # SSL会话缓存
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    
    # OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
    
    # 错误页面配置（可选）
    # 这个是你的网页无法访问时，会被导向的错误页面，根据需求配置
    # error_page 500 502 503 504 /50x.html;
    # location = /50x.html {
    #     root /usr/share/nginx/html;
    # }
}
```

> **💡提示**：确认配置正确后，使用快捷键 `Ctrl+X` 来退出编辑窗口，期间会提示你是否要保存文件，你需要按 `Y` 键确认，随后按 `Enter` 来确认保存即可。

🔹 **应用新配置**
```bash
sudo nginx -t
sudo systemctl restart nginx
```

### **🎉现在你可以尝试使用你的域名来访问你的网站啦！**

---

## 🌍 9. Nginx + Frp 详细配置
> **💡提示**：有关Frp内网穿透的详细教程可以看我的这个帖子 [ FRP - Linux & Win 内网穿透教程](https://linux.do/t/topic/425469)

🔹 **服务端Frps文件配置**
打开你的frps.toml配置文件，然后修改这几项
```toml
# 因为nginx会占用80端口所以要改成8080或者改成你喜欢的数字
vhostHTTPPort = 8080
# 同样的443也要改，防止端口冲突
vhostHTTPSPort = 8443
# 这一项也是必须要配置的，设置为你的域名，
subDomainHost = "域名.com"
```

🔹 **客户端Frpc文件配置**
打开你的frpc.toml配置文件，然后修改这几项
```toml
# 配置这一项为你的服务端的IP
serverAddr = "156.xxx.xxx.xxx"

type = "http"       # 这个必须改为 http 
localIP = "127.0.0.1"
localPort = 8848    # 这是你要穿透的端口
subdomain = "你的子域名"  # 这里要填入你的子域名，用来访问
# 如果有这一项，请去掉它，不然会出问题
# remotePort = 8848
```
🔹 **Nginx文件配置**
这里我们只用配置https服务器内容，修改以下内容，其他保持原样
```nginx
# 把端口改为你frps设置的http端口即可
proxy_pass http://127.0.0.1:8080;  
```

应用nginx新配置，并且重启你的frpc和frps
```bash
sudo nginx -t
sudo systemctl restart nginx
```
### **🎉现在你可以尝试使用你的域名来访问你的网站啦！**

---

## 🚀 10. Nginx进阶配置

🔹 **多个后端服务**
如果你有多个服务运行在不同端口，可以使用以下配置
```
# API服务
location /api/ {
    proxy_pass http://127.0.0.1:8848/;
}

# 管理后台
location /admin/ {
    proxy_pass http://127.0.0.1:8849/;
}

# 静态文件
location /static/ {
    alias /var/www/static/;
    expires 7d;
}
```
🔹 **负载均衡配置**
```
# 定义上游服务器组
upstream backend {
    server 127.0.0.1:8848;
    server 127.0.0.1:8849;
    server 127.0.0.1:8850;
}

location / {
    proxy_pass http://backend;
}
```
>💡 以上配置需要一定的基础知识，也不是本教程的重点，所以就不细说了
---

## 📝 11. 重要提示

💡 **确保 DNS 解析正确**：  
- 使用 `ping 域名.com` 确保你的域名已经指向服务器 IP。

💡 **端口必须开放**：  
- 服务器的 **80 和 443 端口** 必须对外可访问。

💡 **故障排查**：  
- 查看 **Nginx 错误日志**：
  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```
- 查看 **访问日志**：
  ```bash
  sudo tail -f /var/log/nginx/access.log
  ```
💡 **AI，救我**：
- 遇到问题第一时间找他，把AI武装成自己的第二大脑，这是新时代的主旋律。
  
---

## ✅ 12. 至此你已经完成了配置🎉

🔹 **访问你的 HTTPS 网站**  
  ➡  打开浏览器，访问 **`https://域名.com`**  
  ✅ 确保浏览器显示 🔒 **安全锁标志**

🔹 **使用 SSL Labs 测试 SSL 安全性**  
  👉  [SSL Server Test](https://www.ssllabs.com/ssltest/) 检查配置安全性

🔹 **结束语**  
希望我的教程能对您有所帮助，尽管不是非常专业的教学。
但我还是希望能帮助到一些刚刚入门的新人！
若是帮到您，就请给这个帖子点个赞，顺便关注我一下！
这些教程我会同步在我的 [GitHub](https://github.com/CNFlyCat/UsefulTutorials)上发布。
后续我还可能会出一些基础教学，来帮助更多人！

RocketCat
2025/2/13

---