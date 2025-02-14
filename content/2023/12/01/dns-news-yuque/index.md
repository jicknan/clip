---
title: ".net“断网”事件"
date: 2023-12-01T16:15:04+08:00
updated: 2023-12-01T16:15:04+08:00
published: 2022-01-23T16:15:04+08:00
taxonomies:
  tags: []
extra:
  source: https://www.yuque.com/dnswiki/aok7ga/perydk
  hostname: www.yuque.com
  author: 
  original_title: "DNS-News · Yuque"
  original_lang: und
---

2020年1月23日，互联网顶级域名“.net”在F根服务器（由互联网基础软件研发机构ISC运行）以及E根（由美国国家航空航天局运行）的解析出现了故障。由于“.net”和“.com”（通用顶级域名）以及“.cn”(中国的国家代码顶级域名)一样，是互联网使用范围最广的顶级域名之一，有一千三百四十多万注册量，其解析故障导致了大量使用“.net”域名的网站和服务器从互联网“断开网络连接”，持续3小时18分钟。  

这次事件在国际互联网社群造成很大的影响，互联网域名系统国家工程研究中心主任毛伟研究员，针对此次断网事件进行了解读和分析。  

  

复盘：域名解析故障叠加路由控制失效导致的断网事件  

2月22日，F根的运行机构--互联网基础软件研发机构ISC(Internet System Consortium，DNS开源项目BIND的维护单位) 发布了一份报告，对此次事件的原因进行了说明。根据该报告披露，此次断网的原因是：F根部分服务节点部署在美国CDN厂商Cloudflare的网络中。由于Cloudflare在其网络基础软件进行升级时，出现了故障，不能正常对互联网返回F根服务器的寻址信息。在故障修复之前，由于Cloudflare没有及时停止对互联网广播F根的服务地址（F根的IP地址），大量用户流量仍然被路由到Cloudflare运行的故障F根节点访问，导致无法访问所有“.net”域名的互联网服务。1月23日，在收到用户反馈“断网”后，Cloudflare停止了对外广播（BGP）F根的服务地址，互联网用户对F根的访问流量被定位到其他机构运行的F根服务节点上。完成故障修复后，Cloudflare重新对外广播F根的服务地址，向互联网用户提供正常的根区解析服务。  

由美国国家航空航天局(NASA)运行的E根的部分服务节点也部署在美国CDN厂商Cloudflare的网络中，并受此次故障影响，其原因应该是一样的。  

  

启示：“路由断网”和“域名断网”既相互区别，又有所联系  

此次断网事件让“域名系统”和“路由系统”再次成为高亮词。回顾互联网的安全史，大概没有哪个单一系统故障能像域名系统和路由系统，一旦出现故障就可以造成大面积的网络瘫痪或服务中断。但这次事件的原因交织了两个“断网”要素：突然出现的“域名断网”，需要通过“路由断网”来终止不利影响。  

在此次断网事件中，首先是因为域名系统的解析故障，导致了根服务器（F根）反馈了错误的“.net”域名解析结果。ISC官方说此次故障是由于F根节点（Cloudflare公司）的基础软件（underlying software)升级导致的：运行在cloudflare的F根节点软件升级后出了bug，导致返回“.net”顶级域名的NS 记录时没有反馈glue记录（“.net”权威服务器的IP地址）从而导致用户无法进行下一步DNS解析。  

但这一配置故障并不是决定性的。全球的根服务器系统，早就通过部署“镜像节点“并以BGP+Anycast的机制保证根服务器的解析，不会因为部分节点失效而出现故障。但是，这一机制的前提是要驾驭好基于BGP的全球互联网路由控制系统。在发现了Cloudflare运行的F根节点出现问题后，如果第一时间通过路由控制（BGP）停止对全球互联网广播服务地址（让错误的F根节点“断网”），那么该节点提供的错误域名解析就不会影响到用户。用户会通过BGP+Anycast机制找到提供正确域名解析的其他F根节点。  

  

延伸：“断网”是多种因素共同作用的复杂现象，要区别断网的层次  

“互联网域名系统”（简称“域名系统”）和“互联网路由控制系统”（简称“路由控制系统”）在全球网络的互联互通中扮演了什么角色？一般来说，互联网的用户终端（电脑、手机等）要想访问一个网页（网站服务器），首先需要通过域名系统的“查询功能”获取网站的IP地址，然后再在根据路由控制系统提供的“寻址功能”将消息（访问请求）发送给网站。类比邮政系统，域名系统类似收件人的地址查询系统，根据收件人的名字反馈收件地址；路由控制系统类似于导航系统，根据收件地址，在实际的道路网中规划处一条最合理的寄送道路。域名系统故障，称为“域名断网”，也即，用户无法查询到通信对象的IP地址；路由控制系统故障，称为“路由断网”，也即用户无法根据通信对象的IP地址发起访问请求。  

域名故障容易导致大面积断网的本质，是因为域名系统是集中层次化管理，单点失效会传导给所有依赖此服务的网络。路由故障容易导致大面积断网，是因为互联网是以“自治域”为单位互联互通，路由控制一旦失效，就是一个自治域网络级别的断网。同时，路由安全的保护机制RPKI（互联网码号资源公钥基础设施）使得路由控制系统也同域名一样，依赖层次化的IP地址认证体系，这是全球互联网的根本运行机制和资源分配体系决定的。  

  

结语：  

由大量异构网络互联互通而成的“全球互联网”，依旧会依赖 互联网域名系统提供的“统一命名空间”和互联网路由系统提供的“统一寻址空间”。此次根服务器故障和滞后的路由控制，导致的是一个“通用顶级域名”无法解析，如果影响的是一个国家代码顶级域名，将会在国际上引来巨大的争议。尽管学术界和工业界已经不断地提出安全解决方案，但在可预见的未来，伴随着他们运行风险不会消失。域名系统和路由系统的安全保障工作，没有终点，只有不断出现的新的更高水平的起点。  



If you get gains，please give a like
