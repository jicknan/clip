---
title: Introducing Name Server Sets
date: 2023-11-29T07:22:01.000Z
updated: 2023-11-29T07:22:01.000Z
published: 2023-01-11T07:22:01.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: >-
    https://blog.dnsimple.com/2023/01/introducing-name-server-sets/?utm_source=pocket_reader
  hostname: blog.dnsimple.com
  author: null
  original_title: Introducing Name Server Sets - DNSimple Blog
  original_lang: en

---

Managing the name servers for a single domain is easy — you rarely need to make changes, and when you do, it's usually just entering a few name server names. But, if you have more than a few domains, it quickly becomes cumbersome. So we simplified it.  

管理单个域的名称服务器很容易 - 您很少需要进行更改，而且当您这样做时，通常只需输入一些名称服务器名称即可。但是，如果您有多个域，它很快就会变得很麻烦。所以我们简化了它。

Today, the DNSimple team is excited to announce name server sets.  

今天，DNSimple 团队很高兴地宣布名称服务器集。

In the past, customers with existing domains that had fewer than four name servers couldn't migrate those domains into DNSimple. Now, any customer can use vanity name servers plus name server sets to migrate those domains to DNSimple - without changing the delegation at the domain registrar. They also let you reuse the same name servers across multiple zones and domains without having to repeatedly enter the same information.  

过去，现有域的名称服务器少于四个的客户无法将这些域迁移到 DNSimple。现在，任何客户都可以使用虚名服务器和名称服务器集将这些域迁移到 DNSimple，而无需更改域注册商的委派。它们还允许您跨多个区域和域重复使用相同的名称服务器，而无需重复输入相同的信息。

Name server sets are now available on every [DNSimple plan](https://dnsimple.com/pricing).  

现在，每个 DNSimple 计划都提供名称服务器集。

Let's take a closer look at what this means and how to implement it for your domains.  

让我们仔细看看这意味着什么以及如何在您的域中实现它。

## Save time, reduce errors  

节省时间，减少错误

Name server sets simplify name server management, acting as templates you can apply to any domain. They create reusable groups of name server records you can apply to the name server delegation, secondary DNS configuration, and zone NS records of your domains.  

名称服务器集简化了名称服务器管理，充当可应用于任何域的模板。它们创建可重复使用的名称服务器记录组，您可以将其应用于域的名称服务器委派、辅助 DNS 配置和区域 NS 记录。

Whenever name servers are changed, a set can be applied, so you can enter name server and NS records faster while reducing the possibility for errors.  

每当更改名称服务器时，都可以应用一组，因此您可以更快地输入名称服务器和 NS 记录，同时减少出错的可能性。

This feature also allows the creation of sets of vanity name servers with fewer than four name servers (and up to thirteen). So if you have a large number of domains, and are migrating those domains to DNSimple, you can keep your existing name server names and just change their IP addresses.  

此功能还允许创建少于四个名称服务器（最多十三个）的虚名称服务器集。因此，如果您有大量域，并且要将这些域迁移到 DNSimple，您可以保留现有的名称服务器名称，而只需更改其 IP 地址。

## Implementation 执行

Name server sets are private to an account and can contain custom name servers. You can manage your account's name server sets from the Account Name Server Sets page if you have full access to the account.  

名称服务器集是帐户专用的，并且可以包含自定义名称服务器。如果您拥有帐户的完全访问权限，则可以从“帐户名称服务器集”页面管理帐户的名称服务器集。

To create an account name server set, go to the Account page, click the Name Server Sets tab, then click `Add` to create a new name server set. Enter a unique label for the new name server set and the hostname.  

要创建帐户名称服务器集，请转至帐户页面，单击名称服务器集选项卡，然后单击 `Add` 创建新的名称服务器集。为新名称服务器集和主机名输入唯一标签。

You can also provide the IPv4 address and IPv6 address (glue IPs), so they'll be stored and automatically applied later when needed — like when changing a domain's delegation.  

您还可以提供 IPv4 地址和 IPv6 地址（粘合 IP），以便稍后在需要时（例如更改域的委派时）存储并自动应用它们。

The DNSimple UI has changed to make it easier to work with domain name servers, including adding support for name server sets.  

DNSimple UI 已更改，以便更轻松地使用域名服务器，包括添加对名称服务器集的支持。

We've also added a [new endpoint](https://developer.dnsimple.com/v2/zones/ns-records/) to the DNSimple API to support applying name servers and name server sets to a zone. This lets you apply a variable number of NS records, including vanity name server NS records, to a domain's zone.  

我们还在 DNSimple API 中添加了一个新端点，以支持将名称服务器和名称服务器集应用到区域。这使您可以将可变数量的 NS 记录（包括虚名服务器 NS 记录）应用到域的区域。

Changes to a name server set's definition will not affect any existing domain name server or configurations that had included the name server sets.  

对名称服务器集定义的更改不会影响任何现有域名服务器或包含名称服务器集的配置。

For more on editing or deleting name server sets, check out our [support documentation](https://support.dnsimple.com/articles/name-server-sets/).  

有关编辑或删除名称服务器集的更多信息，请查看我们的支持文档。

Share on [Twitter](http://twitter.com/share?text=Introducing%20Name%20Server%20Sets&url=https://blog.dnsimple.com/2023/01/introducing-name-server-sets/ "Tweet this post") and [Facebook](https://www.facebook.com/sharer/sharer.php?u=https://blog.dnsimple.com/2023/01/introducing-name-server-sets/ "Share on Facebook")  

在 Twitter 和 Facebook 上分享

![Anthony Eden's profile picture](31254903db793bf6f84bbd607fe092fd.png "Anthony Eden")

#### Anthony Eden 安东尼·艾登

I break things so Simone continues to have plenty to do. I occasionally have useful ideas, like building a domain and DNS provider that doesn't suck.  

我打破了一切，所以西蒙娜仍然有很多事情要做。我偶尔会有一些有用的想法，比如构建一个不错的域名和 DNS 提供商。
