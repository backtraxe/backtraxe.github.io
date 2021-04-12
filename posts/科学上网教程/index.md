# 科学上网教程


正确访问 Google, Youtube, Twitter, Instagram, reddit ...

<!--more-->

## 脚本

### 七合一共存脚本

[地址](https://github.com/mack-a/v2ray-agent)

支持：

1. VLESS + TCP + TLS
1. VLESS + TCP + XTLS
1. VLESS + WS + TLS
1. VMess + TCP + TLS
1. VMess + WS + TLS
1. Trojan
1. Trojan-Go + WS

安装：

`wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh`

查看配置：`vasma`

### VLESS + XTLS 一键安装脚本

[地址](https://github.com/wulabing/Xray_onekey)

支持：

- VLESS + TCP + XTLS/TLS
- VLESS + TCP + XTLS/TLS 及 VLESS + WS + TLS 回落并存模式

安装：

`wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/Xray_onekey/main/install.sh" && chmod +x install.sh && bash install.sh`

查看配置：同安装指令

### Trojan 和 V2ray xray 一键安装脚本


### 网络跳跃

https://s.hijk.art/xray.sh

## 客户端

### Windows

#### V2rayN

**v2rayN 更新 4.x 版本后利用分流实现 PAC 效果**

[参考文章](https://github.com/2dust/v2rayN/issues/1366)

1. 下载`geoip.dat`和`geosite.dat`文件（https://github.com/Loyalsoldier/v2ray-rules-dat），然后覆盖 v2rayN 根目录下的同名文件。
2. 打开 v2RayN，点击`设置`->`参数设置`->`v2rayN设置`，勾选`更新Core时忽略Geo文件`。

<img src="/科学上网/科学上网01.png" />

<img src="/科学上网/科学上网02.png" />

3. 打开 v2RayN，点击`设置`->`路由设置`，勾选`启用路由高级功能`，点击`高级功能`->`添加规则集`->`导入规则`->`从剪贴板中导入规则`，导入如下规则，别名随意设置。

<img src="/科学上网/科学上网03.png" />

<img src="/科学上网/科学上网04.png" />

<img src="/科学上网/科学上网05.png" />

<img src="/科学上网/科学上网06.png" />

**PAC with ADBlock**

```json
[
  {
    "outboundTag": "block",
    "domain": [
      "geosite:category-ads-all",
      "geosite:win-spy"
    ]
  },
  {
     "outboundTag": "proxy",
     "ip": [
       "geoip:telegram"
     ],
     "domain": [
       "geosite:gfw"
    ]
  },
  {
    "port": "0-65535",
    "outboundTag": "direct"
  }
]
```

**PAC**

```json
[
  {
     "outboundTag": "proxy",
     "ip": [
       "geoip:telegram"
     ],
     "domain": [
       "geosite:gfw"
    ]
  },
  {
    "port": "0-65535",
    "outboundTag": "direct"
  }
]
```

**Global**

```json
[
  {
    "outboundTag": "proxy",
    "port": "0-65535",
  }
]
```

#### Clash for Windows

[下载](https://github.com/Fndroid/clash_for_windows_pkg/releases)

#### Shadowsocks for Windows

[下载](https://github.com/shadowsocks/shadowsocks-windows/releases)

### 安卓

#### V2rayNG

支持：

- VMess
- VLess
- Trojan

[下载](https://github.com/2dust/v2rayNG/releases)

#### igniter

支持：

- Trojan

[下载](https://github.com/trojan-gfw/igniter/releases)

#### Clash for Android

支持：

- VMess
- VLess
- Trojan
- Shadowsocks

[下载](https://github.com/Kr328/ClashForAndroid/releases)

#### Shadowsocks for Android

[下载](https://github.com/shadowsocks/shadowsocks-android/releases)
