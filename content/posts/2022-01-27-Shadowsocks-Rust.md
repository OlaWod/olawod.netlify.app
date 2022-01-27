---
title: Shadowsocks-Rust
date: "2022-01-27T16:30:00.000Z"
template: "post"
draft: false
slug: "Shadowsocks-Rust"
category: "Unarchived"
tags:
  - " "
description: "Shadowsocks-Rust server-side"
socialImage: "/media/test-image.jpg"

---

## 一、下载安装

```bash
mkdir ss
cd ss

wget https://github.com/shadowsocks/shadowsocks-rust/releases/download/v1.12.5/shadowsocks-v1.12.5.x86_64-unknown-linux-gnu.tar.xz

tar -xf shadowsocks-v1.12.5.x86_64-unknown-linux-gnu.tar.xz  
```

## 二、做成服务

```bash
cp ssserver /usr/local/bin
nano /etc/shadowsocks-rust.json
nano /usr/lib/systemd/system/shadowsocks-rust.service
```

**/etc/shadowsocks-rust.json:**

```bash
{
   "server": "0.0.0.0",
   "server_port": 21429,
   "password": "密码",
   "timeout": 300,
   "method": "chacha20-ietf-poly1305",
   "mode": "tcp_only",
   "fast_open": false
}
```

**/usr/lib/systemd/system/shadowsocks-rust.service:**

```bash
[Unit]
Description=shadowsocks-rust service
After=network.target

[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks-rust.json
ExecStop=/usr/bin/killall ssserver
Restart=always
RestartSec=10
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=ssserver
User=root
Group=root

[Install]
WantedBy=multi-user.target
```

## 三、使用

```bash
systemctl enable shadowsocks-rust
systemctl start shadowsocks-rust
systemctl status shadowsocks-rust
```

-----

## References

https://www.oilandfish.com/posts/shadowsocks-rust.html

https://github.com/shadowsocks/shadowsocks-rust
