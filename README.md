# 🚀 shadowrocket-china-list

你好，欢迎来到 Vorfeed 的欢乐满满翻墙提高班。

本项目是 **SmartDNS、Xray、Shadowrocket、SingBox** 统一分流规则的一部分，通过严格同步 [dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)，保证多端分流规则的一致性。

- SmartDNS 规则：[smartdns-china-list](https://github.com/OriginVorfeed/smartdns-china-list)

- Xray 规则：[xray-china-list](https://github.com/OriginVorfeed/xray-china-list)

- SingBox 规则：[singbox-china-list](https://github.com/OriginVorfeed/singbox-china-list)

## 规则说明

参照 dnsmasq-china-list 中 [Makefile](https://github.com/felixonmars/dnsmasq-china-list/blob/master/Makefile) 的逻辑，转换为对应的**小火箭白名单规则**。

每套规则提供2个链接，第1个需要代理才能稳定访问，第2个可以直接访问，但会延迟12小时。

- **accelerated-domains.china.list**：

  主规则，包含10w多个适合直连的域名。据说以前有性能问题，但现在没什么感觉。
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/accelerated-domains.china.list](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/accelerated-domains.china.list)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/accelerated-domains.china.list](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/accelerated-domains.china.list)
- **apple.china.list**：

  可选规则，推荐使用。Apple 相关域名在国内有 CDN 加速，理论上适合直连。但如果你发现解析到了国外，就不要使用。
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/apple.china.list](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/apple.china.list)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/apple.china.list](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/apple.china.list)
- **google.china.list**：

  可选规则，不推荐使用。你要是发现熟悉的网页上，一些图标突然变成了文字，就是直连 fonts.gstatic.com 不稳定导致的。
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/google.china.list](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/google.china.list)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/google.china.list](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/google.china.list)
>这套规则没有转换 bogus-nxdomain.china.conf，是因为正确配置的白名单分流，不会解析到 NXDOMAIN。
>
>NXDOMAIN 只有在直连不适合直连的域名时才会出现，而白名单分流时，不适合直连的域名应该直接走代理，不会在本地发生 DNS 查询。

## 使用方法

- 复制**小火箭白名单基础配置**链接：
  - [https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/whitelist_base.conf](https://raw.githubusercontent.com/OriginVorfeed/shadowrocket-china-list/master/whitelist_base.conf)
  - [https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/whitelist_base.conf](https://cdn.jsdelivr.net/gh/OriginVorfeed/shadowrocket-china-list@master/whitelist_base.conf)
  
  任选其一，在 `小火箭 -> 配置` 里添加。注意这个配置里有些关键选项，界面上配不了，但不配又会导致DNS泄露之类的问题。如果你想和已有配置合并，建议使用 `编辑纯文本` 方式比对。
- `小火箭 -> 配置 -> 模块`，按需添加 [规则说明](#规则说明) 里的规则。
- 配置完成，为确保规则正确生效，推荐进行后续的验证操作。

## 验证配置

 - 建议关闭所有后台 APP，以免影响测试结果
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
 - 检查 DNS 日志记录，测试中用到的域名均**不应该**出现在日志中，否则可能为 DNS 泄露。

## 常见问题

- **一定要用小火箭模块的形式添加吗？**

  并不一定。如果你对自由组合3套规则不感兴趣，完全可以用配置的形式添加。

  然后在 `小火箭 -> 配置 -> whitelist_base.conf -> 通用 -> 包含配置` 里，选择你想用的规则。

  但如果你想和数据源 dnsmasq-china-list 一样组合使用，或者搭配别的规则实现去广告之类的功能，目前只有模块这一种方案。

- **如何覆盖模块中的规则？**

  由于模块的优先级高于配置，要覆盖模块中的规则，只能再建一个模块，把需要的规则写在里面，然后把这个新模块移到最上面。

  注意你只有在真正需要**覆盖**模块中的规则时，才需要执行上述操作。如果你只是想添加一些不冲突的规则，直接修改 whitelist_base.conf 即可。

## 问题反馈

任何问题欢迎在 [Issues](https://github.com/OriginVorfeed/shadowrocket-china-list/issues) 中反馈。
