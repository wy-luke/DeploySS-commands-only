# DeploySS-commands-only

[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)

每次VPS的IP被墙后, 在新部署VPS时都需要重新搭建shadowsocks  
在这里统一一下所有用得到的命令, 方便以后查阅

关于VPS的基础知识等等都不在此赘述, 只包含用得到的命令
***

## server端

### shadowsocks

- 系统支持: CentOS6+ / Debian6+ / Ubuntu14+

```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ss-go.sh && chmod +x ss-go.sh && bash ss-go.sh
```

### shadowsocks-R

- 系统支持: CentOS6+ / Debian6+ / Ubuntu14+

```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

### BBR安装

- BBR魔改版只支持Debian 8

```bash
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

在删除内核环节, 选择**NO**
***

## Client端(Arch Linux)

### shadowsocks-libev安装

```bash
sudo pacman -Syu shadowsocks-libev
```

### 配置

```bash
sudo touch /etc/shadowsocks/ss.json
```

```bash
sudo nano /etc/shadowsocks/ss.json
```

```test
{
    "server":"remote-shadowsocks-server-ip-addr",
    "server_port":443,
    "password":"your-passwd",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "timeout":300,
    "method":"chacha20-poly-ietf",
}
```

### 启动

```bash
sudo systemctl start shadowsocks-libev@ss
```

### 设置开机启动

```bash
sudo systemctl enable shadowsock-libevs@ss
```

### 关闭开启启动

## 终端设置代理

安装privoxy
```bash
sudo pacman -Syu privoxy
```

### 全局模式

```bash
# 添加本地ss服务到配置文件
sudo echo 'forward-socks5 / 127.0.0.1:1080 .' >> /etc/privoxy/config
# Privoxy 默认监听端口是是8118
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
export no_proxy=localhost
# 启动服务
sudo systemctl start privoxy.service
# 开机启动
sudo systemctl enable privoxy.service
```

### PAC模式

```bash
curl -4sSkLO https://raw.github.com/zfl9/gfwlist2privoxy/master/gfwlist2privoxy

# 注意将 127.0.0.1:1080 替换为你的 socks5 地址
bash gfwlist2privoxy 127.0.0.1:1080

# 将 gfwlist.action 移动到 privoxy 配置文件目录
mv -f gfwlist.action /etc/privoxy/

# 应用 gfwlist.action 配置文件
echo 'actionsfile gfwlist.action' >> /etc/privoxy/config

# Privoxy 默认监听端口是是8118
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
export no_proxy=localhost

# 启动服务
systemctl start privoxy.service
```

### 代理测试

```bash
# 访问各大网站，如果都有网页源码输出说明代理没问题
curl -sL www.baidu.com
curl -sL www.google.com
curl -sL www.google.com.hk
curl -sL www.google.co.jp
curl -sL www.youtube.com
curl -sL mail.google.com
curl -sL facebook.com
curl -sL twitter.com
curl -sL www.wikipedia.org
# 获取当前 IP 地址
# 如果使用 privoxy 全局模式，则应该显示 ss 服务器的 IP
# 如果使用 privoxy gfwlist模式，则应该显示本地公网 IP
curl -sL ip.chinaz.com/getip.aspx
```
