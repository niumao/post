---
title: how to set ss ladder
date: 2018-04-28 00:02:08
tags:
---
ladder always broken, no google no good,so i always update the way when current way no good ,the shadowsockets is now the better way
github:https://github.com/shadowsocks
clang server:https://github.com/shadowsocks/shadowsocks-libev
cross os client:https://github.com/shadowsocks/shadowsocks-qt5
openwrt router:https://github.com/shadowsocks/openwrt-shadowsocks
for winOS easy choose:https://github.com/shadowsocks/shadowsocks-windows
server(Debian 9):
1.apt update && apt install shadowsocks-libev
2.vim /etc/shadowsocks-libev/config.json
{
  "server":{server_ip},
  "server_port":{connect_port},
  "local_address":"127.0.0.1",
  "local_port":1080,
  "password":"rifmatIs",
  "timeout":300,
  "method":"aes-256-cfb",
  "fast_open": false
}
3./etc/init.d/shadowsocks-libev start
4.systemctl start shadowsocks-libev
