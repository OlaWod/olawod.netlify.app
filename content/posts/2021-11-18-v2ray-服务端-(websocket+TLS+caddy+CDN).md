---
title: v2ray 服务端 (websocket+TLS+caddy+CDN)
date: "2021-11-18T01:00:00.000Z"
template: "post"
draft: false
slug: "v2ray 服务端 (websocket+TLS+caddy+CDN)"
category: "Unarchived"
tags:
  - " "
description: "v2ray 服务端配置 (websocket+TLS+caddy+CDN)"
socialImage: "/media/v2ray.png"

---

## 一、域名

买域名：https://www.namesilo.com/

在 DNS records 里面把原来的都删掉，添加一个A记录，address 填服务器 IP。

cloudflare：https://www.cloudflare.com/zh-cn/

要把 proxy status 从黄色的 proxied 改成灰色的 DNS only。name 填的是二级域名。

## 二、在服务器上配置

```bash
bash <(curl -s -L https://git.io/v2ray.sh)
# i.e. https://raw.githubusercontent.com/233boy/v2ray/master/install.sh?v
```

```bash
v2ray status # 检查v2ray和caddy状态
v2ray url # 生成链接
v2ray qr # 生成二维码
```

```bash
# 防火墙，不弄的话caddy获取证书时会400，然后进而429
ufw default deny incoming
ufw default allow outgoing # 这句应该是重点
ufw allow ssh
ufw allow http
ufw allow https
ufw allow 41335:60031/tcp
ufw allow 41335:60031/udp
ufw enable
ufw reload
```

```bash
ulimit -n 8192
caddy -conf /etc/caddy/Caddyfile -—agree
systemctl restart caddy
systemctl status caddy
```

## 三、

https://github.com/233boy/v2ray/wiki/V2Ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC

https://www.oilandfish.com/posts/v2ray.html

https://bestminipc.github.io/posts/c2de8ba4/

