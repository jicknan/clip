---
title: CNAME vs AWS Route 53 Alias
date: 2023-11-29T08:41:50.000Z
updated: 2023-11-29T08:41:50.000Z
published: 2023-01-14T08:41:50.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://blog.gowthamparamasivam.com/cname-vs-aws-route-53-alias
  hostname: blog.gowthamparamasivam.com
  author: Gowtham Paramasivam
  original_title: CNAME vs AWS Route 53 Alias
  original_lang: en

---

## CNAME Records 别名记录

A **CNAME (Canonical Name)** record is a type of DNS (Domain Name System) record that maps an alias hostname to the real or canonical hostname. This allows a subdomain to be pointed to another domain or subdomain, effectively redirecting all requests to the canonical hostname.  

CNAME（规范名称）记录是一种 DNS（域名系统）记录，它将别名主机名映射到真实或规范主机名。这允许子域指向另一个域或子域，从而有效地将所有请求重定向到规范主机名。

It's important to note that a **CNAME record can't be used to point to an IP address**, only a domain or subdomain. Also, the domain or subdomain specified in the CNAME record must have its DNS record, typically an A or AAAA record, that contains the IP address that the domain or subdomain resolves to.  

需要注意的是，CNAME 记录不能用于指向 IP 地址，只能用于指向域或子域。此外，CNAME 记录中指定的域或子域必须有其 DNS 记录（通常是 A 或 AAAA 记录），其中包含域或子域解析到的 IP 地址。

When using CNAME records, there are a few key things to keep in mind:  

使用 CNAME 记录时，需要牢记以下几点：

1.  CNAME records can't point to an IP address, only a domain or subdomain.  
    
    CNAME 记录不能指向 IP 地址，只能指向域或子域。
    
2.  CNAME records can't be used in the root domain.  
    
    CNAME 记录不能在根域中使用。
    
3.  CNAME records can cause a delay in DNS resolution.  
    
    CNAME 记录可能会导致 DNS 解析延迟。
    
4.  CNAME records can't coexist with other types of records, such as MX, NS or SOA.  
    
    CNAME 记录不能与其他类型的记录共存，例如 MX、NS 或 SOA。
    

By keeping these things in mind, you can ensure that your CNAME records are set up correctly and are working as intended.  

通过牢记这些事项，您可以确保 CNAME 记录设置正确并按预期工作。

## CNAME Examples CNAME 示例

### Subdomain to parent domain  

子域到父域

### Subdomain to subdomain 子域到子域

### Subdomain to other root domain  

子域到其他根域

A record for [xyz.com](http://xyz.com/) will be in another zone file, hosted on another Name Server.  

xyz.com 的记录将位于另一个区域文件中，托管在另一个名称服务器上。

## AWS Route53 Alias AWS Route53 别名

AWS Route 53 Alias record is a type of DNS record that allows you to map a hostname to a specific Amazon Web Services (AWS) resource, such as an Amazon S3 bucket, an Elastic Load Balancer, or a CloudFront distribution.  

AWS Route 53 别名记录是一种 DNS 记录，允许您将主机名映射到特定 Amazon Web Services (AWS) 资源，例如 Amazon S3 存储桶、弹性负载均衡器或 CloudFront 分配。

An Alias record works similarly to a CNAME record, in that it allows you to point a subdomain to another destination. However, there are some key differences between Alias records and CNAME records:  

别名记录的工作方式与 CNAME 记录类似，因为它允许您将子域指向另一个目标。但是，别名记录和 CNAME 记录之间存在一些关键区别：

-   Alias records can be used to map a hostname to any AWS resource that has a publicly resolvable DNS name, whereas CNAME records can only map to a domain or subdomain.  
    
    别名记录可用于将主机名映射到具有可公开解析的 DNS 名称的任何 AWS 资源，而 CNAME 记录只能映射到域或子域。
    
-   Alias records can be updated automatically if the IP address of the AWS resource changes, whereas CNAME records would need to be updated manually.  
    
    如果AWS资源的IP地址发生变化，别名记录可以自动更新，而CNAME记录则需要手动更新。
    
-   Alias records are free, unlike CNAME records, you don't have to pay for each Alias record.  
    
    别名记录是免费的，与 CNAME 记录不同，您无需为每条别名记录付费。
    

One of the main use cases for Alias records is to map a hostname to an Amazon S3 bucket that is configured as a static website. By using an Alias record, you can map a hostname to the S3 bucket's endpoint, and ensure that your website is available even if the bucket's IP address changes.  

别名记录的主要用例之一是将主机名映射到配置为静态网站的 Amazon S3 存储桶。通过使用别名记录，您可以将主机名映射到 S3 存储桶的终端节点，并确保即使存储桶的 IP 地址发生更改，您的网站也可用。

## AWS Route53 Alias Example  

AWS Route53 别名示例

## Key Differences 主要差异

|  | AWS Route 53 Alias AWS Route 53 别名 | CNAME 别名记录 |
| --- | --- | --- |
| **Target resource 目标资源** | Any AWS resource that has a publicly resolvable DNS name  
具有可公开解析的 DNS 名称的任何 AWS 资源 | Domain or subdomain 域或子域 |
| **Automatic updates 自动更新** | Updated automatically if the IP address of the AWS resource changes  

如果 AWS 资源的 IP 地址发生更改，则会自动更新 | Need to be updated manually  

需要手动更新 |
| **Cost 成本** | Free 自由的 | May incur cost 可能会产生费用 |
| **Root domain 根域** | Can be used on the root domain  

可以在根域上使用 | Can't be used on the root domain  

不能在根域上使用 |
| **Performance 表现** | Resolved quickly 很快就解决了 | Comparatively slower 相对较慢 |

