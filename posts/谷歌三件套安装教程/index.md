# 谷歌三件套安装教程


谷歌三件套包括 Google Play 商店（Google Play Store）、Google Play 服务（Google Play Services）和Google 服务框架（Google Services Framework），只有正确安装了这三件套才能访问 Google Play 商店和使用需要 Google Play 服务的 APP。

<!--more-->

## 安装

### 一键安装



### 手动安装

1. 访问 [APKMirror](https://www.apkmirror.com/)。
2. 搜索`Google Services Framework`，根据安卓版本选择对应的版本下载并安装。（示例：`Google Services Framework 10`）
3. 搜索`Google Play Services`，选择最新版并进入，选择`arm64-v8a + armeabi-v7a`、对应的安卓版本、`nodpi`，下载并安装。
4. 搜索`Google Play Store`，选择最新版下载并安装。
5. 若三件套都安装完，并且正确科学上网后还是打不开`Google Play 商店`，尝试下载旧版本`Google Play Services`安装。

> 不推荐使用`beta`版本。

### Google Play Services 版本号

版本号示例：`20.50.66 (100400-351698872)`，其中`100400`说明：

- 第 1、2 位表示安卓版本

   `00` - `Android 4.1`

   `02` - `Android 5.0`

   `04` - `Android 6.0`

   `05` - `Wear OS`

   `08` - `Android TV`

   `10` - `Android 9.0`

   `12` - `Android 10`

   `15` - `Android 11`

- 第 3、4 位表示CPU架构

   `03` - `armeabi-v7a`

   `04` - `armeabi-v7a + arm64-v8a`

   `07` - `x86`

   `08` - `x86 + x86_64`

- 第 5、6 位表示屏幕 DPI

   `00` - `nodpi`

   `02` - `160dpi`

   `04` - `240dpi`

   `06` - `320dpi`

   `08` - `480dpi`

则`100400`指`Android 9.0`、`arm64-v8a`和`nodpi`。

