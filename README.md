# 🚀 shadowrocket-china-list

你好，欢迎来到Vorfeed的欢乐满满翻墙提高班。

本项目是**自建DNS、浏览器、小火箭**统一分流规则的一部分，通过严格同步[dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)，保证多端分流规则的一致性。

其他部分绝赞更新中。

## 规则说明

参照dnsmasq-china-list中Makefile的逻辑，转换为对应的小火箭白名单规则。

每套规则提供二个链接，第一个需要代理才能稳定访问，第二个可以直接访问，但会延迟12小时。

- **accelerated-domains.china.list**：

  主规则，包含10w多个适合直连的域名。据说以前有性能问题，但现在没什么感觉。
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/accelerated-domains.china.list](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/accelerated-domains.china.list)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/accelerated-domains.china.list](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/accelerated-domains.china.list)
- **apple.china.list**：

  可选规则，推荐使用。Apple相关域名在国内有CDN加速，理论上适合直连。但如果你发现解析到了国外，就不要使用。
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/apple.china.list](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/apple.china.list)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/apple.china.list](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/apple.china.list)
- **google.china.list**：

  可选规则，不推荐使用。你要是发现熟悉的网页上，一些图标突然变成了文字，就是直连fonts.gstatic.com不稳定导致的。
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/google.china.list](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/google.china.list)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/google.china.list](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/google.china.list)
>这套规则没有转换bogus-nxdomain.china.conf，是因为正确配置的白名单分流，不会解析到NXDOMAIN。
>
>NXDOMAIN只有在直连不适合直连的域名时才会出现，而白名单分流时，不适合直连的域名应该直接走代理，不会在本地发生DNS查询。

## 使用方法

- 复制**小火箭白名单基础配置**链接：
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/whitelist_base.conf](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/whitelist_base.conf)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/whitelist_base.conf](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/whitelist_base.conf)
  
  任选其一，在 `小火箭 -> 配置` 里添加。注意这个配置里有些关键选项，界面上配不了，但不配又会导致DNS泄露之类的问题。如果你想和已有配置合并，建议使用 `编辑纯文本` 方式比对。
- `小火箭 -> 配置 -> 模块`，按需添加 [规则说明](#规则说明) 里的规则。
- 配置完成，为确保规则正确生效，推荐进行后续的验证操作。

## 验证配置

 - 建议关闭所有后台APP，以免影响测试结果
 - 在 `小火箭 -> 数据 -> DNS` 里，打开 `启用日志记录`
 - 确认 `全局路由` 已选择 `配置`，开启代理
 - `小火箭 -> 配置 -> 测试规则`，进行以下测试：
   
   HOST | 类型 | 策略
   --- | --- | ---
   baidu.com | DOMAIN-SUFFIX | DIRECT
   x.com | FINAL | PROXY
   223.5.5.5 | GEOIP | DIRECT
   8.8.8.8 |  FINAL | PROXY
 - 如果使用了Apple规则，额外测试：
   
   HOST | 类型 | 策略
   --- | --- | ---
   apps.apple.com | DOMAIN-SUFFIX | DIRECT
 - 如果使用了Google规则，额外测试：
   
   HOST | 类型 | 策略
   --- | --- | ---
   fonts.gstatic.com | DOMAIN-SUFFIX | DIRECT
 - 检查DNS日志记录，测试中用到的域名均**不应该**出现在日志中，否则可能为DNS泄露。

## 问题反馈

任何问题欢迎在 [Issues](https://github.com/OriginVorfeed/shadowrocket-china-list/issues) 中反馈。
