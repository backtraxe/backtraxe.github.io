# 科学上网


Google, Youtube, Twitter, Instagram, reddit ...

<!--more-->

v2rayN 更新 4.x 版本后利用分流实现 PAC 效果：

[参考文章](https://github.com/2dust/v2rayN/issues/1366)

1. 下载`geoip.dat`和`geosite.dat`文件（https://github.com/Loyalsoldier/v2ray-rules-dat），然后覆盖 v2rayN 根目录下的同名文件。
2. 打开 v2RayN，点击`设置`->`参数设置`->`v2rayN设置`，勾选`更新Core时忽略Geo文件`。
3. 打开 v2RayN，点击`设置`->`路由设置`，勾选`启用路由高级功能`，点击`高级功能`->`添加规则集`->`导入规则`->`从剪贴板中导入规则`，导入如下规则，别名随意设置。

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

