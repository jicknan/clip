---
title: Why is DNS still hard to learn? 为什么 DNS 仍然很难学？
date: 2023-12-14T08:35:31.000Z
updated: 2023-12-14T08:35:31.000Z
published: 2023-07-28T08:35:31.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://jvns.ca/blog/2023/07/28/why-is-dns-still-hard-to-learn/
  hostname: jvns.ca
  author: Julia Evans
  original_title: Why is DNS still hard to learn?
  original_lang: en

---

I write a lot about technologies that I found hard to learn about. A while back my friend Sumana asked me an interesting question – why are these things so hard to learn about? Why do they seem so mysterious?  

我写了很多关于我发现很难学习的技术的文章。不久前，我的朋友苏马纳问了我一个有趣的问题——为什么这些东西这么难学？为什么他们看起来如此神秘？

For example, take DNS. We’ve been using DNS since the [80s](https://www.ietf.org/rfc/rfc1034.txt) (for more than 35 years!). It’s used in every website on the internet. And it’s pretty stable – in a lot of ways, it works the exact same way it did 30 years ago.  

以 DNS 为例。我们从 80 年代起就开始使用 DNS（超过 35 年了！）。它被用于互联网上的每个网站。而且它非常稳定——在很多方面，它的工作方式与 30 年前完全相同。

But it took me YEARS to figure out how to confidently debug DNS issues, and I’ve seen a lot of other programmers struggle with debugging DNS problems as well. So what’s going on?  

但我花了很多年才弄清楚如何自信地调试 DNS 问题，而且我看到很多其他程序员也在调试 DNS 问题上遇到困难。发生什么了？

Here are a couple of thoughts about why learning to troubleshoot DNS problems is hard.  

以下是关于为什么学习解决 DNS 问题很困难的一些想法。

(I’m not going to explain DNS very much in this post, see [Implement DNS in a Weekend](https://implement-dns.wizardzines.com/) or [my DNS blog posts](https://jvns.ca/categories/dns/) for more about how DNS works)  

（我不会在这篇文章中详细解释 DNS，请参阅在周末实施 DNS 或我的 DNS 博客文章，了解有关 DNS 工作原理的更多信息）

### it’s not because DNS is super hard  

并不是因为 DNS 超级难

When I finally learned how to troubleshoot DNS problems, my reaction was “what, that was it???? that’s not that hard!“. I felt a little bit cheated! I could explain to you everything that I found confusing about DNS in [a few hours](https://wizardzines.com/zines/dns).  

当我最终学会如何解决 DNS 问题时，我的反应是“什么，就是这样？？？这并不难！”。我感觉有点被骗了！我可以在几个小时内向您解释有关 DNS 的所有令人困惑的内容。

So – if DNS is not all that complicated, why did it take me so many years to figure out how to troubleshoot pretty basic DNS issues (like “my domain doesn’t resolve even though I’ve set it up correctly” or “`dig` and my browser have different DNS results, why?“)?  

因此，如果 DNS 不是那么复杂，为什么我花了这么多年才弄清楚如何解决非常基本的 DNS 问题（例如“即使我已正确设置，我的域名也无法解析”或“ < b0> 和我的浏览器有不同的 DNS 结果，为什么？“）？

And I wasn’t alone in finding DNS hard to learn! I’ve talked to a lot of smart friends who are very experienced programmers about DNS of the years, and many of them either:  

而且我并不是唯一一个发现 DNS 难学的人！我和很多聪明的朋友聊过这些年的 DNS，他们都是非常有经验的程序员，其中很多人都是：

-   didn’t feel comfortable making simple DNS changes to their websites  
    
    对网站进行简单的 DNS 更改感到不舒服
-   or were confused about basic facts about how DNS works (like that records are [pulled and not pushed](https://jvns.ca/blog/2021/12/06/dns-doesn-t-propagate/))  
    
    或者对 DNS 工作原理的基本事实感到困惑（例如记录是拉取而不是推送）
-   or did understand DNS basics pretty well, but had the some of the same knowledge gaps that I’d struggled with (negative caching and the details of how `dig` and your browser do DNS queries differently)  
    
    或者确实非常了解 DNS 基础知识，但也有一些我一直在努力解决的相同知识差距（负缓存以及 `dig` 和您的浏览器如何以不同方式执行 DNS 查询的详细信息）

So if we’re all struggling with the same things about DNS, what’s going on? Why is it so hard to learn for so many people?  

那么，如果我们都在 DNS 方面遇到同样的问题，那是怎么回事呢？为什么很多人学习这么难？

Here are some ideas.  

这里有一些想法。

### a lot of the system is hidden  

很多系统都是隐藏的

When you make a DNS request on your computer, the basic story is:  

当您在计算机上发出 DNS 请求时，基本情况是：

1.  your computer makes a request to a server called **resolver**  
    
    您的计算机向名为解析器的服务器发出请求
2.  the resolver checks its cache, and makes requests to some other servers called **authoritative nameservers**  
    
    解析器检查其缓存，并向称为权威名称服务器的其他一些服务器发出请求

Here are some things you don’t see:  

以下是一些你看不到的东西：

-   the resolver’s **cache**. What’s in there?  
    
    解析器的缓存。里面有什么？
-   which **library code** on your computer is making the DNS request (is it libc `getaddrinfo`? if so, is it the getaddrinfo from glibc, or musl, or apple? is it your browser’s DNS code? is it a different custom DNS implementation?). All of these options behave slightly differently and have different configuration, approaches to caching, available features, etc. For example musl DNS didn’t support TCP until [early 2023](https://www.theregister.com/2023/05/16/alpine_linux_318/).  
    
    您计算机上的哪个库代码正在发出 DNS 请求（是 libc `getaddrinfo` 吗？如果是，是来自 glibc、musl 或 apple 的 getaddrinfo？是您浏览器的 DNS 代码吗？是否有不同自定义 DNS 实施？）。所有这些选项的行为都略有不同，并且具有不同的配置、缓存方法、可用功能等。例如，musl DNS 直到 2023 年初才支持 TCP。
-   the **conversation** between the resolver and the authoritative nameservers. I think a lot of DNS issues would be SO simple to understand if you could magically get a trace of exactly which authoritative nameservers were queried downstream during your request, and what they said. (like, what if you could run `dig +debug google.com` and it gave you a bunch of extra debugging information?)  
    
    解析器和权威名称服务器之间的对话。我认为，如果您能够神奇地准确地跟踪在您的请求期间下游查询了哪些权威域名服务器以及它们所说的内容，那么很多 DNS 问题就会很容易理解。 （例如，如果您可以运行 `dig +debug google.com` 并且它为您提供了一堆额外的调试信息怎么办？）

### dealing with hidden systems  

处理隐藏系统

A couple of ideas for how to deal with hidden systems  

关于如何处理隐藏系统的一些想法

-   just teaching people what the hidden systems are makes a huge difference. For a long time I had no idea that my computer had many different DNS libraries that were used in different situations and I was confused about this for literally years. This is a big part of my approach.  
    
    仅仅教人们什么是隐藏系统就会产生巨大的影响。很长一段时间，我都不知道我的计算机有许多不同的 DNS 库，它们在不同的情况下使用，而且我对此感到困惑了很多年。这是我方法的一个重要部分。
-   with [Mess With DNS](https://messwithdns.net/) we tried out this “fishbowl” approach where it shows you some parts of the system (the conversation with the resolver and the authoritative nameserver) that are normally hidden  
    
    在 Mess With DNS 中，我们尝试了这种“鱼缸”方法，它向您显示系统中通常隐藏的某些部分（与解析器和权威名称服务器的对话）
-   I feel like it would be extremely cool to extend DNS to include a “debugging information” section. (edit: it looks like this already exists! It’s called [Extended DNS Errors](https://blog.nlnetlabs.nl/extended-dns-error-support-for-unbound/), or EDE, and tools are slowly adding support for it.  
    
    我觉得扩展 DNS 以包含“调试信息”部分会非常酷。 （编辑：看起来这个已经存在了！它被称为扩展 DNS 错误，或 EDE，并且工具正在慢慢添加对它的支持。

### Extended DNS Errors seem cool  

扩展 DNS 错误看起来很酷

Extended DNS Errors are a new way for DNS servers to provide extra debugging information in DNS response. Here’s an example of what that looks like:  

扩展 DNS 错误是 DNS 服务器在 DNS 响应中提供额外调试信息的一种新方法。下面是一个示例：

```
$ dig @8.8.8.8 xjwudh.com
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 39830
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
; EDE: 12 (NSEC Missing): (Invalid denial of existence of xjwudh.com/a)
;; QUESTION SECTION:
;xjwudh.com.INA

;; AUTHORITY SECTION:
com.900INSOAa.gtld-servers.net. nstld.verisign-grs.com. 1690634120 1800 900 604800 86400

;; Query time: 92 msec
;; SERVER: 8.8.8.8#53(8.8.8.8) (UDP)
;; WHEN: Sat Jul 29 08:35:45 EDT 2023
;; MSG SIZE  rcvd: 161
```

Here I’ve requested a nonexistent domain, and I got the extended error `EDE: 12 (NSEC Missing): (Invalid denial of existence of xjwudh.com/a)`. I’m not sure what that means (it’s some DNSSEC Thing), but it’s cool to see an extra debug message like that.  

在这里，我请求了一个不存在的域，并且收到了扩展错误 `EDE: 12 (NSEC Missing): (Invalid denial of existence of xjwudh.com/a)` 。我不确定这意味着什么（这是 DNSSEC 的一些东西），但看到这样的额外调试消息很酷。

I did have to install a newer version of `dig` to get the above to work.  

我确实必须安装更新版本的 `dig` 才能使上述功能正常工作。

### confusing tools 令人困惑的工具

Even though a lot of DNS stuff is hidden, there are a lot of ways to figure out what’s going on by using `dig`.  

尽管许多 DNS 内容是隐藏的，但有很多方法可以使用 `dig` 来了解正在发生的情况。

For example, you can use `dig +norecurse` to figure out if a given DNS resolver has a particular record in its cache. `8.8.8.8` seems to return a `SERVFAIL` response if the response isn’t cached.  

例如，您可以使用 `dig +norecurse` 来确定给定的 DNS 解析器在其缓存中是否有特定记录。如果未缓存响应， `8.8.8.8` 似乎会返回 `SERVFAIL` 响应。

here’s what that looks like for `google.com`  

这是 `google.com` 的样子

```
$ dig +norecurse  @8.8.8.8 google.com
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11653
;; flags: qr ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;google.com.INA

;; ANSWER SECTION:
google.com.21INA172.217.4.206

;; Query time: 57 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Jul 28 10:50:45 EDT 2023
;; MSG SIZE  rcvd: 55
```

and for `homestarrunner.com`: 对于 `homestarrunner.com` ：

```
$ dig +norecurse  @8.8.8.8 homestarrunner.com
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 55777
;; flags: qr ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;homestarrunner.com.INA

;; Query time: 52 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Jul 28 10:51:01 EDT 2023
;; MSG SIZE  rcvd: 47
```

Here you can see we got a normal `NOERROR` response for `google.com` (which is in `8.8.8.8`’s cache) but a `SERVFAIL` for `homestarrunner.com` (which isn’t). This doesn’t mean there’s no DNS record `homestarrunner.com` (there is!), it’s just not cached).  

在这里您可以看到我们得到了 `google.com` 的正常 `NOERROR` 响应（位于 `8.8.8.8` 的缓存中），但 `SERVFAIL` 得到了 < b4>（事实并非如此）。这并不意味着没有 DNS 记录 `homestarrunner.com` （有！），它只是没有缓存）。

But this output is really confusing to read if you’re not used to it! Here are a few things that I think are weird about it:  

但如果你不习惯的话，这个输出读起来真的很混乱！我认为有以下几点奇怪的地方：

1.  the headings are weird (there’s `->>HEADER<<-`, `flags:`, `OPT PSEUDOSECTION:`, `QUESTION SECTION:`, `ANSWER SECTION:`)  
    
    标题很奇怪（有 `->>HEADER<<-` 、 `flags:` 、 `OPT PSEUDOSECTION:` 、 `QUESTION SECTION:` 、 `ANSWER SECTION:` ）
2.  the spacing is weird (why is the no newline between `OPT PSEUDOSECTION` and `QUESTION SECTION`?)  
    
    间距很奇怪（为什么 `OPT PSEUDOSECTION` 和 `QUESTION SECTION` 之间没有换行符？）
3.  `MSG SIZE rcvd: 47` is weird (are there other fields in `MSG SIZE` other than `rcvd`? what are they?)  
    
    `MSG SIZE rcvd: 47` 很奇怪（ `MSG SIZE` 中除了 `rcvd` 之外还有其他字段吗？它们是什么？）
4.  it says that there’s 1 record in the ADDITIONAL section but doesn’t show it, you have to somehow magically know that the “OPT PSEUDOSECTION” record is actually in the additional section  
    
    它说 ADDITIONAL 部分中有 1 条记录，但没有显示它，您必须以某种方式神奇地知道“OPT PSEUDOSECTION”记录实际上在附加部分中

In general `dig`’s output has the feeling of a script someone wrote in an adhoc way that grew organically over time and not something that was intentionally designed.  

一般来说， `dig` 的输出给人一种有人以临时方式编写的脚本的感觉，随着时间的推移有机地增长，而不是有意设计的东西。

### dealing with confusing tools  

处理令人困惑的工具

some ideas for improving on confusing tools:  

改进令人困惑的工具的一些想法：

-   **explain the output**. For example I wrote [how to use dig](https://jvns.ca/blog/2021/12/04/how-to-use-dig/) explaining how `dig`’s output works and how to configure it to give you a shorter output by default  
    
    解释输出。例如，我写了如何使用 dig 解释 `dig` 的输出如何工作以及如何配置它以默认为您提供更短的输出
-   **make new, more friendly tools**. For example for DNS there’s [dog](https://github.com/ogham/dog) and [doggo](https://github.com/mr-karan/doggo) and [my dns lookup tool](https://dns-lookup.jvns.ca/). I think these are really cool but personally I don’t use them because sometimes I want to do something a little more advanced (like using `+norecurse`) and as far as I can tell neither `dog` nor `doggo` support `+norecurse`. I’d rather use 1 tool for everything, so I stick to `dig`. Replacing the breadth of functionality of `dig` is a huge undertaking.  
    
    制作新的、更友好的工具。例如，对于 DNS，有dog 和doggo 以及我的dns 查找工具。我认为这些真的很酷，但我个人不使用它们，因为有时我想做一些更高级的事情（比如使用 `+norecurse` ），据我所知 `dog` 也不 `doggo` 支持 `+norecurse` 。我宁愿使用 1 个工具来完成所有事情，所以我坚持使用 `dig` 。替换 `dig` 的广泛功能是一项艰巨的任务。
-   **make dig’s output a little more friendly**. If I were better at C programming, I might try to write a `dig` pull request that adds a `+human` flag to dig that formats the long form output in a more structured and readable way, maybe something like this:  
    
    使 dig 的输出更加友好。如果我更擅长 C 编程，我可能会尝试编写一个 `dig` 拉取请求，添加一个 `+human` 标志来挖掘，以更结构化和可读的方式格式化长格式输出，也许是这样的：

```
$ dig +human +norecurse  @8.8.8.8 google.com 
HEADER:
  opcode: QUERY
  status: NOERROR
  id: 11653
  flags: qr ra
  records: QUESTION: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

QUESTION SECTION:
  google.com.INA

ANSWER SECTION:
  google.com.21INA172.217.4.206
  
ADDITIONAL SECTION:
  EDNS: version: 0, flags:; udp: 512

EXTRA INFO:
  Time: Fri Jul 28 10:51:01 EDT 2023
  Elapsed: 52 msec
  Server: 8.8.8.8:53
  Protocol: UDP
  Response size: 47 bytes
```

This makes the structure of the DNS response more clear – there’s the header, the question, the answer, and the additional section.  

这使得 DNS 响应的结构更加清晰 – 有标头、问题、答案和附加部分。

And it’s not “dumbed down” or anything! It’s the exact same information, just formatted in a more structured way. My biggest frustration with alternative DNS tools that they often remove information in the name of clarity. And though there’s definitely a place for those tools, I want to see all the information! I just want it to be presented clearly.  

而且这并不是“愚蠢”之类的！这是完全相同的信息，只是格式更加结构化。我对替代 DNS 工具最大的不满是它们经常以清晰的名义删除信息。尽管这些工具肯定有一席之地，但我想查看所有信息！我只是想让它清晰地呈现出来。

We’ve learned a lot about how to design more user friendly command line tools in the last 40 years and I think it would be cool to apply some of that knowledge to some of our older crustier tools.  

在过去的 40 年里，我们学到了很多关于如何设计更加用户友好的命令行工具的知识，我认为将其中一些知识应用到我们一些较旧的硬性工具中会很酷。

### dig +yaml 挖掘+yaml

One quick note on dig: newer versions of dig do have a `+yaml` output format which feels a little clearer to me, though it’s too verbose for my taste (a pretty simple DNS response doesn’t fit on my screen)  

关于 dig 的一个快速注释：新版本的 dig 确实有一个 `+yaml` 输出格式，这对我来说感觉更清晰一些，尽管它对我来说太冗长了（一个非常简单的 DNS 响应不适合我的屏幕） ）

### weird gotchas 奇怪的陷阱

DNS has some weird stuff that’s relatively common to run into, but pretty hard to learn about if nobody tells you what’s going on. A few examples (there are more in [some ways DNS can break](https://jvns.ca/blog/2022/01/15/some-ways-dns-can-break/):  

DNS 有一些比较常见的奇怪的东西，但如果没有人告诉你发生了什么，就很难了解。几个例子（DNS 在某些方面可能会破坏更多的例子：

-   negative caching! (which I talk about in [this talk](https://jvns.ca/blog/2023/05/08/new-talk-learning-dns-in-10-years/)) It took me probably 5 years to realize that I shouldn’t visit a domain that doesn’t have a DNS record yet, because then the **nonexistence** of that record will be cached, and it gets cached for HOURS, and it’s really annoying.  
    
    负缓存！ （我在本次演讲中谈到）我大概花了 5 年时间才意识到我不应该访问还没有 DNS 记录的域，因为这样就不存在的记录将被缓存，并且它会被缓存几个小时，这真的很烦人。
-   differences in `getaddrinfo` implementations: until [early 2023](https://www.theregister.com/2023/05/16/alpine_linux_318/), `musl` didn’t support TCP DNS  
    
    `getaddrinfo` 实现的差异：直到 2023 年初， `musl` 不支持 TCP DNS
-   resolvers that ignore TTLs: if you set a TTL on your DNS records (like “5 minutes”), some resolvers will ignore those TTLs completely and cache the records for longer, like maybe 24 hours instead  
    
    忽略 TTL 的解析器：如果您在 DNS 记录上设置 TTL（例如“5 分钟”），某些解析器将完全忽略这些 TTL 并将记录缓存更长时间，例如可能 24 小时
-   if you configure nginx wrong ([like this](https://jvns.ca/blog/2022/01/15/some-ways-dns-can-break/#problem-nginx-caching-dns-records-forever)), it’ll cache DNS records forever.  
    
    如果 nginx 配置错误（像这样），它将永远缓存 DNS 记录。
-   how [ndots](https://pracucci.com/kubernetes-dns-resolution-ndots-options-and-why-it-may-affect-application-performances.html) can make your Kubernetes DNS slow  
    
    ndots 如何使您的 Kubernetes DNS 变慢

### dealing with weird gotchas  

处理奇怪的问题

I don’t have as good answers here as I would like to, but knowledge about weird gotchas is extremely hard won (again, it took me years to figure out negative caching!) and it feels very silly to me that people have to rediscover them for themselves over and over and over again.  

我在这里没有像我想要的那样好的答案，但是关于奇怪的陷阱的知识是非常来之不易的（同样，我花了很多年才弄清楚负缓存！）而且对我来说，人们必须重新发现这些知识是非常愚蠢的他们自己一遍又一遍地。

A few ideas: 一些想法：

-   It’s incredibly helpful when people call out gotchas when explaining a topic. For example (leaving DNS for a moment), Josh Comeau’s Flexbox intro explains this [minimum size gotcha](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/#the-minimum-size-gotcha-11) which I ran into SO MANY times for several years before finally finding an explanation of what was going on.  
    
    当人们在解释某个主题时指出陷阱时，这会非常有帮助。例如（暂时离开 DNS），Josh Comeau 的 Flexbox 介绍解释了这个最小尺寸问题，我在几年中多次遇到这个问题，最后找到了对发生的事情的解释。
-   I’d love to see more community collections of common gotchas. For bash, [shellcheck](https://www.shellcheck.net/) is an incredible collection of bash gotchas.  
    
    我希望看到更多常见问题的社区收藏。对于 bash，shellcheck 是一个令人难以置信的 bash 陷阱集合。

One tricky thing about documenting DNS gotchas is that different people are going to run into different gotchas – if you’re just configuring DNS for your personal domain once every 3 years, you’re probably going to run into different gotchas than someone who administrates DNS for a domain with heavy traffic.  

记录 DNS 陷阱的一个棘手问题是，不同的人会遇到不同的陷阱 - 如果您只是每 3 年为您的个人域配置一次 DNS，那么您可能会遇到与管理 DNS 的人不同的陷阱对于流量大的域。

A couple of more quick reasons:  

几个更简单的原因：

### infrequent exposure 不常接触

A lot of people only deal with DNS extremely infrequently. And of course if you only touch DNS every 3 years it’s going to be harder to learn!  

很多人很少接触 DNS。当然，如果您每 3 年才接触一次 DNS，那么学习起来就会更困难！

I think cheat sheets (like “here are the steps to changing your nameservers”) can really help with this.  

我认为备忘单（例如“以下是更改名称服务器的步骤”）确实可以对此有所帮助。

### it’s hard to experiment with  

很难尝试

DNS can be scary to experiment with – you don’t want to mess up your domain. We built [Mess With DNS](https://messwithdns.net/) to make this one a little easier.  

DNS 尝试起来可能会很可怕——您不想弄乱您的域名。我们构建了 Mess With DNS 来使这一过程变得更容易。

### that’s all for now  

目前为止就这样了

I’d love to hear other thoughts about what makes DNS (or your favourite mysterious technology) hard to learn.  

我很想听听有关 DNS（或您最喜欢的神秘技术）为何难以学习的其他想法。
