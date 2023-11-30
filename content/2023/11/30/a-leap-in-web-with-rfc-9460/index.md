---
title: A Leap in Web with RFC 9460
date: 2023-11-30T09:49:02.000Z
updated: 2023-11-30T09:49:02.000Z
published: 2023-11-16T09:49:02.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: >-
    https://www.linkedin.com/pulse/leap-web-rfc-9460-adam-cassar-lt2sc?trk=public_post_feed-article-content
  hostname: www.linkedin.com
  author: Adam Cassar
  original_title: A Leap in Web with RFC 9460
  original_lang: en

---

**Embracing RFC 9460 拥抱 RFC 9460**

The internet is transforming with the introduction of RFC 9460. This groundbreaking development ushers in two new DNS record types: "SVCB" and "HTTPS". Their role? To revolutionise how browsers connect to websites, enhancing speed, security, and efficiency.  

随着 RFC 9460 的引入，互联网正在发生变革。这一突破性的发展带来了两种新的 DNS 记录类型：“SVCB”和“HTTPS”。他们的角色？彻底改变浏览器连接网站的方式，提高速度、安全性和效率。

**The Traditional Connection Process: Time for Change  

传统的连接过程：变革的时刻到了**

Historically, connecting to a website involved multiple steps: HTTP request, HTTPS redirection, and receiving APLN. This method, though secure, was sluggish and inefficient, particularly in load balancing and failover strategies. RFC 9460 introduces a game-changer, allowing DNS to provide complete connection details, drastically reducing the time to establish a secure connection.  

从历史上看，连接到网站涉及多个步骤：HTTP 请求、HTTPS 重定向和接收 APLN。这种方法虽然安全，但速度缓慢且效率低下，特别是在负载平衡和故障转移策略方面。 RFC 9460 引入了一个游戏规则改变者，允许 DNS 提供完整的连接详细信息，从而大大减少建立安全连接的时间。

**SVCB and HTTPS Records: The Game Changers  

SVCB 和 HTTPS 记录：游戏规则改变者**

These new records mark a significant shift. They accelerate web connections by integrating Alt-Svc HTTP header and ALPN TLS extension into DNS. This enhancement means faster connections, improved load balancing, and failover, plus enhanced privacy through Encrypted Client Hello (ECH).  

这些新记录标志着一个重大转变。它们通过将 Alt-Svc HTTP 标头和 ALPN TLS 扩展集成到 DNS 中来加速 Web 连接。此增强功能意味着更快的连接、改进的负载平衡和故障转移，以及通过加密客户端 Hello (ECH) 增强的隐私性。

**Adoption and Industry Response  

采用和行业反应**

Firefox, Apple's iOS, Safari, macOS, and Chrome are early adopters, integrating these records into their systems. The adoption, as per [Netmeister](https://netmeister.org/blog/https-rrs.html), is promising. As of October 2023, millions of domains have implemented these records, marking a notable shift towards this new technology.  

Firefox、Apple 的 iOS、Safari、macOS 和 Chrome 是早期采用者，将这些记录集成到他们的系统中。根据 Netmeister 的说法，这种采用是有希望的。截至 2023 年 10 月，数百万个域名已实施这些记录，标志着向这项新技术的显着转变。

**What Do These Records Look Like?  

这些记录是什么样的？**

SVCB and HTTPS records detail how services can be accessed. For instance, a typical SVCB record indicates the service's access point, protocols, and port, while an HTTPS record specifies secure access pathways.  

SVCB 和 HTTPS 记录详细说明了如何访问服务。例如，典型的 SVCB 记录指示服务的访问点、协议和端口，而 HTTPS 记录指定安全访问路径。

**Overcoming Traditional DNS Limitations  

克服传统 DNS 限制**

These records address a key DNS limitation - the inability to use CNAME records at the domain apex. They enable efficient content delivery and load balancing without traditional DNS conflicts.  

这些记录解决了关键的 DNS 限制 - 无法在域顶端使用 CNAME 记录。它们可实现高效的内容交付和负载平衡，而不会发生传统的 DNS 冲突。

**Enhancing Load Balancing and Failover Mechanisms  

增强负载平衡和故障转移机制**

SVCB records can be tailored for load balancing and high-availability services, specifying different server endpoints, priorities, and protocols. This flexibility ensures efficient traffic distribution and reliable failover mechanisms.  

SVCB 记录可以针对负载平衡和高可用性服务进行定制，指定不同的服务器端点、优先级和协议。这种灵活性确保了高效的流量分配和可靠的故障转移机制。

**Apex Domain Usage Apex 域使用**

With SVCB/HTTPS records, apex domains can now alias to different service providers, overcoming the constraints of CNAME records. This capability is crucial for root domain aliasing and efficient service delivery.  

借助 SVCB/HTTPS 记录，顶级域现在可以为不同的服务提供商提供别名，从而克服 CNAME 记录的限制。此功能对于根域别名和高效的服务交付至关重要。

**Future Directions 未来发展方向**

Future SVCB enhancements might include Encrypted ClientHello support, boosting privacy and security during the initial TLS handshake. Additionally, these records can direct traffic to specific, more efficient protocols.  

未来的 SVCB 增强功能可能包括加密 ClientHello 支持，从而在初始 TLS 握手期间增强隐私和安全性。此外，这些记录可以将流量引导至特定的、更高效的协议。

**The Journey to RFC 9460 - Why the Delay?  

RFC 9460 之旅 - 为什么会延迟？**

The introduction of RFC 9460 raises a question: why did it take so long? Apex records faced challenges, like the inability to use CNAMEs. RFC 9460's creation is a testament to the perseverance and innovation of its creators. [Peakhour](https://www.peakhour.io/) is embracing these mechanisms to ensure continuous delivery of services to customers.  

RFC 9460 的推出提出了一个问题：为什么花了这么长时间？ Apex 记录面临挑战，例如无法使用 CNAME。 RFC 9460的诞生证明了其创建者的坚持和创新。 Peakhour 正在采用这些机制来确保为客户持续提供服务。

**A New Era in DNS Technology  

DNS 技术的新时代**

RFC 9460's SVCB and HTTPS records represent a significant leap in DNS technology. They offer enhanced control over server-client interactions, leading to improved performance, reliability, and security in web services. This advancement heralds a new era of internet connectivity, with benefits extending to service providers and users alike.  

RFC 9460 的 SVCB 和 HTTPS 记录代表了 DNS 技术的重大飞跃。它们增强了对服务器与客户端交互的控制，从而提高了 Web 服务的性能、可靠性和安全性。这一进步预示着互联网连接的新时代，服务提供商和用户都将受益。
