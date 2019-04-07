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