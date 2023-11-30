---
title: The ZONEMD (message digest) resource record ZONEMD（消息摘要）资源记录
date: 2023-11-27T08:36:21.000Z
updated: 2023-11-27T08:36:21.000Z
published: 2023-04-16T08:36:21.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: >-
    https://jpmens.net/2023/04/16/the-zonemd-message-digest-resource-record/?utm_source=pocket_saves
  hostname: jpmens.net
  author: Jan-Piet Mens
  original_title: The ZONEMD (message digest) resource record
  original_lang: en

---

## [The ZONEMD (message digest) resource record  

ZONEMD（消息摘要）资源记录](https://jpmens.net/2023/04/16/the-zonemd-message-digest-resource-record/)

A zone digest is a cryptographic digest, or hash, of the data in a DNS zone which is embedded in the zone data itself as a ZONEMD resource record. It is computed upon publishing the zone, and it can be verified by zone recipients. ZONEMD is [specified in RFC 8976](https://www.rfc-editor.org/rfc/rfc8976.html).  

区域摘要是 DNS 区域中数据的加密摘要或哈希值，它作为 ZONEMD 资源记录嵌入到区域数据本身中。它是在发布区域时计算的，并且可以由区域接收者验证。 ZONEMD 在 RFC 8976 中指定。

In order to compute the hash, zone data is fed to a digest function using a well-defined and consistent record ordering and format. The data given to the hash excludes the ZONEMD record itself and its signatures if the zone is signed. The digest is then added to the apex of the zone itself and signed with DNSSEC (for signed zones).  

为了计算哈希值，区域数据使用明确定义且一致的记录顺序和格式馈送到摘要函数。如果区域已签名，则提供给哈希的数据不包括 ZONEMD 记录本身及其签名。然后将摘要添加到区域本身的顶点并使用 DNSSEC 进行签名（对于签名区域）。

Let’s see an example at work (with my new [favorite TXT record](https://piaille.fr/@shaft/110192596994475708) in it):  

让我们看一个工作示例（其中有我最喜欢的 TXT 记录）：

```
$ORIGIN example.aa.
$TTL 3600
@    SOA   ns root 4 3H 1H 1W 1H
     NS    ns
     TXT   "DNS is innocent"
ns   A     127.0.0.1
```

I use [ldns-zone-digest](https://github.com/verisign/ldns-zone-digest) to create the digest (hash) and add the ZONEMD record to the zone:  

我使用 ldns-zone-digest 创建摘要（哈希）并将 ZONEMD 记录添加到区域：

```
$ ldns-zone-digest -c  -p 1,1 -o example.aa.digest example.aa ../example.aa
Loading Zone...4 records
Remove existing ZONEMD RRset
Add placeholder ZONEMD with scheme 1 and hash algorithm 1

$ cat example.aa.digest
example.aa.3600INNSns.example.aa.
example.aa.3600INSOAns.example.aa. root.example.aa. 4 10800 3600 604800 3600
example.aa.3600INTXT"DNS is innocent"
example.aa.3600INZONEMD4 1 1 83dbb84f8b78e9bea8badede9316fb238f5c923440def32534aa147298d0912752aaf9b287823df1d1b737e43e71396d
ns.example.aa.3600INA127.0.0.1

$ named-checkzone -q -F text -s relative -o - example.aa example.aa.digest
$ORIGIN .
$TTL 3600; 1 hour
example.aaIN SOAns.example.aa. root.example.aa. (
4          ; serial
10800      ; refresh (3 hours)
3600       ; retry (1 hour)
604800     ; expire (1 week)
3600       ; minimum (1 hour)
)
NSns.example.aa.
TXT"DNS is innocent"
ZONEMD4 1 1 (
83DBB84F8B78E9BEA8BADEDE9316FB238F5C923440DE
F32534AA147298D0912752AAF9B287823DF1D1B737E4
3E71396D )
$ORIGIN example.aa.
nsA127.0.0.1
```

As is to be expected, even if I change the order of the records in the input, the digest remains identical, as it should.  

正如预期的那样，即使我更改输入中记录的顺序，摘要也应该保持不变。

For a signed zone, I use `ldns-zone-digest ... -z <ZSK.private>` so that the utility can compute and add the RRSIG to the ZONEMD record.  

对于签名区域，我使用 `ldns-zone-digest ... -z <ZSK.private>` 以便实用程序可以计算 RRSIG 并将其添加到 ZONEMD 记录中。

The rdata of the ZONEMD record contains the following fields:  

ZONEMD 记录的 rdata 包含以下字段：

1.  serial, which must match the SOA serial of the zone the ZONEMD is being added to  
    
    序列号，必须与 ZONEMD 添加到的区域的 SOA 序列号相匹配
2.  scheme, wich currently is `1` (`SIMPLE`)  
    
    方案，当前为 `1` ( `SIMPLE` )
3.  hash algorithm, `1` for SHA-384 and `2` for SHA-512  
    
    哈希算法，对于 SHA-384 为 `1` ，对于 SHA-512 为 `2`
4.  digest field with 48 octets for SHA-384 and 64 octets for SHA-512  
    
    摘要字段，SHA-384 为 48 个八位字节，SHA-512 为 64 个八位字节

Another offline utility I can use is [dns-tools](https://github.com/niclabs/dns-tools); a single binary program written in Golang which has different functions. The one to create a ZONEMD record in a zone is:  

我可以使用的另一个离线实用程序是 dns-tools；用 Golang 编写的单个二进制程序，具有不同的功能。在区域中创建 ZONEMD 记录的方法是：

```
$ dns-tools digest -f example.aa -o example.aa.digest -d 1 -z example.aa
[dns-tools] 2023/04/16 10:56:42 Using config file: /tmp/dns-tools/dns-tools-config.json
[dns-tools] 2023/04/16 10:56:42 Reading and parsing zone example.aa (updateSerial=false)
[dns-tools] 2023/04/16 10:56:42 Sorting zone
[dns-tools] 2023/04/16 10:56:42 Zone Sorted
[dns-tools] 2023/04/16 10:56:42 Updating ZONEMD Digest
[dns-tools] 2023/04/16 10:56:42 Started digest calculation.
[dns-tools] 2023/04/16 10:56:42 Stopped digest calculation.
[dns-tools] 2023/04/16 10:56:42 Writing zone
[dns-tools] 2023/04/16 10:56:42 Zone written
[dns-tools] 2023/04/16 10:56:42 zone digested successfully in example.aa.digest.

$ cat example.aa.digest
example.aa.3600INSOAns.example.aa. root.example.aa. 4 10800 3600 604800 3600
example.aa.3600INNSns.example.aa.
example.aa.3600INTXT"DNS is innocent"
ns.example.aa.3600INA127.0.0.1
example.aa.3600INZONEMD4 1 1 83dbb84f8b78e9bea8badede9316fb238f5c923440def32534aa147298d0912752aaf9b287823df1d1b737e43e71396d
```

[Knot-DNS](https://www.knot-dns.cz/) can add the digest automatically, if I [configure the zone](https://www.knot-dns.cz/docs/3.2/html/reference.html#zone-section) appropriately:  

如果我正确配置区域，Knot-DNS 可以自动添加摘要：

```
zone:
  - domain: example.aa
    template: cmember
    zonemd-generate: zonemd-sha384
```

Note how the digest equals our examples above as the zone contains the same data and has the same SOA serial number.  

请注意摘要如何等于我们上面的示例，因为区域包含相同的数据并具有相同的 SOA 序列号。

```
$ dig @192.168.1.170 +noall +answer +onesoa example.aa AXFR +multi
example.aa.3600 INSOA ns.example.aa. root.example.aa. (
4          ; serial
10800      ; refresh (3 hours)
3600       ; retry (1 hour)
604800     ; expire (1 week)
3600       ; minimum (1 hour)
)
example.aa.3600 INNS ns.example.aa.
example.aa.3600 INTXT "DNS is innocent"
example.aa.3600 INZONEMD 4 1 1 (
83DBB84F8B78E9BEA8BADEDE9316FB238F5C923440DE
F32534AA147298D0912752AAF9B287823DF1D1B737E4
3E71396D )
ns.example.aa.3600 INA 127.0.0.1
```

### Verification 确认

[Unbound](http://unbound.net/) has supported ZONEMD for some time now, and I configure an authoritative zone as follows:  

Unbound支持ZONEMD已经有一段时间了，我配置一个权威区域如下：

```
server:
     verbosity: 3

auth-zone:
        name: "example.aa"
        primary: 192.168.1.170@5354
        zonemd-check: yes
        zonemd-reject-absence: yes
        zonefile: "example.aa"
```

When I reload the server, I can follow the digest verification in the log file:  

当我重新加载服务器时，我可以遵循日志文件中的摘要验证：

```
[1681636411] unbound[47600:0] debug: auth-zone example.aa. ZONEMD hash is correct
[1681636411] unbound[47600:0] debug: auth zone example.aa. ZONEMD verification successful
```

If I configure Unbound to load the zone from a file, and I change the SOA serial in the file without recomputing ZONEMD, Unbound warns upon loading the zone:  

如果我将 Unbound 配置为从文件加载区域，并且更改文件中的 SOA 序列而不重新计算 ZONEMD，则 Unbound 将在加载区域时发出警告：

```
[1681636681] unbound[47687:0] debug: auth-zone example.aa. ZONEMD failed: ZONEMD serial is wrong
[1681636681] unbound[47687:0] warning: auth zone example.aa.: ZONEMD verification failed: ZONEMD serial is wrong
```

[PowerDNS](http://www.powerdns.com/) has [support for ZONEMD](https://blog.powerdns.com/2022/11/15/powerdns-recursor-zonemd-the-missing-validation/) in `pdnsutil` and in validating ZONEMD in the PowerDNS Recursor in the [zoneToCache function](https://docs.powerdns.com/recursor/lua-config/ztc.html).  

PowerDNS 支持 `pdnsutil` 中的 ZONEMD 以及在 zoneToCache 函数中验证 PowerDNS Recursor 中的 ZONEMD。

[NSD](https://www.nlnetlabs.nl/projects/nsd/about/) parses ZONEMD since release 4.3.4, and [BIND](https://www.isc.org/bind/) parses the record, but cannot as yet produce the record.  

NSD 从 4.3.4 版本开始解析 ZONEMD，BIND 解析记录，但还不能生成记录。

[ldns](https://nlnetlabs.nl/projects/ldns/about/) has support for ZONEMD in both `ldns-signzone` and `ldns-verify-zone`:  

ldns 在 `ldns-signzone` 和 `ldns-verify-zone` 中都支持 ZONEMD：

```
$ ldns-keygen -a13 -k example.aa
Kexample.aa.+013+31040

$ ldns-signzone -z 1:1 example.aa Kexample.aa.+013+31040

$ ldns-verify-zone -ZZ example.aa.signed   # Requires a valid signed ZONEMD RR
Zone is verified and complete

$ ldns-zone-digest -v example.aa example.aa.signed
Loading Zone...16 records
Found and calculated digests for scheme:hashalg 1:1 do MATCH.
```

There are [ongoing tests](http://zonemd-testing.verisignlabs.com/) in adding ZONEMD to the root zone itself.  

将 ZONEMD 添加到根区域本身的测试正在进行中。

ZONEMD protects zone data “at rest” and is useful when transferring data between primaries and secondaries. (I think of it as being like a checksum for a zone which is contained in the zone.) Not only in cases in which servers AXFR the zone but also when zones are distributed outside of the DNS, as is the case, for instance, with the DNS root, published via the Web or on FTP. In all cases the integrity of the zone can be verified after downloading the zone. ZONEMD doesn’t provide origin authenticity; DNSSEC is required for that.  

ZONEMD 保护“静态”区域数据，并且在主节点和辅助节点之间传输数据时非常有用。 （我认为它就像区域中包含的区域的校验和。）不仅在服务器 AXFR 区域的情况下，而且在区域分布在 DNS 之外的情况下，例如，具有 DNS 根，通过 Web 或 FTP 发布。在所有情况下，可以在下载区域后验证区域的完整性。 ZONEMD 不提供原产地真实性；为此需要 DNSSEC。

Otto [makes a good point](https://bsd.network/@otto/110209016675410723):  

奥托提出了一个很好的观点：

> it is also interesting to note that glue records in a zone do note have DNSSEC signatures, but are covered by the ZONEMD record. So the signature of a ZONEMD record does cover glue records in an indirect way.  
> 
> 还值得注意的是，区域中的粘合记录确实具有 DNSSEC 签名，但被 ZONEMD 记录覆盖。因此，ZONEMD 记录的签名确实以间接方式覆盖了粘合记录。

### Further reading 进一步阅读

-   [Message Digest for DNS Zones](https://www.bortzmeyer.org/8976.html), article in French by Stéphane Bortzmeyer  
    
    DNS 区域的消息摘要，Stéphane Bortzmeyer 的法语文章
