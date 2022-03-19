# 科学上网教程


外面的世界。

<!--more-->

## 1.准备工作

- VPS：推荐`CN2 GIA`线路，其次`CN2`
  - [BandwagonHost](https://bandwagonhost.com/), [备用](https://bwh88.net/)
  - [Vultr](https://www.vultr.com/)
  - [DigitalOcean](https://www.digitalocean.com/)
  - [三优云](https://uuuvps.com/)
- 域名
  - 免费：[Freenom](https://www.freenom.com/)
  - 付费：[NameSilo](https://www.namesilo.com/), [GoDaddy](https://www.godaddy.com/)
- DNS & CDN：
  - [Cloudflare](https://www.cloudflare.com/)

## 2.服务端

### 2.1 v2ray-agent

[Github](https://github.com/mack-a/v2ray-agent)

支持协议：

1. VLESS + TCP + TLS
1. VLESS + TCP + xtls-rprx-direct【推荐】
1. VLESS + gRPC + TLS
1. VLESS + WS + TLS
1. Trojan + TCP + TLS【推荐】
1. Trojan + TCP + xtls-rprx-direct【推荐】
1. Trojan + gRPC + TLS
1. VMess + WS + TLS

安装：

```bash
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```

查看配置：`vasma`

### 2.2 Xray_onekey

[Github](https://github.com/wulabing/Xray_onekey)

支持协议：

1. VLESS + TCP + XTLS/TLS
1. VLESS + TCP + XTLS/TLS 及 VLESS + WS + TLS 回落并存模式

安装：

```bash
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/Xray_onekey/main/install.sh" && chmod +x install.sh && bash install.sh
```

查看配置：同安装

### 2.3 x-ui

[Github](https://github.com/vaxilu/x-ui)

支持协议：

1. vmess
1. vless
1. trojan
1. shadowsocks
1. dokodemo-door
1. socks
1. http

安装：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

查看配置：`x-ui`

## 3.客户端

### 3.1 Windows

#### 3.1.1 v2rayN

[Github](https://github.com/2dust/v2rayN)

<img src="/科学上网/科学上网01.png" />

<img src="/科学上网/科学上网02.png" />

<img src="/科学上网/科学上网03.png" />

<img src="/科学上网/科学上网04.png" />

<img src="/科学上网/科学上网05.png" />

#### 3.1.2 Clash for Windows

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

