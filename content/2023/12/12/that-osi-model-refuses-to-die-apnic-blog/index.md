---
title: "That OSI model refuses to die OSI模型拒绝消亡"
date: 2023-12-12T12:22:21+08:00
updated: 2023-12-12T12:22:21+08:00
published: 2023-12-12T12:22:21+08:00
taxonomies:
  tags: []
extra:
  source: https://blog.apnic.net/2023/12/12/that-osi-model-refuses-to-die/
  hostname: blog.apnic.net
  author: George Michaelson
  original_title: "That OSI model refuses to die | APNIC Blog"
  original_lang: en
---

![](OSI-seven_layers_FT-555x202.png)

If you work in networking in any capacity and haven’t seen this passionate article yet, I strongly recommend you read it. It’s by Robert Graham, published as a Google doc instance called ‘[OSI Deprogrammer, Re-conceptualizing cyberspace](https://docs.google.com/document/d/1iL0fYmMmariFoSvLd9U5nPVH1uFKC7bvVasUcYq78So/mobilebasic)‘ in September 2023.  

如果您以任何身份从事网络工作，并且还没有看过这篇充满激情的文章，我强烈建议您阅读它。它由 Robert Graham 于 2023 年 9 月作为 Google 文档实例发布，名为“OSI Deprogrammer，重新概念化网络空间”。

Graham’s thesis is that the OSI model has to die. (Note, that’s his surname. He’s one of those people who has two first names as his full name. I’ve never met Robert, so he’s Mr Graham to me.)  

格雷厄姆的论点是 OSI 模型必须消亡。 （注意，这是他的姓氏。他是那些全名有两个名字的人之一。我从未见过罗伯特，所以他对我来说是格雷厄姆先生。）

Traditionally, the OSI model can be thought of as a roadmap for how different parts of a computer network talk to each other. It’s got seven layers, each doing its own thing:  

传统上，OSI 模型可以被视为计算机网络的不同部分如何相互通信的路线图。它有七层，每一层都做自己的事情：

1.  **Physical Layer:** Handles the hardware like cables and network cards.  
    
    物理层：处理电缆和网卡等硬件。
2.  **Data Link Layer:** Makes sure there’s a solid link between connected devices and takes care of errors.  
    
    数据链路层：确保连接的设备之间存在牢固的链路并处理错误。
3.  **Network Layer:** Sends data between different networks.  
    
    网络层：在不同网络之间发送数据。
4.  **Transport Layer:** Manages the flow of data between devices and fixes errors.  
    
    传输层：管理设备之间的数据流并修复错误。
5.  **Session Layer:** Sets up and maintains connections between applications.  
    
    会话层：建立和维护应用程序之间的连接。
6.  **Presentation Layer:** Makes sure data is readable and secure.  
    
    表示层：确保数据可读且安全。
7.  **Application Layer:** Where software applications interact with the network.  
    
    应用层：软件应用程序与网络交互的地方。

Graham describes the OSI model as ‘wrong’ on many levels, including:  

Graham 将 OSI 模型描述为在许多层面上“错误”，包括：

-   It’s based on archaic, mainframe-era concepts of strict functional separation across the procedure call stack, which he feels no longer apply.  
    
    它基于古老的大型机时代跨过程调用堆栈的严格功能分离概念，他认为这种概念不再适用。
-   It lacks subtlety, when in fact functions in a network can cross contexts. Calls can be exposed into the transport layer (TCP, UDP) to affect how the network and link layers (IP versions 4 or 6, and Ethernet or optical networks, respectively) behave concerning the session.  
    
    它缺乏微妙之处，而实际上网络中的功能可以跨上下文。呼叫可以暴露到传输层（TCP、UDP），以影响网络和链路层（分别是 IP 版本 4 或 6，以及以太网或光纤网络）有关会话的行为方式。
-   It imposes a straitjacket on thinking about which parts of a network exchange take place locally, which take place end-to-end, and the respective roles of the client and server throughout this process.  
    
    它对网络交换的哪些部分在本地进行、哪些部分是端到端进行以及在整个过程中客户端和服务器各自的角色进行思考施加了束缚。

I don’t entirely agree with his analysis. I have to admit I am biased having worked in this space myself from 1982 (on early implementations of the OSI Transport classes 1, 2, and 3, written as a finite state machine in PL/1 on a VAX VMS system in the UK) and again in 1985/6 (working in UCL-CS in London, on what became the ISODE system designed and implemented under the aegis of Marshall Rose) and then latterly working in the applications space on the X.400 and X.500 systems (email and directory services respectively). So, I have far too much skin both ‘in the game’ and ‘left on the roadside’ from working in the space repeatedly.  

我不完全同意他的分析。我必须承认，我自己从 1982 年起就在这个领域工作过（关于 OSI 传输类 1、2 和 3 的早期实现，在英国的 VAX VMS 系统上以 PL/1 形式编写为有限状态机） 1985 年 6 月再次（在伦敦的 UCL-CS 工作，在 Marshall Rose 的支持下设计和实现了 ISODE 系统），后来在 X.400 和 X.500 系统的应用领域工作（分别是电子邮件和目录服务）。因此，由于反复在这个空间工作，我在“游戏中”和“留在路边”都有太多的皮肤。

The QUIC protocol, preserving the connection state and operating above UDP, is well described as a session layer. The state it preserves is the session-specific information to make an application agile as the network underneath changes. This is precisely what some of the function(s) identified for the session layer in the OSI model are. Likewise, TLS performs functions to model session layer behaviours.  

QUIC 协议保留连接状态并在 UDP 之上运行，被很好地描述为会话层。它保留的状态是特定于会话的信息，使应用程序在底层网络发生变化时变得敏捷。这正是 OSI 模型中为会话层确定的一些功能。同样，TLS 执行对会话层行为进行建模的功能。

Despite Graham’s assertions, I do not believe the physical-link-network abstractions can be rejected because the concepts are so strongly mapped to how they’re understood. It’s important to remember that a model is not just a book of rules, it’s a basis for mutual understanding and further discussion.  

尽管格雷厄姆如此断言，但我不认为物理链路网络抽象可以被拒绝，因为这些概念与它们的理解方式有着如此强烈的映射。重要的是要记住，模型不仅仅是一本规则书，它还是相互理解和进一步讨论的基础。

However, Graham’s article is well-reasoned, passionately argued, explosive, and fascinating. I recommend anyone interested in the idea of a network, and the way networks are represented as concepts (that he explicitly addresses at length), read and reflect on the ideas it contains.  

然而，格雷厄姆的文章理由充分、论证热情、爆炸性且引人入胜。我建议任何对网络概念以及网络表示为概念的方式（他明确详细阐述的）感兴趣的人阅读并反思其中包含的想法。

Graham states in the abstract:  

格雷厄姆在摘要中指出：

> \[The OSI model\] is not just a lie, but unhelpful. It needs to be removed _everywhere_, except as a historic footnote about 1970s mainframes.  
> 
> \[OSI 模型\] 不仅是一个谎言，而且毫无帮助。除了作为 20 世纪 70 年代大型机的历史脚注之外，它在所有地方都需要删除。
> 
> [OSI Deprogrammer, Re-conceptualizing cyberspace  
> 
> OSI Deprogrammer，重新概念化网络空间](https://docs.google.com/document/d/1iL0fYmMmariFoSvLd9U5nPVH1uFKC7bvVasUcYq78So/edit)

What do you think?  

你怎么认为？
