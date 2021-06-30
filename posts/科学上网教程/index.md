# 科学上网教程


Google, Youtube, Facebook, Twitter, Instagram, Reddit, ...

<!--more-->

## 准备工作

- VPS：推荐`CN2 GIA`线路，其次`CN2`
  - [Bandwagon](https://bandwagonhost.com)
  - [Vultr](https://www.vultr.com)
  - [DigitalOcean](https://www.digitalocean.com)
  - [UUUVPS](https://uuuvps.com)
- 域名
  - 免费：[Freenom](https://www.freenom.com)
  - 购买：[NameSilo](https://www.namesilo.com), [GoDaddy](https://www.godaddy.com)
- DNS & CDN：[Cloudflare](https://www.cloudflare.com)

### Freenom 免费域名申请

### 域名托管到 Cloudflare

## 脚本

### v2ray-agent

[Github](https://github.com/mack-a/v2ray-agent)

支持：

1. VLESS + TCP + TLS
1. VLESS + TCP + XTLS
1. VLESS + gRPC + TLS
1. VLESS + WS + TLS
1. VMess + TCP + TLS
1. VMess + WS + TLS
1. Trojan
1. Trojan-Go + WS

安装：

```bash
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```

查看配置：`vasma`

### Xray_onekey

[Github](https://github.com/wulabing/Xray_onekey)

支持：

- VLESS + TCP + XTLS/TLS
- VLESS + TCP + XTLS/TLS 及 VLESS + WS + TLS 回落并存模式

安装：

```bash
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/Xray_onekey/main/install.sh" && chmod +x install.sh && bash install.sh
```

查看配置：同安装

## 客户端

### Windows

#### v2rayN

[下载](https://github.com/2dust/v2rayN/releases)

<img src="/科学上网/科学上网01.png" />

<img src="/科学上网/科学上网02.png" />

<img src="/科学上网/科学上网03.png" />

<img src="/科学上网/科学上网04.png" />

<img src="/科学上网/科学上网05.png" />

#### Clash for Windows

[下载](https://github.com/Fndroid/clash_for_windows_pkg/releases)

#### Qv2ray

[下载](https://github.com/Qv2ray/Qv2ray/releases)

内核：
- [v2ray](https://github.com/v2fly/v2ray-core/releases)
- [xray](https://github.com/XTLS/Xray-core/releases)
- [trojan-go](https://github.com/p4gefau1t/trojan-go/releases)

插件：
- [Trojan](https://github.com/Qv2ray/QvPlugin-Trojan/releases)
- [Trojan-Go](https://github.com/Qv2ray/QvPlugin-Trojan-Go/releases)

支持协议：
- Vmess (V2ray)
- Vless (Xray)
- SS (Shadowsocks)
- SSR (ShadowsocksR)
- Trojan
- Trojan-Go
- NaiveProxy

<img src="/科学上网/Qv2ray-01.png" />

<img src="/科学上网/Qv2ray-02.png" />

<img src="/科学上网/Qv2ray-03.png" />

<img src="/科学上网/Qv2ray-04.png" />

<img src="/科学上网/Qv2ray-05.png" />

<img src="/科学上网/Qv2ray-06.png" />

<img src="/科学上网/Qv2ray-07.png" />

<img src="/科学上网/Qv2ray-08.png" />

<img src="/科学上网/Qv2ray-09.png" />

`QvTrojanGoPlugin.v1.0.1.Windows-x64.dll`、`QvTrojanPlugin.v2.0.0.Windows-x64.dll`



### 安卓

#### V2rayNG

[下载](https://github.com/2dust/v2rayNG/releases)

#### igniter

只支持 Trojan

[下载](https://github.com/trojan-gfw/igniter/releases)

#### Clash for Android

[下载](https://github.com/Kr328/ClashForAndroid/releases)

#### AnXray

[下载](https://github.com/XTLS/AnXray/releases)

#### SagerNet

[下载](https://github.com/SagerNet/SagerNet/releases)

