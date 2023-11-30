---
title: State of DNS rebinding in 2023
date: 2023-11-27T07:57:14.000Z
updated: 2023-11-27T07:57:14.000Z
published: 2023-06-27T07:57:14.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://blog.apnic.net/2023/06/27/state-of-dns-rebinding-in-2023/
  hostname: blog.apnic.net
  author: Roger Meyer
  original_title: State of DNS rebinding in 2023 | APNIC Blog
  comment: 作者主要介绍了 DNS 在本地网络面临的安全挑战，主要是防止 DNS 服务被重新绑定。
  original_lang: en

---

![](DNS-rebinding-ft-1-555x202.png)

_Adapted from the original post at NCC Group’s [Research Blog](https://research.nccgroup.com/2023/04/27/state-of-dns-rebinding-in-2023/).  

改编自 NCC 集团研究博客的原始帖子。_

Different forms of DNS rebinding attacks have been described as far back as 1996 for [Java Applets](https://web.archive.org/web/20200301153627/https://sip.cs.princeton.edu/news/dns-scenario.html) and 2002 for [JavaScript (Quick-Swap)](https://seclists.org/bugtraq/2002/Jul/362). It has been four years since [Gérald Doussot](https://twitter.com/gerald_doussot) and I [presented](https://youtu.be/y9-0lICNjOQ) on the State of DNS Rebinding at DEF CON 27 ([slides](https://bit.ly/Singularity_Defcon27)), where we introduced our DNS rebinding attack framework, [Singularity of Origin](https://github.com/nccgroup/singularity). In 2020, we studied the [impact of DNS over HTTPS (DoH) on DNS rebinding attacks](https://research.nccgroup.com/2020/03/30/impact-of-dns-over-https-doh-on-dns-rebinding-attacks/).  

早在 1996 年的 Java Applet 和 2002 年的 JavaScript (Quick-Swap) 中就已经描述了不同形式的 DNS 重新绑定攻击。自 Gérald Doussot 和我在 DEF CON 27 上介绍 DNS 重新绑定的现状（幻灯片）以来已有四年了，我们在会上介绍了我们的 DNS 重新绑定攻击框架“起源奇点”。 2020 年，我们研究了 DNS over HTTPS (DoH) 对 DNS 重新绑定攻击的影响。

This update documents the state of DNS rebinding for April 2023. We describe Local Network Access, a new draft W3C specification currently implemented in some browsers that aim to prevent DNS rebinding and show two potential ways to bypass these restrictions.  

此更新记录了 2023 年 4 月 DNS 重新绑定的状态。我们描述了本地网络访问，这是目前在某些浏览器中实施的一项新的 W3C 规范草案，旨在防止 DNS 重新绑定，并展示了两种绕过这些限制的潜在方法。

We also discuss the effects of WebRTC IP address leak mitigation, and DNS Bit 0x20 on DNS rebinding attacks.  

我们还讨论了 WebRTC IP 地址泄漏缓解和 DNS 位 0x20 对 DNS 重新绑定攻击的影响。

## Local Network Access 本地网络访问

[Local Network Access](https://wicg.github.io/local-network-access/) (previously called Private Network Access or CORS-RFC1918) is a W3C draft specification intended to mitigate the risks of unintentional exposure of web services on a client’s internal network. It does this by segmenting address ranges into different **address spaces**, and will behave differently depending upon the origin of the request. If the request is to a more private address space than the origin, it will first perform a **CORS Preflight** request to the host, allowing the host to perform access control.  

本地网络访问（以前称为专用网络访问或 CORS-RFC1918）是 W3C 草案规范，旨在降低客户端内部网络上无意暴露 Web 服务的风险。它通过将地址范围分段到不同的地址空间来实现这一点，并且根据请求的来源而表现不同。如果请求是针对比源站更私有的地址空间，它会首先向主机执行 CORS Preflight 请求，允许主机执行访问控制。

While this might be a draft standard, it has already been implemented in Chrome and some derived browsers (for example, Edge).  

虽然这可能是一个标准草案，但它已经在 Chrome 和一些衍生浏览器（例如 Edge）中实现。

### Local Network Access address spaces  

本地网络访问地址空间

The specification defines the following three _broad_ IP address spaces:  

该规范定义了以下三个广泛的 IP 地址空间：

-   **loopback**: The loopback address space contains the local host only (127.0.0.0/8, ::1/128).  
    
    环回：环回地址空间仅包含本地主机（127.0.0.0/8，::1/128）。
-   **local**: The local address space contains addresses that are reachable only within the current network (for example, 10.0.0.0/8, 100.64.0.0/10, 172.16.0.0/12, 192.168.0.0/16, 169.254.0.0/16, fc00::/7, fe80::/10).  
    
    local：本地地址空间包含仅在当前网络内可达的地址（例如，10.0.0.0/8、100.64.0.0/10、172.16.0.0/12、192.168.0.0/16、169.254.0.0/16、 fc00::/7、fe80::/10)。
-   **public**: The public address space contains all other addresses.  
    
    public：公共地址空间包含所有其他地址。

### Local network requests 本地网络请求

The concept of a local network request is defined as follows (defined in “[2.2. Local Network Request](https://wicg.github.io/private-network-access/)” of the specification):  

本地网络请求的概念定义如下（定义在规范的“2.2.本地网络请求”中）：

> A request is a local network request if request’s current URL’s host maps to an IP address whose IP address space is less public than request’s policy container’s IP address space.  
> 
> 如果请求的当前 URL 的主机映射到一个 IP 地址，且该 IP 地址的 IP 地址空间比请求的策略容器的 IP 地址空间不那么公开，则该请求是本地网络请求。

This prevents the following two conditions:  

这可以防止出现以下两种情况：

1.  Network access to the loopback address space from an origin that is either in the local or public address space. This is because loopback is less public than the local or public address space.  
    
    从本地或公共地址空间中的源对环回地址空间进行网络访问。这是因为环回比本地或公共地址空间不太公开。
2.  Network access to the local address space from an origin that is in the public address space. This is because local is less public than the public address space.  
    
    从公共地址空间中的源对本地地址空间进行网络访问。这是因为本地地址空间比公共地址空间不太公开。

## CORS preflight CORS 预检

Access to fewer public networks will generate a CORS preflight request. The Local Network Access specification defines the following two additional CORS headers:  

访问较少的公共网络将生成 CORS 预检请求。本地网络访问规范定义了以下两个附加 CORS 标头：

-   The `Access-Control-Request-Local-Network` client request header indicates that the request is a local network request.  
    
    `Access-Control-Request-Local-Network` 客户端请求头表明该请求是本地网络请求。
-   The `Access-Control-Allow-Local-Network` server response header indicates that a resource can be safely shared with external networks.  
    
    `Access-Control-Allow-Local-Network` 服务器响应标头表示资源可以安全地与外部网络共享。

Let’s walk through an example scenario. An attacker entices a home user to browse the attacker’s website at attacker.com. The attacker’s malicious JavaScript triggers a request to the victim’s local router (router.local) trying to modify the DNS settings. As the request to router.local is more private than the attacker’s host in the public address space, the browser will initiate a CORS preflight request:  

让我们来看一个示例场景。攻击者诱使家庭用户浏览攻击者的网站attacker.com。攻击者的恶意 JavaScript 会触发对受害者本地路由器 (router.local) 的请求，尝试修改 DNS 设置。由于对 router.local 的请求比公共地址空间中攻击者的主机更加私密，因此浏览器将发起 CORS 预检请求：

```
OPTIONS /set_dns?... HTTP/1.1
Host: router.local
Access-Control-Request-Method: GET
Access-Control-Request-Local-Network: true
Origin: https://attacker.com
...
```

Note the request header `Access-Control-Request-Local-Network: true` indicating to the router that this request is a local network request. If the router does not understand this request and does not send a valid response, the browser will block access to router.local. If the router wants to allow access from external networks, the router will return the following CORS preflight response:  

请注意请求标头 `Access-Control-Request-Local-Network: true` 向路由器指示此请求是本地网络请求。如果路由器不理解此请求并且未发送有效响应，则浏览器将阻止对 router.local 的访问。如果路由器想要允许来自外部网络的访问，路由器将返回以下 CORS 预检响应：

```
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://public.example.com
Access-Control-Allow-Methods: GET
Access-Control-Allow-Credentials: true
Access-Control-Allow-Local-Network: true
Content-Length: 0
...
```

Note the `Access-Control-Allow-Local-Network: true` notifying the browser that the router allows external network access.  

注意 `Access-Control-Allow-Local-Network: true` 通知浏览器路由器允许外部网络访问。

## Local Network Access bypasses  

本地网络访问绕过

Now that we have a good foundational understanding of Local Network Access, we can talk about the two known ways to bypass it.  

现在我们对本地网络访问有了很好的基础了解，我们可以讨论两种已知的绕过它的方法。

### Local Network Access Bypass using 0.0.0.0  

使用 0.0.0.0 绕过本地网络访问

What is the 0.0.0.0 IP address? According to Wikipedia, “0.0.0.0 is a non-routable meta-address used to designate an invalid, unknown or non-applicable target”. Nevertheless, using 0.0.0.0 allows us to access the localhost on Linux and macOS systems.  

0.0.0.0 IP 地址是什么？根据维基百科，“0.0.0.0 是一个不可路由的元地址，用于指定无效、未知或不适用的目标”。尽管如此，使用 0.0.0.0 允许我们访问 Linux 和 macOS 系统上的本地主机。

During our initial research of DNS rebinding attacks, we documented this attack vector allowing [DNS rebinding protection bypasses](https://github.com/nccgroup/singularity/wiki/Protection-Bypasses#dns-rebinding-protection-bypass-1-0000).  

在我们对 DNS 重新绑定攻击的初步研究期间，我们记录了这种允许绕过 DNS 重新绑定保护的攻击向量。

Using the IP address 0.0.0.0 also bypasses Local Network Access protections in Chrome (and Edge). We filed a Chromium bug report in February 2022; the issue can be tracked in Chromium bug [1300021](https://bugs.chromium.org/p/chromium/issues/detail?id=1300021).  

使用 IP 地址 0.0.0.0 还可以绕过 Chrome（和 Edge）中的本地网络访问保护。我们于 2022 年 2 月提交了 Chromium 错误报告；该问题可以在 Chromium bug 1300021 中跟踪。

This allows us to perform DNS rebinding attacks targeting services listening on the localhost of Linux and macOS systems in Chrome, in approximately three seconds.  

这使我们能够在大约三秒内针对在 Chrome 中的 Linux 和 macOS 系统的本地主机上侦听的服务执行 DNS 重新绑定攻击。

### Local Network Access bypass using a router’s public IP address  

使用路由器的公共 IP 地址绕过本地网络访问

In 2010, Craig Heffner discovered and [developed](https://defcon.org/images/defcon-18/dc-18-presentations/Heffner/DEFCON-18-Heffner-Routers-WP.pdf) a DNS rebinding technique, covered during our [DEF CON 27 presentation](https://bit.ly/Singularity_Defcon27), to exploit the [weak host model](https://en.wikipedia.org/wiki/Host_model), which can be used to bypass Chrome’s Local Network Access protection. In this bypass, we access an internal router’s web interface for example, Wi-Fi router) through the public IP address instead of the internal (private) IP address.  

2010 年，Craig Heffner 发现并开发了一种 DNS 重新绑定技术（在我们的 DEF CON 27 演示中介绍），以利用弱主机模型，该模型可用于绕过 Chrome 的本地网络访问保护。在此旁路中，我们通过公共 IP 地址而不是内部（私有）IP 地址访问内部路由器的 Web 界面（例如 Wi-Fi 路由器）。

Most Wi-Fi routers allow access to their management web interface only through the internal interface using the private IP address to prevent access from the Internet. As the router usually has a public IP address assigned, some routers allow access to the web interface through the public IP address if the access comes from the internal network interface ([Martian packet](https://en.wikipedia.org/wiki/Martian_packet)).

This allows us to perform DNS rebinding attacks targeting the public IP address where Local Network Access does not apply. We have successfully tested DNS rebinding in Chrome targeting a home router’s public IP address. The attack works particularly well with Netgear routers.

## Local Network Access affects Singularity’s JavaScript port scanner

Singularity includes a [browser based JavaScript port scanner](https://github.com/nccgroup/singularity/blob/master/html/scan-manager.html) to discover HTTP services accessible from the victim’s host, including internal networks, and to launch DNS rebinding attacks in an automated manner. The port scanner is implemented using the JavaScript [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch). If a Fetch request does not return an error or does not timeout, the port is determined as being accessible (open and not firewalled), and a candidate for DNS rebinding.

Local Network Access blocks Fetch requests if the target is less public than the request’s address space, as explained above. This prevents the port scanning of loopback and local direct IP address spaces in Chrome based on the Fetch API. While this affects the port scanner when using Chrome, the scanner now only returns ports that we can rebind to. This means that the port scanner is still well suited for its purpose of identifying potentially exploitable services using DNS rebinding. The Local Network Access Chrome bypass using the IP address 0.0.0.0 also works for port scanning and permits attackers to (indirectly) access services bound to the victim host internal network interfaces. Note that the JavaScript port scanner still works in non-Chrome browsers such as Firefox and Safari.

Full HTTP service enumeration (including services that are not exploitable using DNS rebinding) in Chrome is still possible despite Local Network Access using techniques exploiting timing side-channel leaks, such as those implemented in Nikolai Tschacher’s [port scanner](https://www.incolumitas.com/2021/01/10/browser-based-port-scanning/).

## WebRTC leaking the local IP address

Web Real-Time Communication (WebRTC) allows browsers to manage real-time peer-to-peer connections with the websites they visit. WebRTC enables voice and video communication to work in webpages, without the need for extensions or other additional software.

Previous implementations of WebRTC in browsers accidentally exposed a user’s local IP address and associated network range. The local IP address is the client’s IP address in the private home or corporate network. These are commonly private network IP addresses behind a NAT gateway.

Knowing the local IP address allows attackers to perform targeted DNS rebinding attacks without the need to know or guess the victim’s network IP range. Singularity includes IP address and network range detection functionality by abusing WebRTC in the [attack automation feature](https://github.com/nccgroup/singularity/wiki/Attack-Automation) as well as the [port scan functionality](https://github.com/nccgroup/singularity/blob/master/html/scan-manager.html).

[Chrome](https://bugs.chromium.org/p/chromium/issues/detail?id=878465), Edge, [Firefox](https://bugzilla.mozilla.org/show_bug.cgi?id=1588817), and Safari now all obfuscate the private IP addresses of endpoints to prevent the leak during the ICE candidate gathering. Singularity can therefore no longer discover the victim’s local IP address in recent browsers. The Singularity attack framework can use the local IP address leak during its IP address scan to improve the efficiency of automated DNS rebinding attacks for older browsers. For recent browsers, automated DNS rebinding attacks still work, but may require an attacker to know or guess the victim’s private IP address range, or it will take longer to identify vulnerable targets within that range. Note that this does not affect the detection and exploitation of vulnerable services bound to 0.0.0.0, as explained in the previous section.

## DNS bit 0x20

Since around December 2022, we noticed that some DNS rebinding attacks were failing randomly. By examining the log files of the Singularity attack framework we noticed DNS queries that contained random capitalization in the DNS name.

For example, we saw DNS requests such as the following:

```
2023/01/05 10:44:08 DNS: Received A query: S-35.185.206.165-127.0.0.1-1672656847789-Rr-e.d.ReBInD.IT. from: 172.253.2.2:28060
2023/01/05 10:44:08 DNS: Parsed query:  {    }, error: cannot find start tag in DNS query

2023/01/02 10:54:08 DNS: Received A query: S-35.185.206.165-127.0.0.1-1672656848109-rr-e.d.REBInd.It. from: 172.253.2.2:34138
2023/01/02 10:54:08 DNS: Parsed query:  {    }, error: cannot find start tag in DNS query
```

Notice that the DNS name (`S-35.185.206.165-127.0.0.1-1672656847789-Rr-e.d.ReBInD.IT`) is randomly capitalized (that is, `ReBInD.IT` instead of `rebind.it`). This caused issues with the query parsing functionality. Singularity expects the initial `s-` as the start of the query and the trailing `-e` as the end (see the [Singularity DNS query documentation](https://github.com/nccgroup/singularity/wiki/How-to-Create-Manual-DNS-Requests-to-Singularity%3F)). With randomized capitalization, Singularity was unable to correctly parse the query.

The reason for the random capitalization was the introduction of the IETF draft [Use of Bit 0x20 in DNS Labels to Improve Transaction Identity](https://datatracker.ietf.org/doc/html/draft-vixie-dnsext-dns0x20). In January 2023, [Google](https://lists.dns-oarc.net/pipermail/dns-operations/2023-January/021947.html) started deploying this feature in certain regions. The goal of Bit 0x20 is to make cache poisoning attacks less effective by using capitalization to expand the range of possibilities of the 16-bit transaction ID in DNS queries.

We improved the reliability of DNS rebinding attacks in Singularity by [lowercasing](https://github.com/nccgroup/singularity/pull/48/commits/31dbb61912636dcc67ca2a728008b450332884f5#diff-c5697b7494615f70d7f7ad8cd3743eb321bec4d944da9f7c12b5193262b9a2abR120) all incoming DNS queries to ensure consistent processing.

## Recent DNS rebinding vulnerabilities

Security researchers keep discovering DNS rebinding vulnerabilities showing that these issues are still dangerous and exploitable. The following is a list of products that were discovered to be vulnerable to DNS rebinding in 2022:

-   2022-01-17 [DNS rebinding vulnerability in H2 Console](https://github.com/h2database/h2database/releases/tag/version-2.1.210). This vulnerability was discovered by NCC Group and we wrote a [payload to exploit this issue with Singularity](https://github.com/nccgroup/singularity/commit/5c7730e457d1af91fd39a31344ebfa1aec5bbd2e).
-   2022-07-10 DNS rebinding vulnerability in Node.js ([CVE-2022-32212](https://hackerone.com/reports/1632921)).
-   2022-09-05 WordPress Unauthenticated Blind SSRF Via DNS Rebinding Vulnerability ([CVE-2022-3590](https://www.sonarsource.com/blog/wordpress-core-unauthenticated-blind-ssrf/)).
-   2022-11-22 SSRF via DNS Rebinding in Appsmith ([CVE-2022-4096](https://infosecwriteups.com/ssrf-via-dns-rebinding-cve-2022-4096-b7bf75928bb2)).
-   2022-11-22 RCE in Tailscale, DNS Rebinding, and You ([CVE-2022-41924](https://emily.id.au/tailscale)): The detailed write-up includes usage of our [public Singularity Server](http://rebind.it/); see ‘[How to Create Manual DNS Requests to Singularity of Origin](https://github.com/nccgroup/singularity/wiki/How-to-Create-Manual-DNS-Requests-to-Singularity%3F)‘.

## Miscellaneous

Headless Chrome behaves the same as the desktop browser and the 0.0.0.0 IP address can be used to target services listening on the localhost to bypass Local Network Access. Headless Chrome is commonly used for backend operations and can be exploited through Server-Side Request Forgery (SSRF) in many applications.

On the mobile side, DNS rebinding attacks on iOS using Safari still work reliably and fast. Attacks targeting services on the localhost can be exploited in three seconds (using 0.0.0.0 and the multiple answers (MA) strategy) and in less than 20 seconds for all other IP addresses (using DNS cache flooding). On Android, the mobile Chrome browser includes the same limiting factors as Chrome on desktop described above.

## Conclusion

While Local Network Access makes it harder, it is still possible to execute DNS rebinding attacks. Local Network Access breaks `fetch()`\-based HTTP port scanners. Singularity’s port scanner (using Chrome) now only reports DNS rebindable HTTP services. However, it is still feasible to enumerate all (non-rebindable and rebindable) HTTP services through the use of timing side-channel leaks. Common browsers have fixed the WebRTC private IP address leak, which makes it more challenging to effectively perform DNS rebinding attacks, as attackers now have to guess or know private IP address ranges.

_Roger Meyer is a Technical Director at NCC Group with extensive experience in managing and leading complex engagements. Roger specializes in web application security, network penetration testing, configuration reviews, and secure software development and architecture design. Roger has conducted over a hundred security audits, penetration tests, source code reviews, and architecture reviews over the last 11 years at NCC Group._

_Gérald Doussot, NCC Group_, _contributed to this work._

<table data-immersive-translate-effect="1" data-immersive_translate_walked="d95b0d71-2796-41c8-b1db-8bc5f3a77358"><tbody data-immersive-translate-effect="1" data-immersive_translate_walked="d95b0d71-2796-41c8-b1db-8bc5f3a77358"><tr data-immersive-translate-effect="1" data-immersive_translate_walked="d95b0d71-2796-41c8-b1db-8bc5f3a77358"><td data-immersive-translate-effect="1" data-immersive_translate_walked="d95b0d71-2796-41c8-b1db-8bc5f3a77358"><nobr data-immersive-translate-effect="1" data-immersive_translate_walked="d95b0d71-2796-41c8-b1db-8bc5f3a77358">Rate this article</nobr></td><td data-immersive-translate-effect="1" data-immersive_translate_walked="d95b0d71-2796-41c8-b1db-8bc5f3a77358"></td></tr></tbody></table>

___

The views expressed by the authors of this blog are their own and do not necessarily reflect the views of APNIC. Please note a [Code of Conduct](https://blog.apnic.net/?p=395) applies to this blog.
