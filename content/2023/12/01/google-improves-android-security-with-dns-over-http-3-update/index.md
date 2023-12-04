---
title: Google improves Android security with DNS
date: 2023-12-01T08:26:20.000Z
updated: 2023-12-01T08:26:20.000Z
published: 2022-07-21T08:26:20.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: >-
    https://www.androidcentral.com/apps-software/google-improves-android-security-dns-over-http3-update
  hostname: www.androidcentral.com
  author: Nickolas Diaz
  original_title: Google improves Android security with DNS-over-HTTP/3 update
  original_lang: en

---

![Google apps on an Android phone home screen](qsba66QW5qoKryNKBJmM8D-320-80.jpg)

(Image credit: Jay Bonggolto / Android Central)  

（图片来源：Jay Bonggolto / Android Central）

## What you need to know  

你需要知道什么

-   Google implements a new DNS feature, DNS-over-HTTP/3.  
    
    Google 实施了一项新的 DNS 功能：DNS-over-HTTP/3。
-   The new features aim to cut down issues with DNS-over-TLS has, such as longer latency and a slower reconnection when it comes to changing networks.  
    
    这些新功能旨在减少 DNS-over-TLS 带来的问题，例如网络变化时的延迟较长和重新连接速度较慢。
-   The new DoH3 is Google's attempt at providing better privacy for DNS queries on Android devices.  
    
    新的 DoH3 是 Google 为 Android 设备上的 DNS 查询提供更好的隐私性的尝试。

Google has implemented a new security update for Android devices. The new update comes via the addition of  DNS-over-HTTPS/3.  

谷歌为 Android 设备实施了新的安全更新。新的更新是通过添加 DNS-over-HTTPS/3 来实现的。

Google is looking to "help keeps Android users' DNS queries private" with this security addition. Its [Android Team](https://security.googleblog.com/2022/07/dns-over-http3-in-android.html?m=1) is looking at this new DNS-over-HTTP/3 as a good security step forward, seeing as it has "a number of improvements over DNS-over-TLS" already in play.  

谷歌希望通过这一安全补充来“帮助保持 Android 用户的 DNS 查询的私密性”。其 Android 团队将这种新的 DNS-over-HTTP/3 视为一个良好的安全进步，因为它已经“对 DNS-over-TLS 进行了许多改进”。

DNS is the query sent from your device to a server so you can receive what you want. Think of clicking on a link and letting it load. That's your device sending out its request and the server returning to you the content you're interested in, which is essentially connecting you to its IP address.  

DNS 是从您的设备发送到服务器的查询，以便您可以接收您想要的内容。想象一下点击一个链接并让它加载。您的设备发出请求，服务器返回您感兴趣的内容，这实际上是将您连接到其 IP 地址。

Seeing as DNS is what takes you across the web, there are security worries that Google is looking to solve with the inclusion of this new DNS-over-HTTP/3. "DNS lookup has traditionally not been private by default," the company explains. Google referred to its [Android 9 announcement](https://android-developers.googleblog.com/2018/04/dns-over-tls-support-in-android-p.html), where it implemented a new private DNS measure. This new DNS feature, which it says has been "rapidly gaining traction," is being used by the likes of [Cloudflare](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/dns-over-https-client/).  

由于 DNS 是带您浏览网络的工具，因此 Google 希望通过包含这种新的 DNS-over-HTTP/3 来解决安全问题。 “DNS 查找传统上默认情况下不是私有的，”该公司解释道。谷歌提到了其 Android 9 公告，其中实施了新的私有 DNS 措施。这项新的 DNS 功能据称已“迅速获得关注”，目前已被 Cloudflare 等公司使用。

The team notes that DNS-over-HTTP/3 support was included in a [Google Play Store update](https://source.android.com/devices/architecture/modular-system) back in June. The new encrypted DNS protocols should already be in place, which, according to them, avoids some of the issues DNS-over-TLS suffers, such as "head-of-line." Google's Android Team explains that this is caused by DoT running every request to a server on one line, which essentially creates a traffic jam. If one query is held up for some reason, all other queries will have to wait.  

该团队指出，DNS-over-HTTP/3 支持已包含在 6 月份的 Google Play 商店更新中。新的加密 DNS 协议应该已经就位，据他们称，这可以避免 DNS-over-TLS 遇到的一些问题，例如“领先”。谷歌的 Android 团队解释说，这是由于 DoT 在一条线路上运行对服务器的每个请求造成的，这实际上造成了交通拥堵。如果一个查询由于某种原因被搁置，所有其他查询都将不得不等待。

Meanwhile, DoH3 runs each "request" on its own line. This should remove the chance of people pinging a server to meet long-winded delays.  

同时，DoH3 在自己的线路上运行每个“请求”。这应该可以消除人们通过 ping 服务器来解决长时间延迟的可能性。

DoH3 is also supposed to solve an issue with devices on the move. While we're out, the connection on our [Android phones](https://www.androidcentral.com/best-android-phones) is constantly moving and switching from tower to tower. Google explains that while DoT requires your connection to be "renegotiated" to establish itself, DoH3 can resume a session much quicker. Google also touts that DoH3 can "outperform" in terms of latency - the time it takes for information to be returned to you.  

DoH3 还应该解决移动设备的问题。当我们外出时，我们的 Android 手机上的连接会不断移动并从一个塔切换到另一个塔。 Google 解释说，虽然 DoT 要求您“重新协商”连接才能建立连接，但 DoH3 可以更快地恢复会话。谷歌还宣称，DoH3 在延迟（信息返回给您所需的时间）方面可以“表现出色”。

The [Android Team](https://security.googleblog.com/2022/07/dns-over-http3-in-android.html?m=1) finally touched on some safety measures from the inclusion of [Rust](https://security.googleblog.com/2021/04/rust-in-android-platform.html) in 2021. Google's Rust support was brought in some help protect users from malicious attackers while using the internet on an Android device. It was also brought in to cut down on some of the memory safety issues, which Google said represented "~70% of Android's high severity security vulnerabilities." The company has done work with DNS before regarding [Google Wi-Fi and swapping DNS settings](https://www.androidcentral.com/how-change-dns-settings-your-google-wifi) for faster query returns.  

Android 团队最终在 2021 年引入 Rust 后触及了一些安全措施。谷歌的 Rust 支持在一定程度上有助于保护用户在 Android 设备上使用互联网时免受恶意攻击。它还被引入来减少一些内存安全问题，谷歌表示这些问题代表了“Android 高严重安全漏洞的约 70%”。该公司之前曾就 Google Wi-Fi 与 DNS 进行过合作，并交换 DNS 设置以加快查询返回速度。

Get the hottest deals available in your inbox plus news, reviews, opinion, analysis and more from the Android Central team.  

获取收件箱中最热门的优惠以及来自 Android Central 团队的新闻、评论、意见、分析等。

Nickolas is always excited about tech and getting his hands on it. Writing for him can vary from delivering the latest tech story to scribbling in his journal. When Nickolas isn't hitting a story, he's often grinding away at a game or chilling with a book in his hand.  

尼古拉斯总是对科技感到兴奋并亲自尝试。为他写作的内容多种多样，从提供最新的科技故事到在日记中乱写乱画。当尼古拉斯不看故事的时候，他经常会埋头苦干，或者手里拿着一本书。