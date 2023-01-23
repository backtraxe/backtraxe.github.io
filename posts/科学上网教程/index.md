# 科学上网教程


<!--more-->

## 1.准备工作

- VPS：`CN2 GIA-E` > `CN2 GIA` > `CN2 GT` > `CN2` > `KVM`
  - [BandwagonHost](https://bandwagonhost.com/)
  - [Vultr](https://www.vultr.com/)
- 域名
  - 免费：[Freenom](https://www.freenom.com/)
  - 付费：[NameSilo](https://www.namesilo.com/)
- DNS & CDN：
  - [Cloudflare](https://www.cloudflare.com/)

## 2.服务端

### 2.1 Xray_onekey

[Github](https://github.com/wulabing/Xray_onekey)

安装：

```bash
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/Xray_onekey/main/install.sh" && chmod +x install.sh && bash install.sh
```

查看配置：

```bash
~/install.sh
```

### 2.2 x-ui

[Github](https://github.com/vaxilu/x-ui)

安装：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

查看配置：

```bash
x-ui
```

## 3.客户端

### 3.1 Windows

#### v2rayN

[Github](https://github.com/2dust/v2rayN)

<img src="/imgs/科学上网/科学上网01.png" />

<img src="/imgs/科学上网/科学上网02.png" />

<img src="/imgs/科学上网/科学上网03.png" />

<img src="/imgs/科学上网/科学上网04.png" />

<img src="/imgs/科学上网/科学上网05.png" />

#### Clash for Windows

[Github](https://github.com/Fndroid/clash_for_windows_pkg/releases)



### 3.2 安卓

#### v2rayNG

- [Github](https://github.com/2dust/v2rayNG/releases)
- [Google Play](https://play.google.com/store/apps/details?id=com.v2ray.ang)

#### Clash for Android

- [Github](https://github.com/Kr328/ClashForAndroid/releases)
- [Google Play](https://play.google.com/store/apps/details?id=com.github.kr328.clash)

### 3.3 iOS

#### Shadowrocket

### 3.4 macOS

#### Clash for Windows

## 参考

1. [在 WSL 2 中访问主机代理 - Geek 成长录](https://blog.rogerkung-win.top/posts/38819/)
1. [WSL2内使用windows的v2ray代理配置方式。 - 知乎](https://zhuanlan.zhihu.com/p/414627975)
