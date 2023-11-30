---
title: Cloudflare 加入 AS112 项目，以帮助互联网处理被错误引导的 DNS 查询
date: 2023-11-28T09:34:01.000Z
updated: 2023-11-28T09:34:01.000Z
published: 2022-12-15T09:34:01.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://blog.cloudflare.com/zh-cn/the-as112-project-zh-cn/
  hostname: blog.cloudflare.com
  author: Hunts Chen
  original_title: Cloudflare 加入 AS112 项目，以帮助互联网处理被错误引导的 DNS 查询
  original_lang: en

---

![Cloudflare is joining the AS112 project to help the Internet deal with misdirected DNS queries](image2-32.png)

今天，我们隆重宣布加入 [AS112 项目](https://www.as112.net/)，成为这个由社区运营、松散协调的任播 DNS 服务器部署的运营商，这些 DNS 服务器主要应答被错误定向并在互联网上造成大量不必要负载的反向 DNS 查找查询。

随着 Cloudflare 全球网络的加入，我们可以极大地提升这个分布式公共服务的稳定性、可靠性和性能。

## 什么是 AS112

AS112 项目是一个社区运营的重要网络服务，旨在处理私有使用地址的反向 DNS 查询（这些地址永远不会出现在公共 DNS 系统中）。例如，在这篇博客文章发表之前的 7 天里，Cloudflare 的 1.1.1.1 解析器收到了超过 980 亿次此类查询—— _这些查询在域名系统中都没有有用的应答_。

为了帮助理解，让我们回顾一下有关历史。互联网协议(IP)地址对于网络通信至关重要。许多网络使用保留为私有的 IPv4 地址，网络上的设备可以通过网络地址转换(NAT)连接到互联网，这个过程在传输信息之前将一个或多个本地的私有地址映射到一个或多个全球 IP 地址，反之亦然。

您的家庭网络路由器很可能在为您做这件事。您可能会发现，在家里，您的电脑有一个类似 192.168.1.42 的 IP 地址。这是一个私有地址的例子，它可以在家里使用，但不能在公共互联网上使用。您的家庭路由器将它通过 NAT 转换为 ISP 分配给您的家庭并可在互联网上使用的地址。

以下是 [RFC 1918](https://www.rfc-editor.org/rfc/rfc1918) 指定的“私有”地址。

| 地址块 | 地址范围 | IP 地址数量 |
| --- | --- | --- |
| 10.0.0.0/8 | 10.0.0.0 – 10.255.255.255 | 16,777,216 |
| 172.16.0.0/12 | 172.16.0.0 – 172.31.255.255 | 1,048,576 |
| 192.168.0.0/16 | 192.168.0.0 – 192.168.255.255 | 65,536 |

(保留的私有 IPv4 网络地址范围)

尽管保留的地址本身被屏蔽，永远不会出现在公共互联网上，但私有环境中的设备和程序偶尔也会发起与这些地址对应的 DNS 查询。这种查询被称为“反向查找”，因为它们在询问 DNS 是否存在一个与某个地址相关的名称。

### 反向 DNS 查找

反向 DNS 查找是与更常用的 DNS 查找相反的过程(后者每天被用于将 [www.cloudflare.com](https://www.cloudflare.com/) 这样的地址转换为对应的 IP 地址)。这种查询用于查找与给定 IP 地址相关联的域名，特别是与路由器和交换机相关联的那些地址。例如，网络管理员和研究人员使用反向查找来帮助理解数据包在网络中经过的路径，而理解有意义的名称比理解无意义的数字要容易得多。

反向查找是通过向 DNS 服务器查询指针记录 ([PTR](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/))来完成的。PTR 记录中，IP 地址的各段被反向存储 ，并在末尾加上“.in-addr.arpa”。例如，IP 地址 192.0.2.1 会有一个以 1.2.0.192.in-addr.arpa 形式存储的 PTR 记录。在 IPv6 中，PTR 记录存储在 “.ip6.arpa”域内，而不是“.in-addr. Arpa”。下面是使用 dig 命令行工具进行的一些查询示例。

```
# 查找与 IPv4 地址 172.64.35.46 关联的域名
# “+short” 选项使其仅输出简短的结果
$ dig @1.1.1.1 PTR 46.35.64.172.in-addr.arpa +short
hunts.ns.cloudflare.com.

# 或者使用快捷方式 “-x” 进行反向查找
$ dig @1.1.1.1 -x 172.64.35.46 +short
hunts.ns.cloudflare.com.

# 查找与 IPv6 地址 2606:4700:58::a29f:2c2e 关联的域名
$ dig @1.1.1.1 PTR e.2.c.2.f.9.2.a.0.0.0.0.0.0.0.0.0.0.0.0.8.5.0.0.0.0.7.4.6.0.6.2.ip6.arpa. +short
hunts.ns.cloudflare.com.

# 或者使用快捷方式 “-x” 进行反向查找
$ dig @1.1.1.1 -x 2606:4700:58::a29f:2c2e +short  
hunts.ns.cloudflare.com.
```

### 私有地址给 DNS 造成的问题

私有地址仅具有本地意义，无法通过公共 DNS 进行解析。换句话说，公共 DNS 无法为一个没有全局意义的问题提供有用的应答。因此，对于网络管理员来说，确保对私有地址的查询在本地回答是一种良好的做法。然而，此类查询在公共 DNS 中遵循正常的委托路径，而不是在网络中得到回应，这种情况并不罕见。这造成了不必要的负载。

按照私有使用的定义，这些 IP 地址在公共领域没有所有权，因此没有任何权威 DNS 服务器应答此类查询。在最开始的时候，根服务器回应所有这些类型的查询，因为它们支持 IN-ADDR.ARPA 区域。

随着时间推移，由于私有地址的广泛部署和互联网的持续增长，IN-ADDR.ARP DNS 基础设施上的流量越来越大，这些垃圾查询带来的的负载开始引起一些关注。因此，有人提出移除与私有地址相关的 IN-ADDR.ARPA 查询。随后，在根服务器运营商的一次不公开会议上，提出使用任播技术来为以上想法分布权威 DNS 服务。最终，AS112 服务面世了，为这些垃圾查询提供一个替代目标。

### AS112 应运而生

为了解决这个问题，互联网社区设立了特殊的 DNS 服务器，称为“黑洞服务器”，作为权威名称服务器，响应地址块 10.0.0.0/8、172.16.0.0/12、192.168.0.0/16 和链路本地地址 169.254.0.0/16(也只有本地意义)的反向查找。由于相关区域直接委托给黑洞服务器，这种方法被称为“直接委托”。

该项目最早建立的两个黑洞服务器是 blackhole-1.iana.org 和 blackhole-2.iana.org。

任何服务器，包括 DNS 名称服务器，都需要一个 IP 地址才能访问。该 IP 地址还必须与一个自治系统编号(ASN)相关联，以便网络能够识别其他网络并将数据包路由到该 IP 地址的目的地址。为了解决这个问题，将创建一个新的权威 DNS 服务，但要使其工作，社区必须为服务器指定 IP 地址，并提供一个 AS 编号以使其可用，以便网络运营商用于访问(或提供)这个新服务。

选定的 AS 编号(由美国互联网编号注册机构提供)与项目同名，即 112。它最初是由少数几家根服务器运营商发起的，随后扩大为一组志愿名称服务器运营商，其中包括很多其他组织。他们运行以上黑洞服务器任播实例，共同组成一个分布式的接收器，用于处理发送到公共互联网的私有网络和链接本地地址反向 DNS 查找。

对私有地址的反向 DNS 查找将看到如下示例响应，其中名称服务器 blackhole-1.iana.org 是权威的，并表示该名称不存在，在 DNS 响应中由 **NXDOMAIN** 表示。

```
$ dig @blackhole-1.iana.org -x 192.168.1.1 +nord

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 23870
;; flags: qr aa; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;1.1.168.192.in-addr.arpa.INPTR

;; AUTHORITY SECTION:
168.192.in-addr.arpa.10800INSOA168.192.in-addr.arpa. nobody.localhost. 42 86400 43200 604800 10800
```

在项目开始时，节点运营商以直接委托的方式设置服务 ([RFC 7534](https://datatracker.ietf.org/doc/html/rfc7534))。然而，向该服务添加委托需要更新所有 AS112 服务器，这在一个仅松散协调的系统中很难确保。随后通过 [RFC 7535](https://datatracker.ietf.org/doc/html/rfc7535) 引入了另一种使用 DNAME 重定向的方法，允许在不重新配置黑洞服务器的情况下向系统添加新区域。

#### **直接代理**

在这种方法中，DNS 区域直接委托给黑洞服务器。

[RFC 7534](https://datatracker.ietf.org/doc/html/rfc7534) 定义了反向查找区域的静态集合，AS112 名称服务器应该权威地应答这些反向查找区域。如下：

-   10.in-addr-arpa
-   16.172.in-addr.arpa
-   17.172.in-addr.arpa
-   18.172.in-addr.arpa
-   19.172.in-addr.arpa
-   20.172.in-addr.arpa
-   21.172.in-addr.arpa
-   22.172.in-addr.arpa
-   23.172.in-addr.arpa
-   24.172.in-addr.arpa
-   25.172.in-addr.arpa
-   26.172.in-addr.arpa
-   27.172.in-addr.arpa
-   28.172.in-addr.arpa
-   29.172.in-addr.arpa
-   30.172.in-addr.arpa
-   31.172.in-addr.arpa
-   168.192.in-addr.arpa
-   254.169.in-addr.arpa (对应 IPv4 链路本地地址块)

这些区域的区域文件非常简单，因为除了所需的 SOA 和 NS 记录外，它们基本上是空的。该区域文件的一个模板定义如下：

```
  ; db.dd-empty
   ;
   ; Empty zone for direct delegation AS112 service.
   ;
   $TTL    1W
   @  IN  SOA  prisoner.iana.org. hostmaster.root-servers.org. (
                                  1         ; serial number
                                  1W      ; refresh
                                  1M      ; retry
                                  1W      ; expire
                                  1W )    ; negative caching TTL
   ;
          NS     blackhole-1.iana.org.
          NS     blackhole-2.iana.org.
```

直接委托名称服务器的 IP 地址由单一 IPv4 前缀 192.175.48.0/24 和 IPv6 前缀 2620:4f:8000::/48 覆盖。

| 域名服务器 | IPv4 地址 | IPv6 地址 |
| --- | --- | --- |
| blackhole-1.iana.org | 192.175.48.6 | 2620:4f:8000::6 |
| blackhole-2.iana.org | 192.175.48.42 | 2620:4f:8000::42 |

#### **DNAME 重定向**

首先，什么是 DNAME？ 由 [RFC 6672](https://www.rfc-editor.org/rfc/rfc6672) 引入的 DNAME 记录，即委托名称记录，为域名树的一整个子树创建一个别名。相比之下，CNAME 记录为单个域而不是其子域创建别名。对于接收到的 DNS 查询， DNAME 记录指示名称服务器将出现在左手边的全部信息(所有者名称)替换为右手边的信息(别名名称)。被替换的查询名称，如 CNAME，可能位于区域内，也可能位于区域外。

类似于 CNAME 记录，DNS 查找将继续使用替换后的名称重新查找。例如，如果有如下两个 DNS 区域：

```
# zone: example.com
www.example.com.A203.0.113.1
foo.example.com.DNAMEexample.net.

# zone: example.net
example.net.A203.0.113.2
bar.example.net.A203.0.113.3
```

查询解析情况看起来如下：

| 查询(类型+名称) | 替换 | 最终结果 |
| --- | --- | --- |
| A www.example.com | (无 DNAME，不适用) | 203.0.113.1 |
| DNAME foo.example.com | (不适用于所有者名称本身) | example.net |
| A foo.example.com | (不适用于所有者名称本身) | <NXDOMAIN> |
| A bar.foo.example.com | bar.example.net | 203.0.113.2 |

[RFC 7535](https://datatracker.ietf.org/doc/html/rfc7535) 指定添加另一个特殊区域，empty.as112.arpa，以支持 AS112 节点的 DNAME 重定向。当有新域要添加时，AS112 节点运营商不需要更新其配置：相反，该区域的的父节点将为新域建立目标域 empty.as112.arpa 的 DNAME 记录。这种重定向(可以被缓存和重用)会导致客户端将未来的查询发送到对目标区域具有权威的黑洞服务器。

注意，黑洞服务器本身不需要支持 DNAME 记录，但它们需要配置根服务器将查询重定向到的新区域。考虑到可能存在现有节点运营商由于某些原因没有更新其名称服务器配置的情况，为了不造成服务中断，该区域被委托给一个新的黑洞服务器——blackhole.as112.arpa。

这个名称服务器使用了一对新的 IPv4 和 IPv6 地址，192.31.196.1 和 2001:4:112::1，以便涉及 DNAME 重定向的查询将只会到达那些也设置了新名称服务器的实体运营的节点。因为没有必要让所有的 AS112 参与者重新配置他们的服务器以服务 empty.as112.arpa，以便这一新服务器为这个系统工作，这一方法兼容系统整体的松散协调。

empty.as112.arpa 的区域文件定义如下：

```
   ; db.dr-empty
   ;
   ; Empty zone for DNAME redirection AS112 service.
   ;
   $TTL    1W
   @  IN  SOA  blackhole.as112.arpa. noc.dns.icann.org. (
                                  1         ; serial number
                                  1W      ; refresh
                                  1M      ; retry
                                  1W      ; expire
                                  1W )    ; negative caching TTL
   ;
          NS     blackhole.as112.arpa.
```

新 DNAME 重定向名称服务器的地址由单一 IPv4 前缀 192.31.196.0/24 和 IPv6 前缀 2001:4:112::/48 覆盖。

| 域名服务器 | IPv4 地址 | IPv6 地址 |
| --- | --- | --- |
| blackhole.as112.arpa | 192.31.196.1 | 2001:4:112::1 |

#### **节点识别**

[RFC 7534](https://datatracker.ietf.org/doc/html/rfc7534) 建议每个 AS112 节点也承载以下元数据区域：hostname.as112.net 和 hostname.as112.arpa。

这些分区仅承载 TXT 记录，用于查询有关 AS112 节点的元数据信息。Cloudflare 节点的区域文件看起来是这样的：

```
$ORIGIN hostname.as112.net.
;
$TTL    604800
;
@       IN  SOA     ns3.cloudflare.com. dns.cloudflare.com. (
                       1                ; serial number
                       604800           ; refresh
                       60               ; retry
                       604800           ; expire
                       604800 )         ; negative caching TTL
;
            NS      blackhole-1.iana.org.
            NS      blackhole-2.iana.org.
;
            TXT     "Cloudflare DNS, <DATA_CENTER_AIRPORT_CODE>"
            TXT     "See http://www.as112.net/ for more information."
;

$ORIGIN hostname.as112.arpa.
;
$TTL    604800
;
@       IN  SOA     ns3.cloudflare.com. dns.cloudflare.com. (
                       1                ; serial number
                       604800           ; refresh
                       60               ; retry
                       604800           ; expire
                       604800 )         ; negative caching TTL
;
            NS      blackhole.as112.arpa.
;
            TXT     "Cloudflare DNS, <DATA_CENTER_AIRPORT_CODE>"
            TXT     "See http://www.as112.net/ for more information."
;
```

## 协助 AS112 帮助互联网

AS112 项目有助于减轻公共 DNS 基础设施的负载，对维护互联网的稳定和效率起着至关重要的作用。成为这个项目的一部分，符合 Cloudflare 帮助建立更好的互联网的使命。

Cloudflare 是世界上最快的全球任播网络之一，并运营着最大、高性能和可靠的 DNS 服务之一。我们为全球数以百万计的互联网资产运行权威 DNS 服务。我们还运营着注重隐私和性能的公共 DNS 解析器 1.1.1.1 服务。考虑到我们的网络覆盖范围和运营规模，我们相信可以对 AS112 项目做出有意义的贡献。

## 我们的网络是如何打造的

我们过去曾数次公开谈论过 Cloudflare 内部构建的权威 DNS 服务器软件 rrDNS，但没有详细介绍我们为驱动 Cloudflare 公共解析器 1.1.1.1 而打造的软件。趁此机会，我们希望简单介绍下用于打造 1.1.1.1 的软件，因为这个 AS112 服务是在同一平台上建立起来的。

### 用于 DNS 工作负载的平台

![](image1-32.png)

我们创建了一个运行 DNS 工作负载的平台。今天，这个平台驱动着 1.1.1.1、1.1.1.1 for Families、Oblivious DNS over HTTPS (ODoH)、Cloudflare WARP 和 Cloudflare Gateway。

平台的核心部分是一台非传统的 DNS 服务器，它有一个内置的 DNS 递归解析器和一个转发器，负责向其他服务器转发查询。它由四个关键模块组成：

1.  一个高效的监听模块，用于接收传入请求的连接。
2.  一个决定如何解析查询的查询路由模块。
3.  一个指挥模块，负责确定与上游服务器交换 DNS 消息的最佳方式。
4.  一个用于承载访客应用程序的沙盒环境。

DNS 服务器本身不包含任何业务逻辑，取而代之，运行在沙盒环境中的访客应用程序可以实现具体的业务逻辑，如请求过滤、查询处理、日志记录、攻击缓解、缓存清除等。

服务端是用 Rust 编写的，而沙盒环境是构建在 WebAssembly 运行时之上的。Rust 和 WebAssembly 两者结合使我们能够实现高效的连接处理、请求过滤和查询调度模块，同时具备安全高效地实现自定义业务逻辑的灵活性。

主机公开一组名为 hostcall 的 API，以便访客应用程序完成各种任务。您可以将它们想象为 Linux 上的系统调用。以下是 hostcall 提供的一些示例函数：

-   获取当前 UNIX 时间戳
-   查找 IP 地址的地理位置数据
-   生成异步任务
-   创建本地套接字
-   将 DNS 查询转发到指定的服务器
-   注册沙盒钩子的回调函数
-   读取当前请求信息，并写入响应
-   发送应用日志、指标数据点和范围/事件

DNS 请求的生命周期分为几个阶段。请求阶段是处理过程中的一个阶段，此时可以调用沙盒应用程序来改变请求解析的过程。每个访客应用程序都可以为每个阶段注册回调函数。

![](image3-20.png)

### AS112 访客应用程序

AS112 服务是一个用 Rust 编写并编译为 WebAssembly 的访客应用程序。[RFC 7534](https://www.rfc-editor.org/rfc/rfc7534) 和 [RFC 7535](https://www.rfc-editor.org/rfc/rfc7535) 中列出的区域被加载为内存中的静态区域加载，并作为树形数据结构进行索引。传入的查询通过在区域树中查找项目在本地回答。

应用清单文件中添加了一个路由器设置，用于告诉主机，访客应用程序应该处理什么类型的 DNS 查询。还添加了一个 fallback\_action 设置，声明预期的回退行为。

```
# 声明应用程序处理的查询类型。
router = [
    # 该应用程序负责所有的 AS112 IP 前缀。
    "dst in { 192.31.196.0/24 192.175.48.0/24 2001:4:112::/48 2620:4f:8000::/48 }",
]

# 如果应用程序处理查询失败，应该返回 servfail。
fallback_action = "fail"
```

然后，客户应用程序及其清单文件将通过部署管道进行编译和部署，后者利用 [Quicksilver](https://blog.cloudflare.com/introducing-quicksilver-configuration-distribution-at-internet-scale/) 在全球范围内存储和复制资产。

访客应用程序现在已经启动并运行，但是指向新 IP 前缀的 DNS 查询流量如何到达 DNS 服务器呢？每次添加新的访客应用程序时都必须重启 DNS 服务器吗？当然不必。我们使用之前开发和部署的一个软件，叫做 [Tubular](https://blog.cloudflare.com/tubular-fixing-the-socket-api-with-ebpf/)。它允许我们动态更改服务的地址。借助 Tubular，目的地为 AS112 业务 IP 地址前缀的传入数据包被分配到正确的 DNS 服务器进程，而不需要对 DNS 服务器本身进行任何更改或发布。

同时，为了让错误定向的 DNS 查询到达 Cloudflare 网络，我们使用 [BYOIP](https://developers.cloudflare.com/byoip/) (自带 IP 到 Cloudflare，这是一个 Cloudflare 产品，允许在 Cloudflare 的所有地点宣告客户自己的 IP)。4 个 AS112 IP 前缀被加入到 BYOIP 系统，并由它宣告到全球。

### 测试

在向公共互联网宣告之前，我们如何确保我们建立的服务正确运行呢？1.1.1.1 每天处理超过 130 亿个这样的错误定向查询，并实施了逻辑直接在本地为它们返回 NXDOMAIN，这是 [RFC 7534](https://www.rfc-editor.org/rfc/rfc7534)的推荐做法。

但是，我们可以使用动态规则来改变 Cloudflare 测试位置中处理错误定向查询的方式。例如，类似如下的规则：

> `phase = post-cache and qtype in { PTR } and colo in { test1 test2 } and qname-suffix in { 10.in-addr.arpa 16.172.in-addr.arpa 17.172.in-addr.arpa 18.172.in-addr.arpa 19.172.in-addr.arpa 20.172.in-addr.arpa 21.172.in-addr.arpa 22.172.in-addr.arpa 23.172.in-addr.arpa 24.172.in-addr.arpa 25.172.in-addr.arpa 26.172.in-addr.arpa 27.172.in-addr.arpa 28.172.in-addr.arpa 29.172.in-addr.arpa 30.172.in-addr.arpa 31.172.in-addr.arpa 168.192.in-addr.arpa 254.169.in-addr.arpa } forward 192.175.48.6:53`

该规则要求，在数据中心 test1 和 test2 中，当 DNS 查询类型为 PTR，且查询名称以列表中的名称结尾时，将查询转发到 53 端口上的 192.175.48.6 服务器(AS112 服务 IP 中的一个)。

因为我们在同一个节点中配置了 AS112 IP 前缀，新的 AS112 服务将接收查询并响应解析器。

值得一提的是，上述在后缓存阶段拦截查询并更改查询处理方式的动态规则也是由名为 override 的访客应用程序执行。该应用程序加载所有动态规则，解析 DSL 文本并在每个规则声明的阶段注册回调函数。当传入的查询与表达式匹配时，它就会执行指定的操作。

## 公共报告

我们收集以下指标，以生成 AS112 运营商希望分享给运营商社区的公共统计数据：

-   按查询类型分的查询数量
-   按响应码分的查询数量
-   按协议分的查询数量
-   按 IP 版本分的查询数量
-   有 EDNS 支持的查询数量
-   有 DNSSEC 支持的查询数量
-   按 ASN/数据中心分的查询数量

我们将在 [Cloudflare Radar](https://radar.cloudflare.com/) 网站上提供以上公共统计页面。我们仍在努力实现页面所需的后端 API 和前端——一旦可用，我们将分享到此页面的链接。

## 接下来？

我们将从 2022 年 12 月 15 日开始宣告 AS112 前缀。

该服务启动后，您可以运行一个 dig 命令来检查是否到达了 Cloudflare 运行的一个 AS112 节点上，例如：

```
$ dig @blackhole-1.iana.org TXT hostname.as112.arpa +short

"Cloudflare DNS, SFO"
"See http://www.as112.net/ for more information."
```
