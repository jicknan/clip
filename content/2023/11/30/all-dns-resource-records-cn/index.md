---
title: (All) DNS Resource Records 双语版
date: 2023-11-30T10:23:48.000Z
updated: 2023-11-30T10:23:48.000Z
published: 2021-07-15T10:23:48.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://netmeister.org/blog/dns-rrs.html#svcb
  hostname: netmeister.org
  author: null
  original_title: (All) DNS Resource Records
  original_lang: en

---

Ok, the Domain Name System (DNS) is wild. We all know that. Nothing works without it, and when things go really funky and make no sense, chances are it's the DNS. Well, chances are somebody monkeyd around with /etc/hosts, but yeah.  

好的，域名系统 (DNS) 很疯狂。我们都知道。没有它，一切都无法进行，当事情变得非常奇怪且毫无意义时，很可能就是 DNS 的问题。好吧，很可能有人在搞 /etc/hosts ，但是是的。

Now of course you all know the common DNS Resource Records (RRs), right? You got your A, AAAA, CNAME, NS, MX, PTR, and... oh, right, TXT records. And sure, you're probably aware that there are more. But just how many more?  

现在大家当然都知道常见的 DNS 资源记录 (RR) 了，对吧？您得到了 A 、 AAAA 、 CNAME 、 NS 、 MX 、 PTR ，还有...哦，对了， TXT 记录。当然，您可能知道还有更多。但还有多少呢？

All in all, [IANA has almost 100 record types assigned](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml)! A small number of them are _reserved_, some are _experimental_, and many are rarely, if ever, actually used in the wild. But what if you'd like to observe such records on the wire, make such queries and receive a response? Well, here's your answer:  

总而言之，IANA 分配了近 100 种记录类型！其中一小部分是保留的，一些是实验性的，还有许多很少（如果有的话）在野外实际使用。但是，如果您想在线观察此类记录、进行此类查询并收到响应，该怎么办？好吧，这是你的答案：

I created a DNS zone dns.netmeister.org. that contains RRs for every RR type, each one named after the type and each one holding two additional TXT records, one noting the RR format, and one the purpose and originating RFC(s). This zone is [available on GitHub](https://github.com/jschauma/dns-rrs/), and is served live from panix.netmeister.org:  

我创建了一个 DNS 区域 dns.netmeister.org. ，其中包含每种 RR 类型的 RR，每个 RR 均以该类型命名，每个记录包含两条附加 TXT 记录，一条记录 RR 格式，另一条记录目的和原始 RFC。该区域可在 GitHub 上找到，并由 panix.netmeister.org 实时提供：

```
$ dig +nocmd +nocomments +noquestion +nostats +multiline soa dns.netmeister.org.
dns.netmeister.org.     2082 IN SOA panix.netmeister.org. jschauma.netmeister.org. (
                                2021071323 ; serial
                                3600       ; refresh (1 hour)
                                300        ; retry (5 minutes)
                                3600000    ; expire (5 weeks 6 days 16 hours)
                                3600       ; minimum (1 hour)
                                )
$ 
```

(If you run into errors or do not get the responses you expect -- perhaps because something on your local network is monkeying around with your queries -- try to dig or delv @panix.netmeister.org directly.)  

（如果您遇到错误或没有得到您期望的响应 - 可能是因为您的本地网络上的某些内容正在处理您的查询 - 尝试 dig 或 delv @panix.netmeister.org 直接。）

Here, let's go through the RRs, shall we? (You can jump to any individual RR on this page via its respective "#<rr>" anchor.)  

在这里，让我们看一下 RR，好吗？ （您可以通过相应的“#”锚点跳转到此页面上的任何单个 RR。）

___

## RRs from A to ZONEMD  

从 A 到 ZONEMD 的 RR

### A

Ok, no secrets or surprises here. Just an IPv4 address. But let's use this example to illustrate the convenience of this zone:  

好吧，这里没有秘密或惊喜。只是一个 IPv4 地址。但让我们用这个例子来说明这个区域的便利性：

```
$ dig +short a a.dns.netmeister.org.
166.84.7.99
$ dig +short txt a.dns.netmeister.org.
"A 32-bit IPv4 host address. RFC882 (1983); RFC1035 (1987)"
"Format: a single dotted decimal quad IPv4 address"
$ 
```

RFCs: [882](https://datatracker.ietf.org/doc/html/rfc882), [1035](https://datatracker.ietf.org/doc/html/rfc1035) RFC：882、1035

### AAAA

Also no surprises -- your regular old IPv6 address in colon-separated hextets:  

同样，这也不足为奇——您的常规旧 IPv6 地址采用冒号分隔的十六进制数：

```
$ dig +short aaaa aaaa.dns.netmeister.org.
2001:470:30:84:e276:63ff:fe72:3900
$ dig +short txt aaaa.dns.netmeister.org.
"Format: single a hexadecimal IPv6 address"
"A 128-bit IPv6 host address. RFC1884 (1995); RFC4291 (2006)"
$ 
```

RFCs: [1884](https://datatracker.ietf.org/doc/html/rfc1884), [4291](https://datatracker.ietf.org/doc/html/rfc4291) RFC：1884、4291

But... wait a second. Before there were AAAA records, there were...  

但是……等一下。在有 AAAA 记录之前，有...

### A6

... A6 records. This early version included a _prefix_ field and allowed you to recursively construct IPv6 addresses from multiple A6 records, or to simply provide a final address. The following examples illustrate this:  

... A6 记录。此早期版本包含一个前缀字段，允许您从多个 A6 记录递归地构造 IPv6 地址，或者简单地提供最终地址。以下示例说明了这一点：

```
$ dig +short a6 a6.dns.netmeister.org.
64 ::e276:63ff:fe72:3900 a6-prefix.dns.netmeister.org.
0 2001:470:30:84:e276:63ff:fe72:3900
$ dig +short a6 a6-prefix.dns.netmeister.org.
0 2001:470:30:84::
$ dig +short txt a6.dns.netmeister.org.
"Format: <8-bit prefix> <128-bit hex IPv6 address> <prefix-name>"
"Early IPv6 record, obsoleted by AAAA. RFC2874 (2000)"
$ 
```

RFCs: [2874](https://datatracker.ietf.org/doc/html/rfc2874) RFC：2874

### AFSDB

Yep, the DNS is not just a phone book for IP addresses. It's long been used as a service discovery mechanism, and this record type here allows for the definition of an [Andrew File System](https://en.wikipedia.org/wiki/Andrew_File_System) database server in an AFS cell:  

是的，DNS 不仅仅是 IP 地址的电话簿。它长期以来一直被用作服务发现机制，这里的记录类型允许在 AFS 单元中定义 Andrew 文件系统数据库服务器：

```
$ dig +short afsdb afsdb.dns.netmeister.org.
1 panix.netmeister.org.
$ dig +short txt afsdb.dns.netmeister.org.
"Location of a database server of an Andrew File System (AFS) cell. RFC1183 (1990)"
"Format: <16-bit subtype> <domain-name>"
$ 
```

RFCs: [1183](https://datatracker.ietf.org/doc/html/rfc1183) RFC：1183

Note: the responses panix.netmeister.org gives may or may not be meaningful. That is, I don't actually run an AFS database server on this host, so don't bother knocking.  

注意： panix.netmeister.org 给出的响应可能有意义，也可能没有意义。也就是说，我实际上并没有在这台主机上运行 AFS 数据库服务器，所以不用费心去敲。

### AMTRELAY

Ok, now we're deep in wonky territory, where we are offering records to publish Automatic Multicast Tunneling ([AMT](https://datatracker.ietf.org/doc/html/rfc7450)) relays for source-specific multicast channels, aka DNS Reverse IP AMT Discovery (DRIAD):  

好的，现在我们深入到了不稳定的领域，我们将提供记录来发布特定于源的多播通道的自动多播隧道 (AMT) 中继，即 DNS 反向 IP AMT 发现 (DRIAD)：

```
$ dig +short amtrelay amtrelay.dns.netmeister.org.
10 0 2 2001:470:30:84:e276:63ff:fe72:3900
$ dig +short txt amtrelay.dns.netmeister.org.
"Format: <8-bit precedence> <1-bit discover> <7-bit type> <domain-name>"
"Automatic Multicast Tunneling Relay. RFC8777 (2020)"
$ 
```

RFCs: [8777](https://datatracker.ietf.org/doc/html/rfc8777) RFC：8777

Here we see for the first time a common pattern in DNS service discovery: the encoding of a _preference_ or weight, as well as conditional RDATA based on other selectors. We'll see many more examples of this below.  

在这里，我们第一次看到 DNS 服务发现中的常见模式：首选项或权重的编码，以及基于其他选择器的条件 RDATA。我们将在下面看到更多这样的例子。

### ANY

I know, I know, I'm cheating. ANY isn't really an RR type, it's a QTYPE. When asked for ANY (QTYPE=\*), the DNS server will return all of the records for the given name:  

我知道，我知道，我在作弊。 ANY 并不是真正的 RR 类型，它是 QTYPE 。当请求 ANY ( QTYPE=\* ) 时，DNS 服务器将返回给定名称的所有记录：

```
$ dig +nocmd +nocomments +noquestion +nostats +multiline any any.dns.netmeister.org.
any.dns.netmeister.org. 3600 IN RRSIG NSEC 13 4 3600 (
                                20210721144850 20210712212436 56039 dns.netmeister.org.
                                /7MZYmD6foyk/SUvQYo1VSBPE4iNqRcbrWE/FOJaOQXd
                                DxnNcS3u69rINdxhT2H94U5japP1JV+uWBE5+EC9XA==
)
any.dns.netmeister.org. 3600 IN NSEC apl.dns.netmeister.org. TXT RRSIG NSEC
any.dns.netmeister.org. 3600 IN RRSIG TXT 13 4 3600 (
                                20210721144850 20210712212436 56039 dns.netmeister.org.
                                Bxzo6dvL9O+06AKjjixoAm3Olhq5nivIFMudZ735MZs0
                                rwrdIanFFmAbNh2YmCi3TMze7CWVCtmknvSoW39SOA==
)
any.dns.netmeister.org. 3600 IN TXT "Pseudo-RR QTYPE value 255 ('*'). Returns all records. RFC1035 (1987)"
$ 
```

This shows the various records found in the zone for any.dns.netmeister.org., which include the DNSSEC relevant records that we'll see in more detail below.  

这显示了在 any.dns.netmeister.org. 区域中找到的各种记录，其中包括我们将在下面更详细地看到的 DNSSEC 相关记录。

RFCs: [882](https://datatracker.ietf.org/doc/html/rfc882) RFC：882

(Some resolvers may choose not to answer QTYPE=\* (see also: [HINFO](https://netmeister.org/blog/dns-rrs.html#hinfo)); some simply won't have any data in their cache. Try to ask @panix.netmeister.org directly.)  

（有些解析器可能选择不回答 QTYPE=\* （另请参阅： HINFO ）；有些解析器的缓存中根本没有任何数据。尝试询问 @panix.netmeister.org 直接地。）

Oh, and note that of course the QTYPE=\* is different from the _wildcard_ \* in a zone. _That_ is used to return results for a query for a name that's _not_ in the zone:  

哦，请注意，当然 QTYPE=\* 与区域中的通配符 \* 不同。它用于返回不在区域中的名称的查询结果：

```
$ dig +short txt $$.dns.netmeister.org.
"Wildcard record matching any names _not_ in the zone."
$ dig +short txt ${RANDOM}.dns.netmeister.org.
"Wildcard record matching any names _not_ in the zone."
$ dig +short txt not-actually-found-in-the-zone.dns.netmeister.org.
"Wildcard record matching any names _not_ in the zone."
$ 
```

### APL

This one's a fun one. This record can be used to translate a name into not a single address, but an Address Prefix List (APL). What you do with the results is, of course, up to you:  

这是一个有趣的。该记录可用于将名称转换为地址前缀列表 (APL)，而不是单个地址。当然，您如何处理结果取决于您：

```
$ dig +short apl apl.dns.netmeister.org.
1:192.168.32.0/21 !1:192.168.38.0/28 2:2001:db8::/32 !2:2001:470:30:84::/64
$ dig +short txt apl.dns.netmeister.org.
"Address Prefix List. RFC3123 (2001)"
"Format: {[!]afi:address/prefix}* -- whitespace separated strings; an
         optional '!', a numerical address family indicator, ':', an address
         prefix in CIDR notation"
$ 
```

Here, the afi is the Address Family Indicator, defining IPv4 or IPv6 prefixes.  

这里， afi 是地址族指示符，定义IPv4或IPv6前缀。

RFCs: [3123](https://datatracker.ietf.org/doc/html/rfc3123) RFC：3123

### ATMA

RFCs? Who needs RFCs? Why not just go with a paper/proposal from the [ATM Forum](https://web.archive.org/web/20190112072924/http://www.broadband-forum.org/ftp/pub/approved-specs/af-dans-0152.000.pdf) that nowadays is only available from (the admittedly wonderful) [Internet Archive](https://archive.org/)?  

RFC？谁需要 RFC？为什么不直接采用 ATM 论坛的论文/提案，而该论文/提案目前只能从（公认的精彩的）互联网档案馆获得？

The ATMA record operates within the Asynchronous Transfer Mode (ATM) Name System (ANS) (acronyms of acronyms!), allowing you to map a name to an ATM address:  

ATMA 记录在异步传输模式 (ATM) 名称系统 (ANS)（首字母缩写词！）内运行，允许您将名称映射到 ATM 地址：

```
$ dig +short atma atma.dns.netmeister.org.
39246f000e7c9c03120001000100001234567800
$ dig +short txt atma.dns.netmeister.org.
"Format: <address>"
"ATM End System Address. ATM Forum Publication (2000)"
$ 
```

RFCs: _nope_ RFC：没有

### AVC

The AVC resource record was [requested by Cisco](https://www.iana.org/assignments/dns-parameters/AVC/avc-completed-template) to map IP Flow Information Export (IPFIX) names into the DNS:  

Cisco 请求 AVC 资源记录将 IP 流信息导出 (IPFIX) 名称映射到 DNS：

```
$ dig +short avc avc.dns.netmeister.org.
"app-name:Unix" "time|business:default|server-port:TCP/4242,UDP/4242"
$ dig +short txt avc.dns.netmeister.org.
"Application Visibility and Control. RR Submission (2016)"
"Format: <RFC6759 shortened names>"
$ 
```

RFCs: [6759](https://datatracker.ietf.org/doc/html/rfc6759) RFC：6759

See also: 也可以看看：  

[https://www.dns-as.org/what-is/metadata/](https://www.dns-as.org/what-is/metadata/)  

[https://www.dns-as.org/support/avc-rdata/](https://www.dns-as.org/support/avc-rdata/)

### CAA

You've probably come across those, as in recent years Certificate Authorities (CAs) [have been required](https://archive.cabforum.org/pipermail/public/2017-March/009988.html) to honoring these records to determine whether they are actually allowed to issue a certificate for the given domain name:  

您可能已经遇到过这些情况，因为近年来证书颁发机构 (CA) 被要求遵守这些记录，以确定他们是否确实被允许为给定域名颁发证书：

```
$ dig +short caa caa.dns.netmeister.org.
0 iodef "mailto:abuse@netmeister.org"
0 issue ";"
0 issuewild ";"
$ dig +short txt caa.dns.netmeister.org.
"Indication of certificate authorities authorized to issue certificates for this
 domain. RFC6844 (2013)"
"Format: <flags> <tag> <value> -- flag is commonly 1;
         tag one of 'issue', 'issuewild', 'iodef' (others reserved or not yet defined);
         value is a '<character-string>'"

```

RFCs: [6844](https://datatracker.ietf.org/doc/html/rfc6844) RFC：6844

The algorithm by which a CA will determine this authorization starts at the most specific name and then walks up the hierarchy. There is, however, one caveat when you are using [CNAME](https://netmeister.org/blog/dns-rrs.html#cname) records to point to e.g., third parties, as you (currently) _can't_ set a CAA record on a CNAME. (See e.g., [this Twitter thread](https://twitter.com/jschauma/status/1106394543623163904) for a summary of the problem.)  

CA 确定此授权的算法从最具体的名称开始，然后沿着层次结构向上移动。但是，当您使用 [CNAME](https://netmeister.org/blog/dns-rrs.html#cname) 记录指向第三方时，有一个警告，因为您（当前）无法在 CNAME 记录 。 （例如，请参阅此 Twitter 线程以获取问题摘要。）

### CDNSKEY

Yay, our first [DNSSEC](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions) related RR! The CDNSKEY is the child copy of its [DNSKEY](https://netmeister.org/blog/dns-rrs.html#dnskey) record, for transfer to parent. In other words, it's what the child zone would like its parent to use as its DNSKEY record.

In order to facilitate updates to a child zone's [DNSKEY](https://netmeister.org/blog/dns-rrs.html#dnskey), a child can advertize what the parent should include in its zone. This record, like the [CDS](https://netmeister.org/blog/dns-rrs.html#cds) record discussed below, must then be signed by the _current_ DNSKEY.

```
$ dig +short cdnskey cdnskey.dns.netmeister.org.
257 3 13 JErBf5lZ1osSWg7r51+4VfEiWIdONph0L70X0ToT7DkbikKQIp+qvuOOZri7j3qVComv7tgTIBhKxeDQercdKQ==
$ dig +short txt cdnskey.dns.netmeister.org.
"Child Copy of DSNKEY record, for transfer to parent. RFC7344 (2014)"
"Format: <16-bit flags> <8-bit protocol> <8-bit algorithm> <base64-encoded pubkey>"
$ 
```

RFCs: [7344](https://datatracker.ietf.org/doc/html/rfc7344)

### CDS

Just like the [CDNSKEY](https://netmeister.org/blog/dns-rrs.html#cdnskey) record, the CDS record is a child copy of the [DS](https://netmeister.org/blog/dns-rrs.html#ds) resource record for transfer to the parent:

```
$ dig +short cds cds.dns.netmeister.org.
56039 13 2 4104805B43928FC573F0704A2C1B5A10BAA2878DE26B8535DDE77517C154CE9F
$ dig +short txt cds.dns.netmeister.org.
"Format: <16-bit key tag> <8-bit algorithm> <8-bit digest type>
         <digest of owner name concatenated with the DNSKEY RDATA>"
"Child Copy of DS record, for transfer to parent. RFC7344 (2014)"
$  
```

RFCs: [7344](https://datatracker.ietf.org/doc/html/rfc7344)

### CERT

There's a whole lot of different ways of encoding and distributing certificates or public keys via the DNS. Which makes sense, since as a distributed lookup table, it neatly solves many discovery problems inherent in public key cryptography. Of course you do need DNSSEC to be able to assign any trust to any of these, but fortunately that's not a problem, because of course your zone is signed. Right?

The CERT record can include a certificate for many uses, including x509, S/MIME, PGP or IPSec. Here are three examples, an OpenPGP signed public key, a PGP fingerprint, and an x509 certificate (the actual records are shortened here to make the display easier):

```
$ dig +nocmd +nocomments +noquestion +nostats +multiline cert cert.dns.netmeister.org. 
cert.dns.netmeister.org. 3544 IN CERT PGP 0 0 (
mQENBE2L+QkBCADx6DXFdqDEAK1OYYtOeLp54Z0G87t6
Nmz+nodbd9f4Uw0T6v32O2O0yVwA07fCGfPc+3oeCgDa
[...]
odY5Nsz1QchbMHN2FVmmFfrVpocnRQPm1lxqzxwoqJrU
TyWpk/J8/0PbKlSTjRKziFLqudSy/dqFWmk= )
cert.dns.netmeister.org. 3600 IN CERT IPGP 0 0 (
                                99CE1DC7770AC5A809A60DCD66CE4FE96F6BD3D7
)
cert.dns.netmeister.org. 3522 IN CERT PKIX 12848 RSASHA256 (
                                MIIFMzCCBBugAwIBAgISA5tDkCDwHTfvefYEFuzWCFaJ
                                MA0GCSqGSIb3DQEBCwUAMDIxCzAJBgNVBAYTAlVTMRYw
[...]
                                a22la0im/nvFrnQ9exW3T0YkDYGnIE8EewBAJjkBHoxg
                                5CNK5Rj2aPAwNZvWbkXp )
$ dig +short txt cert.dns.netmeister.org. | more
"Format: <16-bit type> <16-bit key tag> <8-bit algorithm> <base64-encoded certificate or CRL>"
"A certificate or certificate revocation list, including x509, S/MIME, PGP or
  IPSec certificates. RFC2538 (1999); RFC4398 (2006)"
$ 
```

(By the way: GnuPG ships with a tool, [make-dns-cert](https://github.com/gpg/gnupg/blob/master/tools/make-dns-cert.c), to generate the CERT RR as a TYPE37 hex encoded record if your server does not support CERT.)

RFCs: [2538](https://datatracker.ietf.org/doc/html/rfc2538), [4398](https://datatracker.ietf.org/doc/html/rfc4398)

See also: [OPENPGPKEY](https://netmeister.org/blog/dns-rrs.html#openpgpkey)

### CNAME

Ah, yes, the infamous CNAME. The record that points to the _canonical name_. The symlink of the DNS zone, endlessly used to redirect people on the internet, and frequent cause of certificate name mismatches.

```
$ dig +short cname cname.dns.netmeister.org.
cname-txt.dns.netmeister.org.
$ dig +short txt cname.dns.netmeister.org
cname-txt.dns.netmeister.org.
"Additional records (besides DNSSEC related records) are not allowed on CNAMEs."
"Format: <domain-name>"
$ dig +short cname cname-loop.dns.netmeister.org.
cname-loop.dns.netmeister.org.
$ 
```

RFCs: [882](https://datatracker.ietf.org/doc/html/rfc882)

That's right: a CNAME can point to anything. A valid name in the same zone, a name in another zone, a name that doesn't exist, another CNAME, or even _itself_. That is, CNAMEs have the potential to cause loops, and you better be prepared to handle those: cname01.dns.netmeister.org.

As noted [above](https://netmeister.org/blog/dns-rrs.html#caa), a CNAME cannot have other RRs associated -- well, aside from the required DNSSEC records -- so there is no accompanying [TXT](https://netmeister.org/blog/dns-rrs.html#txt) record here.

See also: [DNAME](https://netmeister.org/blog/dns-rrs.html#dname)

### CSYNC

This record is used by a child zone to indicate to a parent that it can copy certain records from its zone. This is commonly used for e.g., glue records, like [NS](https://netmeister.org/blog/dns-rrs.html#ns):

```
$ dig +short csync csync.dns.netmeister.org.
2021071001 3 NS
$ dig +short txt csync.dns.netmeister.org.
"Child-to-Parent Synchronization, commonly used for glue records. RFC7477 (2015)"
"Format: <32-bit SOA serial> <16-bit flags> <16-bit type bit map>"
$ 
```

RFCs: [7477](https://datatracker.ietf.org/doc/html/rfc7477)

Note that this is explicitly not intended to synchronize DNSSEC records; for that, see [CDNSKEY](https://netmeister.org/blog/dns-rrs.html#cdnskey) and [CDS](https://netmeister.org/blog/dns-rrs.html#cds).

### DHCID

Why yes, every Sysadmin's favorite magic service, DHCP, joins in on the fun! Here's a record to encode DHCP information to allow dynamic updates of FQDNs after a client has obtained a lease without conflicts.

```
$ dig +short dhcid dhcid.dns.netmeister.org.
AAIBMmFjOTc1NzMyMTk0ZWE1ZTBhN2MzN2M4MzE2NTFiM2M=
$ dig +short txt dhcid.dns.netmeister.org.
"Format: SHA-256(<identifier> <FQDN>)"
"DHCP identifier. RFC4701 (2006)"
$ 
```

RFCs: [4701](https://datatracker.ietf.org/doc/html/rfc4701)

### DLV

More DNSSEC records! This one's the _DNSSEC Lookaside Validation_ record. Normally, DNSSEC depends on building the trust chain from the trust anchor at the root (.); this record facilitates the use of trust anchors published in a zone that's not directly within the hierarchical chain: a _lookaside_ record:

```
$ dig +short dlv dlv.dns.netmeister.org.
56039 13 2 4104805B43928FC573F0704A2C1B5A10BAA2878DE26B8535DDE77517C154CE9F
$ dig +short txt dlv.dns.netmeister.org.
"Format: <16-bit key tag> <8-bit algorithm> <8-bit digest type>
         <digest of owner name concatenated with the DNSKEY RDATA>"
"DNSSEC Lookaside Validation used for off-path validation. RFC4431 (2006); RFC5074 (2007)"
$ 
```

RFCs: [4431](https://datatracker.ietf.org/doc/html/rfc4431), [5074](https://datatracker.ietf.org/doc/html/rfc5074)

[ISC](https://isc.org/) used to run a [DLV Registry](https://dlv.isc.org/); since as of 2021 most TLDs are signed by default, the DLV registry is no longer needed and was discontinued. The use of the DLV record is thus effectively deprecated.

### DNAME

Much like [CNAME](https://netmeister.org/blog/dns-rrs.html#cname) records, DNAME records function as a method of redirection. Unlike CNAMEs, however, DNAME records allow for redirection of _all_ records in a zone without the need to create individual RRs.

That is, if you have a domain example.com and then decide that you want to also allow the use of e.g., foo.example.net, but want to ensure that _all_ existing records under the first domain redirect implicitly in the second, you'd add a DNAME record:

```
$ dig +short dname dname.dns.netmeister.org.
dns.netmeister.org.
$ dig +short txt dname.dns.netmeister.org.
"Format: <domain-name>"
"Delegation name record, used to e.g., redirect an entire domain. RFC2672 (1999); RFC6672 (2012)"
$ 
```

RFCs: [2672](https://datatracker.ietf.org/doc/html/rfc2672), [6672](https://datatracker.ietf.org/doc/html/rfc6672)

This example means that _any_ of the records under dns.netmeister.org. can be resolved under dname.dns.netmeister.org. as well:

```
$ dig +short txt a.dname.dns.netmeister.org.
a.dns.netmeister.org.
"Format: a single dotted decimal quad IPv4 address"
"A 32-bit IPv4 host address. RFC882 (1983); RFC1035 (1987)"
$ 
```

### DNSKEY

One of the many DNSSEC records. This one contains the public key matching the private key used to sign the given zone. This record must match the hash stored in the [DS](https://netmeister.org/blog/dns-rrs.html#ds) record in the _parent_ zone.

```
$ dig +short dnskey dnskey.dns.netmeister.org.
257 3 13 XEn4q8CbG2a4Hw47Ih244BDkwY1tOuprXWKEzMyLPtjO9iIRVt4HLLbx9YaeaYzRcH 91mvCstP8I5liQ0Mn1bA==
$ dig +short txt dnskey.dns.netmeister.org.
"The public key matching the private key used to sign the given zone. RFC4034 (2005)"
"Format: <16-bit flags> <8-bit protocol> <8-bit algorithm> <base64-encoded pubkey>"
$ 
```

RFCs: [4034](https://datatracker.ietf.org/doc/html/rfc4034)

See also: [CDNSKEY](https://netmeister.org/blog/dns-rrs.html#cdnskey), [CDS](https://netmeister.org/blog/dns-rrs.html#cds), [DS](https://netmeister.org/blog/dns-rrs.html#ds), [KEY](https://netmeister.org/blog/dns-rrs.html#key)

### DOA

A Resource Record to stuff the Digital Object Architecture into the DNS, effectively.

```
$ dig +short doa doa.dns.netmeister.org.
0 1 2 "" aHR0cHM6Ly93d3cubmV0bWVpc3Rlci5vcmcvYmxvZy9kbnMtcnJzLmh0bWwK
$ dig +short txt doa.dns.netmeister.org.
"Digital Object Architecture in the DNS. Internet Draft (2017)"
"Format: <32-bit doa-enterprise> <32-bit doa-type> <16-bit doa-location> <doa-media-type> <doa-data>"
$ 
```

RFCs: none -- [expired internet draft](https://www.ietf.org/archive/id/draft-durand-doa-over-dns-03.txt); I guess it was Dead On Arrival, amiright?

### DS

The DNSSEC Delegation Signer public key record of the given child zone stored in the parent zone:

```
$ dig +short ds ds.dns.netmeister.org.
56393 13 2 BD36DD608262A026083721FA19E2F7B474F531BB3179CC00A0C38FF0 0CA11657
$ dig +short txt ds.dns.netmeister.org.
"Delegation signer; the DNSSEC public key of the given child zone stored in the parent zone. RFC4034 (2005)"
"Format: <16-bit key tag> <8-bit algorithm> <8-bit digest type>
         <digest of owner name concatenated with the DNSKEY RDATA>"
$ 
```

RFCs: [4034](https://datatracker.ietf.org/doc/html/rfc4034)

See also: [CDNSKEY](https://netmeister.org/blog/dns-rrs.html#cdnskey), [CDS](https://netmeister.org/blog/dns-rrs.html#cds), [DNSKEY](https://netmeister.org/blog/dns-rrs.html#dnskey)

### EID

There's no shortage of weird architectures that are mapped into or otherwise referenced from within the DNS. This one's for the _Nimrod Routing Architecture_ endpoint identifiers:

```
$ dig +short eid eid.dns.netmeister.org. 
CAFEFACE1234
$ dig +short txt eid.dns.netmeister.org. 
"Format: <octets>"
"Endpoint Identifier in the Nimrod Routing Architecture. Internet Draft (1995)"
$ 
```

RFCs: none -- [expired internet draft](http://ana-3.lcs.mit.edu/~jnc/nimrod/dns.txt)

See also: [NIMLOC](https://netmeister.org/blog/dns-rrs.html#nimloc)

### EUI48

Who needs ARP? Let's just stuff into the DNS, why not. MAC address to DNS name mappings:

```
$ dig +short eui48 eui48.dns.netmeister.org.
bc-a2-b9-82-32-a7
$ dig +short txt eui48.dns.netmeister.org.
"48-bit IEEE Extended Unique Identifier; MAC address. RFC7043 (2013)"
"Format: six two-digit hexadecimal numbers separated by hyphens"
$ 
```

RFCs: [7043](https://datatracker.ietf.org/doc/html/rfc7043)

See also: [EUI64](https://netmeister.org/blog/dns-rrs.html#eui64)

### EUI64

Same as [EUI48](https://netmeister.org/blog/dns-rrs.html#eui48), but for 64-bit Extended Unique Identifiers:

```
$ dig +short eui64 eui64.dns.netmeister.org.
be-a2-b9-ff-fe-82-32-a7
$ dig +short txt eui64.dns.netmeister.org.
"64-bit IEEE Extended Unique Identifier; MAC address. RFC7043 (2013)"
"Format: eight two-digit hexadecimal numbers separated by hyphens"
$ 
```

RFCs: [7043](https://datatracker.ietf.org/doc/html/rfc7043)

See also: [EUI48](https://netmeister.org/blog/dns-rrs.html#eui48)

### GPOS

An older record allowing for the encoding of geographical location, largely superseded by the [LOC](https://netmeister.org/blog/dns-rrs.html#loc) record:

```
$ dig +short gpos gpos.dns.netmeister.org.
"40.731" "-73.9919" "10.0"
$ dig +short txt gpos.dns.netmeister.org.
"Format: <longitude> <latitude> <altitude>"
"Geographical Location, similar to LOC. RFC1712 (1994)"
$ 
```

RFCs: [1712](https://datatracker.ietf.org/doc/html/rfc1712)

See also: [LOC](https://netmeister.org/blog/dns-rrs.html#loc)

### HINFO

An interesting RR from back in the days, when on the internet we regularly advertised just what hardware we were running, what operating system, and what services we might offer. The HINFO record provides the CPU and OS information -- or rather, it used to:

```
$ dig +short hinfo hinfo.dns.netmeister.org.
"PDP-11" "UNIX"
$ dig +short txt hinfo.dns.netmeister.org.
"Originally 'host information' like CPU and OS; now used by
 Cloudflare in response to 'ANY' requests. RFC883 (1983); RFC8482 (2019)"
"Format: two <character-string>s of up to 40 chars each"
$ 
```

It since has since been overloaded by [Cloudflare](https://blog.cloudflare.com/deprecating-dns-any-meta-query-type/) to return minimal responses to [ANY](https://netmeister.org/blog/dns-rrs.html#any) queries:

```
$ dig +short @ns3.cloudflare.com  any cloudflare.com
"RFC8482" ""
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883), [8482](https://datatracker.ietf.org/doc/html/rfc8482)

See also: [ANY](https://netmeister.org/blog/dns-rrs.html#any); the early [DoD Internet Host Table](http://pdp-10.trailing-edge.com/tops20_v6_1_tcpip_installation_tp_ft6/06/new-system/hosts.txt)

### HIP

The [Host Identity Protocol](https://en.wikipedia.org/wiki/Host_Identity_Protocol) (HIP) eliminates the use of IP addresses in favor of Host Identities Tags (HITs) based on the hash of a public key. The HIP RRs allow you to store this information in our favorite distributed database:

```
$ dig +nocmd +nocomments +noquestion +nostats +multiline hip hip.dns.netmeister.org. 
hip.dns.netmeister.org. 2835 IN HIP ( 2
200100107B1A74DF365639CC39F1D578
                                AwEAAbdxyhNuSutc5EMzxTs9LBPCIkOFH8cIvM4p9+LrV4...
                                rvs.example.com. )
$ dig +short txt hip.dns.netmeister.org. 
"Host Identity Protocol mappings of Host Identities and Host Identity Tags to IP addresses.
  RFC5205 (2008); RFC8005 (2016)"
"Format: <pk-algorithm> <base16-encoded-hit> <base64-encoded-public-key>
  <rendezvous-server[1]> ... <rendezvous-server[n]>"
$ 
```

RFCs: [8002"](https://datatracker.ietf.org/doc/html/rfc8002), [8005](https://datatracker.ietf.org/doc/html/rfc8005)

### HTTPS

This is the special purpose [SVCB](https://netmeister.org/blog/dns-rrs.html#svcb) record for use with HTTPS, allowing clients to discover HTTPS parameters beyond the IP address, such as e.g., alternate service endpoints or ports and [Encrypted Client Hello](https://datatracker.ietf.org/doc/html/draft-ietf-tls-esni-08) parameters.

bind-9.16.15 does not yet have support for this resource record, so I defined it as a raw TYPE65 record:

```
;https IN       HTTPS   1 . (
;                               alpn="h3,h2"
;                               ipv6hint="2001:470:30:84:e276:63ff:fe72:3900"
;                               port="8080"
;                               echconfig="ZW5jcnlwdGVkIGNsaWVudCBoZWxsbwo=" )
https   IN      TYPE65 \# 123 2d6e2031202e20616c706e3d2268332c6832222069707...
;       IN      HTTPS   0 www.netmeister.org.                                                       
        IN      TYPE65 \# 25 2d6e2030207777772e6e65746d6569737465722e6f72672e0a                     

$ dig +nocmd +nocomments +noquestion +nostats +multiline TYPE65 https.dns.netmeister.org. 
https.dns.netmeister.org. 3600 IN TYPE65 \# 123 ( 2D6E2031202E20616C706E3D2268332C683222206970
                                763668696E743D22323030313A3437303A33303A3834
                                3A653237363A363366663A666537323A333930302220
                                706F72743D22383038302220656368636F6E6669673D
                                225A57356A636E6C776447566B49474E736157567564
                                43426F5A57787362776F3D220A
)
https.dns.netmeister.org. 3600 IN TYPE65 \# 25 ( 2D6E2030207777772E6E65746D6569737465722E6F72
                                672E0A )
$ dig +short txt https.dns.netmeister.org. 
"Format: <16-bit SvcFieldPriority> <SvcDomainName> <SvcFieldValue>"
"SVCB variation specifically for HTTP/HTTPS. IETF Draft (2020)"
$ 
```

RFCs: [RFC9460](https://www.rfc-editor.org/rfc/rfc9460.html)

See also:  

[Cloudflare blog post](https://blog.cloudflare.com/speeding-up-https-and-http-3-negotiation-with-dns/)  

[Announce HTTPS via DNS (HTTPS + SVCB records)](https://ypcs.fi/howto/2020/09/30/announce-https-via-dns/)

### IPSECKEY

One more RR to share key materials. This one for use with [IPsec](https://en.wikipedia.org/wiki/IPsec). (As we'll see in a moment, there is a generic [KEY](https://netmeister.org/blog/dns-rrs.html#key) record as well, but IPSECKEY was explicitly created as a special purpose RR here.)

```
$ dig +short ipseckey ipseckey.dns.netmeister.org. 
10 0 2 . AQNRU3mG7TVTO2BkR47usntb102uFJtugbo6BSGvgqt4AQ==
$ dig +short txt ipseckey.dns.netmeister.org. 
"Format: <8-bit precedence> <8-bit gateway type> <8-bit algorithm> <40-bit gateway> <base64-encoded public-key>"
"Public Key for use with IPSec; usually stored in the relevant in-addr.arpa / ip6.arpa zone. RFC4025 (2005)"
$ 
```

RFCs: [4025"](https://datatracker.ietf.org/doc/html/rfc4025)

As with other, similar, records, we get a _precedence_ as well as an _algorithm_ selector. Note also that in IPSec, you often don't have a name, but only an IP address, so the IPSECKEY RR would then generally be found in the "[temporary](https://twitter.com/jschauma/status/713903333375868928)" in-addr.arpa / ip6.arpa domain.

### ISDN

The DNS is a phonebook -- everybody knows that. So sure, let's put [ISDN](https://en.wikipedia.org/wiki/Integrated_Services_Digital_Network) phone numbers in it:

```
$ dig +short isdn isdn.dns.netmeister.org. 
"150862028003217" "004"
$ dig +short txt isdn.dns.netmeister.org. 
"Format: <ISDN-address> <optional sa>"
"ISDN Telephone Number. RFC1183 (1990)"
$ 
```

RFCs: [1183](https://datatracker.ietf.org/doc/html/rfc1183)

See also: [X25](https://netmeister.org/blog/dns-rrs.html#x25)

### KEY

The initial DNSSEC KEY record, now obsoleted for DNSSEC by [DNSKEY](https://netmeister.org/blog/dns-rrs.html#dnskey) and for IPsec by [IPSECKEY](https://netmeister.org/blog/dns-rrs.html#ipseckey). Used in combination with the [SIG](https://netmeister.org/blog/dns-rrs.html#sig), [TKEY](https://netmeister.org/blog/dns-rrs.html#tkey) / [TSIG](https://netmeister.org/blog/dns-rrs.html#tsig) records.

```
$ dig +short key key.dns.netmeister.org. 
512 255 2 ACDtkdVR2HWmc0HPEwkrM+SOrWZd8yPTAytLYZj2u33KgwABAgAg6jav 9rTK68C8j+kfLv7+re8KAb1qJXqdSrmL+1l3Js4=
$ dig +short txt key.dns.netmeister.org. 
"Format: <16-bit flags> <8-bit protocol> <8-bit algorithm> <base64-encoded public key>"
"Public Key associated name; used with e.g., TSIG / SIG(0). Obsoleted for DNSSEC keys via DNSKEY,
  for IPSec via IPSECKEY RRs. RFC2535 (1999); RFC2930 (2000); RFC2931 (2000)"
$ 
```

RFCs: [2931](https://datatracker.ietf.org/doc/html/rfc2931)

### KX

Key distribution and discovery is hard. So what do you do if you need to find a specific key, but don't know whom to ask? You ask the DNS!

This record, again useful in e.g., an IPSec context, lets you discover the remote key exchanger systems for a given destination:

```
$ dig +short kx kx.dns.netmeister.org. 
1 panix.netmeister.org.
$ dig +short txt kx.dns.netmeister.org. 
"Format: <16-bit preference> <domain-name>"
"Key Exchange Delegation. RFC2230 (1997)"
$ 
```

RFCs: [2230](https://datatracker.ietf.org/doc/html/rfc2230)

### L32

The [Identifier/Locator Network Protocol](https://en.wikipedia.org/wiki/Identifier/Locator_Network_Protocol) (ILNP) is a network protocol aimed to address multi-homing and inter-domain routing scalabilitiy issues. It comes in two flavors (v4 and v6); the L32 record is used to provide a 32-bit locator value:

```
$ dig +short l32 l32.dns.netmeister.org. 
10 203.0.113.44
$ dig +short txt l32.dns.netmeister.org. 
"Identifier-Locator Network Protocol; 32-bit Locator. RFC6742 (2012)"
"Format: <16-bit preference> <32-bit locator32>"
$ 
```

RFCs: [6742](https://datatracker.ietf.org/doc/html/rfc6742)

See also: [L64](https://netmeister.org/blog/dns-rrs.html#l64), [LP](https://netmeister.org/blog/dns-rrs.html#lp), [NID](https://netmeister.org/blog/dns-rrs.html#nid)

### L64

The v6 version of the [L32](https://netmeister.org/blog/dns-rrs.html#l32) record. Looks like an IPv6 address, but is only 64 bits and must not use the compressed display format (::):

```
$ dig +short l64 l64.dns.netmeister.org. 
10 2001:db8:1140:1000
$ dig +short txt l64.dns.netmeister.org. 
"Format: <16-bit preference> <64-bit locator64>"
"Identifier-Locator Network Protocol; 64-bit Locator. RFC6742 (2012)"
$ 
```

RFCs:[](https://datatracker.ietf.org/doc/html/rfc)

See also: [L32](https://netmeister.org/blog/dns-rrs.html#l32), [LP](https://netmeister.org/blog/dns-rrs.html#lp), [NID](https://netmeister.org/blog/dns-rrs.html#nid)

### LOC

Similar to [GPOS](https://netmeister.org/blog/dns-rrs.html#gpos), but providing more detail, this record is used to stash geolocation information in the DNS:

```
$ dig +short loc loc.dns.netmeister.org. 
40 44 9.000 N 73 59 26.000 W 10.00m 1m 10000m 10m
$ dig +short txt loc.dns.netmeister.org. 
"Format: d-lat [m-lat [s-lat]] {" "N" "|" "S" "}
         d-long [m-long [s-long]] {" "E" "|" "W" "} alt[" "m""]
         [siz[" "m" "] [hp[" "m" "] [vp[" "m" "]]]]"
"Geographical information associated with a domain name. RFC1876 (1996)"
$ 
```

RFCs: [1876](https://datatracker.ietf.org/doc/html/rfc1876)

See also: [GPOS](https://netmeister.org/blog/dns-rrs.html#gpos)

### LP

Another ILNP record, this time used to hold the name of a subnetwork, which should be a FQDN to then be queried for their L32/L64 records:

```
$ dig +short lp lp.dns.netmeister.org. 
lp.dns.netmeister.org. 
20 l32.dns.netmeister.org.
10 l64.dns.netmeister.org.
$ dig +short txt lp.dns.netmeister.org. 
"Format: <16-bit preference> <domain-name>"
"Identifier-Locator Network Protocol; Locator Pointer. RFC6742 (2012)"
$ 
```

RFCs: [6742](https://datatracker.ietf.org/doc/html/rfc6742)

See also: [L32](https://netmeister.org/blog/dns-rrs.html#l32), [L64](https://netmeister.org/blog/dns-rrs.html#l64), [NID](https://netmeister.org/blog/dns-rrs.html#nid)

### MAILA

The first of a number of mail related entries largely obsoleted by the [MX](https://netmeister.org/blog/dns-rrs.html#mx) record. This is not actually a resource record, but again a QTYPE that would match the (equally obsolete) MF and MD records.

### MAILB

Similar to [MAILA](https://netmeister.org/blog/dns-rrs.html#maila), a query of type MAILB would return the MB, MB, or MR records.

### MB

While obsolete, the MB RR is still supported and points to a mailbox name:

```
$ dig +short mb mb.dns.netmeister.org. 
panix.netmeister.org.
$ dig +short txt mb.dns.netmeister.org. 
"Mailbox record. RFC883 (1983); not formally obsoleted"
"Format: <domain-name>"
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883)

### MD

An obsolete and no longer supported "mail delivery" record, specifying the host that is expected to have the mailbox in question.

### MF

An obsolete and no longer supported "mail forwarding" record to specify hosts that are expected to be intermediaries willing to accept the mail for eventual forwarding.

### MG

A "mail group". That is, we used to use the DNS as a mailing list expander, allowing you to point one name to multiple mailboxes. Unlike some of the other mailbox related records, MG is not formally obsoleted:

```
$ dig +short mg mg.dns.netmeister.org. 
jschauma.netmeister.org.
digestingducks.netmeister.org.
jschauma.yahoo.com.
$ dig +short txt mg.dns.netmeister.org. 
"Format: <mailbox>"
"Mail Group (mailing list) record. RFC883 (1983); not formally obsoleted"
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883)

### MINFO

Mail Information. This record is generally associated with an [MG](https://netmeister.org/blog/dns-rrs.html#mg) and contains two mailbox names: one listing the responsible person for the mail group (i.e., the list owner whom you can bother to subscribe or unsubscribe you), the second a mailbox that should receive error messages relating to the mail group:

```
$ dig +short minfo minfo.dns.netmeister.org. 
jschauma.netmeister.org. postmaster.netmeister.org.
$ dig +short txt minfo.dns.netmeister.org. 
"Format: <responsible mailbox> <error mailbox>"
"Responsible and error handling mailbox. RFC883 (1983); not formally obsoleted"
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883)

### MR

A Mail Rename record. The mailer should replace the old mailbox with the new record:

```
$ dig +short mr mr.dns.netmeister.org. 
panix.netmeister.org.
$ dig +short txt mr.dns.netmeister.org. 
"Format: <domain-name>"
"Mail Rename record. RFC883 (1983); not formally obsoleted"
$ 
```

RFCs: [833](https://datatracker.ietf.org/doc/html/rfc833)

### MX

Finally, a record that we're familiar with, the Mail Exchange delegation. Like so many other service discovery records, it comes with a _preference_ to let you specify multiple mail servers that should be tried in order

```
$ dig +short mx mx.dns.netmeister.org. 
50 panix.netmeister.org.
$ dig +short txt mx.dns.netmeister.org. 
"Mail Exchange Delegation. RFC1035 (1987)"
"Format: <16-bit preference> <domain-name>"
$ 
```

RFCs: [974](https://datatracker.ietf.org/doc/html/rfc974), [1035](https://datatracker.ietf.org/doc/html/rfc1035)

### NAPTR

Ok, we're back to weird again. The [Dynamic Delegation Discovery System](https://en.wikipedia.org/wiki/Dynamic_Delegation_Discovery_System) uses the DNS as, what else, a convenient distributed database, encoding rules in "Naming Authority Pointer" records.

These records allow you to specify e.g., regular expressions to rewrite domain names or URNs, and, depending on the context, may be found in the reverse .arpa zones. Here are some contrived examples:

```
$ dig +short naptr naptr.dns.netmeister.org. 
10 10 "u" "smtp+E2U" "!.*([^.]+[^.]+)$!mailto:postmaster@$1!i" .
20 10 "s" "http+N2L+N2C+N2R" "" www.netmeister.org.
$ dig +short txt naptr.dns.netmeister.org. 
"Naming Authority Pointer; regular expression rewriting of domain names, commonly used
 with e.g., SIP. RFC2915 (2000); RFC3403 (2002)"
"Format: <16-bit order> <16-bit preference> <character-string flags> <characer-string services>
         <character-string regex> <domain-name>"
$ 
```

RFCs: [3403](https://datatracker.ietf.org/doc/html/rfc3403)

See also: [Telephone number mapping](https://en.wikipedia.org/wiki/Telephone_number_mapping) using [E.164](https://en.wikipedia.org/wiki/E.164) number to URI mapping (ENUM); [RFC6116](https://datatracker.ietf.org/doc/html/rfc6116)

### NID

Another ILNP record, this time used to hold the _Node Identifier_ with an assigned preference:

```
$ dig +short nid nid.dns.netmeister.org. 
10 14:4fff:ff20:ee64
$ dig +short txt nid.dns.netmeister.org. 
"Identifier-Locator Network Protocol; Node Identifier. RFC6742 (2012)"
"Format: <16-bit preference> <64-bit nodeid>"
$ 
```

RFCs:[](https://datatracker.ietf.org/doc/html/rfc)

See also: [L32](https://netmeister.org/blog/dns-rrs.html#l32), [L64](https://netmeister.org/blog/dns-rrs.html#l64), [LP](https://netmeister.org/blog/dns-rrs.html#lp)

### NIMLOC

Part of the Nimrod Routing Architecture, the NIMLOC record is used ro reference a Nimrod "Locator", a topologically significant "name=" for a Nimrod node:

```
$ dig +short nimloc nimloc.dns.netmeister.org. 
DEADBEEF1234
$ dig +short txt nimloc.dns.netmeister.org. 
"Format: <octets>"
"Nimrod Locator in the Nimrod Routing Architecture. Internet Draft (1995)"
$ 
```

RFCs: none -- [expired internet draft](http://ana-3.lcs.mit.edu/~jnc/nimrod/dns.txt)

See also: [EID](https://netmeister.org/blog/dns-rrs.html#eid)

### NINFO

This record was initially requested to be named ZS for Zone Status, and is intended to provide (arbitary) information about the zone itself, such as the significance of its contents or the last update date etc. (Some of this information is relayed via the [SOA](https://netmeister.org/blog/dns-rrs.html#soa) record.)

```
$ dig +short ninfo ninfo.dns.netmeister.org. 
"The zone owner is asleep, so don't bother trying voice-based communication."
$ dig +short txt ninfo.dns.netmeister.org. 
"Zone Status information, initially requested as 'ZS'. Internet Draft (2008)"
"Format: <text>"
$ 
```

RFCs: none -- [expired internet draft](https://www.ietf.org/archive/id/draft-reid-dnsext-zs-01.txt)

### NS

Hey, NS records! I know what those are! Records that tell you what nameservers are authoritative for a given zone. The records that completely pwn you when controlled by an adversary. The ones that take down your entire site if they go down, and which several companies [nowadays distribute across different TLDs for global redundancy](https://twitter.com/jschauma/status/1373019238470983680).

```
$ dig +short ns ns.dns.netmeister.org. 
panix.netmeister.org.
$ dig +short txt ns.dns.netmeister.org. 
"Format: <domain-name>"
"Naming Authority Pointer; delegates authority of the given domain to the given name server.
 RFC883 (1983); RFC1035 (1987)"
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883)

### NSAP

Here we have Network Service Access Point addresses used in the [Connectionless-mode Network Service](https://en.wikipedia.org/wiki/Connectionless-mode_Network_Service) (CLNS) datagram services mapped into the DNS, why not:

```
$ dig +short nsap nsap.dns.netmeister.org. 
0x47000580005a0000000001e133ffffff00016100
$ dig +short txt nsap.dns.netmeister.org. 
"Network Service Access Point. RFC1706 (1994)"
"Format: <nsap>"
$ 
```

RFCs: [1706](https://datatracker.ietf.org/doc/html/rfc1706)

### NSAP-PTR

Reversing [NSAP](https://netmeister.org/blog/dns-rrs.html#nsap) addresses is done via the NSAP-PTR record, which would normally be found in the respective nsap.int domain:

```
$ dig +short nsap-ptr nsap-ptr.dns.netmeister.org. 
nsap.dns.netmeister.org.
$ dig +short txt nsap-ptr.dns.netmeister.org. 
"Format: <domain-name>"
"NSAP to name mapping. Usually found in the 'nsap.int' domain. RFC1706 (1994)"
$ 
```

RFCs: [1706](https://datatracker.ietf.org/doc/html/rfc1706)

### NSEC

And we're back to DNSSEC. In DNSSEC, the NSEC record indicates the _next secure_ record. This can be used to prove the non-existence of a given record, and by walking these records, you are able to iterate the entire zone.

```
$ dig +short nsec nsec.dns.netmeister.org. 
nsec3.dns.netmeister.org. TXT RRSIG NSEC
$ dig +short txt nsec.dns.netmeister.org. 
"Next secure record. Used to e.g., prove non-existence of a record. RFC4034 (2005)"
"Format: <domain-name>gt; <16-bit type bit map>gt;"
$ 
```

RFCs: [4304](https://datatracker.ietf.org/doc/html/rfc4304)

Now since iterating over an entire zone may not be what you want to let everybody do, we then came up with...

### NSEC3

...the _next secure record, version 3_. This record doesn't leak the names, but uses the next _hashed_ owner name, which still lets you prove non-existence:

```
$ dig +dnssec +nocmd +nocomments +noquestion +nostats +multiline nsec3 nsec3.dns.netmeister.org.
nsec3.dns.netmeister.org. 3600 IN SOA panix.netmeister.org. jschauma.netmeister.org. (
                                2021071404 ; serial
                                3600       ; refresh (1 hour)
                                300        ; retry (5 minutes)
                                3600000    ; expire (5 weeks 6 days 16 hours)
                                3600       ; minimum (1 hour)
                                )
nsec3.dns.netmeister.org. 3600 IN RRSIG SOA 13 4 3600 (
                                20210728163906 20210714153906 24381 nsec3.dns.netmeister.org.
                                f7EGHoqXBB20QAQEqyxuVZPcWWwQXqMWsbJkp/2viWtt
                                oOvI4elv3l4MJle/4GlaTO4sBan9oPxU/r7B+pO1cQ== )
SVFTOLJMUBKMAV5F31Q74AIE460GHTPE.nsec3.dns.netmeister.org. 3600 IN NSEC3 1 0 15 B07196F16C26C4EA (
                                SVFTOLJMUBKMAV5F31Q74AIE460GHTPE
                                NS SOA TXT RRSIG DNSKEY NSEC3PARAM CDS CDNSKEY )
SVFTOLJMUBKMAV5F31Q74AIE460GHTPE.nsec3.dns.netmeister.org. 3600 IN RRSIG NSEC3 13 5 3600 (
                                20210728001204 20210714153906 24381 nsec3.dns.netmeister.org.
                                ZmRCKWj3Puqor3gTgOlxKrv1wvVl62deOKm1U+YmhGHR
                                UQOKsQzvUJ7fb9jshIo3cyN1GpKQmGfH1F9NrxhSfQ== )
$ dig +short txt nsec3.dns.netmeister.org. 
"Next secure record, v3, to prove authenticated denial of existence. RFC5155  (2008)"
"Format: <8-bit hash algorithm> <8-bit flags> <16-bits iterations> <8-bit salt length> <24-bit salt>
         <8-bit hash-length> <24-bit next hashed owner name> <16-bit type bit map>"
$ 
```

RFCs: [5155](https://datatracker.ietf.org/doc/html/rfc5155)

Note that you need the records are generated by the server, and you thus need to query a resolver that supports DNSSEC (+dnssec).

### NSEC3PARAM

The NSEC3 records are calculated using specific paramters which are included in the NSEC3PARAM record. They include the hash algorithm, iterations, and a salt:

```
$ dig +short nsec3param nsec3param.dns.netmeister.org. 
1 0 15 4DE657B8A848081D
$ dig +short txt nsec3param.dns.netmeister.org. 
"Parameters used for NSEC3. RFC5155 (2008)"
"Format: <8-bit hash algorithm> <8-bit flags> <16-bit iterations> <8-bit salt length> <24-bit salt>"
$ 
```

RFCs: [5155](https://datatracker.ietf.org/doc/html/rfc5155)

Note: the NSEC3PARAM value is actually included in the NSEC3 response and can then be used to confirm the correct next name using e.g., [this useful tool](https://github.com/shuque/nsec3hash):

```
$ dig +dnssec +nocmd +nocomments +noquestion +nostats nsec3 nsec3.dns.netmeister.org. | \
        grep "IN NSEC3"
2RM96JUOJE08UCPDC2LVLUDUEFAPNLHE.nsec3.dns.netmeister.org. 3600 IN NSEC3                \
        1 0 15 1C9B2F7BBFB115E4                                                         \
        08R5BALN9R7QHEACJTNHAKHFDMD89ULQ                                                \
        NS SOA TXT RRSIG DNSKEY NSEC3PARAM CDS CDNSKEY
$ python nsec3hash.py 1C9B2F7BBFB115E4 1 15 nsec3.dns.netmeister.org.
2RM96JUOJE08UCPDC2LVLUDUEFAPNLHE
$ dig +short txt next.nsec3.dns.netmeister.org.
"A text record so that we can show the use of NSEC3."
$ python nsec3hash.py 1C9B2F7BBFB115E4 1 15 next.nsec3.dns.netmeister.org.
08R5BALN9R7QHEACJTNHAKHFDMD89ULQ
$ dig +dnssec +nocmd +nocomments +noquestion +nostats nsec3 next.nsec3.dns.netmeister.org. | \
        grep "IN NSEC3"
08R5BALN9R7QHEACJTNHAKHFDMD89ULQ.nsec3.dns.netmeister.org. 3600 IN NSEC3                     \
        1 0 15 1C9B2F7BBFB115E4                                                              \
        2RM96JUOJE08UCPDC2LVLUDUEFAPNLHE                                                     \
        TXT RRSIG

```

In other words, we have shown that there are no other records in the zone besides the apex records and the "next" text record, but we could not have discovered "next" without knowing it!

### NULL

This one's sneaky: [RFC1035](https://datatracker.ietf.org/doc/html/rfc1035) defines it as an "experimental" RR allowing any data whatsoever, "so long as it is 65535 octets or less." However, it also specifies that NULL records are not allowed in master files, so e.g., bind will refuse to load a zone with a NULL record.

However, you can specify it in the zone file using the [generic opaque record format](https://www.rfc-editor.org/rfc/rfc3597):

```
null    IN        TYPE10 \# 7 61766f6361646f

$ dig +short null.dns.netmeister.org
null \# 7 61766F6361646F
$ dig +short null.dns.netmeister.org txt
"Format: "
"Placeholder records in some experimental extensions. RFC883 (1983)"
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883), [1035](https://datatracker.ietf.org/doc/html/rfc1035)

### NXT

When DNSSEC was first drafted, the problem of how to assert authoritatively the non-existence of a record was initially solved using the NXT record. This is no longer automatically calculated (since we have [NSEC](https://netmeister.org/blog/dns-rrs.html#nsec) and [NSEC3](https://netmeister.org/blog/dns-rrs.html#nsec3)), so I've stubbed one out here:

```
$ dig +short nxt nxt.dns.netmeister.org. 
openpgpkey.dns.netmeister.org. TXT OPENPGPKEY
$ dig +short txt nxt.dns.netmeister.org. 
"Precursor to NSEC/NSEC3. RFC2065 (1997)"
"Format: <domain-name> <16-bit type bit map>"
$ 
```

RFCs: [2065](https://datatracker.ietf.org/doc/html/rfc2065), [3755](https://datatracker.ietf.org/doc/html/rfc3755)

### OPENPGPKEY

We already saw one way to store a PGP key in the DNS by using the [CERT](https://netmeister.org/blog/dns-rrs.html#cert) record above. But there are [quite a few ways](https://slxh.nl/blog/2016/pgp-and-dns/), and here we use the OPENPGP record, in context of [DANE](https://en.wikipedia.org/wiki/DNS-based_Authentication_of_Named_Entities):

```
$ dig +multiline +nocmd +nocomments +noquestion +nostats openpgpkey openpgpkey.dns.netmeister.org.
openpgpkey.dns.netmeister.org. 3600 IN OPENPGPKEY ( mQENBE2L+QkBCADx6DXFdqDEAK1OYYtOeLp54Z0G87t6
                                 Nmz+nodbd9f4Uw0T6v32O2O0yVwA07fCGfPc+3oeCgDa
[...]
                                 odY5Nsz1QchbMHN2FVmmFfrVpocnRQPm1lxqzxwoqJrU
                                 TyWpk/J8/0PbKlSTjRKziFLqudSy/dqFWmk= )
$ dig +short txt openpgpkey.dns.netmeister.org. 
"OpenPGP Public Key record, used within DANE. RFC7929 (2016)"
"Format: base64-encoded OpenPGP Transferable Public Key"
$ 
```

RFCs: [7929](https://datatracker.ietf.org/doc/html/rfc7929)

DNSSEC is valuable here, since you want to establish authenticity of the key. But you also want to have a way to automatically discover these records without having to know what the owner name to look up is. For that, RFC7929 establishes a standardized location -- only, that gets tricky, because of course [email addresses can be all sorts of funky](https://netmeister.org/blog/email.html). So instead of the rather convenient jschauma.netmeister.org. label, I have to use:

```
$ dig +multiline +nocmd +nocomments +noquestion +nostats openpgpkey                           \
        f6d6048431f8b67313b5b8011e0be5b03f21b4458a7e67f3fb298900._openpgpkey.netmeister.org.
f6d6048431f8b67313b5b8011e0be5b03f21b4458a7e67f3fb298900._openpgpkey.netmeister.org.          \
        10800 IN OPENPGPKEY (                                                                 \
                                mQENBE2L+QkBCADx6DXFdqDEAK1OYYtOeLp54Z0G87t6                  \
                                Nmz+nodbd9f4Uw0T6v32O2O0yVwA07fCGfPc+3oeCgDa                  \
                                ct5cpicAm1C1nF3XrcV6YCAccswybl11ZnlJBOtu1ieP                  \
[...]
                                odY5Nsz1QchbMHN2FVmmFfrVpocnRQPm1lxqzxwoqJrU                  \
                                TyWpk/J8/0PbKlSTjRKziFLqudSy/dqFWmkAAguQ )
$ 
```

You can use [this site](https://www.huque.com/bin/openpgpkey) to generate the right OPENPGPKEY entry from your key.

See also: [CERT](https://netmeister.org/blog/cert), [New Adventures in DNSSEC and DANE](https://netmeister.org/blog/dnssec-dane.html)

### PTR

Ok, back in save waters. PTR is something we all know and use all the time. Although virtually every lookup you perform of this type is likely to be for IP addresses reversed in the in-addr.arpa / ip6.arpa domains, but you can add it in any zone (even if that may not be useful):

```
$ dig +short ptr ptr.dns.netmeister.org.
ptr.dns.netmeister.org.
$ dig +short txt ptr.dns.netmeister.org.
"Format: <domain-name>"
"Domain Name Pointer; commonly found in the in-addr.arpa and ip6.arpa domains and
 used in reverse lookups. RFC1035 (1987)"
$ 
```

As you probably know, the usual host lookup requires you to reverse the IP address (and translate from hex to decimal for IPv6 nibbles). So a normal PTR lookup of an address will not actually work. Fortunately, your common host lookup tools are kind enough to do the right thing for you when you pass it an IP address without specifying the RR type:

```
$ host www.netmeister.org
www.netmeister.org is an alias for panix.netmeister.org.
panix.netmeister.org has address 166.84.7.99
panix.netmeister.org has IPv6 address 2001:470:30:84:e276:63ff:fe72:3900
$ dig +short ptr 166.84.7.99
$ dig +short ptr 2001:470:30:84:e276:63ff:fe72:3900
$ dig +short ptr 99.7.84.166.in-addr.arpa
panix.netmeister.org.
$ dig +short ptr 0.0.9.3.2.7.e.f.f.f.3.6.6.7.2.e.4.8.0.0.0.3.0.0.0.7.4.0.1.0.0.2.ip6.arpa
panix.netmeister.org.
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883)

### PX

Aaaaaaand we're back to stashing weird things into the DNS. This time mapping information needed by [Mime Internet X.400 Enhanced Relay](https://datatracker.ietf.org/doc/html/rfc2156) (MIXER) conformant e-mail gateways and other tools to map [RFC822](https://datatracker.ietf.org/doc/html/rfc822) domain names into X.400 O/R names and vice versa.

```
$ dig +short px px.dns.netmeister.org.
10 px.dns.netmeister.org. PRMD-netmeister.C-us.G-Jan.S-Schaumann.dns.netmeister.org.
$ dig +short txt px.dns.netmeister.org.
"Map domain names into X.400 O/R names. RFC2163 (1998)"
"Format: <16-bit preference> <domain-name> <x400-in-domain-syntax>"
$ 
```

RFCs: [2163](https://datatracker.ietf.org/doc/html/rfc2163)

### RP

The RP record assigns a _responsible person_ for the given domain name. Because we know that there's a lot of responsible people involved in putting data into the DNS, I suppose.

The [SOA](https://netmeister.org/blog/dns-rrs.html#soa) record includes a responsible person for the zone, but that person may not be responsible for each individual node, so the RP record lets you be more specific by (a) defining a contact and (b) a separate domain name, which should contain additional information in it's [TXT](https://netmeister.org/blog/dns-rrs.html#txt) record:

```
$ dig +short rp rp.dns.netmeister.org.
jschauma.netmeister.org. contact.netmeister.org.
$ dig +short txt rp.dns.netmeister.org.
"Format: <mbox-dname> <txt-dname>"
"Responsible Person. RFC1183 (1990)"
$ dig +short txt contact.netmeister.org.
"Email preferred, but you can also find me at https://twitter.com/jschauma."
$ 
```

RFCs: [1183](https://datatracker.ietf.org/doc/html/rfc1183)

### RRSIG

The DNSSEC _Resource Record Digital Signature_ RRSIG record contains the digital signatures associated with the given name and are generated automatically by the (DNSSEC enabled) nameserver:

```
$ dig +multiline +nocmd +nocomments +noquestion +nostats rrsig rrsig.dns.netmeister.org.
rrsig.dns.netmeister.org. 3521 IN RRSIG NSEC 13 4 3600 (
                                20210726001627 20210713021645 56039 dns.netmeister.org.
                                zp+/jdn4LXLMcsrg47CvsykcTh3gQBrq+f78yOVqwjgE
                                P9iWJOoSk3mK/rNu0VeBLSwMEpWCu4H6DFv6cFJWVw== )
rrsig.dns.netmeister.org. 3521 IN RRSIG TXT 13 4 3600 (
                                20210728112730 20210714104850 56039 dns.netmeister.org.
                                voZozkv+icMI8Z4Whg2QxcboK2RUbXMS/2IHqxcLUc62
                                ATR1zo4RwlacuspVqzcbvKYq7o8aCfCZh7rIiPG4rA== )
$ dig +short txt rrsig.dns.netmeister.org.
"DNSSEC Signature of the Resource Record Set. RFC4034 (2005)"
"Format: <16-bit type covered> <8-bit algorithm> <8-bit labels> <32-bit TTL>        \
        <32-bit signature expiration> <32-bit signature inception> <16-bit key tag> \
        <40-bit signers name> <base64-encoded signature>"
$ 
```

RFCs: [4034](https://datatracker.ietf.org/doc/html/rfc4034)

Here we again see the [NSEC](https://netmeister.org/blog/dns-rrs.html#nsec) record, which is signed just like the [TXT](https://netmeister.org/blog/dns-rrs.html#txt) record. The records are signed with the domain's private key matching the [DNSKEY](https://netmeister.org/blog/dns-rrs.html#dnskey) record.

Note that a given RRSIG covers _all_ records of a given type, which is why we see only _one_ RRSIG in the above for _two_ TXT records.

### RT

The _Route Through_ (RT) record provides a preference and intermediate host to be used to route or relay through when talking to the owner:

```
$ dig +short rt rt.dns.netmeister.org 
10 panix.netmeister.org.
$ dig +short txt rt.dns.netmeister.org 
"Format: <16-bit preference> <intermediate-host>"
"Route Through RR. RFC1183 (1990)"
$ 
```

RFCs: [1183](https://datatracker.ietf.org/doc/html/rfc1183)

### SIG

Another signature record! This was replaced in DNSSEC by the [RRSIG](https://netmeister.org/blog/dns-rrs.html#rrsig) records, but may still be used in other context. These records are generated by the server in combination with the [KEY](https://netmeister.org/blog/dns-rrs.html#key), [TKEY](https://netmeister.org/blog/dns-rrs.html#tkey) / [TSIG](https://netmeister.org/blog/dns-rrs.html#tsig) records.

bind(8) only uses SIG records when using nsupdate(8).

RFCs: [2535](https://datatracker.ietf.org/doc/html/rfc2535), [2931](https://datatracker.ietf.org/doc/html/rfc2931), [4034](https://datatracker.ietf.org/doc/html/rfc4034)

### SINK

Ah, finally a record that speaks the truth! The SINK record was intended to provide a general purpos "kitchen sink" record to obviate the need to stuff weird things into, say, [TXT](https://netmeister.org/blog/dns-rrs.html#txt) records. It never took off, though, and [bind even implements it with an undocumented additional field](https://gitlab.isc.org/isc-projects/bind9/-/issues/1202). But this is what it'd look like:

```
$ dig +short sink sink.dns.netmeister.org 
0 64 1 ZG5zLm5ldG1laXN0ZXIub3JnLg==
$ dig +short txt sink.dns.netmeister.org 
"Kitchen Sink record to allow stuffing just about anything into the DNS without
  requiring new RRs to be defined. Internet draft (1997)"
"Format: <coding> <subcoding> <base64 data>"
$ 
```

RFCs: none -- [expired internet draft](https://tools.ietf.org/html/draft-eastlake-kitchen-sink)

### SMIMEA

This one's kind of like [OPENPGPDATA](https://netmeister.org/blog/dns-rrs.html#openpgpdata), in that you want to make an [S/MIME](https://en.wikipedia.org/wiki/S/MIME) certificate to an identity, and so normally you'd also use the same mechanism to map a user's email address.

```
$ dig +short smimea smimea.dns.netmeister.org 
3 1 1 8CE14CBE1FAFAE9FB25845D335E00E416BC2FAE02E8746689C006DA5 9C1F9382
$ dig +short txt smimea.dns.netmeister.org 
"S/MIME certificate association. RFC8162 (2017)"
"Format: <8-bit cert usage> <8-bit selector> <8-bit matching type>
         <base64-encoded certificate data>"
$ 
```

RFCs: [8162](https://datatracker.ietf.org/doc/html/rfc8162)

### SOA

Every zone has a _Start of Authority_ record, which includes the responsible name server, responsible person, and information about the zone:

```
$ dig +multiline +nocmd +nocomments +noquestion +nostats soa soa.dns.netmeister.org.
soa.dns.netmeister.org. 3600 IN SOA panix.netmeister.org. jschauma.netmeister.org. (
                                2021071108 ; serial
                                3600       ; refresh (1 hour)
                                300        ; retry (5 minutes)
                                3600000    ; expire (5 weeks 6 days 16 hours)
                                3600       ; minimum (1 hour)
$ dig +short txt soa.dns.netmeister.org 
"Start of Authority information about the given zone. RFC883 (1983); RFC1035 (1987)"
"Format: <domain-name> <domain-name> <32-bit serial> <32-bit refresh interval> \
  <32-bit retry interval> <32-bit expiration interval> <32-bit minimum TTL>"
$ 
```

RFCs: [883](https://datatracker.ietf.org/doc/html/rfc883)

### SPF

The [Sender Policy Framework](https://en.wikipedia.org/wiki/Sender_Policy_Framework) started using [TXT](https://netmeister.org/blog/dns-rrs.html#txt) records to define a policy for a domain, but of course stashing even more information into TXT records is silly, so a new RR was created: SPF. Nobody adopted this record, and everybody still uses TXT records, however.

```
$ dig +short spf spf.dns.netmeister.org 
"v=spf1 a mx -all"
$ dig +short txt spf.dns.netmeister.org 
"Format: <spf text>"
"Sender Policy Framework alternative to TXT record. RFC4408 (2006)"
$ 
```

RFCs: [4408](https://datatracker.ietf.org/doc/html/rfc4408)

### SRV

The SRV record is used to help clients find the correct service endpoint for the given name. It is most commonly used via \_port.\_protocol records, such as e.g., \_kerberos.\_udp or \_ldap.\_tcp:

```
$ dig +short srv srv.dns.netmeister.org 
0 1 80 panix.netmeister.org.
$ dig +short txt srv.dns.netmeister.org 
"Format: <16-bit priority> <16-bit weight> <16-bit port> <domain-name>"
"Service location records. Commonly something like _port._protocol. RFC2052 (1996); RFC2782 (2000)"
$ 
```

RFCs: [2782](https://datatracker.ietf.org/doc/html/rfc2782)

See also: [SVCB](https://netmeister.org/blog/dns-rrs.html#svcb), [RFC6763](https://datatracker.ietf.org/doc/html/rfc6763)

### SSHFP

What do you do when you ssh to a host and get this warning?

```
$ ssh somehost
The authenticity of host 'somehost' ([203.0.113.4]:22)' can't be established.
ECDSA key fingerprint is SHA256:YkdaIvHk8JWUIGU5qv+Qpu2qurG6b0pnqzkGF3RVz4Q.
Are you sure you want to continue connecting (yes/no/[fingerprint])?

```

You type yes. And in all likelihood, you change your ~/.ssh/config to set StrictHostKeyChecking no, to boot. In the history of the internet, not a single person has ever validated the SSH hostkey.

And it's a difficult problem, anyway. Rebuilding a system changes the key and then you get a conflict. Now, you _could_ use SSH certificates, which solves many of these problems, but you could also... guess what... stuff the information into the DNS!

```
$ dig +short sshfp sshfp.dns.netmeister.org 
3 2 62475A22F1E4F09594206539AAFF90A6EDAABAB1BA6F4A67AB390617 7455CF84
1 1 53A76D5284C91E140DEC9AD1A757DA123B95B081
$ dig +short txt sshfp.dns.netmeister.org 
"SSH Public Key Fingerprints. RFC4255 (2006)"
"Format: <8-bit algorithm> <8-bit fingerprint type> <fingerprint>"
$ 
```

RFCs: [4255](https://datatracker.ietf.org/doc/html/rfc4255)

Now note that of course you (again) need DNSSEC for OpenSSH to accept and trust the fingerprints from the DNS. It's almost as if DNSSEC was a good idea to assert the authenticity of the data in our favorite globally distributed database...

### SVCB

The more generic version of the [HTTPS](https://netmeister.org/blog/dns-rrs.html#https) record we saw above. Similar to before, we're using TYPE64 since bind(8) hasn't implemented SVCB yet:

```
$ dig +multiline +nocmd +nocomments +noquestion +nostats TYPE64 svcb.dns.netmeister.org.
svcb.dns.netmeister.org. 3575 IN TYPE64 \# 85 ( 2D6E20312070616E69782E6E65746D6569737465722E
                                6F72672E206970763668696E743D22323030313A3437
                                303A33303A38343A653237363A363366663A66653732
                                3A333930302220706F72743D2238383838220A )
$ dig +short txt svcb.dns.netmeister.org.
"General Purpose Service Binding. IETF Draft (2020)"
"Format: <16-bit SvcFieldPriority> <SvcDomainName> <SvcFieldValue>"
$ 
```

RFCs: none -- [RFC9460](https://www.rfc-editor.org/rfc/rfc9460.html)

See also: [HTTPS](https://netmeister.org/blog/dns-rrs.html#https)  

[Cloudflare blog post](https://blog.cloudflare.com/speeding-up-https-and-http-3-negotiation-with-dns/)  

[Announce HTTPS via DNS (HTTPS + SVCB records)](https://ypcs.fi/howto/2020/09/30/announce-https-via-dns/)

### TA

DNSSEC depends on an established trust hierarchy down from the root (.). The proposal for a TA record allows for DNSSEC without a signed root by defining _Trusted Authorities_ that may make statements for their target zones:

```
$ dig +short ta ta.dns.netmeister.org. 
56039 13 2 4104805B43928FC573F0704A2C1B5A10BAA2878DE26B8535DDE77517 C154CE9F
$ dig +short txt ta.dns.netmeister.org. 
"DNSSEC Trust Authorities; proposed for DNSSEC without a signed root. No RFC (2005)."
"Format: <16-bit key tag> <8-bit algorithm> <8-bit digest type>
         <digest of owner name concatenated with the DNSKEY RDATA>"
$ 
```

RFCs: none -- [proposal from CMU](http://www.watson.org/~weiler/INI1999-19.pdf)

### TALINK

In a way similar to the [TA](https://netmeister.org/blog/dns-rrs.html#ta) record, this record allows the zone administrator to establish a doubly linked list of names that contain [DNSKEY](https://netmeister.org/blog/dns-rrs.html#dnskey) data.

```
$ dig +short talink talink.dns.netmeister.org. 
. _talink1.dns.netmeister.org.
$ dig +short talink _talink1.dns.netmeister.org. 
talink.dns.netmeister.org. _talink2.dns.netmeister.org.
$ dig +short talink _talink2.dns.netmeister.org. 
_talink2.dns.netmeister.org. .
$ dig +short txt talink.dns.netmeister.org. 
"Format: <domain-name> <domain-name>"
"DNSSEC Trust Anchor History. Internet Draft (2009)"
$ 
```

RFCs: none -- [expired internet draft](https://datatracker.ietf.org/doc/html/draft-wijngaards-dnsop-trust-history-02)

### TKEY

Hey, we have _yet_ another record to hold keys, this one for _Transaction Keys_, used to establish shared secret keys for use with Transaction Signature ([TSIG](https://netmeister.org/blog/dns-rrs.html#tsig)) records. This record is really a meta-RR that is not actually stored in the DNS, nor found in the zone.

bind(8) only supports Diffie-Hellman key exchange modes and requires that the client includes an appropriate [KEY](https://netmeister.org/blog/dns-rrs.html#key) record in the "additional" section of the query and be signed using either a [TSIG](https://netmeister.org/blog/dns-rrs.html#tsig) or [SIG(0)](https://netmeister.org/blog/dns-rrs.html#sig) using a previously established key.

In other words: it's complicated.

RFCs: [2930](https://datatracker.ietf.org/doc/html/rfc2930)

### TLSA

Look, it's our [good friend DANE](https://netmeister.org/blog/dnssec-dane.html)! This time coming to the rescue and making the traditional public CA hierarchy for Transport Layer Security (TLS) obsolete.

That is, using the TLSA records in a DNSSEC signed zone, a client could verify the authenticity of the certificate offered by a given remote service without having to rely on a trust bundle of hundreds of certificates provided by CAs with a profit interest in selling certificates.

```
$ dig +short tlsa tlsa.dns.netmeister.org. 
3 1 1 8CE14CBE1FAFAE9FB25845D335E00E416BC2FAE02E8746689C006DA5 9C1F9382
$ dig +short txt tlsa.dns.netmeister.org. 
"Format: <8-bit usage> <8-bit selector> <8-bit matching type> <cert data>"
"DANE record for TLS. RFC6698 (2012)"
$ 
```

RFCs: [6698](https://datatracker.ietf.org/doc/html/rfc6698)

Note: the TLSA record is usually associated with a service name using the \_port.\_protocol format. The above result is actually the correct TLSA record for this website, found at \_443.\_tcp.panix.netmeister.org..

### TSIG

This signature record is generated by the server and requires a shared key between the client and server. That is, you'd first generate a secret key to be included in your server configuration, then share this with the client and allow the client to make the request:

```
$ tsig-keygen tsig.dns.netmeister.org.
key "tsig.dns.netmeister.org." {
        algorithm hmac-sha256;
        secret "g3VNiujhmzuXdEVTV0SiVG0ad2ViTI/AtiPMCDjj77s=";
};
$ dig +multiline +nocmd +nocomments +noquestion +nostats                                  \
      -y hmac-sha256:tsig.dns.netmeister.org:g3VNiujhmzuXdEVTV0SiVG0ad2ViTI/AtiPMCDjj77s= \
      tsig tsig.dns.netmeister.org.
tsig.dns.netmeister.org. 0 ANY TSIG hmac-sha256. 1626303929 300 32 (
                                qNZ1VviSJTtqX+czOMuAxU34Zx2FG0qsTb/EypPLOC8= ) 30681 NOERROR 0 
# Using an old or invalid key:
$ dig +multiline +nocmd +nocomments +noquestion +nostats   \
      -y hmac-sha256:tsig.dns.netmeister.org:d2hhdGV2ZXIK  \
      tsig tsig.dns.netmeister.org.
;; Couldn't verify signature: tsig indicates error
tsig.dns.netmeister.org. 0 ANY TSIG hmac-sha256. 1626304045 300 0 44982 BADSIG 0 
$ dig +short txt tsig.dns.netmeister.org.
"Format: <algorithm name> <48-bit time signed> <16-bit fudge> <16-bit MAC size>
 <MAC> <16-bit oid> <16-bit error> <16-bit other size> <other data>"
"Transaction Signature, used to authenticate e.g., dynamic client updates or server responses
 by way of a shared secret (e.g., TKEY). RFC2845 (2000); RFC8945 (2020)"
$ 
```

RFCs: [8945](https://datatracker.ietf.org/doc/html/rfc8945)

See also: [KEY](https://netmeister.org/blog/dns-rrs.html#key), [SIG](https://netmeister.org/blog/dns-rrs.html#sig), [TKEY](https://netmeister.org/blog/dns-rrs.html#tkey).

Note: since I'm sharing the shared key here, there is of course now _no_ guarantee of authenticity of the signatures. Don't trust TSIG's from this DNS server. :-)

### TXT

Our true kitchen sink. None of this [SINK](https://netmeister.org/blog/dns-rrs.html#sink) nonsense. A record intended for "descriptive text" that is now completely overloaded and used for... well, let's see:

-   GnuPG [discovery of public keys](https://www.grepular.com/Publishing_PGP_Keys_in_the_DNS) using local.\_pka labels
-   SPF instead of, uhm, [SPF](https://netmeister.org/blog/dns-rrs.html#spf) records
-   [DKIM records](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail)
-   [DMARC](https://en.wikipedia.org/wiki/DMARC) records
-   [SMTP MTA Strict Transport Security](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol#SMTP_MTA_Strict_Transport_Security)
-   [SMTP TLS Reporting](https://datatracker.ietf.org/doc/html/rfc8460)
-   proof of domain overship for [Certificate Authority Domain Validation](https://uk.godaddy.com/help/verify-domain-ownership-html-or-dns-for-my-ssl-certificate-7452)
-   [Google Site Verification](https://support.google.com/webmasters/answer/9008080)
-   [Facebook Domain Verification](https://developers.facebook.com/docs/sharing/domain-verification/)
-   [Amazon SES domain validation](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/dns-txt-records.html)
-   [Use of domain validation by just about any cloud service](https://twitter.com/jschauma/status/1361437634648805378)
-   [Security contacts via DNS records](https://dnssecuritytxt.org/), similar to [security.txt](https://securitytxt.org/): e.g., \_security.netmeister.org
-   [DNS-based Service Discovery](https://datatracker.ietf.org/doc/html/rfc6763) (RFC6763)
-   ...

```
$ dig +short txt txt.dns.netmeister.org.
"Format: <text>"
"Descriptive text. Completely overloaded for all sorts of things. RFC1035 (1987)"
$ dig +short txt jschauma._pka.netmeister.org.
"v=pka1;fpr=99CE1DC7770AC5A809A60DCD66CE4FE96F6BD3D7;uri=https://www.netmeister.org/public_key.gpg.asc"
$ dig +short txt netmeister.org.
"v=spf1 a mx -all"
$ dig +short txt 2021._domainkey.netmeister.org.
"v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCwSwZZHRcoVIHxjlETstEBKt/
YiLFpZ0VGvz1ufZLVWKuOF1iKJOF/rDjzehyNK2CkJscPzcuMV6zMQyxiEOOPl4Pkugd4G27G4klqLK9TZ
EC3a77Iy4c1gu+10CSjsPPZgNCflLxmw/VtPs6n/ENgZyH6HUAv04yw8aMQnVnwlQIDAQAB"
$ dig +short txt _dmarc.netmeister.org.
"v=DMARC1; p=quarantine; pct=100; rua=mailto:postmaster@netmeister.org"
$ dig +short txt _mta-sts.netmeister.org.
"v=STSv1; id=20210324T224413"
$ dig +short txt _smtp._tls.netmeister.org.
"v=TLSRPTv1; rua=https://www.netmeister.org/cgi-bin/report-uri?tls-rpt,mailto:postmaster@netmeister.org"
$ 
```

RFCs: [1035](https://datatracker.ietf.org/doc/html/rfc1035), [6376](https://datatracker.ietf.org/doc/html/rfc6376), [7489](https://datatracker.ietf.org/doc/html/rfc7489), [7208](https://datatracker.ietf.org/doc/html/rfc7208), [8461](https://datatracker.ietf.org/doc/html/rfc8461)

### URI

This is another one of those service discovery records overlapping with [SRV](https://netmeister.org/blog/dns-rrs.html#srv) and [NAPTR](https://netmeister.org/blog/dns-rrs.html#naptr) records, for example. Normally, you'd set these on \_service.\_proto names.

```
$ dig +short uri uri.dns.netmeister.org.
10 1 "https://www.netmeister.org/blog/dns-rrs.html"
$ dig +short txt uri.dns.netmeister.org.
"Format: <16-bit priority> <16-bit weight> <uri>"
"URI selection. Improvement / complement to NAPTR / SRV. RFC7553 (2015)"
$ 
```

RFCs: [7553](https://datatracker.ietf.org/doc/html/rfc7553)

### WKS

The _Well Known Services_ record again reflects the olden days much like [HINFO](https://netmeister.org/blog/dns-rrs.html#hinfo) did: a way to tell the world what services your host is offering, i.e., what ports it has open:

```
$ dig +short wks wks.dns.netmeister.org.
166.84.7.99 6 25 80 443
166.84.7.99 17 53
$ dig +short txt wks.dns.netmeister.org.
"Format: <32-bit IP address> <16-bit protocol> <8-bit bit map>"
"Well Known Services. RFC883 (1983); not formally obsoleted, but recommended
 against in e.g., RFC1123 (1989)"
$ 
```

RFCs: [833](https://datatracker.ietf.org/doc/html/rfc833)

### X25

Similar to [ISDN](https://netmeister.org/blog/dns-rrs.html#isdn) records, [X.25](https://en.wikipedia.org/wiki/X.25) Public Switched Data Network (PSDN) addresses can be mapped into the DNS using the X25 RR:

```
$ dig +short x25 x25.dns.netmeister.org.
"311061700956"
$ dig +short txt x25.dns.netmeister.org.
"Format: <PSDN-address>"
"Experimental representation of X.25 addresses. RFC1183 (1990)"
$ 
```

RFCs: [1183](https://datatracker.ietf.org/doc/html/rfc1183)

### ZONEMD

A message digest for the entire zone - why would you need that, if you have DNSSEC? Well, DNSSEC covers individual records, but ZONEMD covers the entire zone all in one go, and as such is set on the apex:

```
$ dig +short zonemd zonemd.dns.netmeister.org.
2021071219 1 1 4274F6BC562CF8CE512B21AA0A4CCC1EB9F4FAAAECD01642D0A07BDE A890C8845849D6015CC590F54B0AC7E87B9E41ED
$ dig +short txt zonemd.dns.netmeister.org.
"Message Digest for zone data. RFC8976 (2021)"
"Format: <32-bit serial> <8-bit scheme> <8-bit hash algorithm> <digest>"
$ 
```

RFCs: [8976](https://datatracker.ietf.org/doc/html/rfc8976)

You can calculate the ZONEMD hash using e.g., [this tool](https://github.com/niclabs/dns-tools).

___

And there you have it: 80 DNS Resource Records from [A](https://netmeister.org/blog/dns-rrs.html#a) to [ZONEMD](https://netmeister.org/blog/dns-rrs.html#zonemd), all able to be queried and returning reasonable results. Quite a few more than you normally would encounter, I'd wager, but all in all a pretty good illustration of the ubiquity and flexibility of the DNS beyond just mapping hostnames to IP addresses.

You may also encounter an RR of TYPE65534, which bind(8) [uses to signal the signing state](https://bind9.readthedocs.io/en/latest/advanced.html#private-type-records), or the OPT pseudo-RR used per [RFC6891](https://datatracker.ietf.org/doc/html/rfc6891) in the additional data section for EDNS requests (e.g., [EDNS Client Subnet](https://en.wikipedia.org/wiki/EDNS_Client_Subnet) (ECS)).

In addition it's worth noting that the DNS does not only cover the IN (i.e., Internet) class, but is also used within the [Hesiod](https://en.wikipedia.org/wiki/Hesiod_(name_service)) and [Chaosnet](https://en.wikipedia.org/wiki/Chaosnet) protocols. For example, you can ask the [F-Root](https://www.isc.org/f-root/) to show you which of the many locations you conncted to by querying it for a (what else) TXT record in the CHAOS class:

```
$ dig +short @f.root-servers.net hostname.bind chaos txt
"EWR.cf.f.root-servers.org"
$ 
```

The first three letters of the returned hostname are the IATA three letter airport code (although use of the [LOC](https://netmeister.org/blog/dns-rrs.html#loc) record would have been neat, I think).

As noted above, the zone file served from panix.netmeister.org for the dns.netmeister.org zone is available [on GitHub](https://github.com/jschauma/dns-rrs/), so if I made any mistakes, please [let me know](mailto:jschauma@netmeister.org) and/or submit a pull request!

July 15th, 2021

___

See also:

-   This blog post as [a Twitter thread](https://twitter.com/jschauma/status/1415858462064533505)
-   A related Twitter thread: [Rule 53: if you can think of it, someone's done it in the DNS](https://twitter.com/pgl/status/1405614755000295427)
-   [WHOIS: Fragile, unparseable, obsolete... and universally relied upon](https://netmeister.org/blog/whois.html)
-   [What's in a hostname?](https://netmeister.org/blog/hostnames.html)
-   [TLDs -- Putting the 'Fun' in the top of the DNS](https://netmeister.org/blog/tlds.html)
-   [New Adventures in DNSSEC and DANE](https://netmeister.org/blog/dnssec-dane.html)
-   [DNS Security: Threat Modeling DNSSEC, DoT, and DoH](https://netmeister.org/blog/doh-dot-dnssec.html)
-   [DNS tcpdump by example](https://netmeister.org/blog/dns-tcpdump.html)
-   Video series: The Domain Name System, [Part I](https://youtu.be/-bpIT7M9i00), [Part II](https://youtu.be/z55ULZcKP8A), [Part III](https://youtu.be/XDJEJFVNoko)
-   [DNS Looking Glass](https://www.bortzmeyer.org/dns-lg-usage.html)
-   Discussion on [Hacker News](https://news.ycombinator.com/item?id=27852601)
-   Discussion on [Lobsters](https://lobste.rs/s/a9ruj0)
