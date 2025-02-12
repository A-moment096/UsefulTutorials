# ? FRP - Linux & Win ������͸�̳� �ִ��Ѷ� ????

---

> **? �����ڣ�** ���ط����������ԡ���ݮ�ɡ������������͸
> **?? ����**��FRP��fast reverse proxy��  
> **?? ϵͳ**��Linux��Windows
> **? �ܹ�**��x86��amd��arm
> **?Frp �汾**��v0.61.1
> **? �̳�����**��2025/2/12

---

## ? Ŀ¼

1. [? ʲô�� FRP��](#-ʲô��-frp)
2. [? ��װ������](#-��װ������)
3. [? ������������](#-������������)
4. [? �ͻ�������](#-�ͻ�������)
5. [? ��������֤](#-��������֤)
6. [? ����](#-����)

---

## ? ʲô�� FRP��

**FRP**��Fast Reverse Proxy����һ���������**������͸**�Ŀ�Դ���ߣ�
֧�� **TCP/UDP/HTTP/HTTPS Э��**�����Խ� **��������** ��¶�� **����**
**ʵ��**���������绷�����ʵ���ķ���������ԡ�

? **Frp ����**��[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

---

## ? ��װ������

> **?? ��ʼ֮ǰ��ȷ��** >**?? һ���й��� IP �ķ�����** >**?? ����Ҫʵ��������͸�ķ����������**

> ����㲻֪����Щ����ʲô������ȥ��ѯ�˽�һ�£���Ȼ����̳��㿴���� Like this -> 0.o

### ? 1. ���� FRP

ǰ�� [GitHub FRP Releases](https://github.com/fatedier/frp/releases) �����ʺ���ϵͳ�İ汾��

> **? ��ʾ**��
> �㿴�����ļ���ʽ�� frp_0.61.1_xxx(����ϵͳ)\_yyy(ϵͳ�ܹ�).tar.gz
> Windows ϵͳ���� [frp_0.61.1_windows_amd64.zip](https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_windows_amd64.zip)
> Linux ϵͳ amd �ܹ����� [frp_0.61.1_linux_amd64.tar.gz](https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz)
> Linux ϵͳ arm �ܹ����� [frp_0.61.1_linux_arm64.tar.gz](https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_arm64.tar.gz)
> ͨ������������ amd �ܹ����������ݮ�ɻ�����ɾ��� arm �ܹ���

Linux �鿴ϵͳ�ܹ���ָ� **`uname -m`**

- ���ʾ����
  - `x86_64`: ��ʾ 64 λ x86 �ܹ���Ҳ��Ϊ AMD64����
  - `i686` �� `i386`: ��ʾ 32 λ x86 �ܹ���
  - `aarch64`: ��ʾ 64 λ ARM �ܹ���
  - `mips`: ��ʾ MIPS �ܹ���

**? ������Լ���ϵͳ�ͼܹ���Ϳ��������ļ���**

**Windows ���ط�ʽ**����Ÿ�ʲô��ȥ������ذ���������� zip ��ѹ������Ȼ������ļ��С�

**Linux ���ط�ʽ**:

```sh
#  AMD�ܹ�����
wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz
```

```sh
#  ARM�ܹ����� ��ݮ�� ����� �������
wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_arm64.tar.gz
```

**Linux ��ѹ�ļ���**

```sh
# ������ָ��������ϵͳ�ܹ�ѡ��
# AMD�ܹ������
tar -xvzf frp_0.61.1_linux_amd64.tar.gz

# ARM�ܹ������
tar -xvzf frp_0.61.1_linux_arm64.tar.gz
```

**�����ļ���**

```sh
cd frp_0.61.1_linux_amd64   # AMD

cd frp_0.61.1_linux_arm64    #ARM
```

> ? **��ʾ��** ����ָ�ֻ��ִ������һ�����Լ������ϵͳ�ܹ���

---

## ? ������������

��������ͨ����Ϊ **frps**���� FRP **�����**��
ͨ�׵���˵����**�й��� IP �ķ�����**��

---

### ? 1. �������� `frps.toml` ����

> **? �ٷ������ĵ�**��[frps_full_example.toml](https://github.com/fatedier/frp/blob/dev/conf/frps_full_example.toml) <-������ο���� 0.o

�������ǵ���ӵ�й��� ip �ķ�����
ͨ�����������ϵͳ���� linux
����� windows �Ǿ͸��򵥣����ŵ���޸�һ�������ļ����ɡ�

�������ǸղŽ�ѹ����ļ������ǿ��Կ�����������Щ����

```
# Linuxָ�� ls
frpc  frpc.toml  frps  frps.toml  LICENSE
```

������Ҫ�޸ĵ��� **`frps.toml`** ������ļ� (windows �ü��±�ɶ�Ķ���)

```
nano frps.toml  #linux ָ��
```

����֮������ֻ�ῴ��������һ�� **`bindPort = 7000`** ���������
����ɾ�����������ҵģ�

```toml
# ==============================
# FRP �������ˣ�frps.ini������
# ==============================

# �󶨼�����ַ��Ĭ�� `0.0.0.0` ����������� IP��
bindAddr = "0.0.0.0"

#  �����������˿ڣ��ͻ�����Ҫͨ���ö˿����� FRP ��������
bindPort = 7000

# HTTP �˿ڣ��������� HTTP ����͸��
vhostHTTPPort = 80

# HTTPS �˿ڣ��������� HTTPS ����͸��
vhostHTTPSPort = 443

# ������֧��
# ����ͨ�� `subDomainHost` ������̬������
# ���磺��� `subDomainHost` ����Ϊ "example.com"
# ��ô�ͻ��˿���ʹ�� `test.example.com` ������������
# �����û��������ʹ�ô˹��ܣ���ɾ�����У�
# �����Ҫ��IPֱ������:168.0.0.1:8848���Ͱ�����ɾ������Ҫ���ã�
subDomainHost = "xxxx.com"  # ���滻Ϊ�����ʵ����

# =============================================
# Web ����̨��Dashboard������
# =============================================

# ��ؽ��������ַ��`0.0.0.0` �������� IP �ɷ��ʣ�
webServer.addr = "0.0.0.0"

# Web �������˿ڣ�������������ʣ�Ĭ�� 7500��
# �����ͨ�� `http://��Ĺ���IP:7500` ���� FRP �������
webServer.port = 7500

# Web ����̨�����˺ţ����Զ��壩
webServer.user = "admin"

# Web ����̨���루�������޸ģ�
webServer.password = "xxxx"

# =============================================
# �����֤��Authentication������
# =============================================

#  ��֤��ʽ����ֹδ����Ȩ�Ŀͻ������ӣ�
# Ŀǰ FRP ֧�� `token` �� `oidc` ��ʽ������ѡ��token
auth.method = "token"

#  Token ��֤���ͻ�����Ҫƥ����ͬ token �������ӣ�
# ͨ����˵�������룬дһ�����ܼ�ס�ģ�������һ��
# ʾ��: 123-abc-123abc
auth.token = "123-abc-123abc"   # �������޸ģ���Ҫ���ҵ�
```

### ? 2. ��������� frps

> **?? ע��**������ָ������� frp �ļ�Ŀ¼��ִ�У�

**Linux ����ָ��**

```sh
screen -S frps ./frps -c frps.toml
```

**Windows ����ָ��**

> ���ļ�Ŀ¼�����Ϸ���·�������� cmd �󰴻س��������

![ʾ��|690x261](upload://tiuu5VTf5q3Li3GAncjCaDklgrv.png)

����������루��������Բ�Ҫ�رմ��ڣ���

```
frps.exe -c frps.toml
```

**�ɹ����������֣�**

```log
[frps/root.go:105] frps uses config file: frps.toml
[server/service.go:237] frps tcp listen on 0.0.0.0:7000
[server/service.go:305] http service listen on 0.0.0.0:80
[server/service.go:319] https service listen on 0.0.0.0:443
[frps/root.go:114] frps started successfully
[server/service.go:351] dashboard listen on 0.0.0.0:7500
```

**? ��ϲ�㣡�������Ѿ��ɹ����÷���˲��������У�**

---

### ? **�������� & ע������**

- **�˿ڳ�ͻ����**
  ��������� **80 �� 443 �˿��ѱ�ռ��**���ɸ��� `vhostHTTPPort` �� `vhostHTTPSPort` �Ķ˿ںš�
  ���磺
  `vhostHTTPPort = 8080`
  `vhostHTTPSPort = 8443`
  ���߸������Լ��ķ���������������ã�����ʵ��������ӣ��ҾͲ�ϸ˵�ˡ�

- **���� Web ������**
  ��������� `webServer.port = 7500`�������ͨ����
  ` http://��ķ���IP:7500`
  ���� FRP **�������**���ڴ˲鿴����״̬������ܽ���֤���������������ˡ�

- **Linux �˳����򴰿�**
  ����`Ctrl+A`�ɿ����ٰ�һ��`D`�Ϳ����˳����򴰿�
  Ҫ�ص����򴰿�����ָ��`screen -r frps`����
  �������������������ָ��`pkill frps`����

- **Windows ���򴰿�**
  �������к�**ǧ��Ҫ**�رմ��ڣ���ᵼ�³���رգ�
  ��Ȼ������������˾Ϳ���ֱ�ӹص������򵥴ֱ���

---

## ? �ͻ�������

�ͻ����������ӷ�������һ���������������Ĵ���
���ͨ������ĸ��˵��ԣ������ݮ�ɡ�����ɻ� NAS��

> **? ��ʾ**����Ŀͻ���ҲҪ������ͬ�汾�� frp ѹ�������ҽ�ѹ�����̺͵�һ��һ��������Ͳ���˵��

---

### ? 1. ���� `frpc.toml`

> **? �ٷ������ĵ�**��[frpc_full_example.toml](https://github.com/fatedier/frp/blob/dev/conf/frpc_full_example.toml) <-������ο���� o.0

> **?? ע��**����**frpc.toml**����**FRPC**����Ҫ����ˣ��͸ղŵķ���˲�һ���Ǹ���**frps.toml**��

```toml
# ����˵�ַ������Ҫ�����й���IP�ķ�������IP�����Ƿ�������������
serverAddr = "192.xxx.x.x"
# �������˿ڣ�Frp ����˼����Ķ˿ڣ�
serverPort = 7000

# ����Э��
transport.protocol = "tcp"

# ��֤��ʽ
auth.method = "token"
# ��֤��ʹ�õ� Token��Ҫ����ղ����õķ����token��ȫһ������
auth.token = "123-abc-123abc"

# ��������
[[proxies]]
# ��������(��ʶ�ô�������ƣ��������ϲ����д��
name = "rocketcat"
# �������ͣ�http��https��tcp�ȣ�
# ����Ҫ���������������д�������������������http
# �����û���������Ǿ���IPֱ��������:165.0.0.1:8848,��ʱ����Ӧ��дtcpЭ��
# �������tcpЭ��ͱ���Ѹղŷ������subDomainHost = "xxxx.com"������ɾ����
# type = "tcp"   # IP+�˿�ֱ�������
type = "http"
# ���� IP��Frp �ͻ�����Ҫ������ת�����ı��ص�ַ��
localIP = "127.0.0.1"
# ���ض˿ڣ�Frp �ͻ�����Ҫ������ת�����ı��ض˿ڣ�������Ҫ��͸�Ķ˿�����д��
localPort = 8848
# ���ʴ˴����������
# �����û������Ҫ��IPֱ����������ɾ����������ᵼ���޷����ӣ�
subdomain = "rocket" # �������������ӵ�е��������ã����ú�ͨ��rocket.xxx.com��ʽ����

# ����㲻��������Ҫ��ip+�˿�ֱ��������������䣡
# ����ɾ�� subdomain = "rocket"
# remotePort = 8848    # ����˿ں�localPort ���õ�һģһ�������������������ʣ�
```

### ? 2. ��������� frpc

> **?? ע��**������ָ������� frp �ļ�Ŀ¼��ִ�У�

**Linux ����ָ��:**

```sh
screen -S frpc ./frpc -c frpc.toml
```

**Windows ����ָ��**

> ���ļ�Ŀ¼�����Ϸ���·�������� cmd �󰴻س��������

����������루��������Բ�Ҫ�رմ��ڣ���

```sh
frpc.exe -c frpc.toml
```

**�ɹ����������֣�**

```log
 [I] [sub/root.go:142] start frpc service for config file [frpc.toml]
 [I] [client/service.go:295] try to connect to server...
 [I] [client/service.go:287] [7c9de41e30e15c46] login to server success, get run id [7c9de41e30e15c46]
 [I] [proxy/proxy_manager.go:173] [7c9de41e30e15c46] proxy added: [rocketcat]
 [I] [client/control.go:168] [7c9de41e30e15c46] [rocketcat] start proxy success

```

**? ��ϲ�㣡�������Ѿ��ɹ����ÿͻ��˲��������У���ȥ����һ�����԰ɣ�**

> ����ʹ����������� IP+�˿���������Ŀͻ��˰ɣ�

### ? **�������� & ע������**

- **�ļ����ô���**
  һ��Ҫע��ͻ������õ���`frpc.toml`�ļ���������ô��ˣ������޷��������У�

- **IP+�˿ڷ���ģʽ����**
  ���õ�ʱ�����ɾ������˵�`subDomainHost`
  ����`type`Ϊ TCP ģʽ`type = "tcp"`
  ����`remotePort`������Ķ˿�`remotePort= 8848`

- **Linux �˳����򴰿�**
  ����`Ctrl+A`�ɿ����ٰ�һ��`D`�Ϳ����˳����򴰿�
  Ҫ�ص����򴰿�����ָ��`screen -r frpc`����
  �������������������ָ��`pkill frpc`����

---

## ? ��������֤

### ? 1. ���˿�

�ڷ������˼�� FRPS �Ƿ������

```sh
ss -tunlp | grep 7000
ss -tunlp | grep 7500
```

### ? 2. ���� HTTP ����

����������������վ��

```
http://xxxx.xxxxx.com �� 165.0.0.1:8848 ����
```

### ? 3. �鿴�������е���־

Linux ͨ��ָ�������򴰿ڲ鿴��־��������ֱ������Է��� AI ���

### ? 4. AI���������ң�������

��������ͨͨ���� AI ����ͺ��ˣ����������**������־**�����**�����ļ�**
���϶�֪������������ˣ�����ٶȿ϶����ҿ졣

---

## ? ����

�������������˵û��̫�ѣ�ֻ�����������ļ���
��д����̳�����Ϊ�����ϵĽ�ѧ̫��ͳ��̫�Ͼɡ�
����Ҫ��ԭ����ԭ��Ŀ�Ѿ��ϳ�����.ini ��ʽ������ frp �ļ���
ϣ������̶̳����а��������ϲ�������ްɣ�
����̳����ҵ� GitHub Ҳ��ͬ��������ϣ������ȥ���`Star ?` ��

RocketCat  
Time: 2025/2/12

---
