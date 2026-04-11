# cfnat-cfd-cfdata

---



## cfnat

```
Usage of cfnat:
  -addr string
        本地监听的 IP 和端口 (default "0.0.0.0:1234")
  -code int
        HTTP/HTTPS 响应状态码 (default 200)
  -colo string
        筛选数据中心例如 HKG,SJC,LAX (多个数据中心用逗号隔开,留空则忽略匹配)
  -delay int
        有效延迟（毫秒），超过此延迟将断开连接 (default 300)
  -domain string
        响应状态码检查的域名地址 (default "cloudflaremirrors.com/debian")
  -ipnum int
        提取的有效IP数量 (default 20)
  -ips string
        指定生成IPv4还是IPv6地址 (4或6) (default "4")
  -num int
        目标负载 IP 数量 (default 5)
  -port int
        转发的目标端口 (default 443)
  -random
        是否随机生成IP，如果为false，则从CIDR中拆分出所有IP (default true)
  -task int
        并发请求最大协程数 (default 100)
  -tls
        是否为 TLS 端口 (default true)
```

2024年9月8日 已经更新对windows 7 以及 安卓termux支持
2024年9月22日 修复一些bug，删除一些不必要的参数
2024年10月22日 更改状态检测机制，一次检查失败直接切换IP,新增armv5 v6 v7 独立版本
2024年10月30日 修复了一些bug，状态检测改成2次连续失败再切换，此版本后续基本没有再更新的必要
2025年12月10日 通过引入 IPManager 封装状态管理 + Context 控制 goroutine 生命周期，解决了旧版本全局变量不安全、goroutine 无法优雅退出的问题，同时进行了代码模块化拆分和 API 现代化升级。

---



## cfd

```
Usage of ./cfd:
  -file string
        IP地址文件名称 (default "ip.txt")
  -max int
        最大允许延迟（毫秒），超过此延迟将断开连接 (default 500)
  -min int
        最小允许延迟（毫秒），低于此延迟的连接将被视为有效 (default 300)
  -multi
        是否启用多目标模式 (default true)
  -num int
        转发的IP数量 (default 10)
  -task int
        并发请求最大协程数 (default 100)./cfd -multi=false 就变成了单一IP转发模式，属于垃圾堆里淘金的那种意思。
```

> 为了方便国内更好的使用cloudflared(原argo tunnel)
>
> 做了一个简易的cloudflared HTTP2协议回源的优选方案
>
> 其中原理是给cloudflared的服务强制指定host ip到127.0.0.1 并且本地转发7844端口到优选的cloudflared的回源ip
>
> 其中hosts需要添加下面的解析, 
>
> 然后再运行cfd优选程序，启动转发完毕后再运行cloudflared
> 其中cloudflared需要指定回源协议HTTP2
> cloudflared --protocol http2

```
127.0.0.1 region1.v2.argotunnel.com
127.0.0.1 region2.v2.argotunnel.com
127.0.0.1 us-region1.v2.argotunnel.com
127.0.0.1 us-region2.v2.argotunnel.com
```

---



## cfdata

Cloudflare CDN 网络 IP 扫描与性能测试工具
针对 Cloudflare CDN 网络进行 IP 扫描与性能测试，输出详细的测试结果和横向柱状图，方便用户直观分析 Cloudflare CDN 网络的表现。
该工具的设计目的是帮助用户筛选出适合自己的数据中心，并提取优质的 IP 段以供 cfnat 使用。
工具由 DeepSeek R1 构建，内置源码！
**开箱即用，傻瓜化操作**！😁
