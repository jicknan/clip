---
title: 'New talk: Learning DNS in 10 years'
date: 2023-12-14T08:44:45.000Z
updated: 2023-12-14T08:44:45.000Z
published: 2023-05-08T08:44:45.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://jvns.ca/blog/2023/05/08/new-talk-learning-dns-in-10-years/
  hostname: jvns.ca
  author: Julia Evans
  original_title: 'New talk: Learning DNS in 10 years'
  original_lang: en

---

Here’s a keynote I gave at [RubyConf Mini](https://www.rubyconfmini.com/) last year: Learning DNS in 10 years. It’s about strategies I use to learn hard things. I just noticed that they’d released the video the other day, so I’m just posting it now even though I gave the talk 6 months ago.  

这是我去年在 RubyConf Mini 上发表的主题演讲：10 年学习 DNS。这是关于我用来学习困难事物的策略。我刚刚注意到他们前几天发布了该视频，所以我现在才发布它，尽管我在 6 个月前发表了演讲。

Here’s the video, as well as the slides and a transcript of (roughly) what I said in the talk.  

这是视频、幻灯片和我在演讲中所说的（大致）文字记录。

### the video 视频

<iframe width="560" height="315" src="https://www.youtube.com/embed/tsxjNsFu_2g" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen=""></iframe>

### the transcript 成绩单

[![](slide-01.png)](https://jvns.ca/images/2022-railsconf/slide-01.png)

[![](slide-02.png)](https://jvns.ca/images/2022-railsconf/slide-02.png)

You all got this zine ([How DNS Works](https://wizardzines.com/zines/dns/)) in your swag bags -- thanks to RubyConf for printing it!  

你们的礼物袋里都藏着这本杂志（DNS 的工作原理）——感谢 RubyConf 打印它！

[![](slide-03.png)](https://jvns.ca/images/2022-railsconf/slide-03.png)

But this talk is not really about DNS. I mean, this is a Ruby conference, right? So this talk is really about learning hard things, and DNS is an example of something that was hard for me to learn.  

但本次演讲实际上并不是关于 DNS。我的意思是，这是 Ruby 会议，对吗？所以这次演讲实际上是关于学习困难的事情，DNS 就是一个对我来说很难学习的事情的例子。

[![](slide-04.png)](https://jvns.ca/images/2022-railsconf/slide-04.png)

It took me maybe 16 years from the first time that like I bought a domain name and set up my DNS records to when I really felt like I understood how the system worked.  

从我第一次购买域名并设置 DNS 记录到我真正觉得我了解系统是如何工作的，我花了大约 16 年的时间。

[![](slide-05.png)](https://jvns.ca/images/2022-railsconf/slide-05.png)

And one thing I want to say at the beginning of this talk, is that I think that taking like 16 years to learn something like DNS is kind of normal. The idea that "I should understand this already" is a bit silly. For me, I was doing other stuff for most of the 16 years! There was other stuff I wanted to learn.  

在本次演讲开始时我想说的一件事是，我认为花 16 年的时间来学习 DNS 这样的东西是很正常的。 “我应该已经明白这一点了”的想法有点愚蠢。对我来说，这 16 年的大部分时间我都在做其他事情！我还有其他东西想学。

[![](slide-06.png)](https://jvns.ca/images/2022-railsconf/slide-06.png)

And so, this talk is not about how you should learn about any particular thing. I don't care if you learn how DNS works! It's really about how to approach learning something hard that's a priority for you to learn.  

因此，本次演讲并不是关于您应该如何学习任何特定的事情。我不在乎你是否了解 DNS 的工作原理！这实际上是关于如何学习一些困难的东西，这是你要优先学习的。

[![](slide-07.png)](https://jvns.ca/images/2022-railsconf/slide-07.png)

So, we're going to talk about learning through a series of tiny deep dives. My favorite way of learning things is to do nothing, most of the time.  

因此，我们将讨论通过一系列微小的深入研究来学习。大多数时候，我最喜欢的学习方式就是什么也不做。

That's why it takes 10 years.  

这就是为什么需要10年的时间。

So for six months I'll do nothing and then like I'll furiously learn something for maybe 30 minutes or three hours or an afternoon. And then I'll declare success and go back to doing nothing for months. I find this works really well for me.  

所以六个月里我什么都不做，然后我会花 30 分钟、三个小时或一个下午的时间疯狂地学习一些东西。然后我会宣布成功，然后几个月什么都不做。我发现这对我来说非常有效。

[![](slide-08.png)](https://jvns.ca/images/2022-railsconf/slide-08.png)

Here are some of the strategies we're going to talk about for doing these tiny deep dives  

以下是我们将要讨论的一些进行这些微小深入研究的策略

First, we're going to start briefly by talking about what DNS is.  

首先，我们将首先简要讨论一下什么是 DNS。

Next, we're going to talk about spying on DNS.  

接下来，我们将讨论 DNS 监视。

Then we're gonna talk about being confused, which is my main mode. (I'm always confused about something!)  

然后我们要谈谈困惑，这是我的主要模式。 （我总是对某些事情感到困惑！）

Then we'll talk about reading the specification, we'll going to do some experiments, and we're going to implement our own terrible version of DNS.  

然后我们将讨论阅读规范，我们将做一些实验，我们将实现我们自己的糟糕版本的 DNS。

[![](slide-09.png)](https://jvns.ca/images/2022-railsconf/slide-09.png)

And so what's DNS really briefly? DNS stands for the Domain Name System. And every time you go to a website like `www.example.com`, your browser needs to look up that website's IP address. So DNS translates domain names into IP addresses. It looks up other information about domain names too, but we're mostly just going to talk about IP addresses today.  

那么 DNS 到底是什么？ DNS 代表域名系统。每次您访问 `www.example.com` 这样的网站时，您的浏览器都需要查找该网站的 IP 地址。所以DNS将域名翻译成IP地址。它还会查找有关域名的其他信息，但我们今天主要讨论 IP 地址。

[![](slide-10.png)](https://jvns.ca/images/2022-railsconf/slide-10.png)

I want to briefly sell why I think DNS is cool, because we're going to be talking about it a lot.  

我想简单介绍一下为什么我认为 DNS 很酷，因为我们将经常讨论它。

[![](slide-11.png)](https://jvns.ca/images/2022-railsconf/slide-11.png)

One cool thing about DNS is that it's this invisible system that controls the entire internet.  

DNS 的一个很酷的事情是，它是一个控制整个互联网的无形系统。

[![](slide-12.png)](https://jvns.ca/images/2022-railsconf/slide-12.png)

For example, you're on your phone, you're using Google Maps, it needs to know, where is maps.google.com, right? Or on your computer, where's reddit.com? What's the IP address? And if we didn't have DNS, the entire internet would collapse.  

例如，你在手机上，你在使用谷歌地图，它需要知道maps.google.com在哪里，对吗？或者在您的计算机上，reddit.com 在哪里？ IP 地址是多少？如果我们没有 DNS，整个互联网就会崩溃。

I think it's fun to learn how this behind the scenes stuff works.  

我认为了解幕后的工作原理很有趣。

[![](slide-13.png)](https://jvns.ca/images/2022-railsconf/slide-13.png)

The other thing about DNS I find interesting is that it's really old. There's this document ([RFC 1035](https://datatracker.ietf.org/doc/html/rfc1035)) which defines how DNS works, that was written in 1987. And if you take that document and you write a program that works the way that documents says to work, your program will work. And I think that's kind of wild, right?  

关于 DNS 我发现有趣的另一件事是它确实很古老。有一个文档 (RFC 1035) 定义了 DNS 的工作原理，该文档于 1987 年编写。如果您使用该文档并编写一个按照文档所述工作方式工作的程序，那么您的程序就会工作。我认为这有点疯狂，对吧？

The basics haven't changed since before I was born. So if you're a little slow about learning about it, that's ok: it's not going to change out from under you.  

从我出生之前起，基本的情况就没有改变。因此，如果你对它的了解有点慢，那也没关系：它不会在你的领导下发生改变。

[![](slide-14.png)](https://jvns.ca/images/2022-railsconf/slide-14.png)

Next I want to talk about spying on DNS, which is one of my favorite ways to learn about things.  

接下来我想谈谈对 DNS 的监视，这是我最喜欢的了解事物的方式之一。

[![](slide-15.png)](https://jvns.ca/images/2022-railsconf/slide-15.png)

I'm going to talk about two spy tools for DNS: dig and wireshark.  

我将讨论两种 DNS 间谍工具：dig 和wireshark。

[![](slide-16.png)](https://jvns.ca/images/2022-railsconf/slide-16.png)

dig is a tool for making DNS queries. We talked about you know, how your browser needs to look up the IP address for `maps.google.com`. We can do that in dig!  

dig 是一个用于进行 DNS 查询的工具。我们讨论过您的浏览器需要如何查找 `maps.google.com` 的 IP 地址。我们可以在挖掘中做到这一点！

[![](demo-dig-small.png)](https://jvns.ca/images/2022-railsconf/demo-dig.png)

When we run `dig maps.google.com`, it prints out 5 fields. Let's talk about what those 5 fields are.  

当我们运行 `dig maps.google.com` 时，它会打印出 5 个字段。我们来谈谈这5个字段是什么。

[![](slide-17.png)](https://jvns.ca/images/2022-railsconf/slide-17.png)

I've used example.com instead of maps.google.com on this slide, but the fields are the same. Let's talk about 4 of them:  

我在这张幻灯片上使用了 example.com 而不是maps.google.com，但字段是相同的。我们来谈谈其中的 4 个：

We have the domain name, no big deal  

我们有域名，没什么大不了的

The Time To Live, which is how long to cache that record for so this is a one day  

生存时间，即缓存该记录的时间，因此这是一天

You have the record type, A stands for address because this is an IP address  

您有记录类型，A 代表地址，因为这是一个 IP 地址

And you have the content, which is the IP address  

你就有了内容，即 IP 地址

[![](slide-18.png)](https://jvns.ca/images/2022-railsconf/slide-18.png)

But I think that the funniest field in a DNS record is this field in the middle, IN, which stands for INternet. I guess in 1987, they thought that we might be on a lot of different networks. So they made an option for it. In reality, we're all on the internet. And every DNS query has class set to "internet". There are a couple of others query classes (CHAOS and HESIOD), which truly almost nobody uses.  

但我认为 DNS 记录中最有趣的字段是中间的 IN 字段，它代表 Internet。我猜想在 1987 年，他们认为我们可能位于很多不同的网络上。所以他们为此做出了选择。事实上，我们都在互联网上。每个 DNS 查询的类别都设置为“internet”。还有其他几个查询类（CHAOS 和 HESIOD），实际上几乎没有人使用它们。

[![](demo-dig2-small.png)](https://jvns.ca/images/2022-railsconf/demo-dig2.png)

We can also kind of poke around on the internet with Dig. We've talked about A records to look up IP addresses.  

我们还可以使用 Dig 在互联网上闲逛。我们已经讨论过用于查找 IP 地址的 A 记录。

[![](demo-dig3-small.png)](https://jvns.ca/images/2022-railsconf/demo-dig3.png)

But there are other kinds of records like TXT records. So we're going to look at a TXT record really quickly just because I think this is very fun. We're going to look at twitter.com's TXT records.  

但还有其他类型的记录，例如 TXT 记录。因此，我们将非常快速地查看 TXT 记录，因为我认为这非常有趣。我们将查看 twitter.com 的 TXT 记录。

So TXT records are something that people use for domain verification, for example to prove to Google that you own twitter.com.  

因此，TXT 记录可用于域验证，例如向 Google 证明您拥有 twitter.com。

So what you can do is you can set this DNS record `google-site-verification`. Google will tell you what to set it to, you'll set it, and then Google will believe you.  

所以你可以做的是设置这个 DNS 记录 `google-site-verification` 。谷歌会告诉你要设置什么，你会设置它，然后谷歌就会相信你。

I think it's kind of fun that you can like kind of poke around with DNS and see that Twitter is using Miro or Canva or Mixpanel, that's all public. It's like a little peek into what people are doing inside their companies  

我认为这很有趣，你可以喜欢探索 DNS，并看到 Twitter 正在使用 Miro、Canva 或 Mixpanel，这些都是公开的。这就像了解人们在公司内部所做的事情一样

[![](demo-dig4-small.png)](https://jvns.ca/images/2022-railsconf/demo-dig4.png)

Oh, the other thing about dig is that by default, dig's output looks like this, which is very ugly and unreadable. There's a lot of nonsense here.  

哦，关于 dig 的另一件事是，默认情况下，dig 的输出看起来像这样，非常难看且不可读。这里有很多废话。

[![](slide-19.png)](https://jvns.ca/images/2022-railsconf/slide-19.png)

So dig has a configuration file, where you can put `+noall +answer` and then your dig responses look much nicer (like they did in the screenshots above) instead of having a lot of nonsense in them. Whenever possible, I try to make my tools behave in a more human way.  

所以 dig 有一个配置文件，你可以在其中放置 `+noall +answer` 然后你的 dig 响应看起来会更好（就像上面的屏幕截图中那样），而不是有很多废话。只要有可能，我都会尝试让我的工具以更人性化的方式运行。

[![](slide-20.png)](https://jvns.ca/images/2022-railsconf/slide-20.png)

The other thing I want to talk about is Wireshark, which is my favorite computer networking tool in the universe for spying on all things computer networks. In this case, DNS queries. So let's go look at Wireshark.  

我想谈的另一件事是 Wireshark，它是我最喜欢的计算机网络工具，用于监视计算机网络的所有事物。在这种情况下，DNS 查询。那么让我们来看看 Wireshark。

[![](slide-21.png)](https://jvns.ca/images/2022-railsconf/slide-21.png)

[![](demo-wireshark1-small.png)](https://jvns.ca/images/2022-railsconf/demo-wireshark1.png)

When we make a DNS query like this and look up example.com, Wireshark can capture it.  

当我们进行这样的 DNS 查询并查找 example.com 时，Wireshark 可以捕获它。

[![](demo-wireshark2-small.png)](https://jvns.ca/images/2022-railsconf/demo-wireshark2.png)

When you start looking in the guts of things, I think it can be a bit scary at first. Like what do all these numbers? It kind of seems like a lot. So when I'm looking at something new, I try to start by looking at stuff that I understand.  

当你开始深入了解事物的本质时，我认为一开始可能会有点可怕。这些数字代表什么意思？看起来好像很多。因此，当我研究新事物时，我会尝试从我理解的东西开始。

[![](demo-wireshark3-small.png)](https://jvns.ca/images/2022-railsconf/demo-wireshark3.png)

For example, I know that example.com is a domain name, right? So we should able to use Wireshark to go find that domain name in the DNS query. If we click into the "query" part of the DNS packet, we can see 3 fields that we recognize. First, the domain name.  

例如，我知道 example.com 是一个域名，对吗？所以我们应该能够使用 Wireshark 在 DNS 查询中找到该域名。如果我们点击 DNS 数据包的“查询”部分，我们可以看到 3 个我们识别的字段。首先，域名。

[![](demo-wireshark4-small.png)](https://jvns.ca/images/2022-railsconf/demo-wireshark4.png)

We can also see the type ("A")  

我们还可以看到类型（“A”）

[![](demo-wireshark5-small.png)](https://jvns.ca/images/2022-railsconf/demo-wireshark5.png)

And the third one is the class which is INternet, which is always the same. What I find comforting here is that in the query, there are really only 2 important fields: a DNS query is just saying "I want the IP address for example.com". There's just two fields. And that that always makes me feel a little bit better about understanding something.  

第三个类别是INTERnet，它总是相同的。让我感到欣慰的是，在查询中，实际上只有 2 个重要字段：DNS 查询只是说“我想要 example.com 的 IP 地址”。只有两个字段。这总是让我在理解某些事情时感觉更好一些。

[![](slide-22.png)](https://jvns.ca/images/2022-railsconf/slide-22.png)

A quick caveat: your browser might be using encrypted DNS and spying on your DNS queries with Wireshark will not work if your DNS is encrypted. But there's lots of non-encrypted DNS to spy on.  

快速警告：您的浏览器可能正在使用加密的 DNS，并且如果您的 DNS 已加密，则使用 Wireshark 监视您的 DNS 查询将不起作用。但有很多未加密的 DNS 可供监视。

[![](slide-23.png)](https://jvns.ca/images/2022-railsconf/slide-23.png)

The second thing I want to talk about for learning new things is to notice when you're confused about something.  

关于学习新事物，我想谈的第二件事是当你对某些事情感到困惑时要注意。

[![](slide-24.png)](https://jvns.ca/images/2022-railsconf/slide-24.png)

I want to tell you a story, "the case of the mysterious caching", of something that happened to me with DNS that really confused me.  

我想告诉你一个故事，“神秘缓存的案例”，发生在我身上的 DNS 事件让我非常困惑。

[![](slide-25.png)](https://jvns.ca/images/2022-railsconf/slide-25.png)

First, I want to talk to you a little bit about how DNS works a little bit more. So on the left here, you have your browser. And when your browser makes a DNS query, it asks a server called a resolver. And all you need to know about the resolver is that it's cache, which as we know is like the worst thing in computer science. So the resolver is a cache, and it gets its information from the source of truth, which has the real answers.  

首先，我想和您谈谈 DNS 的工作原理。所以在左边，你有你的浏览器。当您的浏览器进行 DNS 查询时，它会询问称为解析器的服务器。关于解析器，您需要了解的只是它的缓存，据我们所知，这就像计算机科学中最糟糕的事情。因此解析器是一个缓存，它从具有真实答案的事实来源获取信息。

[![](slide-26.png)](https://jvns.ca/images/2022-railsconf/slide-26.png)

So your browser talks to a resolver, which is a cache.  

因此，您的浏览器与解析器（即缓存）进行对话。

[![](slide-27.png)](https://jvns.ca/images/2022-railsconf/slide-27.png)

At the time of this story, I had this mental model for like how I thought about DNS, which is that if I set a TTL (the cache time) of 5 minutes when configuring my DNS records, then I would never have to wait more than five minutes. Something you need to know about me is that I'm a very impatient person. And I hate waiting. So this model was mostly working for me at the time, though there are a few other very important caveats that we're not going to get into.  

在写这个故事时，我有这样的心理模型，就像我对 DNS 的看法一样，如果我在配置 DNS 记录时将 TTL（缓存时间）设置为 5 分钟，那么我将永远不必等待更多不到五分钟。关于我，你需要了解的是，我是一个非常没有耐心的人。我讨厌等待。所以这个模型当时主要对我有用，尽管还有一些其他非常重要的警告我们不会讨论。

[![](slide-28.png)](https://jvns.ca/images/2022-railsconf/slide-28.png)

But one day I was setting up a new subdomain for some new project. Let's say it was new.jvns.ca. So I set it up. I made its DNS records, and I refreshed the page. And it wasn't working. So I figured, that's fine, my model says, I only have to wait five minutes, right? Because that's what I was used to. But I waited five minutes and still didn't work.  

但有一天我正在为一些新项目设置一个新的子域。假设它是 new.jvns.ca。所以我设置了它。我做了它的 DNS 记录，然后刷新了页面。但它不起作用。所以我想，没关系，我的模型说，我只需要等五分钟，对吗？因为我已经习惯了。但我等了五分钟还是没有成功。

[![](slide-29.png)](https://jvns.ca/images/2022-railsconf/slide-29.png)

And I was like, oh, no. My mental model was broken! I did not feel good.  

我当时想，哦，不。我的思维模式被打破了！我感觉不太好。

[![](slide-30.png)](https://jvns.ca/images/2022-railsconf/slide-30.png)

And often when this happens to me, and I think for most of us, if something weird happens with a computer, you let it go, right? You might decide okay, I don't have time to go into a deep investigation here. I'll just wait longer.  

通常，当这种情况发生在我身上时，我认为对于我们大多数人来说，如果计算机发生了奇怪的事情，你就会放弃它，对吗？你可能会决定，好吧，我没有时间在这里进行深入调查。我就再等一会儿吧。

[![](slide-31.png)](https://jvns.ca/images/2022-railsconf/slide-31.png)

But sometimes I have a lot of energy, and maybe I'm feeling mad, like "the computer can't beat me today"! Because there's a reason that this is happening, right? And I want to find out what it is. So this day for some reason. I had a lot of energy.  

但有时我精力充沛，也许我感觉很生气，就像“今天电脑赢不了我”！因为这件事的发生是有原因的，对吧？我想知道它是什么。所以这一天出于某种原因。我精力充沛。

[![](slide-32.png)](https://jvns.ca/images/2022-railsconf/slide-32.png)

So I started Googling furiously. And I found a useful comment on Stack Overflow.  

于是我开始疯狂地谷歌搜索。我在 Stack Overflow 上发现了一条有用的评论。

[![](slide-33.png)](https://jvns.ca/images/2022-railsconf/slide-33.png)

The Stack Overflow comment talked about something called negative caching. What's that?  

Stack Overflow 评论谈到了一种叫做负缓存的东西。那是什么？

[![](slide-34.png)](https://jvns.ca/images/2022-railsconf/slide-34.png)

And so here's what it said might be going on. The first time I opened the website (before the DNS records had been set up), the DNS servers returned a negative answer, saying hey,this domain doesn't exist yet. The code for that is NXDOMAIN, which is like a 404 for DNS.  

这就是它所说的可能发生的情况。我第一次打开网站时（在设置 DNS 记录之前），DNS 服务器返回否定答案，说嘿，该域尚不存在。其代码是 NXDOMAIN，类似于 DNS 的 404。

And the resolver cached that negative NXDOMAIN response. So the fact that it didn't exist was cached.  

解析器缓存了否定的 NXDOMAIN 响应。所以它不存在的事实被缓存了。

[![](slide-35.png)](https://jvns.ca/images/2022-railsconf/slide-35.png)

So my next question was: how long do I have to wait for the cache to expire? This brings us to a another learning technique.  

所以我的下一个问题是：我需要等待缓存过期多长时间？这给我们带来了另一种学习技巧。

[![](slide-36.png)](https://jvns.ca/images/2022-railsconf/slide-36.png)

I think like maybe the most upsetting learning technique to me is to read a very boring technical document. I'm like very impatient. I kind of hate reading boring things. And so when I read something very boring, I like to bring a specific question. So in this case, I had a specific question, which is how long do I have to wait for the cache to expire?  

我认为对我来说最令人沮丧的学习技巧可能是阅读一份非常无聊的技术文档。我好像很不耐烦。我有点讨厌读无聊的东西。所以当我读到一些非常无聊的东西时，我喜欢提出一个具体的问题。那么在这种情况下，我有一个具体的问题，那就是我需要等待缓存过期多长时间？

[![](slide-37.png)](https://jvns.ca/images/2022-railsconf/slide-37.png)

In networking, everything has a specification. The boring technical documents are called RFC is for request for comments. I find this name a bit funny, because for DNS, some of the main RFCs are RFC 1034 and 1035. These were written in 1987, and the comment period ended in 1987. You can definitely no longer make comments. But anyway, that's what they're called.  

在网络中，一切都有规范。枯燥的技术文档称为RFC，用于征求意见。我觉得这个名字有点搞笑，因为对于DNS来说，一些主要的RFC是RFC 1034和1035。这些是1987年写的，评论期是1987年结束的。你绝对不能再发表评论了。但无论如何，他们就是这么称呼的。

I personally kind of love RFCs because they're like the ultimate answer to many questions. There's a great series of HTTP RFCs, 9110 to 9114. DNS actually has a million different RFCs, it's very upsetting, but the answers are often there. So I went looking. And I think I went looking because when I read comments on StackOverflow, I don't always trust them. How do I know if they're accurate? So I wanted to go to an authoritative source.  

我个人比较喜欢 RFC，因为它们就像许多问题的最终答案。有一系列很棒的 HTTP RFC，从 9110 到 9114。DNS 实际上有一百万个不同的 RFC，这非常令人沮丧，但答案通常就在那里。于是我就去找了。我想我之所以去寻找，是因为当我读到 StackOverflow 上的评论时，我并不总是相信他们。我怎么知道它们是否准确？所以我想去找权威人士。

[![](slide-38.png)](https://jvns.ca/images/2022-railsconf/slide-38.png)

So I found this document called RFC 2308. In section 3, it has this very boring sentence, the TTL of this record is set to the minimum of the minimum field of the SOA record and the TTL of the SOA itself. It indicates how long a resolver may cache the negative answer.  

于是我找到了这个叫做RFC 2308的文档。在第3节中，它有这样一个很无聊的句子，这条记录的TTL被设置为SOA记录的最小字段和SOA本身的TTL中的最小值。它指示解析器可以缓存否定答案的时间长度。

[![](slide-39.png)](https://jvns.ca/images/2022-railsconf/slide-39.png)

So, um, ok, cool. What does that mean, right? Luckily, we only have one question: I don't need to read the entire boring document. I just need to like analyze this one sentence and figure it out.  

所以，嗯，好吧，酷。这是什么意思，对吧？幸运的是，我们只有一个问题：我不需要阅读整个无聊的文档。我只需要分析这一句话并找出答案即可。

So it's saying that the cache time depends on two fields. I want to show you the actual data it's talking about, the SOA record.  

所以说缓存时间取决于两个字段。我想向您展示它所讨论的实际数据，即 SOA 记录。

[![](demo-negative-caching-small.png)](https://jvns.ca/images/2022-railsconf/demo-negative-caching.png)

Let's look at what happens when we run `dig +all asdfasdfasdfasdfasdf.jvns.ca` It says that the domain doesn't exist, NXDOMAIN. But it also returns this record called the SOA record, which has some domain metadata. And there are two fields here that are relevant.  

让我们看看当我们运行 `dig +all asdfasdfasdfasdfasdf.jvns.ca` 时会发生什么 它说该域不存在，NXDOMAIN。但它还返回称为 SOA 记录的记录，其中包含一些域元数据。这里有两个相关的字段。

[![](slide-40.png)](https://jvns.ca/images/2022-railsconf/slide-40.png)

Here. I put this on a slide to try to make it a little bit clearer. This slide is a bit messed up, but there's this field at the end that's called the MINIMUM field, and there's the TTL, time to live of the record, that I've tried to circle.  

这里。我把它放在幻灯片上，试图让它更清晰一些。这张幻灯片有点混乱，但最后有一个字段，称为“最小值”字段，还有 TTL（记录的生存时间），我试图将其圈出。

And what it's saying is that if a record doesn't exist, the amount of time the resolver should cache "it doesn't exist" for is the minimum of those two numbers.  

它的意思是，如果记录不存在，解析器应该缓存“它不存在”的时间是这两个数字中的最小值。

[![](slide-41.png)](https://jvns.ca/images/2022-railsconf/slide-41.png)

In this case, both of those numbers are 10800. So that's how long have to wait. We have to wait 10,800 seconds. That's 3 hours.  

在本例中，这两个数字都是 10800。所以这就是需要等待的时间。我们必须等待 10,800 秒。那是3个小时。

[![](slide-42.png)](https://jvns.ca/images/2022-railsconf/slide-42.png)

And so I waited three hours and then everything worked. And I found this kind of fun to know because often like if you look up DNS advice it will say something like, if something has gone wrong, you need to wait 48 hours. And I do not want to wait 48 hours! I hate waiting. So I love it when I can like use my brain to figure out that I can wait for less time.  

所以我等了三个小时，然后一切正常。我发现了解这种情况很有趣，因为通常如果你查找 DNS 建议，它会说类似的话，如果出现问题，你需要等待 48 小时。我不想等48小时！我讨厌等待。所以我喜欢用我的大脑来弄清楚我可以等待更少的时间。

[![](slide-43.png)](https://jvns.ca/images/2022-railsconf/slide-43.png)

Sometimes when I find my mental model is broken, it feels like I don't know anything  

有时候，当我发现自己的思维模式被打破时，感觉自己什么都不知道

But in this case, and I think in a lot of cases, there's often just a few things I'm missing? Like this negative caching thing is like kind of weird, but it really was the one thing I was missing. There are a few more important facts about how DNS caching works that I haven't mentioned, but I haven't run into more problems I didn't understand since then. Though I'm sure there's something I don't know.  

但在这种情况下，我认为在很多情况下，通常只是我遗漏了一些东西？就像这种负面缓存的事情有点奇怪，但这确实是我错过的一件事。还有一些关于 DNS 缓存如何工作的更重要的事实我没有提到，但从那以后我没有遇到更多我不明白的问题。虽然我确信有一些我不知道的事情。

So sometimes learning one small thing really can solve all your problems.  

所以有时候学习一件小事确实可以解决你所有的问题。

[![](slide-44.png)](https://jvns.ca/images/2022-railsconf/slide-44.png)

I want to say briefly that there's a solution to this negative caching problem. We talked about how like if you visit a domain that's nonexistent, it gets cached. The solution is if you haven't set up your domain's DNS, don't visit the domain! Only visit it after you set it up. So I've learned to do that and now I almost never have this problem anymore. It's great.  

我想简单地说一下，这个负面缓存问题是有解决方案的。我们讨论过如果您访问一个不存在的域，它会被缓存。解决方案是，如果您尚未设置域的 DNS，请不要访问该域！仅在设置后才能访问它。所以我学会了这样做，现在我几乎不再遇到这个问题了。这很棒。

[![](slide-45.png)](https://jvns.ca/images/2022-railsconf/slide-45.png)

The next thing I want to talk about is doing experiments.  

接下来我要讲的是做实验。

[![](slide-46.png)](https://jvns.ca/images/2022-railsconf/slide-46.png)

So let's say we want to do some experiments with caching.  

假设我们想要做一些缓存实验。

[![](slide-47.png)](https://jvns.ca/images/2022-railsconf/slide-47.png)

I think most people don't want to make experimental changes to their domain names, because they're worried about breaking something. Which I think is very understandable.  

我认为大多数人不想对他们的域名进行实验性更改，因为他们担心会破坏某些东西。我认为这是非常可以理解的。

[![](slide-48.png)](https://jvns.ca/images/2022-railsconf/slide-48.png)

[![](demo-mess1-small.png)](https://jvns.ca/images/2022-railsconf/demo-mess1.png)

Because I was really into DNS, I wanted to experiment with DNS. And I also wanted other people to experiment with DNS without having to worry about breaking something. So I made this little website with my friend, Marie, called [Mess with DNS](https://messwithdns.net/)  

因为我真的很喜欢 DNS，所以我想尝试一下 DNS。我还希望其他人能够尝试 DNS，而不必担心会破坏某些东西。所以我和我的朋友 Marie 一起制作了这个小网站，名为 Mess with DNS

The idea is, if you don't want to do that DNS experiments on your domain, you can do them on my domain. And if you mess something up, it's my problem, it's not your problem. And there have been no problems, so that's fine.  

这个想法是，如果您不想在您的域上进行 DNS 实验，您可以在我的域上进行。如果你把事情搞砸了，那是我的问题，不是你的问题。而且没有出现任何问题，所以很好。

So let's use Mess With DNS to do a little DNS experimentation  

那么让我们使用 Mess With DNS 来做一些 DNS 实验

[![](demo-mess2-small.png)](https://jvns.ca/images/2022-railsconf/demo-mess2.png)

The way this works is you get a little subdomain. This one is chair131.messwithdns.com. And then you can make DNS records on it and try things out. Here we're making a record for test.char131.messwithdns.net, with type A, the IP 7.7.7.7, and TTL 3000 seconds.  

其工作原理是您获得一个小子域。这是 chair131.messwithdns.com。然后你可以在上面做 DNS 记录并尝试一下。这里我们为 test.char131.messwithdns.net 创建一条记录，类型为 A，IP 7.7.7.7，TTL 3000 秒。

[![](slide-49.png)](https://jvns.ca/images/2022-railsconf/slide-49.png)

What we would expect to see is that if we make a query to the resolver, then it asks kind of like the source of truth, which we control. And we should expect the resolver to make only one query, because it's cached. So I want to do an experiment and see if it's true that we get only 1 query.  

我们期望看到的是，如果我们向解析器发出查询，那么它会询问我们控制的事实来源。我们应该期望解析器只进行一个查询，因为它被缓存了。所以我想做一个实验，看看我们是否只得到 1 个查询。

[![](demo-mess3-small.png)](https://jvns.ca/images/2022-railsconf/demo-mess3.png)

So I'm going to make a few queries for it, with `dig @1.1.1.1 test.chair131.messwithdns.com`. I've queried it a bunch of times, maybe 10 or 20.  

因此，我将使用 `dig @1.1.1.1 test.chair131.messwithdns.com` 对其进行一些查询。我已经查询过很多次了，大概有10次或20次。

[![](demo-mess4-small.png)](https://jvns.ca/images/2022-railsconf/demo-mess4.png)

Oh, cool. This isn't what I expected to see. This is fun, though, that's great. We made about 20 queries for that DNS record. The server logs all queries it receives, so we can count them. Our server got 1, 2, 3, 4, 5, 6, 7, 8 queries. That's kind of fun. 8 is less than 20.  

哦，酷。这不是我期望看到的。不过，这很有趣，这很棒。我们对该 DNS 记录进行了大约 20 次查询。服务器记录它收到的所有查询，以便我们可以对它们进行计数。我们的服务器收到 1、2、3、4、5、6、7、8 个查询。这很有趣。 8 小于 20。

One reason I like to do demos live on stage is that sometimes what I what happens isn't exactly what I think will happen. When I do this exact experiment at home, I just get 1 query to the resolver.  

我喜欢在舞台上进行现场演示的原因之一是，有时我所发生的事情并不完全是我认为会发生的事情。当我在家做这个精确的实验时，我只向解析器发出 1 个查询。

So we only saw like eight queries here. And I assume that this is because the resolver, 1.1.1.1, we're talking to has more than one independent cache, I guess there are 8 caches. This makes sense to me because Cloudflare's network is distributed -- the exact machines I'm talking to here in Providence are not the same as the ones in Montreal.  

所以我们在这里只看到了八个查询。我假设这是因为我们正在讨论的解析器 1.1.1.1 有多个独立的缓存，我猜有 8 个缓存。这对我来说很有意义，因为 Cloudflare 的网络是分布式的——我在普罗维登斯谈论的确切机器与蒙特利尔的机器不一样。

This is interesting because it complicates your idea about how caching works a little bit, right? Like maybe a given DNS resolver actually has like eight caches and which one you get is random, and you're not always talking to the same one. I think that's what's going on here.  

这很有趣，因为它使您对缓存如何工作的想法变得有点复杂，对吧？就像一个给定的 DNS 解析器实际上有大约八个缓存，而您获得的缓存是随机的，并且您并不总是与同一个缓存进行通信。我想这就是这里发生的事情。

[![](demo-mess5-small.png)](https://jvns.ca/images/2022-railsconf/demo-mess5.png)

We can also do the same experiment, but ask Google's resolver, 8.8.8.8, instead of Cloudflare's resolver.  

我们也可以做同样的实验，但是询问 Google 的解析器 8.8.8.8，而不是 Cloudflare 的解析器。

[![](demo-mess6-small.png)](https://jvns.ca/images/2022-railsconf/demo-mess6.png)

And we're seeing a similar thing here to what we saw with Cloudflare, there are maybe 4 independent caches.  

我们在这里看到的情况与我们在 Cloudflare 中看到的类似，可能有 4 个独立的缓存。

[![](slide-50.png)](https://jvns.ca/images/2022-railsconf/slide-50.png)

We could also do an experiment with negative caching, but no, I'm not going to do this demo. Sorry. I could just see it going downhill. The problem is that there's too many different caches, and I really want there to be one cache, but there's like seven. That's fine, let's move on.  

我们还可以进行负缓存实验，但是不，我不会做这个演示。对不起。我只能眼睁睁地看着它走下坡路。问题是有太多不同的缓存，我真的希望只有一个缓存，但大约有七个。没关系，我们继续吧。

[![](slide-51.png)](https://jvns.ca/images/2022-railsconf/slide-51.png)

Now I'm going to talk about my favorite strategy for learning about stuff, which is to write my own very bad version of the thing. And I want to say that writing my very bad implementation gives me a really unreasonable amount of confidence.  

现在我要谈谈我最喜欢的学习策略，即编写我自己的非常糟糕的版本。我想说的是，编写我的非常糟糕的实现给了我一种非常不合理的信心。

[![](slide-52.png)](https://jvns.ca/images/2022-railsconf/slide-52.png)

So you might think that writing DNS software is complicated, right? But it's easier than you might think, as long as you keep your expectations low.  

所以你可能会认为编写 DNS 软件很复杂，对吧？但这比您想象的要容易，只要您保持较低的期望即可。

[![](slide-53.png)](https://jvns.ca/images/2022-railsconf/slide-53.png)

To make the DNS queries, the first thing we need to do is we need to make a network connection. Let's do that.  

要进行 DNS 查询，我们需要做的第一件事是建立网络连接。让我们这样做吧。

[![](slide-54.png)](https://jvns.ca/images/2022-railsconf/slide-54.png)

These four lines of Ruby connect to 8.8.8.8, the Google DNS resolver, on UDP port 53. Now we're like halfway there. So after we've made a connection, we need to send Google a DNS query. You might be thinking, Julia, I don't know how to write a DNS query.  

这四行 Ruby 连接到 8.8.8.8（Google DNS 解析器）的 UDP 端口 53。现在我们已经完成了一半。因此，在建立连接后，我们需要向 Google 发送 DNS 查询。您可能会想，Julia，我不知道如何编写 DNS 查询。

[![](slide-55.png)](https://jvns.ca/images/2022-railsconf/slide-55.png)

But there's no problem. We can copy one from something else that knows what a DNS query looks like. AKA Wireshark.  

但没有问题。我们可以从其他知道 DNS 查询的东西中复制一个。又名 Wireshark。

[![](demo-write1-small.png)](https://jvns.ca/images/2022-railsconf/demo-write1.png)

So if I right click on this DNS query, it's very small, but I'm clicking on "copy", and then "copy as hex stream". You might not know what this means yet, but this is a DNS query. And you might think that like, Hey, you can't just copy and paste something and then send the exact same thing and it'll reply, but you can. And it works.  

因此，如果我右键单击此 DNS 查询，它非常小，但我单击“复制”，然后单击“复制为十六进制流”。您可能还不知道这意味着什么，但这是一个 DNS 查询。您可能会想，嘿，您不能只复制并粘贴某些内容，然后发送完全相同的内容，它就会回复，但您可以。它有效。

[![](slide-56.png)](https://jvns.ca/images/2022-railsconf/slide-56.png)

Here's what the code looks like to send this hex string we copied and pasted to 8.8.8.8.  

下面是将我们复制并粘贴的十六进制字符串发送到 8.8.8.8 的代码。

[![](demo-write2-small.png)](https://jvns.ca/images/2022-railsconf/demo-write2.png)

So we take this like hex string that we copy and pasted, and paste it into our tiny Ruby program, and use \`.pack\` to convert into a string of bytes and send it.  

因此，我们将复制并粘贴的十六进制字符串粘贴到我们的小型 Ruby 程序中，然后使用“.pack”将其转换为字节字符串并将其发送。

[![](demo-write3-small.png)](https://jvns.ca/images/2022-railsconf/demo-write3.png)

Now we run the Ruby program.  

现在我们运行 Ruby 程序。

[![](demo-write4-small.png)](https://jvns.ca/images/2022-railsconf/demo-write4.png)

Let's go to Wireshark and look for the packet we just sent. And we can see it there! There's some other noise in between, so I'll stop the capture.  

让我们到 Wireshark 中查找我们刚刚发送的数据包。我们可以在那里看到它！中间还有一些其他噪音，所以我会停止拍摄。

We can see that it's the same packet because the query ID matches, B962.  

我们可以看到这是同一个数据包，因为查询 ID 匹配，B962。

So we sent a query to Google the answer server and we got a response right? It was like this is totally legitimate. There's no problem. It doesn't know that we copied and pasted it and that we have no idea what it means!  

所以我们向 Google 应答服务器发送了一个查询，我们得到了响应，对吗？就好像这是完全合法的。这里没有问题。它不知道我们复制并粘贴了它并且我们不知道它意味着什么！

[![](slide-57.png)](https://jvns.ca/images/2022-railsconf/slide-57.png)

But we do want to know what this means, right? And so we'll take this hex string and split it into 2 parts. The first part is the header. And the second part is the question, which contains the actual domain name we're looking up.  

但我们确实想知道这意味着什么，对吗？因此，我们将把这个十六进制字符串分成两部分。第一部分是标题。第二部分是问题，其中包含我们正在查找的实际域名。

[![](slide-58.png)](https://jvns.ca/images/2022-railsconf/slide-58.png)

We're going to see how to construct these in Ruby, but first I want to talk about what a byte is for one second. So this (b9) is the hexadecimal representation of a byte. The way I like to look at figure out what that means is just type it into IRB, if you type in 0xB9 it'll print out, that's the number 184.  

我们将了解如何在 Ruby 中构建这些，但首先我想谈谈字节是什么。所以这个(b9)是一个字节的十六进制表示。我想弄清楚这意味着什么，只需将其输入 IRB，如果输入 0xB9，它就会打印出来，那就是数字 184。

So the question is 12 bytes  

所以问题是12字节

[![](slide-59.png)](https://jvns.ca/images/2022-railsconf/slide-59.png)

Those 12 bytes correspond six numbers, which are two bytes each. So the first number is the thing `b962` which is the query ID. The next number is the flags, which basically in this case, means like this is a query like hello, I have a question. And then there's four more sections, the number of questions and then the number of answers. We do not have any answers. We only have a question. So we're saying, hello, I have one question. That's what the header means.  

这 12 个字节对应 6 个数字，每个数字有两个字节。所以第一个数字是 `b962` ，它是查询 ID。下一个数字是标志，在本例中基本上意味着这是一个像“你好，我有一个问题”这样的查询。然后还有四个部分，问题的数量，然后是答案的数量。我们没有任何答案。我们只有一个问题。所以我们说，你好，我有一个问题。这就是标题的意思。

[![](slide-60.png)](https://jvns.ca/images/2022-railsconf/slide-60.png)

And the way that we can do this in Ruby, is we can make a little array that has the query ID, and then these numbers which correspond to the other the other header fields, the flags and then 1 for 1 question, and then three zeroes for each of the 3 sections of answers.  

在 Ruby 中我们可以做到这一点的方法是，我们可以创建一个包含查询 ID 的小数组，然后这些数字对应于其他标头字段、标志，然后是 1 代表 1 个问题，然后是 3答案的 3 个部分中的每一个都为零。

And then we need to tell Ruby how to take these like six numbers and then represent them as bytes. So n here means each of these is supposed to represent it as two bytes, and it also means to use big endian byte order.  

然后我们需要告诉 Ruby 如何获取这六个数字，然后将它们表示为字节。所以这里的 n 意味着这些中的每一个都应该将其表示为两个字节，并且它也意味着使用大端字节顺序。

[![](slide-61.png)](https://jvns.ca/images/2022-railsconf/slide-61.png)

Now let's talk about the question.  

现在我们来谈谈这个问题。

[![](slide-62.png)](https://jvns.ca/images/2022-railsconf/slide-62.png)

I broke up the question section here. There are two parts you might recognize from `example.com`: there's example, and com. The way it works is that first you have a number (like 7), and then a 7-character string, like "example". The number tells you how many characters to expect in each part of the domain name. So it's 7, example, 3, com, 0.  

我在这里把问题部分分开了。您可能会从 `example.com` 中识别出两个部分：示例和 com.它的工作原理是，首先有一个数字（例如 7），然后是一个 7 个字符的字符串，例如“example”。该数字告诉您域名每个部分中预期有多少个字符。所以它是 7，例如，3，com，0。

And then at the end, you have two more fields for the type and the class. Class 1 is code for "internet". And type 1 is code for "IP address", because we want to look up the IP address. is  

最后，您还有两个用于类型和类的字段。 1 类是“互联网”的代码。类型1是“IP地址”的代码，因为我们要查找IP地址。是

[![](slide-63.png)](https://jvns.ca/images/2022-railsconf/slide-63.png)

So we can write a little bit of code to do this. If we want to translate example.com into seven example three column zero, can like split the domain on a dot and then like get its length and concatenate that together and put a 0 on the end. It's just a little bit of Ruby. how to encode a domain name.  

所以我们可以编写一些代码来做到这一点。如果我们想将 example.com 转换为七个示例三列零，可以将域拆分为一个点，然后获取其长度并将其连接在一起并在末尾添加一个 0。这只是一点点 Ruby。如何对域名进行编码。

[![](slide-64.png)](https://jvns.ca/images/2022-railsconf/slide-64.png)

And then we can wrap all this up together where we make a random query ID. And then you make the header, encode the domain name, and then we add the type and the class, 1 and 1, and then we can just concatenate everything together and that's our query.  

然后我们可以将所有这些包装在一起，生成一个随机查询 ID。然后创建标头，对域名进行编码，然后添加类型和类，1 和 1，然后我们可以将所有内容连接在一起，这就是我们的查询。

[![](slide-65.png)](https://jvns.ca/images/2022-railsconf/slide-65.png)

There's definitely more work to do here to print out the response, but I wrote a 120-line Ruby script that parses the response too, and I want to show you a quick demo of it working.  

打印响应肯定还有更多工作要做，但我编写了一个 120 行的 Ruby 脚本来解析响应，我想向您展示它的工作原理的快速演示。

[![](demo-write7-small.png)](https://jvns.ca/images/2022-railsconf/demo-write7.png)

What domain should we look up>. rubyconfmini.com. All right, let's do it. Hey, it works!  

我们应该查找什么域名>。 rubyconfmini.com。好吧，我们开始吧。嘿，它有效！

[![](slide-66.png)](https://jvns.ca/images/2022-railsconf/slide-66.png)

I have a blog post that breaks down the whole thing on my blog, [Making a DNS query in Ruby from scratch](https://jvns.ca/blog/2022/11/06/making-a-dns-query-in-ruby-from-scratch/). It talks about how to decode the response.  

我有一篇博客文章详细介绍了我的博客中的整个内容：从头开始用 Ruby 进行 DNS 查询。它讨论如何解码响应。

[![](slide-67.png)](https://jvns.ca/images/2022-railsconf/slide-67.png)

We're at the end! Let's do a recap.  

我们到了最后！让我们回顾一下。

[![](slide-68.png)](https://jvns.ca/images/2022-railsconf/slide-68.png)

Okay. Let's go over the ways we've talked about learning things!  

好的。让我们回顾一下我们讨论过的学习方式！

First, spy on it. I find that when I look at things like to see like really what's happening under the hood, and when I look at like, what's in the bytes, you know what's going on? It's often like not as complicated as I think. Like, oh, there's just the domain name and the type. It really makes me feel far more confident that I understand that thing.  

首先，监视它。我发现当我观察事物时，就像看到幕后到底发生了什么，当我观察字节中的内容时，你知道发生了什么吗？通常情况并不像我想象的那么复杂。就像，哦，只有域名和类型。这确实让我对自己理解那件事感到更有信心。

I try to notice when I'm confused, and I want to say again, that noticing when you're confused is something that like we don't always have time for right? It's something to do when you have the energy. For example there's this weird DNS query I saw in one of the demos today that I don't understand, but I ignored it because, well, I'm giving a talk. But maybe one day I'll feel like looking at it.  

我试着注意到自己何时感到困惑，我想再说一遍，当我们感到困惑时，我们并不总是有时间去注意，对吗？当你有精力的时候，这是要做的事情。例如，我今天在一个演示中看到了这个奇怪的 DNS 查询，我不明白它，但我忽略了它，因为，好吧，我正在做演讲。但也许有一天我会想看看它。

We talked about reading the specification, which, there are few times I feel like more powerful than when I'm in like a discussion with someone, and I KNOW that I have the right answer because, well, I read the specification! It's a really nice way to feel certain.  

我们讨论了阅读规范，很少有时候我感觉比与某人讨论时更强大，而且我知道我有正确的答案，因为，好吧，我阅读了规范！这是一种非常好的感觉确定的方式。

I love to do experiments to check that my understanding of stuff is right. And often I learn that my understanding of something is wrong! I had an example in this talk that I was going to include and I did an experiment to check that that example was true, and it wasn't! And now I know that. I love that experiments on computers are very fast and cheap and usually have no consequences.  

我喜欢做实验来检查我对事物的理解是否正确。我常常发现我对某些事情的理解是错误的！我在这次演讲中有一个例子，我打算包括在内，我做了一个实验来检查这个例子是否正确，但事实并非如此！现在我知道了。我喜欢计算机上的实验，速度快、成本低，而且通常不会产生任何后果。

And then the last thing we talked about and truly my favorite, but the most work is like implementing your own terrible version. For me, the confidence I get from writing like a terrible DNS implementation that works on 11 different domain names is unmatched. If my thing works at all, I feel like, wow, you can't tell me that I don't know how DNS works! I implemented it! And it doesn't matter if my implementation is "bad" because I know that it works! I've tested it. I've seen it with my own eyes. And I think that just feels amazing. And there are also no consequences because you're never going to run it in production. So it doesn't matter if it's terrible. It just exists to give you huge amounts of confidence in yourself. And I think that's really nice.  

然后我们谈论的最后一件事确实是我最喜欢的，但最多的工作就是实现你自己的糟糕版本。对我来说，通过编写适用于 11 个不同域名的糟糕 DNS 实现，我获得的信心是无与伦比的。如果我的东西完全有效，我会觉得，哇，你不能告诉我我不知道 DNS 是如何工作的！我实现了！如果我的实现“不好”也没关系，因为我知道它有效！我已经测试过了。我亲眼所见。我认为这感觉棒极了。而且也不会有任何后果，因为你永远不会在生产中运行它。所以即使很可怕也没关系。它的存在只是为了让你对自己充满信心。我认为这真的很好。

[![](slide-69.png)](https://jvns.ca/images/2022-railsconf/slide-69.png)

That's all for me. Thank you for listening.  

这就是我的全部。感谢您的聆听。

### thanks to the organizers!  

感谢主办方！

Thanks to the RubyConf Mini organizers for doing such a great job with the conference – it was the first conference I’d been to since 2019, and I had a great time.  

感谢 RubyConf Mini 组织者为这次会议做了如此出色的工作——这是我自 2019 年以来参加的第一次会议，我度过了一段愉快的时光。

### a quick plug for “How DNS Works”  

“DNS 如何工作”的快速插件

If you liked this talk and want to to spend _less_ than 10 years learning about how DNS works, I spent 6 months condensing everything I know about DNS into 28 pages. It’s here and you can get it for $12: [How DNS Works](https://wizardzines.com/zines/dns/).  

如果您喜欢这个演讲，并且想花不到 10 年的时间来了解 DNS 的工作原理，我花了 6 个月的时间将我所知道的有关 DNS 的所有内容浓缩为 28 页。您可以花 12 美元购买：DNS 工作原理。
