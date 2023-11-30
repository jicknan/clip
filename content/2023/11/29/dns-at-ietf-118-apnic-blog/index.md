---
title: DNS at IETF 118
date: 2023-11-29T10:02:35.000Z
updated: 2023-11-29T10:02:35.000Z
published: 2023-11-29T10:02:35.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://blog.apnic.net/2023/11/29/dns-at-ietf-118/
  hostname: blog.apnic.net
  author: Geoff Huston
  original_title: DNS at IETF 118 | APNIC Blog
  original_lang: en

---

![](DNS-ietf-118-ft-555x202.png)

The [IETF met in Prague](https://www.ietf.org/how/meetings/118/) in the first week of November 2023, and, as usual, there was a flurry of activity in the DNS-related Working Groups. Here’s a roundup of those DNS topics I found to be of interest at that meeting.  

IETF 于 2023 年 11 月的第一周在布拉格召开会议，与往常一样，DNS 相关工作组开展了一系列活动。以下是我在那次会议上感兴趣的 DNS 主题的综述。

## Rethinking the DNS 重新思考 DNS

Prior to IETF meetings, there is an opportunity for informal discussions, under the label of a ‘hackathon’. The topic in the DNS was to reconsider the DNS. A major constraint in any such discussion is that the installed base of the DNS is highly resistant to many forms of change. The namespace definition, the data model of the DNS, and the model of the collection of delegated authorities assembled in a hierarchical manner are all fixed. With so much that must be retained, such conversations about a ‘new’ DNS quickly turn to discussions about incremental evolution that preserve backward compatibility.  

在 IETF 会议之前，有机会以“黑客马拉松”的名义进行非正式讨论。 DNS 中的主题是重新考虑 DNS。任何此类讨论的一个主要限制是 DNS 的安装基础对多种形式的变化具有很强的抵抗力。命名空间的定义、DNS的数据模型以及以分层方式组装的授权集合的模型都是固定的。由于必须保留的内容太多，因此有关“新”DNS 的讨论很快就转向了有关保持向后兼容性的渐进式演进的讨论。

There are a couple of areas of useful consideration. One is the way that DNS uses the underlying transport layer, its model of a preference for using UDP for queries and responses and the visible trend of placing more information into DNS responses. The work on DNS over HTTP (DoH) opens some interesting conversations about using a reliable, authenticated and encrypted streaming transport by default, and also opens up a conversation about push vs pull as a model of information delivery. However, this year’s hackathon headed in a different direction, namely the delegation record in the DNS.  

有几个方面值得考虑。一是 DNS 使用底层传输层的方式、其优先使用 UDP 进行查询和响应的模型以及将更多信息放入 DNS 响应的明显趋势。 DNS over HTTP (DoH) 的工作开启了一些关于默认情况下使用可靠、经过身份验证和加密的流传输的有趣对话，并且还开启了关于推与拉作为信息传递模型的对话。然而，今年的黑客马拉松却走向了不同的方向，即 DNS 中的代表团记录。

Confusion over the nameserver (NS) record has been a significant issue for the DNS over its entire lifespan. It’s a piece of data that is held in both the child and parent zones, yet, confusingly, it’s the child that is authoritative for this data, while logically in terms of the DNS nameserver discovery process, it’s the parent’s copy of this data that is used to guide the name resolution process. So, the thought process is to consider what a compound _delegation record_ might look like.  

名称服务器 (NS) 记录的混乱一直是 DNS 整个生命周期中的一个重大问题。这是一条数据，同时保存在子区域和父区域中，但令人困惑的是，子区域对该数据具有权威性，而从逻辑上讲，就 DNS 名称服务器发现过程而言，父区域的数据副本才是该数据的权威。用于指导名称解析过程。因此，思考过程是考虑复合委托记录可能是什么样子。

These days such a record may refer to multiple zone hosting providers, it may allow for suggested query transports in a similar manner to the SVCB record, and it would likely be authoritative at the parent and not be part of the child zone (similar to the existing DS record).  

如今，这样的记录可能引用多个区域托管提供商，它可能允许以与 SVCB 记录类似的方式进行建议的查询传输，并且它可能在父区域具有权威性，而不是子区域的一部分（类似于现有 DS 记录）。

It’s an interesting area of thought for the DNS as it applies the same consideration that the DNS is a provisioning protocol for accessing named applications and services to the DNS query resolution protocol itself in terms of querying the nameservers for a zone.  

对于 DNS 来说，这是一个有趣的思考领域，因为它在查询区域的名称服务器方面应用了与 DNS 查询解析协议本身相同的考虑因素，即 DNS 是一种用于访问命名应用程序和服务的配置协议。

The hackathon report at the DNSOP Working Group meeting made mention of a new DELEG record that was able to carry this revised form of delegation in the DNS, but the report was also quick to note that further consideration of this idea would probably move further along in the coming months, and what might emerge as a proposal may not necessarily include a DELEG resource record per se.  

DNSOP 工作组会议上的黑客马拉松报告提到了一个新的 DELEG 记录，该记录能够在 DNS 中携带这种修改后的授权形式，但该报告也很快指出，对这一想法的进一步考虑可能会在未来几个月，可能出现的提案可能不一定包括 DELEG 资源记录本身。

The report noted that the administrative structure of the upper layers of the DNS hierarchy, with shared registry operators, multiple registrars, and a collection of zone publishers has as much influence on the evolution of DNS design as the technical issues of efficiency of name resolution within the DNS protocol. I suspect that admitting this new reality of the administrative complexity of these outsourcing relationships is a major concession within many hardcore DNS circles!  

该报告指出，DNS 层次结构上层的管理结构（包括共享注册管理机构运营商、多个注册商和区域发布者集合）对 DNS 设计的演变的影响与域名解析效率的技术问题一样大。 DNS 协议。我怀疑，承认这些外包关系的管理复杂性这一新现实是许多核心 DNS 圈子的重大让步！

## Domain Control Validation  

域控制验证

The DNS is used for many things besides the simple translation of a DNS name to an IP address. Some services want to know if the party requesting a service for a domain name is the real controller of that name, and the most common way of doing this is to ask the requestor to place a token into the domain name to demonstrate that level of control. It seems to have become a bit of a mess!  

除了将 DNS 名称简单转换为 IP 地址之外，DNS 还可用于许多用途。一些服务想要知道请求域名服务的一方是否是该域名的真正控制者，最常见的方法是要求请求者在域名中放置一个令牌以证明该控制级别。好像变得有点乱了！

```
$ dig +short TXT bbc.co.uk
"google-site-verification=ITX3CwHXxGVfkCmhF4eSwdfo8h2ZGLAZ3zRpYvZi5XA"
"google-site-verification=RaiMXJBIiFvqXHd43kv_ekzmXT2l8ibq5Xy0mulndvU"
"voUGv5zARbEV516E/S8Ugsy9/FOgDGg4n/rpmKZQRROVOj0+2tgzKw3Tk9+Ks6qVbNKU18KTrR5khxTQutDvBg=="
"miro-verification=1a94b0fef7a6d5136a272d5cb425e8dc034e8cfc"
"apple-domain-verification=jFFO0rdS9IrxgWUR"
"msfpkey=3e29l8m08bqxp19k63t73fj5b"
"dropbox-domain-verification=l5djk65wpy3z"
"atlassian-domain-verification=SQsgJ5h/FqwMTXuSG/G4Nd1Gx6uX2keREOsZSa22D5XT46EsEuyaic8Aej4cR4Tr"
"2RLXso9TrRPyhWOEhYggL0U/r1D+g8H7z9RqDBOmcJjSbj88TobGKimtkCrXZNBkDXQDj89lS4mDskNOJyWLdg=="
"MSUEdqCJpCtrI1JuH2-U"
"MS=ms10378910"
"docusign=a10ad7b6-cf7e-472d-8157-23061f5b5116"
"adobe-idp-site-verification=9b850a4a56e3fac19aea1e0ac5db302e5cefab444cd73519dce1c72ccd4db058"
"Huddle"
"docusign=50f10407-e3e4-4f6a-aae4-712d4eb31329"
"docker-verification=aab67462-78f7-4ade-a86b-358645923430"
"J0kgGm0XqA3/6pLD4DHeC5x/dAduzT809P1Iwx/PRCYvVS32rv75RIHKC2aVz47dJxKhPlxGf3h3KXiL6+dyXw=="
"_globalsign-domain-verification=AQ2dURU9RbDOuheuLrx89LSUlA_btgMS6vmFXngBtE"
"v=spf1 a ip4:212.58.224.0/19 ip4:132.185.0.0/16 ip4:78.136.53.80/28 ip4:78.136.14.192/27 ip4:89.234.10.72/29 ip4:89.234.53.236 ip4:212.111.33.181 ip4:78.137.117.8 ip4:46.37.176.74 ip4:185.184.237.181" " ip4:185.119.233.144/30 ip4:185.119.232.158 +include:sf.sis.bbc.co.uk +include:spf.messagelabs.com ~all"
```

The more this technique is used by various service operators, the greater the number of TXT records that are stashed at the zone apex. Of course, a querier can’t just ask for a particular TXT record. Each query gets the entire bundle of records!  

各种服务运营商使用此技术的次数越多，存储在区域顶点的 TXT 记录的数量就越多。当然，查询者不能只请求特定的 TXT 记录。每个查询都会获取整个记录包！

There is a [proposal](https://datatracker.ietf.org/doc/draft-ietf-dnsop-domain-verification-techniques/) to pull these bundles of zone apex TXT records apart by using service-specific challenge records and creating specialized records such as:  

有人建议通过使用特定于服务的质询记录并创建专门的记录来将这些区域 apex TXT 记录捆绑分开，例如：

```
_foo-challenge.example.com.  IN   TXT  "3419sdqa32453243d206c4"
```

The proposal contains a useful survey of currently used techniques in Appendix A, which is helpful, but otherwise, the proposal is unclear as to what problem it is attempting to solve and why. It should be remembered these days that the DNS is quite convoluted in terms of roles and responsibilities. The entity that is the notional zone holder (or ‘owner’) may not be the same as the zone administrator who maintains the zone content, who, in turn, may not be the zone publisher (or publishers). When we talk about ‘validation’ which of these parties is providing validation? Should a validation structure make these distinctions in various name administrative roles explicit?  

该提案在附录 A 中包含了对当前使用的技术的有用调查，这很有帮助，但除此之外，该提案还不清楚它试图解决什么问题以及原因。应该记住，如今 DNS 的角色和职责相当复杂。作为名义区域持有者（或“所有者”）的实体可能与维护区域内容的区域管理员不同，而区域管理员也可能不是区域发布者（或多个发布者）。当我们谈论“验证”时，哪一方在提供验证？验证结构是否应该明确各种名称管理角色的这些区别？

The techniques we use today for name validation certainly have their idiosyncrasies, but in many ways, it’s a simple hack that works adequately. It’s unclear what additional value this proposal is bringing to the table.  

我们今天使用的名称验证技术当然有其特性，但在很多方面，这是一个足够有效的简单技巧。目前尚不清楚该提案会带来什么额外价值。

## Generalized notify 广义通知

There are several theories about why DNSSEC is not being taken up with widespread enthusiasm.  

关于为什么 DNSSEC 没有受到广泛的热情，有多种理论。

Aside from the obvious issues related to the efficiency of name resolution, such as dealing with larger responses and addressing UDP truncation by switching to TCP, there is an additional overhead due to the extra queries involved in DNSSEC validation. Additionally, it’s worth noting that very few stub resolvers at the network’s user edge perform validation, leaving the last hop in DNS resolution essentially unprotected.  

除了与名称解析效率相关的明显问题（例如处理较大的响应和通过切换到 TCP 解决 UDP 截断问题）之外，由于 DNSSEC 验证中涉及的额外查询，还会产生额外的开销。此外，值得注意的是，网络用户边缘的存根解析器很少执行验证，导致 DNS 解析的最后一跳基本上不受保护。

Aside from these rather obvious (and quite compelling) performance and efficiency reasons to hold off DNSSEC-signing a zone, there remains a persistent school of thought that the real showstopper for DNSSEC adoption is the lack of a standard way for a delegated zone to communicate the DS record to the parent zone.  

除了这些相当明显（而且非常引人注目）的性能和效率原因来阻止 DNSSEC 签署区域之外，仍然存在一种持续的思想流派，即采用 DNSSEC 的真正障碍是缺乏授权区域进行通信的标准方式DS 记录到父区域。

Almost ten years ago, in September 2014, [RFC 7344](https://www.rfc-editor.org/rfc/rfc7344.html) proposed an automated way of doing this. This RFC defined a new pair of resource records, CDS and CDNSKEY, which are respectively a digest of the zone’s DNSKEY in the same format as a DS record, and the zone’s DNSKEY records.  

大约十年前，即 2014 年 9 月，RFC 7344 提出了一种自动化的方法来执行此操作。该 RFC 定义了一对新的资源记录 CDS 和 CDNSKEY，它们分别是与 DS 记录格式相同的区域 DNSKEY 和区域 DNSKEY 记录的摘要。

Like other entries in the delegated zone, these records are signed with the zone’s key. The basic approach is that the parent regularly checks the child zone for published CDS records, and if a new value is detected then the parent lifts a copy of the records from the child zone, performs a standard DNSSEC validation on the value, and then adds them into the parent zone as a new DS record. The parent may use the CDS record and copy it to the parent zone DS record, or it may prefer to use its own hash function over the child zone key, in which case it would use the CDNSKEY and perform the hash operation on that key value.  

与委托区域中的其他条目一样，这些记录是使用区域密钥进行签名的。基本方法是，父区域定期检查子区域中是否有已发布的 CDS 记录，如果检测到新值，则父区域从子区域中提取记录的副本，对该值执行标准 DNSSEC 验证，然后添加将它们作为新的 DS 记录添加到父区域中。父区域可以使用 CDS 记录并将其复制到父区域 DS 记录，或者它可能更愿意使用自己的哈希函数而不是子区域密钥，在这种情况下，它将使用 CDNSKEY 并对该密钥值执行哈希操作。

Several domain admins have taken up this approach over the past decade. On the positive side, it eliminates a whole new set of administrative processes to pass a value from the child zone to the parent, as the parent simply performs a regular poll against the child zone’s CDS record to detect a change in value.  

在过去的十年中，一些域管理员已经采用了这种方法。从积极的一面来看，它消除了将值从子区域传递到父区域的一套全新的管理流程，因为父区域只需对子区域的 CDS 记录执行定期轮询以检测值的变化。

But in the DNS, nothing is ever simple. A key question is how responsive should this polling system be? A polling interval of months is clearly just too infrequent, while a polling interval of fractions of a second is also clearly going too far the other way!  

但在 DNS 中，没有什么是简单的。一个关键问题是这个投票系统的响应能力如何？几个月的轮询间隔显然太少了，而几分之一秒的轮询间隔显然也太过分了！

A highly responsive system can imply a very high polling load if the parent zone contains many delegations, while a more infrequent polling rate causes slower convergence between parent and child zones for DNSSEC key transitions. More generally, polling is a highly inefficient way of communicating change.  

如果父区域包含许多委托，则高响应系统可能意味着非常高的轮询负载，而轮询频率较低会导致父区域和子区域之间 DNSSEC 密钥转换的收敛速度变慢。更一般地说，民意调查是传达变革的一种非常低效的方式。

This line of thought leads to the concept of borrowing the DNS mechanism used by a primary nameserver to inform its secondary servers that the zone has changed, namely the NOTIFY mechanism ([RFC 1996](https://www.rfc-editor.org/rfc/rfc1966.html)). In a more general case of extending these NOTIFY messages, the child zone could send NOTIFY(CDS) and NOTIFY(CDNSKEY) messages to the parent to trigger a poll for new data from the child zone.  

这种思路引出了借用主域名服务器使用的 DNS 机制来通知其辅助服务器区域已更改的概念，即 NOTIFY 机制（RFC 1996）。在扩展这些 NOTIFY 消息的更一般情况下，子区域可以向父区域发送 NOTIFY(CDS) 和 NOTIFY(CDNSKEY) 消息，以触发对来自子区域的新数据的轮询。

This could also include NOTIFY(CSYNC) messages to allow the child to directly signal NS record changes to the parent. This _generalized NOTIFY_ is described in a [draft](https://datatracker.ietf.org/doc/draft-thomassen-dnsop-generalized-dns-notify/) that was introduced to the DNSOP Working Group in this week’s meeting. This work also proposes a new DNS record type, tentatively referred to as NOTIFY record, which is used in the parent zone to publish details about where such generalized notifications should be sent for each delegation.  

这还可以包括 NOTIFY(CSYNC) 消息，以允许子进程直接向父进程发出 NS 记录更改信号。在本周的会议上向 DNSOP 工作组提交的草案中描述了这种广义的 NOTIFY。这项工作还提出了一种新的 DNS 记录类型，暂称为 NOTIFY 记录，用于在父区域中发布有关应为每个委派发送此类通用通知的详细信息。

Johan Stenstam has pointed out that this could all be simplified if instead of coupling a NOTIFY message to trigger a pull, we could just use the DNS UPDATE mechanism (DNS Dynamic Updates), which could generalize this dynamic UPDATE to include NS as well as DS/DNSKEY records, without a prerequisite of a DNSSEC-signed zone.  

Johan Stenstam 指出，如果我们不耦合 NOTIFY 消息来触发拉取，而是使用 DNS UPDATE 机制（DNS 动态更新），这一切都可以得到简化，该机制可以将这种动态 UPDATE 概括为包括 NS 和 DS /DNSKEY 记录，无需 DNSSEC 签名区域的先决条件。

This is not a novel idea and was described in a [draft](https://datatracker.ietf.org/doc/html/draft-andrews-dnsop-update-parent-zones-04) from Mark Andrews back in 2013. Having new thoughts in the DNS is harder than it looks! Johan’s point is that the original work required some effort to understand where to send the UPDATE, and for this reason, work was abandoned, yet the same problem is addressed in the generalized NOTIFY draft. NOTIFY and UPDATE are alternative methods for parent synchronization.  

这并不是一个新颖的想法，Mark Andrews 在 2013 年的草稿中对此进行了描述。在 DNS 中拥有新想法比看起来更难！ Johan 的观点是，最初的工作需要一些努力来理解将 UPDATE 发送到哪里，因此，工作被放弃，但在广义的 NOTIFY 草案中解决了同样的问题。 NOTIFY 和 UPDATE 是父级同步的替代方法。

The key insight here is that sending the UPDATE to a DNS ‘service’ destination rather than the primary nameserver allows the parent zone admin to implement its own checks on the contents of the UPDATE before applying them. It also can work for both signed and unsigned child zones.  

这里的关键见解是，将更新发送到 DNS“服务”目的地而不是主名称服务器允许父区域管理员在应用更新之前对更新的内容实施自己的检查。它还适用于已签名和未签名的子区域。

There is no doubt that the area of the DNS that uses shared data is one of the more operationally troubled areas of misconfiguration in the DNS. In the case of delegation NS records, the data used by resolvers is the parent’s non-authoritative copy of such records in the downward traversal of delegations to find the authoritative nameservers for the domain name, while the child’s copy of the NS records is the authoritative version of the data.  

毫无疑问，使用共享数据的 DNS 区域是 DNS 中操作上最容易出现配置错误的区域之一。在委托 NS 记录的情况下，解析器使用的数据是父级记录的非权威副本，在委托的向下遍历中查找域名的权威名称服务器，而子级 NS 记录的副本是权威性的。数据的版本。

In the case of the delegation signer (DS) records, the authoritative data is published by the parent, but the data is derived from the DNSKEY key value that is authoritative in the child domain. Whenever there is replicated data in the distributed system the key question is: What should a client do when the various sources of the data disagree with each other? The DNS has no answers here.  

对于委托签名者 (DS) 记录，权威数据由父域发布，但数据源自子域中权威的 DNSKEY 密钥值。每当分布式系统中存在复制数据时，关键问题是：当各个数据源彼此不一致时，客户端应该做什么？ DNS 在这里没有答案。

The DNS is getting to a point where the collection of signalling and action tools is large enough to mean that there are multiple ways to customize these existing tools to achieve a desired outcome. The parent domain might be happy using polling to detect changes in the child domain for DNSSEC-signed domains, or they might prefer that the child domain signals the change of key with a NOTIFY signal, and then leave it to the parent to pick up the new data via a query. There is also the option of the child passing the new data to the parent via an UPDATE.  

DNS 已经达到了信令和操作工具集合足够大的地步，这意味着可以通过多种方法来定制这些现有工具以实现所需的结果。父域可能很乐意使用轮询来检测子域中 DNSSEC 签名域的更改，或者他们可能更喜欢子域使用 NOTIFY 信号来发出密钥更改信号，然后将其留给父域来获取通过查询获得新数据。子级还可以选择通过 UPDATE 将新数据传递给父级。

## Deep space DNS 深空DNS

When the work on protocol development work for an ‘InterPlanetary Internet’ first started it was a DARPA-funded project supporting work by NASA, MITRE, and others to go beyond simple point-to-point long-distance communications and examine how to support a network of communicating devices operating in the very high delay environment of deep space.  

当“星际互联网”协议开发工作首次启动时，这是一个由 DARPA 资助的项目，支持 NASA、MITRE 和其他机构的工作，超越简单的点对点长距离通信，并研究如何支持在深空极高延迟环境中运行的通信设备网络。

The work evolved into the more generic work of ‘delay-tolerant networks’ and the IETF published [RFC 4838](https://datatracker.ietf.org/doc/html/rfc4838) and [RFC 5050](https://datatracker.ietf.org/doc/html/rfc5050) as experimental individual contributions in 2007. In many ways, it’s a form of returning to the past of dial-up store and forward relay networking. The bundle approach was taken up by the Delay-Tolerant Networking Working Group revising RFC 5050 with a set of RFCs: [9171](https://datatracker.ietf.org/doc/html/rfc9171), [9172](https://datatracker.ietf.org/doc/html/rfc9172), [9173](https://datatracker.ietf.org/doc/html/rfc9173), and [9174](https://datatracker.ietf.org/doc/html/rfc9174), in January 2022.  

这项工作演变成更通用的“延迟容忍网络”工作，IETF 在 2007 年发布了 RFC 4838 和 RFC 5050 作为实验性个人贡献。在许多方面，它是一种回到过去拨号存储和转发的形式。中继网络。延迟容忍网络工作组于 2022 年 1 月采用了捆绑方法，修订了 RFC 5050，其中包括一组 RFC：9171、9172、9173 和 9174。

Of course, many of the networking issues remain in such hostile environments, including naming and name resolution. Marc Blanchet has been working on the issues that lurk behind a desire to map the DNS into such deep space environments. Approaches include pre-loading local caches with resolution outcomes, a mapping of needed names and RR values into a ‘special’ version of a deep space zone, a new root zone for this namespace, or some form of split-horizon DNS.  

当然，在这种恶劣的环境中仍然存在许多网络问题，包括命名和名称解析。马克·布兰切特 (Marc Blanchet) 一直致力于解决隐藏在将 DNS 映射到如此深空环境的愿望背后的问题。方法包括预加载具有解析结果的本地缓存、将所需名称和 RR 值映射到深空区域的“特殊”版本、该命名空间的新根区域或某种形式的水平分割 DNS。

The question posed by the presentation of DNS in deep space is whether this is a topic of interest to the DNSOP Working Group. It does seem to be a very esoteric area of application of the DNS and somehow, I just can’t see DNSOP spending its collective time and effort on this. But my track record of guessing what Working Groups choose to do and choose not to do is pretty poor, so any outcome is possible!  

深空 DNS 的展示提出的问题是这是否是 DNSOP 工作组感兴趣的话题。这似乎确实是 DNS 应用的一个非常深奥的领域，不知何故，我就是看不到 DNSOP 在这方面花费集体时间和精力。但我猜测工作组选择做什么和选择不做什么的记录非常差，所以任何结果都是可能的！

## Compact Denial of Existence in DNSSEC  

DNSSEC 中的紧凑否认存在

There are two fundamental performance and efficiency criticisms of DNSSEC. The first is that the inclusion of signed records in DNS responses bloats the DNS response size and this makes DNS over UDP an uncomfortable situation that requires UDP fragmentation or a rapid transition to TCP. The second is that validation of DNSSEC-signed responses can be very time-consuming when using incremental queriers to assemble the validation path.  

DNSSEC 有两个基本的性能和效率批评。首先，在 DNS 响应中包含签名记录会使 DNS 响应大小膨胀，这使得 UDP 上的 DNS 成为一种不舒服的情况，需要 UDP 分段或快速过渡到 TCP。第二个是，当使用增量查询器组装验证路径时，DNSSEC 签名响应的验证可能非常耗时。

The DNS response size has been seen as a challenge, particularly in the case of signed NXDOMAIN responses. These negative responses contain the zone’s SOA record (to let the querier know how long to cache the negative answer) and its RRSIG signature, an NSEC record that spans the query name; its RRSIG signature and an NSEC record for a wildcard, to indicate that there is no wildcard in the zone; and it’s RRSIG signature. That’s three resource records and three signature resource records. At this point, large responses are more likely, particularly with RSA-2048 crypto.  

DNS 响应大小被视为一个挑战，特别是在签名 NXDOMAIN 响应的情况下。这些否定响应包含区域的 SOA 记录（让查询者知道将否定答案缓存多长时间）及其 RRSIG 签名（跨越查询名称的 NSEC 记录）；其 RRSIG 签名和通配符的 NSEC 记录，以指示该区域中没有通配符；这是 RRSIG 签名。这是三个资源记录和三个签名资源记录。此时，更有可能出现较大的响应，特别是对于 RSA-2048 加密。

Work by Cloudflare in 2016 [pointed out](https://blog.cloudflare.com/black-lies/) that responding in that manner to a query where the query name does not exist is perhaps more information than what was strictly asked for. The query contains a query name and a query type. A very minimal negative response to such a query would be to indicate that the requested query type does not exist for this name (NODATA). The subtle shift in this NODATA response is to indicate that all query types other than the type in the query itself exist in the zone.   

Cloudflare 在 2016 年的工作指出，以这种方式响应查询名称不存在的查询可能比严格要求的信息更多。查询包含查询名称和查询类型。对此类查询的最小否定响应是指示该名称 (NODATA) 所请求的查询类型不存在。此 NODATA 响应中的微妙变化是表明该区域中存在除查询本身类型之外的所有查询类型。

This approach has been taken up in DNSOP under the name of ‘[Compact Denial of Existence](https://datatracker.ietf.org/doc/html/draft-ietf-dnsop-compact-denial-of-existence-01)‘. The draft defines NXNAME, a pseudo–resource record type, used in the NSEC type bit map for non-existent names.  

这种方法已在 DNSOP 中采用，名为“紧凑否认存在”。该草案定义了 NXNAME，一种伪资源记录类型，在 NSEC 类型位图中用于不存在的名称。

This approach to Denial of Existence, coupled with elliptical curve crypto, can reduce the size of DNSSEC-signed responses to the point that signed responses should fit into UDP and front-end response signers can work even without complete zone knowledge.  

这种拒绝存在的方法与椭圆曲线加密相结合，可以将 DNSSEC 签名响应的大小减小到签名响应应适合 UDP 的程度，并且即使没有完整的区域知识，前端响应签名者也可以工作。

Now, what can we do about the time taken to perform validation?  

现在，我们可以对执行验证所需的时间做些什么？

### SVCB and DANE SVCB 和 DANE

There’s little doubt that the DNS is the new steering protocol for the Internet (see [DNS is the new BGP](https://blog.apnic.net/2023/09/22/dns-is-the-new-bgp/)), and these days it appears that the two most useful records to achieve this in the DNS are the name translation record (CNAME), with its ability to transfer control of an individual name out of its original zone to a target zone that is operated by a service hosting provider, and the service binding record (SVCB), with its ability to define the parameters of a service connection without performing additional DNS queries.  

毫无疑问，DNS 是互联网的新转向协议（参见 DNS 是新的 BGP），目前看来，在 DNS 中实现这一目标的两个最有用的记录是名称转换记录 (CNAME)，能够将单个名称的控制权从其原始区域转移到由服务托管提供商和服务绑定记录 (SVCB) 运营的目标区域，并且能够定义服务连接的参数，而无需执行额外的操作DNS 查询。

There are a number of RFCs describing the use of Domain Name keys in the DNS, including [RFC 7671](https://datatracker.ietf.org/doc/html/rfc7671), which describes DNAME itself, [RFC 7672](https://datatracker.ietf.org/doc/html/rfc7672), which describes DNAME and MX records and [RFC 7673](https://datatracker.ietf.org/doc/html/rfc7673) for DNAME and SDRV records. This work is performing the same function for DANE and SVCB records.  

有许多 RFC 描述了 DNS 中域名密钥的使用，包括描述 DNAME 本身的 RFC 7671、描述 DNAME 和 MX 记录的 RFC 7672 以及描述 DNAME 和 SDRV 记录的 RFC 7673。这项工作对 DANE 和 SVCB 记录执行相同的功能。

The DANE specification contains the advice that a client should follow the CNAME alias chain to its ultimate target hostname and use that target name as the base name for a TLSA query, but where no TLSA records exist for that name, it should also query for a TLSA record using the initial domain name. This work suggests that this is not the desired behaviour and only the target name should be used, as in the following example:  

DANE 规范包含以下建议：客户端应遵循 CNAME 别名链到达其最终目标主机名，并使用该目标名称作为 TLSA 查询的基本名称，但如果该名称不存在 TLSA 记录，则还应该查询使用初始域名的 TLSA 记录。这项工作表明这不是所需的行为，只应使用目标名称，如下例所示：

```
example.com.           HTTPS 0 xyz.provider.example.
www.example.com.       CNAME xyz.provider.example. 
xyz.provider.example.  HTTPS 1 . alpn=h2,h3 ... 
xyz.provider.example.  A     192.0.2.1 

_443._tcp.xyz.provider.example.  TLSA … 
_443._quic.xyz.provider.example. TLSA …
```

Some protocols that can run over TLS, such as HTTP/0.9 and HTTP/1.0, do not confirm the name of the service after connecting. With DANE, these protocols are subject to an [Unknown Key Share (UKS)](https://datatracker.ietf.org/doc/html/draft-barnes-dane-uks-00) attack, which allows an attacker to deceive one peer of a secure communication as to the identity of the remote peer.  

某些可以通过 TLS 运行的协议（例如 HTTP/0.9 和 HTTP/1.0）在连接后不会确认服务名称。使用 DANE 时，这些协议会受到未知密钥共享 (UKS) 攻击，攻击者可以利用该攻击欺骗安全通信的一个对等方，了解远程对等方的身份。

## Current focus of DNS activity  

当前 DNS 活动的焦点

The focus points of DNS activity are a constantly moving target. The frenetic pace of work on channel encryption for the DNS appears to have calmed down and attention has shifted to the server side of the DNS, looking at the issues related to the role of the DNS in content hosting.  

DNS 活动的焦点是一个不断移动的目标。 DNS 通道加密的狂热工作似乎已经平静下来，注意力已转移到 DNS 的服务器端，研究与 DNS 在内容托管中的作用相关的问题。

At the extreme end of the DNS on the client side, the stub resolvers appear to be highly resistant to change. They use DNS over UDP queries in the clear and the overwhelming majority of these queries are directed to the recursive e resolver operated by the client’s ISP.  

在客户端 DNS 的最末端，存根解析器似乎对更改具有很强的抵抗力。他们以明文方式使用基于 UDP 的 DNS 查询，并且这些查询中的绝大多数都定向到由客户端 ISP 操作的递归 e 解析器。

Enterprise users have a higher level of willingness to use non-local open DNS resolvers, notably Google’s Public DNS service, and in this space, there is also a higher level of use of DoH and DNS over TLS. However, the [measurements](https://stats.labs.apnic.net/edns/XA) do not show any dramatic movement in these patterns of use, and the efforts to add channel encryption between the recursive resolver and the authoritative name servers appear to not have gathered much momentum.  

企业用户更愿意使用非本地开放 DNS 解析器，特别是 Google 的公共 DNS 服务，并且在这个领域，DoH 和 DNS over TLS 的使用也更高。然而，测量结果并没有显示这些使用模式有任何显着变化，并且在递归解析器和权威名称服务器之间添加通道加密的努力似乎没有取得太大进展。

These days there is more activity on the server side of the DNS, looking at ways to make administration of delegated zones more efficient, and also looking at ways to make the initial rendezvous process more efficient by adding service profile data into the DNS.  

如今，DNS 服务器端的活动越来越多，正在寻找使委派区域的管理更加高效的方法，并且还寻找通过将服务配置文件数据添加到 DNS 中来使初始会合过程更加高效的方法。

I am interested in watching the evolution of the combination of DANE, CNAME, and SVCB constructs in the DNS as they apply to encrypting the last open peephole in TLS, the encryption of the SNI field. It strikes me that progress in this area has a pretty high barrier to surmount, given that a truly end-to-end approach that does not entail trusted third-party intermediaries will need DNSSEC validation all the way to the stub resolver.  

我有兴趣观察 DNS 中 DANE、CNAME 和 SVCB 构造组合的演变，因为它们适用于加密 TLS 中最后一个开放的窥视孔，即 SNI 字段的加密。让我印象深刻的是，这一领域的进展需要克服相当高的障碍，因为真正的端到端方法不需要可信的第三方中介机构，需要一直到存根解析器进行 DNSSEC 验证。

In today’s DNS, that looks like it’s still a big ask.  

在今天的 DNS 中，这看起来仍然是一个很大的问题。

<table data-immersive-translate-effect="1" data-immersive_translate_walked="a1426c2b-f064-45b0-92ec-bfd323e7e9b1"><tbody data-immersive-translate-effect="1" data-immersive_translate_walked="a1426c2b-f064-45b0-92ec-bfd323e7e9b1"><tr data-immersive-translate-effect="1" data-immersive_translate_walked="a1426c2b-f064-45b0-92ec-bfd323e7e9b1"><td data-immersive-translate-effect="1" data-immersive_translate_walked="a1426c2b-f064-45b0-92ec-bfd323e7e9b1"><nobr data-immersive-translate-effect="1" data-immersive_translate_walked="a1426c2b-f064-45b0-92ec-bfd323e7e9b1">Rate this article<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">评价这篇文章</span></span></span></nobr></td><td data-immersive-translate-effect="1" data-immersive_translate_walked="a1426c2b-f064-45b0-92ec-bfd323e7e9b1"></td></tr></tbody></table>

___

The views expressed by the authors of this blog are their own and do not necessarily reflect the views of APNIC. Please note a [Code of Conduct](https://blog.apnic.net/?p=395) applies to this blog.  

本博客作者表达的观点仅代表他们自己的观点，并不一定反映 APNIC 的观点。请注意，行为准则适用于本博客。
