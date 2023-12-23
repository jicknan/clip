---
title: 'RFC 9518: Centralization, Decentralization, and Internet Standards'
date: 2023-12-21T10:02:24.000Z
updated: 2023-12-21T10:02:24.000Z
published: 2023-12-21T10:02:24.000Z
taxonomies:
  tags:
    - Tech
extra:
  source: https://www.rfc-editor.org/rfc/rfc9518.html
  hostname: www.rfc-editor.org
  author: Mark Nottingham
  original_title: >-
    Centralization, Decentralization, and Internet Standards --- RFC
    9518：集中化、分散化和互联网标准
  original_lang: en

---

## RFC9518 Centralization, Decentralization, and Internet Standards  

集中化、分散化和互联网标准

## [Abstract  摘要](https://www.rfc-editor.org/rfc/rfc9518.html#abstract)

This document discusses aspects of centralization that relate to Internet standards efforts. It argues that, while standards bodies have a limited ability to prevent many forms of centralization, they can still make contributions that assist in the decentralization of the Internet.  

本文档讨论了与互联网标准工作相关的集中化方面。它认为，虽然标准机构阻止多种形式的中心化的能力有限，但他们仍然可以为协助互联网的去中心化做出贡献。

## [Status of This Memo  

本备忘录的状态](https://www.rfc-editor.org/rfc/rfc9518.html#name-status-of-this-memo)

This document is not an Internet Standards Track specification; it is published for informational purposes.  

本文档不是互联网标准跟踪规范；其发布仅供参考。

This is a contribution to the RFC Series, independently of any other RFC stream. The RFC Editor has chosen to publish this document at its discretion and makes no statement about its value for implementation or deployment. Documents approved for publication by the RFC Editor are not candidates for any level of Internet Standard; see Section 2 of RFC 7841.  

这是对 RFC 系列的贡献，独立于任何其他 RFC 流。 RFC 编辑自行决定发布本文档，并且未声明其实施或部署的价值。 RFC 编辑批准发布的文档不是任何级别的互联网标准的候选文档；请参阅 RFC 7841 第 2 节。

Information about the current status of this document, any errata, and how to provide feedback on it may be obtained at [https://www.rfc-editor.org/info/rfc9518](https://www.rfc-editor.org/info/rfc9518).  

有关本文档当前状态、任何勘误表以及如何提供反馈的信息，请访问 https://www.rfc-editor.org/info/rfc9518。

## [Copyright Notice  版权声明](https://www.rfc-editor.org/rfc/rfc9518.html#name-copyright-notice)

Copyright (c) 2023 IETF Trust and the persons identified as the document authors. All rights reserved.  

版权所有 (c) 2023 IETF Trust 和文档作者。版权所有。

This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents ([https://trustee.ietf.org/license-info](https://trustee.ietf.org/license-info)) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document.  

本文档受本文档发布之日生效的 BCP 78 和 IETF Trust 与 IETF 文件相关的法律规定 (https://trustee.ietf.org/license-info) 的约束。请仔细阅读这些文件，因为它们描述了您与本文件相关的权利和限制。

[▲](https://www.rfc-editor.org/rfc/rfc9518.html#)

## [Table of Contents  

目录](https://www.rfc-editor.org/rfc/rfc9518.html#name-table-of-contents)

-   [1](https://www.rfc-editor.org/rfc/rfc9518.html#section-1).  [Introduction](https://www.rfc-editor.org/rfc/rfc9518.html#name-introduction)
    
-   [2](https://www.rfc-editor.org/rfc/rfc9518.html#section-2).  [Centralization](https://www.rfc-editor.org/rfc/rfc9518.html#name-centralization)
    
    -   [2.1](https://www.rfc-editor.org/rfc/rfc9518.html#section-2.1).  [Centralization Can Be Harmful](https://www.rfc-editor.org/rfc/rfc9518.html#name-centralization-can-be-harmf)
        
    -   [2.2](https://www.rfc-editor.org/rfc/rfc9518.html#section-2.2).  [Centralization Can Be Helpful](https://www.rfc-editor.org/rfc/rfc9518.html#name-centralization-can-be-helpf)
        
-   [3](https://www.rfc-editor.org/rfc/rfc9518.html#section-3).  [Decentralization](https://www.rfc-editor.org/rfc/rfc9518.html#name-decentralization)
    
    -   [3.1](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1).  [Decentralization Strategies](https://www.rfc-editor.org/rfc/rfc9518.html#name-decentralization-strategies)
        
        -   [3.1.1](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1.1).  [Federation](https://www.rfc-editor.org/rfc/rfc9518.html#name-federation)
            
        -   [3.1.2](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1.2).  [Distributed Consensus](https://www.rfc-editor.org/rfc/rfc9518.html#name-distributed-consensus)
            
        -   [3.1.3](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1.3).  [Operational Governance](https://www.rfc-editor.org/rfc/rfc9518.html#name-operational-governance)
            
-   [4](https://www.rfc-editor.org/rfc/rfc9518.html#section-4).  [What Can Internet Standards Do?](https://www.rfc-editor.org/rfc/rfc9518.html#name-what-can-internet-standards)
    
    -   [4.1](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.1).  [Bolster Legitimacy](https://www.rfc-editor.org/rfc/rfc9518.html#name-bolster-legitimacy)
        
    -   [4.2](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.2).  [Focus Discussion of Centralization](https://www.rfc-editor.org/rfc/rfc9518.html#name-focus-discussion-of-central)
        
    -   [4.3](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.3).  [Target Proprietary Functions](https://www.rfc-editor.org/rfc/rfc9518.html#name-target-proprietary-function)
        
    -   [4.4](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.4).  [Enable Switching](https://www.rfc-editor.org/rfc/rfc9518.html#name-enable-switching)
        
    -   [4.5](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.5).  [Control Delegation of Power](https://www.rfc-editor.org/rfc/rfc9518.html#name-control-delegation-of-power)
        
    -   [4.6](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.6).  [Enforce Boundaries](https://www.rfc-editor.org/rfc/rfc9518.html#name-enforce-boundaries)
        
    -   [4.7](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.7).  [Consider Extensibility Carefully](https://www.rfc-editor.org/rfc/rfc9518.html#name-consider-extensibility-care)
        
    -   [4.8](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.8).  [Reuse What Works](https://www.rfc-editor.org/rfc/rfc9518.html#name-reuse-what-works)
        
-   [5](https://www.rfc-editor.org/rfc/rfc9518.html#section-5).  [Future Work](https://www.rfc-editor.org/rfc/rfc9518.html#name-future-work)
    
-   [6](https://www.rfc-editor.org/rfc/rfc9518.html#section-6).  [Security Considerations](https://www.rfc-editor.org/rfc/rfc9518.html#name-security-considerations)
    
-   [7](https://www.rfc-editor.org/rfc/rfc9518.html#section-7).  [IANA Considerations](https://www.rfc-editor.org/rfc/rfc9518.html#name-iana-considerations)
    
-   [8](https://www.rfc-editor.org/rfc/rfc9518.html#section-8).  [Informative References](https://www.rfc-editor.org/rfc/rfc9518.html#name-informative-references)
    
-   [](https://www.rfc-editor.org/rfc/rfc9518.html#appendix-A)[Acknowledgements](https://www.rfc-editor.org/rfc/rfc9518.html#name-acknowledgements)
    
-   [](https://www.rfc-editor.org/rfc/rfc9518.html#appendix-B)[Author's Address](https://www.rfc-editor.org/rfc/rfc9518.html#name-authors-address)
    

## [1\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-1) [Introduction](https://www.rfc-editor.org/rfc/rfc9518.html#name-introduction)  一、简介

One of the Internet's defining features is its lack of any single point of technical, political, or economic control. Arguably, that characteristic assisted the Internet's early adoption and broad reach: permission is not required to connect to, deploy an application on, or use the Internet for a particular purpose, so it can meet diverse needs and be deployed in many different environments.  

互联网的显着特征之一是它缺乏任何技术、政治或经济的单点控制。可以说，这一特性有助于互联网的早期采用和广泛覆盖：连接到互联网、在其上部署应用程序或出于特定目的使用互联网不需要许可，因此它可以满足不同的需求并部署在许多不同的环境中。

Although maintaining that state of affairs remains a widely espoused goal, consistently preserving it across the range of services and applications that people see as "the Internet" has proven elusive. Whereas early services like the Network News Transfer Protocol (NNTP) and email had multiple interoperable providers, many contemporary platforms for content and services are operated by single commercial entities without any interoperable alternative -- to the point where some have become so well-known and important to people's experiences that they are commonly mistaken for the Internet itself \[[Komaitis](https://www.rfc-editor.org/rfc/rfc9518.html#FB-INTERNET)\].  

尽管维持这种状态仍然是一个广泛支持的目标，但事实证明，在人们视为“互联网”的各种服务和应用程序中始终如一地保持这种状态是难以实现的。虽然网络新闻传输协议 (NNTP) 和电子邮件等早期服务拥有多个可互操作的提供商，但许多当代内容和服务平台由单一商业实体运营，没有任何可互操作的替代方案 - 以至于有些平台变得如此知名和对人们的体验很重要的是，他们通常被误认为是互联网本身 \[Komaitis\]。

These difficulties call into question what role architectural design -- in particular, that overseen by open standards bodies such as the IETF -- can and should play in controlling centralization of the Internet.  

这些困难让人质疑架构设计（特别是由 IETF 等开放标准机构监督的架构设计）能够而且应该在控制互联网集中化方面发挥什么作用。

This document argues that, while decentralized technical standards may be necessary to avoid centralization of Internet functions, they are not sufficient to achieve that goal because centralization is often caused by non-technical factors outside the control of standards bodies. As a result, standards bodies should not fixate on preventing all forms of centralization; instead, they should take steps to ensure that the specifications they produce enable decentralized operation.  

该文件认为，虽然分散的技术标准可能是避免互联网功能集中化所必需的，但它们不足以实现该目标，因为集中化往往是由标准机构控制之外的非技术因素造成的。因此，标准机构不应专注于防止一切形式的集中化；相反，他们应该采取措施确保他们制定的规范能够实现去中心化操作。

Although this document has been discussed widely in the IETF community (see the Acknowledgements section), it represents the views of the author, not community consensus. Its primary audience is the engineers who design and standardize Internet protocols. Designers of proprietary protocols and applications can benefit from considering these issues, especially if they intend their work to be considered for eventual standardization. Policymakers can use this document to help characterize abuses that involve centralized protocols and applications and evaluate proposed remedies for them.  

尽管本文档已在 IETF 社区中进行了广泛讨论（请参阅致谢部分），但它代表了作者的观点，而不是社区共识。它的主要受众是设计和标准化互联网协议的工程师。专有协议和应用程序的设计者可以从考虑这些问题中受益，特别是如果他们希望他们的工作被考虑用于最终标准化。政策制定者可以使用本文件来帮助描述涉及集中式协议和应用程序的滥用行为，并评估针对这些行为提出的补救措施。

[Section 2](https://www.rfc-editor.org/rfc/rfc9518.html#centralization) defines centralization, explains why it is often undesirable but sometimes beneficial, and surveys how it occurs on the Internet. [Section 3](https://www.rfc-editor.org/rfc/rfc9518.html#decentralization) explores decentralization and highlights some relevant strategies, along with their limitations. [Section 4](https://www.rfc-editor.org/rfc/rfc9518.html#considerations) makes recommendations about the role that Internet standards can play in controlling centralization. [Section 5](https://www.rfc-editor.org/rfc/rfc9518.html#conclude) concludes by identifying areas for future work.  

第 2 节定义了集中化，解释了为什么集中化常常是不可取的，但有时却是有益的，并调查了集中化是如何在互联网上发生的。第 3 节探讨了去中心化并强调了一些相关策略及其局限性。第 4 节就互联网标准在控制集中化方面可以发挥的作用提出了建议。第五部分最后确定了未来工作的领域。

## [2\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-2) [Centralization](https://www.rfc-editor.org/rfc/rfc9518.html#name-centralization)  2. 中心化

In this document, "centralization" is the state of affairs where a single entity or a small group of them can observe, capture, control, or extract rent from the operation or use of an Internet function exclusively.  

在本文件中，“集中化”是指单个实体或一小群实体可以专门观察、捕获、控制互联网功能的运营或使用或从中提取租金的情况。

Here, "entity" could be a person, group, or corporation. An organization might be subject to governance that mitigates centralization risk (see [Section 3.1.3](https://www.rfc-editor.org/rfc/rfc9518.html#multi)), but that organization is still a centralizing entity.  

这里，“实体”可以是个人、团体或公司。一个组织可能会受到减轻中心化风险的治理（参见第 3.1.3 节），但该组织仍然是一个中心化实体。

"Internet function" is used broadly in this document. Most directly, it might be an enabling protocol already defined by standards, such as IP \[[RFC791](https://www.rfc-editor.org/rfc/rfc9518.html#RFC791)\], BGP \[[RFC4271](https://www.rfc-editor.org/rfc/rfc9518.html#RFC4271)\], TCP \[[RFC9293](https://www.rfc-editor.org/rfc/rfc9518.html#RFC9293)\], or HTTP \[[HTTP](https://www.rfc-editor.org/rfc/rfc9518.html#RFC9110)\]. It might also be a proposal for a new enabling protocol or an extension to an existing one.  

本文档中广泛使用“Internet 功能”。最直接的是，它可能是已由标准定义的启用协议，例如 IP \[RFC791\]、BGP \[RFC4271\]、TCP \[RFC9293\] 或 HTTP \[HTTP\]。它也可能是一项新的启用协议的提案或现有协议的扩展。

Because people's experience of the Internet are not limited to standards-defined protocols and applications, this document also considers centralization in functions built on top of standards -- for example, social networking, file sharing, financial services, and news dissemination. Likewise, the networking equipment, hardware, operating systems, and software that act as enabling technologies for the Internet can also impact centralization. The supply of Internet connectivity to end users in a particular area or situation can exhibit centralization, as can the supply of transit between networks (so called "Tier 1" networks).  

由于人们对互联网的体验并不局限于标准定义的协议和应用程序，因此本文档还考虑了基于标准构建的功能的集中化，例如社交网络、文件共享、金融服务和新闻传播。同样，作为互联网支持技术的网络设备、硬件、操作系统和软件也会影响集中化。向特定区域或情况下的最终用户提供互联网连接可以表现出集中化，网络（所谓的“第一层”网络）之间的传输供应也可以表现出集中化。

This definition of centralization does not capture all types of centralization. Notably, technical centralization (for example, where a machine or network link is a single point of failure) is relatively well understood by engineers; it can be mitigated, typically by distributing a function across multiple components. As we will see, such techniques might address that type of centralization while failing to prevent control of the function falling into few hands. A failure because of a cut cable, power outage, or failed server is well understood by the technical community but is qualitatively different from the issues encountered when a core Internet function has a gatekeeper.  

这种集中化的定义并未涵盖所有类型的集中化。值得注意的是，工程师对技术集中化（例如，机器或网络链路是单点故障）的理解相对较好；通常可以通过将功能分布到多个组件来减轻这种影响。正如我们将看到的，此类技术可能会解决这种类型的集中化问题，但无法防止功能的控制落入少数人手中。由于电缆切断、断电或服务器故障而导致的故障已为技术界所熟知，但与核心互联网功能有网守时遇到的问题有本质上的不同。

Likewise, political centralization (for example, where a country is able to control how a function is supplied across the whole Internet) is equally concerning but is not considered in depth here.  

同样，政治集中化（例如，一个国家能够控制整个互联网上功能的提供方式）同样令人担忧，但这里没有深入考虑。

Even when centralization is not currently present in a function, some conditions make it more likely that centralization will emerge in the future. This document uses "centralization risk" to characterize that possibility.  

即使某个职能目前不存在集中化，某些条件也使得未来更有可能出现集中化。本文档使用“中心化风险”来描述这种可能性。

### [2.1.](https://www.rfc-editor.org/rfc/rfc9518.html#section-2.1) [Centralization Can Be Harmful](https://www.rfc-editor.org/rfc/rfc9518.html#name-centralization-can-be-harmf)  

2.1.中心化可能是有害的

Many engineers who participate in Internet standards efforts have an inclination to prevent and counteract centralization because they see the Internet's history and architecture as incompatible with it. As a "large, heterogeneous collection of interconnected systems" \[[BCP95](https://www.rfc-editor.org/rfc/rfc9518.html#BCP95)\] the Internet is often characterized as a "network of networks" whose operators relate as peers that agree to facilitate communication rather than experiencing coercion or requiring subservience to others' requirements. This focus on independence of action is prevalent in the Internet's design -- for example, in the concept of an "autonomous system".  

许多参与互联网标准工作的工程师倾向于防止和抵制中心化，因为他们认为互联网的历史和架构与其不兼容。作为一个“大型、异构的互连系统集合”\[BCP95\]，互联网通常被描述为“网络的网络”，其运营商作为对等体相互联系，同意促进通信，而不是遭受胁迫或要求屈从于他人的要求。这种对行动独立性的关注在互联网的设计中很普遍——例如，在“自治系统”的概念中。

Reluctance to countenance centralization is also rooted in the many potentially damaging effects that have been associated with it, including:  

不愿支持集权的根源还在于与之相关的许多潜在破坏性影响，包括：

-   _Power Imbalance_: When a third party has unavoidable access to communications, they gain informational and positional advantages that allow observation of behavior (the "panopticon effect") and shaping or even denial of behavior (the "chokepoint effect"): capabilities that those parties (or the states that have authority over them) can use for coercive ends \[[FarrellH](https://www.rfc-editor.org/rfc/rfc9518.html#INTERDEPENDENCE)\] or even to disrupt society itself. Just as \[[Madison](https://www.rfc-editor.org/rfc/rfc9518.html#FEDERALIST-51)\] describes good governance of the US states, good governance of the Internet requires that power over any function not be consolidated in one place without appropriate checks and balances.  
    
    权力不平衡：当第三方不可避免地获得通信权限时，他们就获得了信息和位置优势，从而可以观察行为（“圆形监狱效应”）并塑造甚至否认行为（“阻塞点效应”）：这些各方的能力（或对它们拥有权力的国家）可以用于强制目的\[FarrellH\]，甚至扰乱社会本身。正如\[麦迪逊\]描述美国各州的良好治理一样，互联网的良好治理要求在没有适当制衡的情况下，任何职能的权力都不能集中在一处。
    
-   _Limits on Innovation_: A party with the ability to control communication can preclude the possibility of "permissionless innovation", i.e., the ability to deploy new, unforeseen applications without requiring coordination with parties other than those you are communicating with.  
    
    创新的限制：有能力控制通信的一方可以排除“未经许可的创新”的可能性，即部署新的、不可预见的应用程序的能力，而无需与您通信的各方以外的各方进行协调。
    
-   _Constraints on Competition_: The Internet and its users benefit from robust competition when applications and services are available from many providers, especially when those users can build their own applications and services based upon interoperable standards. When a centralized service or platform must be used because no substitutes are suitable, it effectively becomes an essential facility, which opens the door to abuse of power.  
    
    竞争的限制：当许多提供商都提供应用程序和服务时，特别是当这些用户可以基于互操作标准构建自己的应用程序和服务时，互联网及其用户将从激烈的竞争中受益。当由于没有合适的替代品而必须使用集中式服务或平台时，它实际上就成为一种必要的设施，从而为滥用权力打开了大门。
    
-   _Reduced Availability_: Availability of the Internet (and applications and services built upon it) improves when there are many ways to obtain access. While service availability can benefit from the focused attention of a large centralized provider, that provider's failure can have a disproportionate impact on availability.  
    
    可用性降低：当有多种访问方式时，互联网（以及基于其构建的应用程序和服务）的可用性会提高。虽然服务可用性可以受益于大型集中式提供商的集中关注，但该提供商的故障可能会对可用性产生不成比例的影响。
    
-   _Monoculture_: The scale available to a centralized provider can magnify minor flaws in features to a degree that can have broad consequences. For example, a single codebase for routers elevates the impact of a bug or vulnerability; a single recommendation algorithm for content can have severe social impact. Diversity in functions' implementations leads to a more robust outcome when viewed systemically because "progress is the outcome of a trial-and-error evolutionary process of many agents interacting freely" \[[Aligia](https://www.rfc-editor.org/rfc/rfc9518.html#POLYCENTRIC)\].  
    
    单一文化：中心化提供商可用的规模可能会放大功能中的小缺陷，达到可能产生广泛后果的程度。例如，路由器的单一代码库会增加错误或漏洞的影响；单一的内容推荐算法可能会产生严重的社会影响。从系统角度来看，功能实现的多样性会带来更稳健的结果，因为“进步是许多代理自由交互的试错进化过程的结果”\[Aligia\]。
    
-   _Self-Reinforcement_: As widely noted (e.g., see \[[Abrahamson](https://www.rfc-editor.org/rfc/rfc9518.html#ACCESS)\]), a centralized provider's access to data allows it the opportunity to make improvements to its offerings while denying such access to others.  
    
    自我强化：正如广泛指出的那样（例如，参见\[亚伯拉罕森\]），中心化提供商对数据的访问使其有机会改进其产品，同时拒绝其他人进行此类访问。
    

The relationship between these harms and centralization is often complex. It is not always the case that centralization will lead to them; when it does, there is not always a direct and simple trade-off.  

这些危害与中心化之间的关系往往很复杂。集中化并不总是会带来这些结果；当它发生时，并不总是存在直接和简单的权衡。

For example, consider the relationship between centralization and availability. A centrally operated system might be more available because of the resources available to a larger operator, but their size creates greater impact when a fault is encountered. Decentralized systems can be more resilient in the face of some forms of failure but less so in other ways; for example, they may be less able to react to systemic issues and might be exposed to a larger collection of security vulnerabilities in total. As such, it cannot be said that centralization reduces availability in all cases: nor does it improve it in all cases.  

例如，考虑集中化和可用性之间的关系。集中操作系统可能更可用，因为更大的操作员可以使用资源，但它们的规模在遇到故障时会产生更大的影响。去中心化系统在面对某些形式的故障时可能更具弹性，但在其他方面则较差；例如，他们对系统性问题的反应能力可能较差，并且可能面临更多的安全漏洞。因此，不能说集中化在所有情况下都会降低可用性：也不能说在所有情况下都会提高可用性。

This tension can be seen in areas like the cloud and mobile Internet access. If a popular cloud-hosting provider were to become unavailable (whether for technical or other reasons), many Internet experiences might be disrupted (especially due to the multiple dependencies that a modern website often has; see \[[Kashaf](https://www.rfc-editor.org/rfc/rfc9518.html#DEPENDENCIES)\]). Likewise, a large mobile Internet access provider might have an outage that affects hundreds of thousands of its users or more -- just as previous issues at large telephone companies precipitated widespread outages \[[PHONE](https://www.rfc-editor.org/rfc/rfc9518.html#PHONE)\].  

这种紧张关系可以在云和移动互联网接入等领域看到。如果流行的云托管提供商不可用（无论是出于技术原因还是其他原因），许多互联网体验可能会受到干扰（特别是由于现代网站经常具有的多重依赖关系；请参阅 \[Kashaf\]）。同样，大型移动互联网接入提供商可能会出现影响数十万甚至更多用户的中断，就像大型电话公司之前的问题导致大范围的中断一样 \[PHONE\]。

In both cases, the services are not technically centralized; these operators have strong incentives to have multiple redundancies in place and use various techniques to mitigate the risk of any one component failing. However, they generally do rely upon a single codebase, a limited selection of hardware, a unified control plane, and a uniform administrative practice: each of which might precipitate a widespread failure.  

在这两种情况下，服务在技术上都不是集中的；这些运营商有强烈的动机采取多种冗余措施，并使用各种技术来降低任何一个组件发生故障的风险。然而，它们通常依赖于单一的代码库、有限的硬件选择、统一的控制平面和统一的管理实践：每一个都可能导致广泛的失败。

If there were only one provider for these services (like the telephone networks of old), they would easily be considered to be centralized in a way that has significant impact upon availability. However, many cloud providers offer similar services. In most places, there are multiple mobile operators available. That weakens the argument that there is a link between centralization and their availability because the function's users can switch to other providers or use more than one provider simultaneously; see [Section 4.4](https://www.rfc-editor.org/rfc/rfc9518.html#switch).  

如果这些服务只有一个提供商（如旧的电话网络），那么它们很容易被认为是以一种对可用性产生重大影响的方式集中化的。然而，许多云提供商提供类似的服务。在大多数地方，都有多个移动运营商可供选择。这削弱了集中化与其可用性之间存在联系的论点，因为该功能的用户可以切换到其他提供商或同时使用多个提供商；参见第 4.4 节。

These circumstances suggest one area of inquiry when considering the relationship between centralization and availability of a given function: what barriers are there to switching to other providers (thereby making any disruptions temporary and manageable) or to using multiple providers simultaneously (to mask the failure of a single operator)?  

这些情况表明，在考虑某一特定功能的集中化和可用性之间的关系时，需要考虑一个问题：切换到其他提供商（从而使任何中断暂时且易于管理）或同时使用多个提供商（以掩盖服务的失败）存在哪些障碍？单个操作员）？

Another example of the need for nuance can be seen when evaluating competitive constraints. While making provision of various Internet functions more competitive may be a motivation for many engineers, only courts (and sometimes regulators) have the authority to define a relevant market and determine that a behavior is anticompetitive. In particular, market concentration does not always indicate competition issues; therefore, what might be considered undesirable centralization by the technical community might not attract competition regulation.  

在评估竞争约束时可以看到需要细微差别的另一个例子。虽然使各种互联网功能的提供更具竞争力可能是许多工程师的动机，但只有法院（有时是监管机构）有权定义相关市场并确定某种行为是反竞争的。特别是，市场集中度并不总是表明存在竞争问题；因此，技术界可能认为不受欢迎的集中化可能不会引起竞争监管。

### [2.2.](https://www.rfc-editor.org/rfc/rfc9518.html#section-2.2) [Centralization Can Be Helpful](https://www.rfc-editor.org/rfc/rfc9518.html#name-centralization-can-be-helpf)  

2.2.集中化可能会有帮助

The potential damaging effects of centralization listed above are widely appreciated. Less widely explored is the reliance on centralization by some protocols and applications to deliver their functionality.  

上述集中化的潜在破坏性影响已被广泛认识到。较少广泛探讨的是某些协议和应用程序依赖集中化来提供其功能。

Centralization is often present due to technical necessity. For example, a single globally coordinated "source of truth" is by nature centralized -- such as in the root zone of the Domain Name System (DNS), which allows human-friendly naming to be converted into network addresses in a globally consistent fashion.  

由于技术上的需要，集中化常常存在。例如，单个全球协调的“事实来源”本质上是集中的——例如在域名系统（DNS）的根区域中，它允许以全球一致的方式将人性化的命名转换为网络地址。

Or, consider IP address allocation. Internet routing requires addresses to be allocated uniquely, but if a single government or company were to capture the addressing function, the entire Internet would be at risk of abuse by that entity. Similarly, the Web's trust model requires a Certificate Authority (CA) to serve as the root of trust for communication between browsers and servers, bringing the centralization risk, which needs to be considered in the design of that system.  

或者，考虑 IP 地址分配。互联网路由要求地址被唯一分配，但如果单个政府或公司夺取寻址功能，整个互联网将面临被该实体滥用的风险。同样，Web的信任模型需要证书颁发机构（CA）作为浏览器和服务器之间通信的信任根，带来中心化风险，需要在系统设计时考虑。

Protocols that need to solve the "rendezvous problem" to coordinate communication between two parties who are not in direct contact also require centralization. For example, chat protocols need to coordinate communication between two parties that wish to talk; while the actual communication can be direct between them (so long as the protocol facilitates that), the endpoints' mutual discovery typically requires a third party at some point. From the perspective of those two users, the rendezvous function has a centralization risk.  

需要解决“交会问题”以协调不直接接触的两方之间的通信的协议也需要中心化。例如，聊天协议需要协调希望交谈的两方之间的通信；虽然实际通信可以在它们之间直接进行（只要协议有助于实现），但端点的相互发现通常在某些时候需要第三方。从这两个用户的角度来看，集合点功能存在中心化风险。

Even when not strictly necessary, centralization can create benefits for a function's users and for society.  

即使不是绝对必要，集中化也可以为功能的用户和社会创造利益。

For example, it has long been recognized that the efficiencies that come with economies of scale can lead to concentration \[[Demsetz](https://www.rfc-editor.org/rfc/rfc9518.html#SCALE)\]. Those efficiencies can be passed on to users as higher quality products and lower costs, and they might even enable provision of a function that was not viable at smaller scale.  

例如，人们早已认识到规模经济带来的效率可以导致集中度\[Demsetz\]。这些效率可以作为更高质量的产品和更低的成本传递给用户，甚至可以提供在较小规模下不可行的功能。

Complex and risky functions like financial services (e.g., credit card processing) are often concentrated into a few specialized organizations where they can receive the focused attention and expertise that they require.  

金融服务（例如信用卡处理）等复杂且有风险的职能通常集中在几个专门的组织中，在那里它们可以获得所需的集中关注和专业知识。

Centralization can also provide an opportunity for beneficial controls to be imposed. \[[Schneider2](https://www.rfc-editor.org/rfc/rfc9518.html#AMBITION)\] notes that "centralized structures can have virtues, such as enabling publics to focus their limited attention for oversight, or forming a power bloc capable of challenging less-accountable blocs that might emerge. Centralized structures that have earned widespread respect in recent centuries -- including governments, corporations, and nonprofit organizations -- have done so in no small part because of the intentional design that went into those structures".  

集中化还可以提供实施有益控制的机会。 \[ Schneider2\] 指出，“中心化结构具有优点，例如使公众能够将有限的注意力集中在监督上，或者形成一个能够挑战可能出现的不负责任集团的权力集团。近几个世纪以来，中心化结构赢得了广泛的尊重——包括政府、企业和非营利组织——这样做在很大程度上是因为这些结构的有意设计”。

This can be seen when a function requires governance to realize common goals and protect minority interests. For example, content moderation functions impose community values that many see as a benefit. Of course, they can also be viewed as a choke point where inappropriate controls are able to be imposed if that governance mechanism has inadequate oversight, transparency, or accountability.  

当一项职能需要治理来实现共同目标并保护少数利益时，就可以看出这一点。例如，内容审核功能强加了许多人视为好处的社区价值观。当然，如果治理机制的监督、透明度或问责制不足，它们也可以被视为一个瓶颈，可以实施不适当的控制。

Ultimately, deciding when centralization is beneficial is a judgment call. Some protocols cannot operate without a centralized function; others might be significantly enhanced for certain use cases if a function is centralized or might merely be more efficient. Although, in general, centralization is most concerning when it is not broadly held to be necessary or beneficial; when it has no checks, balances, or other mechanisms of accountability; when it selects "favorites" that are difficult (or impossible) to displace; and when it threatens the architectural features that make the Internet successful.  

最终，决定集中化何时有益取决于判断。有些协议如果没有中心化功能就无法运行；如果某个功能是集中的或者可能只是更高效，那么其他功能可能会在某些用例中得到显着增强。尽管一般来说，当集中化没有被广泛认为是必要或有益的时候，它才是最令人担忧的；当它没有制衡、平衡或其他问责机制时；当它选择难以（或不可能）取代的“最爱”时；当它威胁到使互联网成功的架构特征时。

## [3\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-3) [Decentralization](https://www.rfc-editor.org/rfc/rfc9518.html#name-decentralization) 3\. 去中心化

While the term "decentralization" has a long history of use in economics, politics, religion, and international development, \[[Baran](https://www.rfc-editor.org/rfc/rfc9518.html#RAND)\] gave one of the first definitions relevant to computer networking as a condition when "complete reliance upon a single point is not always required".  

虽然“去中心化”一词在经济、政治、宗教和国际发展中有着悠久的使用历史，但 \[Baran\] 给出了与计算机网络相关的第一个定义，作为“完全依赖于一个点并不总是”的条件。必需的”。

Such technical centralization (while not a trivial topic) is relatively well understood. Avoiding all forms of centralization -- including non-technical ones -- using only technical tools (like protocol design) is considerably more difficult. Several issues are encountered.  

这种技术集中化（虽然不是一个微不足道的话题）是相对容易理解的。仅使用技术工具（如协议设计）来避免所有形式的中心化（包括非技术性中心化）要困难得多。遇到了几个问题。

First, and most critically, technical decentralization measures have, at best, limited effects on non-technical forms of centralization. Or, per \[[Schneider1](https://www.rfc-editor.org/rfc/rfc9518.html#SCHNEIDER)\], "decentralized technology alone does not guarantee decentralized outcomes". As explored below in [Section 3.1](https://www.rfc-editor.org/rfc/rfc9518.html#techniques), technical measures are better characterized as necessary but insufficient to achieve full decentralization of a function.  

首先，也是最关键的一点是，技术性分权措施对非技术形式的集权的影响充其量是有限的。或者，根据 \[Schneider1\]，“单独的去中心化技术并不能保证去中心化的结果”。正如下面第 3.1 节所探讨的，技术措施被更好地描述为必要但不足以实现职能的完全权力下放。

Second, decentralizing a function requires overcoming challenges that centralized ones do not face. A decentralized function can be more difficult to adapt to user needs (for example, introducing new features or experimenting with user interfaces) because doing so often requires coordination between many different actors \[[Marlinspike](https://www.rfc-editor.org/rfc/rfc9518.html#MOXIE)\]. Economies of scale are more available to centralized functions, as is data that can be used to refine a function's design. All of these factors make centralized solutions more attractive to service providers and, in some cases, can make a decentralized solution uneconomic.  

其次，去中心化功能需要克服中心化功能不会面临的挑战。去中心化功能可能更难以适应用户需求（例如，引入新功能或尝试用户界面），因为这样做通常需要许多不同参与者之间的协调\[Marlinspike\]。规模经济对于集中式职能来说更容易实现，就像可用于完善职能设计的数据一样。所有这些因素使得集中式解决方案对服务提供商更具吸引力，并且在某些情况下可能使分散式解决方案变得不经济。

Third, identifying which aspects of a function to decentralize can be difficult, both because there are often many interactions between different types and sources of centralization and because centralization sometimes only becomes clear after the function is deployed at scale. Efforts to decentralize often have the effect of merely shifting centralization to a different place -- for example, in its governance, implementation, deployment, or ancillary functions.  

第三，确定功能的哪些方面需要去中心化可能很困难，因为不同类型和中心化来源之间经常存在许多相互作用，而且中心化有时只有在功能大规模部署后才会变得清晰。去中心化的努力往往只会产生将集权转移到其他地方的效果——例如，在治理、实施、部署或辅助职能方面。

For example, the Web was envisioned and widely held to be a decentralizing force in its early life. Its potential as an enabler of centralization only became apparent when large websites successfully leveraged network effects (and secured legal prohibitions against interoperability, thus increasing switching costs; see \[[Doctorow](https://www.rfc-editor.org/rfc/rfc9518.html#ADVERSARIAL)\]) to achieve dominance of social networking, marketplaces, and similar functions.  

例如，网络在其早期就被设想并被广泛认为是一种去中心化的力量。只有当大型网站成功利用网络效应（并确保法律禁止互操作性，从而增加转换成本；参见 \[Doctorow\]）来实现社交网络、市场和类似功能的主导地位时，它作为中心化推动者的潜力才变得明显。

Fourth, different parties might have good-faith differences on what "sufficiently decentralized" means based upon their beliefs, perceptions, and goals. Just as centralization is a continuum, so is decentralization, and not everyone agrees what the "right" level or type is, how to weigh different forms of centralization against each other, or how to weigh potential centralization against other architectural goals (such as security or privacy).  

第四，不同各方可能基于各自的信念、看法和目标，对“充分去中心化”的含义存在真诚的分歧。正如集中化是一个连续体一样，去中心化也是如此，并不是每个人都同意什么是“正确”的级别或类型，如何权衡不同形式的集中化，或者如何权衡潜在的集中化与其他架构目标（例如安全性）或隐私）。

These tensions can be seen, for example, in the DNS. While some aspects of the system are decentralized -- for example, the distribution of the lookup function to local servers that users have the option to override -- an essentially centralized aspect of the DNS is its operation as a name space: a single global "source of truth" with inherent (if beneficial) centralization in its management. ICANN mitigates the associated risk through multi-stakeholder governance (see [Section 3.1.3](https://www.rfc-editor.org/rfc/rfc9518.html#multi)). While many believe that this arrangement is sufficient and might even have desirable qualities (such as the ability to impose community standards over the operation of the name space), others reject ICANN's oversight of the DNS as illegitimate, favoring decentralization based upon distributed consensus protocols rather than human governance \[[Musiani](https://www.rfc-editor.org/rfc/rfc9518.html#MUSIANI)\].  

这些紧张关系可以在 DNS 等领域看到。虽然系统的某些方面是分散的（例如，将查找功能分配给用户可以选择覆盖的本地服务器），但 DNS 的一个本质上集中的方面是其作为名称空间的操作：单个全局“事实来源”，其管理具有固有的（如果有益的话）集中化。 ICANN 通过多利益相关方治理来降低相关风险（请参阅第 3.1.3 节）。虽然许多人认为这种安排已经足够，甚至可能具有理想的品质（例如对名称空间的运营实施社区标准的能力），但其他人则认为 ICANN 对 DNS 的监督是非法的，赞成基于分布式共识协议的权力下放，而不是而非人类治理\[Musiani\]。

Fifth, decentralization unavoidably involves adjustments to the power relationships between protocol participants, especially when it opens up the possibility of centralization elsewhere. As \[[Schneider2](https://www.rfc-editor.org/rfc/rfc9518.html#AMBITION)\] notes, decentralization "appears to operate as a rhetorical strategy that directs attention toward some aspects of a proposed social order and away from others", so "we cannot accept technology as a substitute for taking social, cultural, and political considerations seriously". Or, more bluntly, "without governance mechanisms in place, nodes may collude, people may lie to each other, markets can be rigged, and there can be significant cost to people entering and exiting markets" \[[Bodo](https://www.rfc-editor.org/rfc/rfc9518.html#PERSPECTIVE)\].  

第五，去中心化不可避免地涉及协议参与者之间权力关系的调整，特别是当它开启了其他地方中心化的可能性时。正如 \[Schneider2\] 指出的那样，权力下放“似乎是一种修辞策略，将注意力引向所提议的社会秩序的某些方面，而不是其他方面”，因此“我们不能接受技术作为社会、文化和政治考虑的替代品”严重地”。或者，更直白地说，“如果没有适当的治理机制，节点可能会串通一气，人们可能会互相撒谎，市场可能被操纵，人们进入和退出市场可能会付出巨大的代价”\[Bodo\]。

For example, while blockchain-based cryptocurrencies purport to address the centralization inherent in existing currencies through technical means, many exhibit considerable concentration of power due to voting/mining power, distribution of funds, and diversity of the codebase \[[Makarov](https://www.rfc-editor.org/rfc/rfc9518.html#BITCOIN)\]. Overreliance on technical measures also brings an opportunity for latent, informal power structures that have their own risks -- including centralization \[[Freeman](https://www.rfc-editor.org/rfc/rfc9518.html#STRUCTURELESS)\].  

例如，虽然基于区块链的加密货币旨在通过技术手段解决现有货币固有的中心化问题，但由于投票/采矿权、资金分配和代码库的多样性，许多货币表现出相当大的权力集中度\[Makarov\]。对技术措施的过度依赖也为潜在的、非正式的权力结构带来了机会，而这些权力结构也有其自身的风险——包括集权化\[弗里曼\]。

Overall, decentralizing a function requires considerable work, is inherently political, and involves a large degree of uncertainty about the outcome. If one considers decentralization as a larger social goal (in the spirit of how the term is used in other, non-computing contexts), merely rearranging technical functions may lead to frustration. "A distributed network does not automatically yield an egalitarian, equitable or just social, economic, political landscape" \[[Bodo](https://www.rfc-editor.org/rfc/rfc9518.html#PERSPECTIVE)\].  

总体而言，职能下放需要大量工作，本质上是政治性的，并且结果存在很大程度的不确定性。如果人们将去中心化视为一个更大的社会目标（本着该术语在其他非计算环境中的使用方式的精神），那么仅仅重新安排技术功能可能会导致挫败感。 “分布式网络不会自动产生平等、公平或公正的社会、经济、政治格局”\[Bodo\]。

### [3.1.](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1) [Decentralization Strategies](https://www.rfc-editor.org/rfc/rfc9518.html#name-decentralization-strategies)  

3.1.权力下放策略

This section examines some common strategies that are employed to decentralize Internet functions and discusses their limitations.  

本节研究了一些用于分散互联网功能的常见策略并讨论了它们的局限性。

#### [3.1.1.](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1.1) [Federation](https://www.rfc-editor.org/rfc/rfc9518.html#name-federation)  3.1.1.联邦

Protocol designers often attempt to address centralization through federation, i.e., designing a function in a way that uses independent instances that maintain connectivity and interoperability to provide a single cohesive service. Federation promises to allow users to choose the instance they associate with and accommodates substitution of one instance for another, lowering switching costs.  

协议设计者经常尝试通过联合来解决集中化问题，即以使用独立实例的方式设计功能，以保持连接性和互操作性以提供单一的内聚服务。联合承诺允许用户选择与其关联的实例，并允许用一个实例替换另一个实例，从而降低切换成本。

However, federation alone is insufficient to prevent or mitigate centralization of a function because non-technical factors can create pressure to use a central solution.  

然而，仅联合不足以防止或减轻功能的集中化，因为非技术因素可能会产生使用集中解决方案的压力。

For example, the email suite of protocols needs to route messages to a user even when that user changes network locations or becomes disconnected for a long period. To facilitate this, SMTP \[[RFC5321](https://www.rfc-editor.org/rfc/rfc9518.html#RFC5321)\] defines a specific role for routing users' messages, the Message Transfer Agent (MTA). By allowing anyone to deploy an MTA and defining rules for interconnecting them, the protocol avoids a requirement for a single central server in that role; users can (and often do) choose to delegate it to someone else or they can run their own MTA.  

例如，电子邮件协议套件需要将消息路由到用户，即使该用户更改网络位置或长时间断开连接也是如此。为了实现这一点，SMTP \[RFC5321\] 定义了一个用于路由用户消息的特定角色，即消息传输代理 (MTA)。通过允许任何人部署 MTA 并定义互连规则，该协议避免了对单一中央服务器担任该角色的要求；用户可以（而且经常）选择将其委托给其他人，也可以运行自己的 MTA。

Running one's own MTA has become considerably more onerous over the years due, in part, to the increasingly complex mechanisms introduced to fight unwanted commercial emails. These costs create an incentive to delegate one's MTA to a third party who has the appropriate expertise and resources, contributing to market concentration \[[Holzbauer](https://www.rfc-editor.org/rfc/rfc9518.html#DELIVERABILITY)\].  

多年来，运行自己的 MTA 变得更加繁重，部分原因是为了对抗不需要的商业电子邮件而引入的机制日益复杂。这些成本促使人们将 MTA 委托给拥有适当专业知识和资源的第三方，从而促进市场集中度 \[Holzbauer\]。

Additionally, the measures that MTAs use to identify unwanted commercial emails are often site specific. Because large MTAs handle so many more addresses, there is a power imbalance with smaller ones; if a large MTA decides that email from a small one is unwanted, there is significant impact on its ability to function and little recourse.  

此外，MTA 用于识别不需要的商业电子邮件的措施通常是特定于站点的。由于大型 MTA 处理更多地址，因此与较小的 MTA 之间存在功率不平衡；如果大型 MTA 认为不需要小型 MTA 发送的电子邮件，则会对其运作能力产生重大影响，并且追索权很少。

The Extensible Messaging and Presence Protocol (XMPP) \[[RFC6120](https://www.rfc-editor.org/rfc/rfc9518.html#RFC6120)\] is a chat protocol that demonstrates another issue with federation: the voluntary nature of technical standards.  

可扩展消息传递和状态协议 (XMPP) \[RFC6120\] 是一种聊天协议，它演示了联合的另一个问题：技术标准的自愿性。

Like email, XMPP is federated to facilitate the rendezvous of users from different systems - if they allow it. While some XMPP deployments do support truly federated messaging (i.e., a person using service A can interoperably chat with someone using service B), many of the largest do not. Because federation is voluntary, some operators captured their users into a single service, deliberately denying them the benefits of global interoperability.  

与电子邮件一样，XMPP 是联合的，以便于来自不同系统的用户会合 - 如果他们允许的话。虽然某些 XMPP 部署确实支持真正的联合消息传递（即，使用服务 A 的人可以与使用服务 B 的人进行互操作聊天），但许多最大的部署并不支持。由于联盟是自愿的，一些运营商将其用户捕获到单一服务中，故意拒绝他们享受全球互操作性的好处。

The examples above illustrate that, while federation can create the conditions necessary for a function to be decentralized, it does not guarantee that outcome.  

上面的例子说明，虽然联邦可以为功能去中心化创造必要的条件，但它并不能保证结果。

#### [3.1.2.](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1.2) [Distributed Consensus](https://www.rfc-editor.org/rfc/rfc9518.html#name-distributed-consensus)  

3.1.2.分布式共识

Increasingly, distributed consensus technologies (such as a blockchain) are touted as a solution to centralization. A complete survey of this rapidly changing area is beyond the scope of this document, but we can generalize about its properties.  

分布式共识技术（例如区块链）越来越多地被吹捧为集中化的解决方案。对这个快速变化的领域的全面调查超出了本文档的范围，但我们可以概括其属性。

These techniques typically guarantee proper performance of a function using cryptographic techniques (often, an append-only transaction ledger). They attempt to avoid centralization by distributing the operation of a function to members of a sometimes large pool of protocol participants. Usually, the participants are unknown and untrusted, and a particular task's assignment to a node for handling cannot be predicted or controlled.  

这些技术通常使用加密技术（通常是仅附加交易分类账）来保证函数的正确性能。他们试图通过将功能的操作分配给有时大量协议参与者的成员来避免集中化。通常，参与者是未知的且不受信任的，并且无法预测或控制将特定任务分配给节点进行处理。

Sybil attacks (where a party or coordinated parties cheaply create enough protocol participants to affect how consensus is judged) are a major concern for these protocols because they would have the effect of concentrating power into the hands of the attacker. Therefore, they encourage diversity in the pool of participants using indirect techniques, such as proof-of-work (where each participant has to show a significant consumption of resources) or proof-of-stake (where each participant has some other incentive to execute correctly).  

Sybil 攻击（一方或协调各方廉价地创建足够的协议参与者来影响共识的判断方式）是这些协议的主要关注点，因为它们会将权力集中到攻击者手中。因此，他们使用间接技术鼓励参与者池的多样性，例如工作量证明（每个参与者必须证明资源的大量消耗）或权益证明（每个参与者都有其他执行动机）正确）。

While these measures can be effective in decentralizing a function's operation, other aspects of its provision can still be centralized: for example, governance of its design, creation of shared implementations, and documentation of wire protocols. That need for coordination is an avenue for centralization even when the function's operation remains decentralized. For example, the Ethereum "merge" demonstrated that the blockchain could address environmental concerns but only through coordinated community effort and governance: coordination that was benign in most eyes but, nevertheless, centralized \[[ETHEREUM](https://www.rfc-editor.org/rfc/rfc9518.html#ETHEREUM)\].  

虽然这些措施可以有效地分散功能的操作，但其提供的其他方面仍然可以集中：例如，其设计的治理、共享实现的创建以及有线协议的文档。即使职能的运作仍然是分散的，这种协调的需求也是一种集中化的途径。例如，以太坊“合并”表明区块链可以解决环境问题，但只能通过协调的社区努力和治理：在大多数人看来，协调是良性的，但无论如何，它是中心化的\[以太坊\]。

Furthermore, a protocol or an application composed of many functions can use distributed consensus for some but still be centralized elsewhere -- either because those other functions cannot be decentralized (most commonly, rendezvous and global naming; see [Section 2.2](https://www.rfc-editor.org/rfc/rfc9518.html#necessary)) or because the designer has chosen not to because of the associated costs and lost opportunities.  

此外，由许多功能组成的协议或应用程序可以对某些功能使用分布式共识，但仍然在其他地方集中 - 要么是因为这些其他功能不能去中心化（最常见的是集合点和全局命名；请参见第 2.2 节），要么是因为设计者选择不这样做是因为相关成本和失去机会。

These potential shortcomings do not rule out the use of distributed consensus technologies in every instance, but they do merit caution against uncritically relying upon these technologies to avoid or mitigate centralization. Too often, the use of distributed consensus is perceived as imbuing all parts of a project with "decentralization".  

这些潜在的缺点并不排除在每种情况下使用分布式共识技术，但确实值得警惕，不要不加批判地依赖这些技术来避免或减轻集中化。很多时候，分布式共识的使用被认为是让项目的所有部分都充满了“去中心化”。

#### [3.1.3.](https://www.rfc-editor.org/rfc/rfc9518.html#section-3.1.3) [Operational Governance](https://www.rfc-editor.org/rfc/rfc9518.html#name-operational-governance)  

3.1.3.运营治理

Federation and distributed consensus can both create the conditions for the provision of a function by multiple providers, but they cannot guarantee it. However, when providers require access to a resource or cooperation of others to provide that service, that choke point can itself be used to influence provider behavior -- including in ways that can counteract centralization.  

联邦和分布式共识都可以为多个提供者提供功能创造条件，但不能保证这一点。然而，当提供商需要访问某种资源或与其他人合作来提供该服务时，这个瓶颈本身就可以用来影响提供商的行为——包括以抵制集中化的方式。

In these circumstances, some form of governance over that choke point is necessary to assure the desired outcome. Often, this is through the establishment of a multi-stakeholder body, which is an institution that includes representatives of the different kinds of parties that are affected by the system's operation ("stakeholders") in an attempt to make well-reasoned, legitimate, and authoritative decisions.  

在这种情况下，有必要对这一瓶颈进行某种形式的治理，以确保达到预期的结果。通常，这是通过建立一个多利益相关方机构来实现的，该机构包括受系统运行影响的不同类型各方（“利益相关方”）的代表，试图使合理的、合法的、和权威的决定。

A widely studied example of this technique is the governance of the DNS name space, which exhibits centralization as a "single source of truth" \[[Moura](https://www.rfc-editor.org/rfc/rfc9518.html#CENTRALIZATION)\]. That source of truth is overseen by the Internet Corporation for Assigned Names and Numbers (ICANN) <[https://www.icann.org/resources/pages/governance/governance-en](https://www.icann.org/resources/pages/governance/governance-en)\>, a global multi-stakeholder body with representation from end users, governments, operators, and others.  

这种技术的一个被广泛研究的例子是 DNS 名称空间的治理，它表现出集中化作为“单一事实来源”\[Moura\]。该事实来源由互联网名称与数字地址分配机构 (ICANN) < https://www.icann.org/resources/pages/governance/governance-en> 监管，该机构是一个全球性多利益相关方机构，拥有来自终端的代表用户、政府、运营商和其他人。

Another example is the governance of the Web's trust model, implemented by web browsers as relying parties that have strong incentives to protect user privacy and security and CAs as trust anchors that have a strong incentive to be included in browser trust stores. To promote the operational and security requirements necessary to provide the desired properties, the CA/Browser Forum <[https://cabforum.org](https://cabforum.org/)\> was established as an oversight body that involves both parties as stakeholders.  

另一个例子是网络信任模型的治理，由网络浏览器作为信赖方实施，它们有强烈的动机保护用户隐私和安全，而 CA 作为信任锚，有强烈的动机包含在浏览器信任存储中。为了促进提供所需属性所需的操作和安全要求，建立了 CA/浏览器论坛 < https://cabforum.org> 作为一个监督机构，双方都作为利益相关者参与其中。

These examples are notable in that the governance mechanism is not specified in the protocol documents directly; rather, they are layered on operationally, but in a manner that takes advantage of protocol features that enable the imposition of governance.  

这些例子的值得注意之处在于，治理机制并未直接在协议文件中指定；相反，它们在操作上是分层的，但其方式是利用能够实施治理的协议功能。

Governance in this manner is suited to very limited functions like the examples above. Even then, the setup and ongoing operation of a governance mechanism is not trivial, and their legitimacy may be difficult to establish and maintain (e.g., see \[[Palladino](https://www.rfc-editor.org/rfc/rfc9518.html#MULTISTAKEHOLDER)\]); by their nature, they are vulnerable to capture by the interests that are being governed.  

这种方式的治理适用于非常有限的功能，如上面的示例。即便如此，治理机制的建立和持续运行也并非微不足道，其合法性可能难以建立和维护（例如，参见\[Palladino\]）；就其本质而言，它们很容易被所统治的利益集团所俘虏。

## [4\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4) [What Can Internet Standards Do?](https://www.rfc-editor.org/rfc/rfc9518.html#name-what-can-internet-standards)  

4\. 互联网标准能做什么？

Given the limits of decentralization techniques like federation and distributed consensus, the voluntary nature of standards compliance, and the powerful forces that can drive centralization, it is reasonable to ask what standards efforts like those at the IETF can do to accommodate helpful centralization while avoiding the associated harms and acknowledging that the distinction itself is a judgment call and, therefore, inherently political.  

考虑到联邦和分布式共识等去中心化技术的局限性、标准合规的自愿性质以及推动中心化的强大力量，我们有理由问，像 IETF 那样的标准工作可以采取哪些措施来适应有用的中心化，同时避免相关的危害，并承认这种区别本身就是一种判断，因此本质上是政治性的。

The subsections below suggest a few concrete, meaningful steps that standards bodies can take.  

以下小节提出了标准机构可以采取的一些具体、有意义的步骤。

### [4.1.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.1) [Bolster Legitimacy](https://www.rfc-editor.org/rfc/rfc9518.html#name-bolster-legitimacy)  

4.1.增强合法性

Where technical standards have only limited ability to control centralization of the Internet, legal standards (whether regulation, legislation, or case law) show more promise and are actively being considered and implemented in various jurisdictions. However, regulating the Internet is risky without a firm grounding in the effects on the architecture informed by a technical viewpoint.  

在技术标准控制互联网集中化的能力有限的情况下，法律标准（无论是法规、立法还是判例法）显示出更大的希望，并正在各个司法管辖区积极考虑和实施。然而，如果没有技术观点对架构影响的坚实基础，监管互联网是有风险的。

That viewpoint can and should be provided by the Internet standards community. To effectively do so, its institutions must be seen as legitimate by the relevant parties -- for example, competition regulators. If the IETF is perceived as representing or being controlled by "big tech" concerns or only US-based companies, its ability to guide decisions that affect the Internet will be diminished considerably.  

这一观点可以而且应该由互联网标准社区提供。为了有效地做到这一点，其机构必须被相关方（例如竞争监管机构）视为合法。如果 IETF 被视为代表“大型科技”公司或仅受美国公司控制，那么其指导影响互联网决策的能力将大大削弱。

The IETF already has features that arguably provide considerable legitimacy. Examples of these features include open participation and representation by individuals rather than by companies, both of which enhance input legitimacy); a well-defined process with multiple layers of appeals and transparency, which contributes to throughput legitimacy; and a long history of successful Internet standards, which provides perhaps the strongest source of legitimacy for the IETF -- its output.  

IETF 已经拥有可以说具有相当大合法性的功能。这些特征的例子包括个人而不是公司的公开参与和代表，这两者都增强了投入的合法性）；具有多层诉求和透明度的明确流程，有助于提高吞吐量合法性；以及成功的互联网标准的悠久历史，这或许为 IETF 提供了最强有力的合法性来源——它的成果。

However, it is also widely recognized that the considerable costs (not just financial) involved in successfully participating in the IETF have a tendency to favor larger companies over smaller concerns. Additionally, the specialized and highly technical nature of the work creates barriers to entry for non-technical stakeholders. These factors have the potential to reduce the legitimacy of the IETF's decisions, at least in some eyes.  

然而，人们还普遍认识到，成功参与 IETF 所涉及的相当大的成本（不仅仅是财务成本）往往有利于较大的公司而不是较小的公司。此外，这项工作的专业性和高技术性给非技术利益相关者造成了进入壁垒。至少在某些人看来，这些因素有可能降低 IETF 决策的合法性。

Efforts to address these shortcomings are ongoing; for example, see \[[RFC8890](https://www.rfc-editor.org/rfc/rfc9518.html#RFC8890)\]. Overall, bolstering the legitimacy of the organization should be seen as a continuous effort.  

正在努力解决这些缺点；例如，请参阅 \[RFC8890\]。总体而言，加强组织的合法性应被视为一项持续的努力。

When engaging in external efforts, the IETF community (especially its leadership) should keep firmly in mind that its voice is most authoritative when focused on technical and architectural impact.  

在参与外部工作时，IETF 社区（尤其是其领导层）应牢记，当关注技术和架构影响时，其声音最具权威性。

### [4.2.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.2) [Focus Discussion of Centralization](https://www.rfc-editor.org/rfc/rfc9518.html#name-focus-discussion-of-central)  

4.2.中心化的焦点讨论

Centralization and decentralization are increasingly being raised in technical standards discussions. Any claim needs to be critically evaluated. As discussed in [Section 2](https://www.rfc-editor.org/rfc/rfc9518.html#centralization), not all centralization is automatically harmful. Per [Section 3](https://www.rfc-editor.org/rfc/rfc9518.html#decentralization), decentralization techniques do not automatically address all centralization harms and may bring their own risks.  

技术标准讨论中越来越多地提出集中化和分散化。任何主张都需要进行严格评估。正如第 2 节中所讨论的，并非所有中心化都会自然而然地有害。根据第 3 节，去中心化技术并不能自动解决所有中心化的危害，并且可能会带来自身的风险。

However, standards participants rarely have the expertise or information available to completely evaluate those claims, because the analysis involves not only technical factors, but also economic, social, commercial, and legal aspects. For example, economies of scale can cause concentration due to the associated efficiencies \[[Demsetz](https://www.rfc-editor.org/rfc/rfc9518.html#SCALE)\], and so determining whether that concentration is appropriate requires a detailed economic analysis that is not in scope for a technical standards body. Furthermore, claims of centralization may have other motivations; in particular, they can be proxies for power struggles between actors with competing interests, and a claim of centralization might be used to deny market participants and (perhaps more importantly) users the benefits of standardization.  

然而，标准参与者很少拥有可用于完整评估这些主张的专业知识或信息，因为分析不仅涉及技术因素，还涉及经济、社会、商业和法律方面。例如，由于相关的效率，规模经济可能会导致集中\[Demsetz\]，因此确定这种集中是否合适需要进行详细的经济分析，而这不属于技术标准机构的范围。此外，集中化的主张可能还有其他动机；特别是，它们可以成为具有竞争利益的参与者之间权力斗争的代理人，并且中心化的主张可能被用来剥夺市场参与者和（也许更重要的是）用户标准化的好处。

Therefore, approaches like requiring a "Centralization Considerations" section in documents, gatekeeping publication on a centralization review, or committing significant resources to searching for centralization in protocols are unlikely to improve the Internet.  

因此，要求文档中包含“集中化注意事项”部分、集中化审查的把关出版物或投入大量资源来寻找协议集中化等方法不太可能改善互联网。

Similarly, refusing to standardize a protocol because it does not actively prevent all forms of centralization ignores the very limited power that standards efforts have to do so. Almost all existing Internet protocols -- including IP, TCP, HTTP, and DNS -- fail to prevent centralized applications from using them. While the imprimatur of the standards track \[[RFC2026](https://www.rfc-editor.org/rfc/rfc9518.html#RFC2026)\] is not without value, merely withholding it cannot prevent centralization.  

同样，因为协议没有积极阻止所有形式的中心化而拒绝对其进行标准化，就忽视了标准工作所拥有的非常有限的权力。几乎所有现有的互联网协议——包括 IP、TCP、HTTP 和 DNS——都无法阻止集中式应用程序使用它们。虽然标准轨道 \[RFC2026\] 的认可并非没有价值，但仅仅保留它并不能阻止集中化。

Thus, discussions should be very focused and limited, and any proposals for decentralization should be detailed so their full effects can be evaluated. \[[Schneider1](https://www.rfc-editor.org/rfc/rfc9518.html#SCHNEIDER)\] implores those who propose decentralization to be "really, really clear about what particular features of a system a given design seeks to decentralize" and promotes considered use of tools like separation of powers and accountability from "old, institutional liberal political theory".  

因此，讨论应该非常集中和有限，任何权力下放提案都应该详细说明，以便评估其全部效果。 \[施耐德1\]恳请那些提出去中心化的人“非常非常清楚特定设计试图去中心化的系统的哪些具体特征”，并促进深思熟虑地使用“旧的、制度化的自由主义政治理论”中的权力分立和问责制等工具。

When evaluating claims that a given proposal is centralized, the context of those statements should be examined for presuppositions, assumptions, and omissions. \[[Bacchi](https://www.rfc-editor.org/rfc/rfc9518.html#BACCHI)\] offers one framework for critical interrogations, which can be adapted for centralization-related discussions:  

在评估给定提案是中心化的主张时，应检查这些陈述的上下文是否有预设、假设和遗漏。 \[ Bacchi\] 提供了一种批判性质疑的框架，可以适用于与中心化相关的讨论：

1.  What is the nature of the centralization that is represented as being problematic?  
    
    被认为有问题的集权的本质是什么？
    
2.  What deep-seated presuppositions or assumptions (conceptual logics) underlie this representation of the "problem"?  
    
    这种“问题”的表述背后有哪些根深蒂固的预设或假设（概念逻辑）？
    
3.  How has this representation of the problem come about?  
    
    这种对问题的表述是如何产生的？
    
4.  What is left unproblematic in this problem representation? Where are the silences? Can the "problem" be conceptualized differently?  
    
    在这个问题表述中还有什么是没有问题的？沉默在哪里？ “问题”可以有不同的概念吗？
    
5.  What effects are produced by this representation of the "problem"?  
    
    这种“问题”的表述会产生什么影响？
    
6.  How and where has this representation of the "problem" been produced, disseminated, and defended? How has it been and/or how can it be disrupted and replaced?  
    
    这种“问题”的表述是如何以及在哪里产生、传播和辩护的？它是怎样的和/或如何被破坏和取代？
    

### [4.3.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.3) [Target Proprietary Functions](https://www.rfc-editor.org/rfc/rfc9518.html#name-target-proprietary-function)  

4.3.目标专有功能

Functions that are widely used but lacking in interoperability are ripe for standardization efforts. Targeting prominent and proprietary functions (e.g., chat) is appropriate, but so are smaller efforts to improve interoperability and portability of specific features that are often used to lock users into a platform, for example, a format for lists of contacts in a social network.  

广泛使用但缺乏互操作性的功能对于标准化工作来说已经成熟。针对突出的专有功能（例如聊天）是适当的，但改进通常用于将用户锁定到平台的特定功能的互操作性和可移植性的较小努力也是适当的，例如社交网络中联系人列表的格式。

A common objection to this approach is that adoption is voluntary; there are no "standards police" to mandate their use or enforce correct implementation. For example, specifications like \[[ACTIVITYSTREAMS](https://www.rfc-editor.org/rfc/rfc9518.html#ACTIVITYSTREAMS)\] were available for some time without being used in a federated manner by commercial social-networking providers.  

对这种方法的一个普遍反对意见是，采用是自愿的；没有“标准警察”来强制使用或强制正确实施。例如，像 \[ACTIVITYSTREAMS\] 这样的规范已经存在了一段时间，但商业社交网络提供商并未以联合方式使用。

That objection ignores the fact that while standards aren't mandatory, legal regulation is. Legal mandates for interoperability are increasingly proposed by policymakers as a remedy for competition issues (e.g., see \[[DMA](https://www.rfc-editor.org/rfc/rfc9518.html#DMA)\]). This appetite for regulation presents an opportunity for new specifications to decentralize these functions, backed by a legal mandate in combination with changing norms and the resulting market forces \[[Lessig](https://www.rfc-editor.org/rfc/rfc9518.html#NEW-CHICAGO)\].  

这种反对意见忽视了这样一个事实：虽然标准不是强制性的，但法律监管却是强制性的。政策制定者越来越多地提出互操作性的法律规定，作为竞争问题的补救措施（例如，参见 \[DMA\]）。这种对监管的兴趣为新规范提供了一个机会，可以在法律授权、不断变化的规范和由此产生的市场力量的支持下，分散这些职能\[Lessig\]。

That opportunity also presents a risk, if the resulting legal regulation is at odds with the Internet architecture. Successfully creating standards that work in concert with legal regulation presents many potential pitfalls and will require new and improved capabilities (especially liaison) and considerable effort. If the technical community does not make that effort, it is likely that regulators will turn to other sources for interoperability specifications.  

如果由此产生的法律法规与互联网架构不一致，那么这个机会也带来了风险。成功地制定与法律法规相配合的标准会带来许多潜在的陷阱，并且需要新的和改进的能力（特别是联络）和大量的努力。如果技术社区不做出这样的努力，监管机构很可能会转向其他来源来获取互操作性规范。

### [4.4.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.4) [Enable Switching](https://www.rfc-editor.org/rfc/rfc9518.html#name-enable-switching)  

4.4.启用切换

The ability to switch between different function providers is a core mechanism to control centralization. If users are unable to switch, they cannot exercise choice or fully realize the value of their efforts because, for example, "learning to use a vendor's product takes time, and the skill may not be fully transferable to a competitor's product if there is inadequate standardization" \[[FarrellJ](https://www.rfc-editor.org/rfc/rfc9518.html#SWITCHING)\].  

不同功能提供者之间的切换能力是控制中心化的核心机制。如果用户无法切换，他们就无法做出选择或完全实现其努力的价值，因为，例如，“学习使用供应商的产品需要时间，如果没有足够的资源，该技能可能无法完全转移到竞争对手的产品上”。标准化”\[FarrellJ\]。

Therefore, standards should have an explicit goal of facilitating users switching between implementations and deployments of the functions they define or enable.  

因此，标准应该有一个明确的目标，即促进用户在他们定义或启用的功能的实现和部署之间切换。

One necessary condition for switching is the availability of alternatives; breadth and diversity of implementation and deployment are required. For example, if there is only a single implementation of a protocol, applications that use it are vulnerable to the control it has over their operation. Even open source projects can be an issue in this regard if there are factors that make forking difficult (for example, the cost of maintaining that fork). [Section 2.1](https://rfc-editor.org/rfc/rfc5218#section-2.1) of \[[RFC5218](https://www.rfc-editor.org/rfc/rfc9518.html#RFC5218)\] explores some factors in protocol design that encourage diversity of implementation.  

转换的一个必要条件是有替代方案；需要实施和部署的广度和多样性。例如，如果协议只有一个实现，则使用该协议的应用程序很容易受到协议对其操作的控制。如果存在导致分叉困难的因素（例如，维护该分叉的成本），即使是开源项目也可能成为这方面的问题。 \[RFC5218\] 的 2.1 节探讨了协议设计中鼓励实现多样性的一些因素。

The cost of substituting an alternative implementation or deployment by users is another important factor to consider. This includes minimizing the amount of time, resources, expertise, coordination, loss of functionality, and effort required to use a different provider or implementation -- with the implication that the standard needs to be functionally complete and specified precisely enough to allow substitution.  

用户替换替代实施或部署的成本是另一个需要考虑的重要因素。这包括最大限度地减少时间、资源、专业知识、协调、功能损失以及使用不同提供者或实施所需的工作量——这意味着该标准需要功能完整并且指定得足够精确以允许替代。

These goals of completeness and diversity are sometimes at odds. If a standard becomes extremely complex, it may discourage implementation diversity because the cost of a complete implementation is too high (consider web browsers). On the other hand, if the specification is too simple, it may not enable easy switching, especially if proprietary extensions are necessary to complete it (see [Section 4.7](https://www.rfc-editor.org/rfc/rfc9518.html#evolution)).  

这些完整性和多样性的目标有时是不一致的。如果一个标准变得极其复杂，它可能会阻碍实现的多样性，因为完整实现的成本太高（考虑网络浏览器）。另一方面，如果规范太简单，可能无法轻松切换，特别是需要专有扩展来完成它时（参见第 4.7 节）。

One objection to protocols that enable easy switching is that they reduce the incentives for implementation by commercial vendors. While a completely commoditized protocol might not allow implementations to differentiate themselves, they provide opportunities for specialization and improvement elsewhere in the value chain \[[Christensen](https://www.rfc-editor.org/rfc/rfc9518.html#ATTRACTIVE-PROFITS)\]. Well-timed standards efforts leverage these forces to focus proprietary interests on top of open technology rather than as a replacement for it.  

对实现轻松切换的协议的一种反对意见是，它们减少了商业供应商实施的动力。虽然完全商品化的协议可能不允许实现脱颖而出，但它们为价值链其他地方的专业化和改进提供了机会\[Christensen\]。适时的标准工作利用这些力量将专有利益集中在开放技术之上，而不是作为开放技术的替代品。

### [4.6.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.6) [Enforce Boundaries](https://www.rfc-editor.org/rfc/rfc9518.html#name-enforce-boundaries)  

4.6.强化界限

Most Internet protocols and applications depend on other, "lower-layer" functions and their implementations. The features, deployment, and operation of these dependencies can become centralization risks for the functions and applications built "on top" of them.  

大多数互联网协议和应用程序都依赖于其他“较低层”功能及其实现。这些依赖项的功能、部署和操作可能会成为构建在其“之上”的功能和应用程序的集中化风险。

For example, application protocols require a network to function; therefore, a degree of power over communication is available to the network provider. They might block access to, slow down, or change the content of a specific service for financial, political, operational, or criminal reasons, creating a disincentive (or even removing the ability) to use a specific provider of a function. By selectively hindering the use of some services but not others, network interventions can be composed to create pressure to use those other services -- intentionally or not.  

例如，应用程序协议需要网络才能运行；因此，网络提供商可以获得一定程度的通信权力。他们可能出于财务、政治、运营或犯罪原因阻止对特定服务的访问、减慢或更改特定服务的内容，从而抑制（甚至消除使用特定功能提供商的能力）。通过有选择地阻止某些服务的使用而不是其他服务，网络干预可以有意或无意地施加使用其他服务的压力。

Techniques like encryption can discourage such centralization by enforcing such boundaries. When the number of parties who have access to the content of communication is limited, other parties who handle it but are not party to it can be prevented from interfering with and observing it. Although those parties might still prevent communication, encryption also makes it more difficult to discriminate a target from other users' traffic.  

加密等技术可以通过强制执行这样的边界来阻止这种集中化。当有权访问通信内容的各方数量受到限制时，可以防止处理该内容但不是该内容的其他方对其进行干扰和观察。尽管这些各方可能仍然会阻止通信，但加密也使得区分目标和其他用户的流量变得更加困难。

### [4.7.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.7) [Consider Extensibility Carefully](https://www.rfc-editor.org/rfc/rfc9518.html#name-consider-extensibility-care)  

4.7.仔细考虑可扩展性

The Internet's ability to evolve is critical, allowing it to meet new requirements and adapt to new conditions without requiring a "flag day" to upgrade implementations. Typically, functions accommodate evolution by defining extension interfaces, which allow optional features to be added or change over time in an interoperable fashion.  

互联网的发展能力至关重要，使其能够满足新的要求并适应新的条件，而无需“旗舰日”来升级实施。通常，功能通过定义扩展接口来适应演进，扩展接口允许以可互操作的方式添加或随时间更改可选功能。

However, these interfaces can also be leveraged by a powerful entity if they can change the target for meaningful interoperability by adding proprietary extensions to a standard. This is especially true when the core standard does not itself provide sufficient utility on its own.  

然而，如果这些接口可以通过向标准添加专有扩展来改变有意义的互操作性的目标，那么强大的实体也可以利用这些接口。当核心标准本身不能提供足够的实用性时尤其如此。

For example, the extreme flexibility of SOAP \[[SOAP](https://www.rfc-editor.org/rfc/rfc9518.html#SOAP)\] and its failure to provide significant standalone value allowed vendors to require use of their preferred extensions, favoring those who had more market power.  

例如，SOAP \[ SOAP\] 的极端灵活性及其无法提供显着的独立价值，使得供应商可以要求使用他们首选的扩展，从而有利于那些拥有更多市场力量的人。

Therefore, standards efforts should focus on providing concrete utility to the majority of their users as published, rather than being a "framework" where interoperability is not immediately available. Internet functions should not make every aspect of their operation extensible; boundaries between modules should be designed in a way that allows evolution, while still offering meaningful functionality.  

因此，标准工作应侧重于为发布的大多数用户提供具体实用性，而不是成为无法立即提供互操作性的“框架”。互联网功能不应使其操作的每个方面都可扩展；模块之间的边界应该以允许进化的方式设计，同时仍然提供有意义的功能。

Beyond allowing evolution, well-considered interfaces can also aid decentralization efforts. The structural boundaries that emerge between the sub-modules of the function -- as well as those with adjacent functions -- provide touchpoints for interoperability and an opportunity for substitution of providers.  

除了允许进化之外，经过深思熟虑的界面还可以帮助去中心化工作。功能子模块之间以及具有相邻功能的子模块之间出现的结构边界为互操作性提供了接触点，并提供了替代提供商的机会。

In particular, if the interfaces of a function are well-defined and stable, there is an opportunity to use different providers for that function. When those interfaces are open standards, change control resides with the technical community instead of remaining in proprietary hands, further enhancing stability and enabling (but not ensuring) decentralization.  

特别是，如果函数的接口定义良好且稳定，则有机会为该函数使用不同的提供程序。当这些接口是开放标准时，变更控制权属于技术社区，而不是保留在专有手中，从而进一步增强稳定性并实现（但不确保）去中心化。

### [4.8.](https://www.rfc-editor.org/rfc/rfc9518.html#section-4.8) [Reuse What Works](https://www.rfc-editor.org/rfc/rfc9518.html#name-reuse-what-works)  

4.8.重用有效的东西

When centralization is purposefully allowed in an Internet function, protocol designers often attempt to mitigate the associated risks using technical measures such as federation (see [Section 3.1.1](https://www.rfc-editor.org/rfc/rfc9518.html#federation)) and operational governance structures (see [Section 3.1.3](https://www.rfc-editor.org/rfc/rfc9518.html#multi)).  

当互联网功能有意允许集中化时，协议设计者通常会尝试使用联合（参见第 3.1.1 节）和运营治理结构（参见第 3.1.3 节）等技术措施来减轻相关风险。

Protocols that successfully do so are often reused to avoid the considerable cost and risk of reimplementing those mitigations. For example, if a protocol requires a coordinated global naming function, incorporating the Domain Name System is usually preferable to establishing a new system.  

成功做到这一点的协议通常会被重复使用，以避免重新实施这些缓解措施的巨大成本和风险。例如，如果协议需要协调的全球命名功能，则合并域名系统通常比建立新系统更可取。

## [5\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-5) [Future Work](https://www.rfc-editor.org/rfc/rfc9518.html#name-future-work)  

5\. 未来的工作

This document has argued that, while standards bodies have little means of effectively controlling or preventing centralization of the Internet through protocol design, there are still concrete and useful steps they can take to improve the Internet.  

该文件认为，虽然标准机构几乎没有办法通过协议设计来有效控制或防止互联网的集中化，但他们仍然可以采取具体且有用的步骤来改进互联网。

Those steps might be elaborated upon and extended in future work; however, it is doubtless there is more that can be done. New decentralization techniques might be identified and examined; what we learn from relationships with other, more effective regulators in this space can be documented.  

这些步骤可能会在未来的工作中得到详细阐述和扩展；然而，毫无疑问还有更多的事情可以做。可能会确定和审查新的权力下放技术；我们从与该领域其他更有效的监管机构的关系中学到的东西可以记录下来。

Some have suggested creating a how-to guide or checklist for dealing with centralization. Because centralization is so contextual and so varied in how it manifests, this might best be attempted within very limited areas -- for example, for a particular type of function or a function at a particular layer.  

一些人建议创建一个处理集中化的操作指南或清单。由于集中化的具体情况和表现方式各不相同，因此最好在非常有限的领域内进行尝试，例如，对于特定类型的功能或特定层的功能。

The nature of centralization also deserves further study; in particular, its causes. While there is much commentary on factors like network effects and switching costs, other aspects -- such as behavioral, cognitive, social, and economic factors have received comparatively little attention, although that is changing (e.g., \[[Fletcher](https://www.rfc-editor.org/rfc/rfc9518.html#BEHAVIOUR)\]).  

中心化的本质也值得进一步研究；特别是其原因。尽管对网络效应和转换成本等因素有很多评论，但其他方面（例如行为、认知、社会和经济因素）相对较少受到关注，尽管这种情况正在发生变化（例如，\[Fletcher\]）。

## [6\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-6) [Security Considerations](https://www.rfc-editor.org/rfc/rfc9518.html#name-security-considerations)  

6\. 安全考虑

This document does not have a direct security impact on Internet protocols. That said, failure to consider centralization might cause a myriad of security issues; see [Section 2.1](https://www.rfc-editor.org/rfc/rfc9518.html#why) for a preliminary discussion.  

本文档对 Internet 协议没有直接的安全影响。也就是说，不考虑集中化可能会导致无数的安全问题；初步讨论见 2.1 节。

## [7\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-7) [IANA Considerations](https://www.rfc-editor.org/rfc/rfc9518.html#name-iana-considerations)  

7\. IANA 考虑因素

This document has no IANA actions.  

本文档没有 IANA 行动。

## [8\.](https://www.rfc-editor.org/rfc/rfc9518.html#section-8) [Informative References](https://www.rfc-editor.org/rfc/rfc9518.html#name-informative-references)  

8\. 参考资料

\[Abrahamson\] \[亚伯拉罕森\]

Abrahamson, Z., "Essential Data", Yale Law Journal, Vol. 124, No. 3, December 2014, <[https://www.yalelawjournal.org/comment/essential-data](https://www.yalelawjournal.org/comment/essential-data)\>.  

亚伯拉罕森，Z.，“基本数据”，耶鲁大学法律杂志，卷。 124，第 3 期，2014 年 12 月，< https://www.yalelawjournal.org/comment/essential-data>。

\[ACTIVITYSTREAMS\] \[活动流\]

Prodromou, E., Ed. and J. Snell, Ed., "Activity Streams 2.0", W3C Recommendation, 23 May 2017, <[https://www.w3.org/TR/2017/REC-activitystreams-core-20170523/](https://www.w3.org/TR/2017/REC-activitystreams-core-20170523/)\>. Latest version available at <[https://www.w3.org/TR/activitystreams-core/](https://www.w3.org/TR/activitystreams-core/)\>.  

普罗德罗穆，E.，埃德。和 J. Snell 编辑，“Activity Streams 2.0”，W3C 建议，2017 年 5 月 23 日，< https://www.w3.org/TR/2017/REC-activitystreams-core-20170523/>。最新版本可在 < https://www.w3.org/TR/activitystreams-core/> 获取。

\[Aligia\] \[阿丽吉亚\]

Aligia, P. and V. Tarko, "Polycentricity: From Polanyi to Ostrom, and Beyond", Governance: An International Journal of Policy, Administration, and Institutions, Vol. 25, No. 2, p. 237, DOI 10.1111/j.1468-0491.2011.01550.x, April 2012, <[https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1468-0491.2011.01550.x](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1468-0491.2011.01550.x)\>.  

Aligia, P. 和 V. Tarko，“多中心性：从 Polanyi 到 Ostrom 及其他”，治理：国际政策、行政和机构杂志，卷。 25，第 2 期，第 14 页。 237，DOI 10.1111/j.1468-0491.2011.01550.x，2012 年 4 月，< https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1468-0491.2011.01550.x>。

\[Bacchi\] \[巴基\]

Bacchi, C., "Introducing the 'What's the Problem Represented to be?' approach", Chapter 2, Engaging with Carol Bacchi, 2012, <[https://library.oapen.org/bitstream/handle/20.500.12657/33181/560097.pdf?sequence=1#page=34](https://library.oapen.org/bitstream/handle/20.500.12657/33181/560097.pdf?sequence=1#page=34)\>.  

Bacchi, C.，“介绍‘问题是什么？’方法”，第 2 章，与 Carol Bacchi 合作，2012 年，< https://library.oapen.org/bitstream/handle/20.500.12657/33181/560097.pdf?sequence=1#page=34>。

\[Baran\] \[巴兰\]

Baran, P., "On Distributed Communications: Introduction to Distributed Communications Networks", DOI 10.7249/RM3420, 1964, <[https://www.rand.org/pubs/research\_memoranda/RM3420.html](https://www.rand.org/pubs/research_memoranda/RM3420.html)\>.  

Baran, P.，“论分布式通信：分布式通信网络简介”，DOI 10.7249/RM3420，1964，< https://www.rand.org/pubs/research\_memoranda/RM3420.html>。

\[BCP95\]

Alvestrand, H., "A Mission Statement for the IETF", BCP 95, RFC 3935, October 2004.  

Alvestrand, H.，“IETF 的使命声明”，BCP 95，RFC 3935，2004 年 10 月。

<[https://www.rfc-editor.org/info/bcp95](https://www.rfc-editor.org/info/bcp95)\>

\[Bodo\] \[博多\]

Bodó, B., Brekke, J. K., and J.-H. Hoepman, "Decentralization: a multidisciplinary perspective", Internet Policy Review, Vol. 10, No. 2, DOI 10.14763/2021.2.1563, June 2021, <[https://policyreview.info/concepts/decentralisation](https://policyreview.info/concepts/decentralisation)\>.  

Bodó, B.、Brekke, J. K. 和 J.-H.霍普曼，“权力下放：多学科视角”，互联网政策评论，卷。 10，第 2 号，DOI 10.14763/2021.2.1563，2021 年 6 月，< https://policyreview.info/concepts/decentralization>。

\[Chaum\] \[乔姆\]

Chaum, D., "Untraceable electronic mail, return addresses, and digital pseudonyms", Communications of the ACM, Vol. 24, No. 2, DOI 10.1145/358549.358563, February 1981, <[https://dl.acm.org/doi/10.1145/358549.358563](https://dl.acm.org/doi/10.1145/358549.358563)\>.  

Chaum, D.，“无法追踪的电子邮件、回信地址和数字假名”，ACM 通讯，卷。 24，第 2 号，DOI 10.1145/358549.358563，1981 年 2 月，< https://dl.acm.org/doi/10.1145/358549.358563>。

\[Christensen\] \[克里斯滕森\]

Christensen, C., "The Law of Conservation of Attractive Profits", Harvard Business Review, "Breakthrough Ideas for 2004", February 2004.  

Christensen, C.，“有吸引力的利润守恒定律”，《哈佛商业评论》，“2004 年突破性创意”，2004 年 2 月。

\[Demsetz\] \[德姆塞茨\]

Demsetz, H., "Industry Structure, Market Rivalry, and Public Policy", Journal of Law and Economics, Vol. 16, No. 1, April 1973, <[https://www.jstor.org/stable/724822](https://www.jstor.org/stable/724822)\>.  

Demsetz, H.，“产业结构、市场竞争和公共政策”，《法律与经济学杂志》，卷。 16，第 1 期，1973 年 4 月，< https://www.jstor.org/stable/724822>。

\[DMA\]

The European Parliament and the Council of the European Union, "Regulation (EU) 2022/1925 of the European Parliament and of the Council of 14 September 2022 on contestable and fair markets in the digital sector and amending Directives (EU) 2019/1937 and (EU) 2020/1828 (Digital Markets Act)", OJ L 265/1, 12.10.2022, September 2022, <[https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R1925](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R1925)\>.  

欧洲议会和欧盟理事会，“欧洲议会和理事会 2022 年 9 月 14 日关于数字领域可竞争和公平市场的条例 (EU) 2022/1925 以及修订指令 (EU) 2019/1937 和(EU) 2020/1828（数字市场法）”，OJ L 265/1，2022 年 10 月 12 日，2022 年 9 月，< https://eur-lex.europa.eu/legal-content/EN/TXT/?uri= CELEX%3A32022R1925>。

\[Doctorow\] \[多克托罗\]

Doctorow, C., "Adversarial Interoperability", October 2019, <[https://www.eff.org/deeplinks/2019/10/adversarial-interoperability](https://www.eff.org/deeplinks/2019/10/adversarial-interoperability)\>.  

Doctorow, C.，“对抗性互操作性”，2019 年 10 月，< https://www.eff.org/deeplinks/2019/10/adversarial-interoperability>。

\[ETHEREUM\] \[以太坊\]

Ethereum, "The Merge", February 2023, <[https://ethereum.org/en/roadmap/merge/](https://ethereum.org/en/roadmap/merge/)\>.  

以太坊，“合并”，2023 年 2 月，< https://ethereum.org/en/roadmap/merge/>。

\[FarrellH\] \[法雷尔\]

Farrell, H. and A. Newman, "Weaponized Interdependence: How Global Economic Networks Shape State Coercion", International Security, Vol. 44, No. 1, p. 42, DOI 10.1162/ISEC\_a\_00351, July 2019, <[https://doi.org/10.1162/ISEC\_a\_00351](https://doi.org/10.1162/ISEC_a_00351)\>.  

Farrell, H. 和 A. Newman，“武器化的相互依赖：全球经济网络如何塑造国家胁迫”，《国际安全》，卷。 44，第 1 期，第 44 页。 42，DOI 10.1162/ISEC\_a\_00351，2019 年 7 月，< https://doi.org/10.1162/ISEC\_a\_00351>。

\[FarrellJ\] \[法雷尔\]

Farrell, J. and C. Shapiro, "Dynamic Competition with Switching Costs", UC Berkeley Department of Economics Working Paper 8865, DOI 10.2307/2555402, January 1988, <[https://doi.org/10.2307/2555402](https://doi.org/10.2307/2555402)\>.  

Farrell, J. 和 C. Shapiro，“动态竞争与转换成本”，加州大学伯克利分校经济系工作论文 8865，DOI 10.2307/2555402，1988 年 1 月，< https://doi.org/10.2307/2555402>。

\[Fletcher\] \[弗莱彻\]

Fletcher, A., "The Role of Behavioural Economics in Competition Policy", DOI 10.2139/ssrn.4389681, March 2023, <[https://doi.org/10.2139/ssrn.4389681](https://doi.org/10.2139/ssrn.4389681)\>.  

Fletcher, A.，“行为经济学在竞争政策中的作用”，DOI 10.2139/ssrn.4389681，2023 年 3 月，< https://doi.org/10.2139/ssrn.4389681>。

\[Freeman\] \[弗里曼\]

Freeman, J., "The Tyranny of Structurelessness", Berkeley Journal of Sociology, Vol. 17, 1972, <[https://www.jstor.org/stable/41035187](https://www.jstor.org/stable/41035187)\>.  

弗里曼，J.，“无结构的暴政”，伯克利社会学杂志，卷。 1972 年 12 月 17 日，< https://www.jstor.org/stable/41035187>。

\[Holzbauer\] \[霍尔兹鲍尔\]

Holzbauer, F., Ullrich, J., Lindorfer, M., and T. Fiebig, "Not that Simple: Email Delivery in the 21st Century", July 2022, <[https://www.usenix.org/system/files/atc22-holzbauer.pdf](https://www.usenix.org/system/files/atc22-holzbauer.pdf)\>.  

Holzbauer, F.、Ullrich, J.、Lindorfer, M. 和 T. Fiebig，“没那么简单：21 世纪的电子邮件传送”，2022 年 7 月，< https://www.usenix.org/system/files /atc22-holzbauer.pdf>。

\[HTTP\]

Fielding, R., Ed., Nottingham, M., Ed., and J. Reschke, Ed., "HTTP Semantics", STD 97, RFC 9110, DOI 10.17487/RFC9110, June 2022, <[https://www.rfc-editor.org/info/rfc9110](https://www.rfc-editor.org/info/rfc9110)\>.  

Fielding, R., Ed.、Nottingham, M., Ed. 和 J. Reschke, Ed.，“HTTP 语义”，STD 97，RFC 9110，DOI 10.17487/RFC9110，2022 年 6 月，< https://www. rfc-editor.org/info/rfc9110>。

\[Kashaf\] \[卡沙夫\]

Kashaf, A., Sekar, V., and Y. Agarwal, "Analyzing Third Party Service Dependencies in Modern Web Services: Have We Learned from the Mirai-Dyn Incident?", DOI 10.1145/3419394.3423664, October 2020, <[https://dl.acm.org/doi/pdf/10.1145/3419394.3423664](https://dl.acm.org/doi/pdf/10.1145/3419394.3423664)\>.  

Kashaf, A.、Sekar, V. 和 Y. Agarwal，“分析现代 Web 服务中的第三方服务依赖性：我们从 Mirai-Dyn 事件中吸取了教训吗？”，DOI 10.1145/3419394.3423664，2020 年 10 月，< https:/ /dl.acm.org/doi/pdf/10.1145/3419394.3423664>。

\[Komaitis\] \[科麦蒂斯\]

Komaitis, K., "Regulators Seem to Think That Facebook Is the Internet", August 2021, <[https://slate.com/technology/2021/08/facebook-internet-regulation.html](https://slate.com/technology/2021/08/facebook-internet-regulation.html)\>.  

Komaitis, K.，“监管机构似乎认为 Facebook 就是互联网”，2021 年 8 月，< https://slate.com/technology/2021/08/facebook-internet-regulation.html>。

\[Lessig\] \[莱西格\]

Lessig, L., "The New Chicago School", Journal of Legal Studies, Vol. 27, DOI 10.1086/468039, June 1998, <[https://www.journals.uchicago.edu/doi/10.1086/468039](https://www.journals.uchicago.edu/doi/10.1086/468039)\>.  

Lessig, L.，“新芝加哥学派”，《法律研究杂志》，卷。 27，DOI 10.1086/468039，1998 年 6 月，< https://www.journals.uchicago.edu/doi/10.1086/468039>。

\[Madison\] \[麦迪逊\]

Madison, J., "The Structure of the Government Must Furnish the Proper Checks and Balances Between the Different Departments", The Federalist Papers, No. 51, February 1788.  

Madison, J.，“政府结构必须在不同部门之间提供适当的制衡”，《联邦党人文集》，第 51 期，1788 年 2 月。

\[Makarov\] \[马卡洛夫\]

Makarov, I. and A. Schoar, "Blockchain Analysis of the Bitcoin Market", National Bureau of Economic Research, Working Paper 29396, DOI 10.3386/w29396, October 2021, <[https://www.nber.org/papers/w29396](https://www.nber.org/papers/w29396)\>.  

Makarov, I. 和 A. Schoar，“比特币市场的区块链分析”，国家经济研究局，工作文件 29396，DOI 10.3386/w29396，2021 年 10 月，< https://www.nber.org/papers/w29396 >。

\[Marlinspike\] \[马林鱼穗\]

Marlinspike, M., "Reflections: The ecosystem is moving", May 2016, <[https://signal.org/blog/the-ecosystem-is-moving/](https://signal.org/blog/the-ecosystem-is-moving/)\>.  

Marlinspike, M.，“反思：生态系统正在移动”，2016 年 5 月，< https://signal.org/blog/the-ecosystem-is-moving/>。

\[Moura\] \[莫拉\]

Moura, G., Castro, S., Hardaker, W., Wullink, M., and C. Hesselman, "Clouding up the Internet: how centralized is DNS traffic becoming?", DOI 10.1145/3419394.3423625, October 2020, <[https://dl.acm.org/doi/abs/10.1145/3419394.3423625](https://dl.acm.org/doi/abs/10.1145/3419394.3423625)\>.  

Moura, G.、Castro, S.、Hardaker, W.、Wullink, M. 和 C. Hesselman，“互联网云化：DNS 流量的中心化程度如何？”，DOI 10.1145/3419394.3423625，2020 年 10 月，< https ://dl.acm.org/doi/abs/10.1145/3419394.3423625>。

\[Musiani\] \[穆夏尼\]

Musiani, F., "Alternative Technologies as Alternative Institutions: The Case of the Domain Name System", The Turn to Infrastructure in Internet Governance, DOI 10.1057/9781137483591\_4, 2016, <[https://link.springer.com/chapter/10.1057/9781137483591\_4](https://link.springer.com/chapter/10.1057/9781137483591_4)\>.  

Musiani, F.，“替代技术作为替代机构：域名系统案例”，转向互联网治理基础设施，DOI 10.1057/9781137483591\_4，2016，< https://link.springer.com/chapter/10.1057 /9781137483591\_4>。

\[Palladino\] \[帕拉迪诺\]

Palladino, N. and M. Santaniello, "Legitimacy, Power, and Inequalities in the Multistakeholder Internet Governance", DOI 10.1007/978-3-030-56131-4, November 2020, <[https://link.springer.com/book/10.1007/978-3-030-56131-4](https://link.springer.com/book/10.1007/978-3-030-56131-4)\>.  

Palladino, N. 和 M. Santaniello，“多利益相关方互联网治理中的合法性、权力和不平等”，DOI 10.1007/978-3-030-56131-4，2020 年 11 月，< https://link.springer.com/书/10.1007/978-3-030-56131-4>。

\[PHONE\] \[电话\]

Skrzycki, C. and J. Burgess, "Computer Failure Paralyzes Region's Phone Service", June 1991, <[https://www.washingtonpost.com/archive/politics/1991/06/27/computer-failure-paralyzes-regions-phone-service/0db94ac7-89f0-446e-ba33-c126c751b251/](https://www.washingtonpost.com/archive/politics/1991/06/27/computer-failure-paralyzes-regions-phone-service/0db94ac7-89f0-446e-ba33-c126c751b251/)\>.  

Skrzycki, C. 和 J. Burgess，“计算机故障导致地区电话服务瘫痪”，1991 年 6 月，< https://www.washingtonpost.com/archive/politics/1991/06/27/computer-failure-paralyzes-regions-电话服务/0db94ac7-89f0-446e-ba33-c126c751b251/>。

\[RFC2026\]

Bradner, S., "The Internet Standards Process -- Revision 3", BCP 9, RFC 2026, DOI 10.17487/RFC2026, October 1996, <[https://www.rfc-editor.org/info/rfc2026](https://www.rfc-editor.org/info/rfc2026)\>.  

Bradner, S.，“互联网标准流程 - 第 3 版”，BCP 9，RFC 2026，DOI 10.17487/RFC2026，1996 年 10 月，< https://www.rfc-editor.org/info/rfc2026>。

\[RFC4271\]

Rekhter, Y., Ed., Li, T., Ed., and S. Hares, Ed., "A Border Gateway Protocol 4 (BGP-4)", RFC 4271, DOI 10.17487/RFC4271, January 2006, <[https://www.rfc-editor.org/info/rfc4271](https://www.rfc-editor.org/info/rfc4271)\>.  

Rekhter, Y., Ed.、Li, T., Ed. 和 S. Hares, Ed.，“边界网关协议 4 (BGP-4)”，RFC 4271，DOI 10.17487/RFC4271，2006 年 1 月，< https ://www.rfc-editor.org/info/rfc4271>。

\[RFC5218\]

Thaler, D. and B. Aboba, "What Makes for a Successful Protocol?", RFC 5218, DOI 10.17487/RFC5218, July 2008, <[https://www.rfc-editor.org/info/rfc5218](https://www.rfc-editor.org/info/rfc5218)\>.  

Thaler, D. 和 B. Aboba，“成功的协议是什么？”，RFC 5218，DOI 10.17487/RFC5218，2008 年 7 月，< https://www.rfc-editor.org/info/rfc5218>。

\[RFC5321\]

Klensin, J., "Simple Mail Transfer Protocol", RFC 5321, DOI 10.17487/RFC5321, October 2008, <[https://www.rfc-editor.org/info/rfc5321](https://www.rfc-editor.org/info/rfc5321)\>.  

Klensin, J.，“简单邮件传输协议”，RFC 5321，DOI 10.17487/RFC5321，2008 年 10 月，< https://www.rfc-editor.org/info/rfc5321>。

\[RFC6120\]

Saint-Andre, P., "Extensible Messaging and Presence Protocol (XMPP): Core", RFC 6120, DOI 10.17487/RFC6120, March 2011, <[https://www.rfc-editor.org/info/rfc6120](https://www.rfc-editor.org/info/rfc6120)\>.  

Saint-Andre, P.，“可扩展消息传递和状态协议 (XMPP)：核心”，RFC 6120，DOI 10.17487/RFC6120，2011 年 3 月，< https://www.rfc-editor.org/info/rfc6120>。

\[RFC791\]

Postel, J., "Internet Protocol", STD 5, RFC 791, DOI 10.17487/RFC0791, September 1981, <[https://www.rfc-editor.org/info/rfc791](https://www.rfc-editor.org/info/rfc791)\>.  

Postel, J.，“互联网协议”，STD 5，RFC 791，DOI 10.17487/RFC0791，1981 年 9 月，< https://www.rfc-editor.org/info/rfc791>。

\[RFC8890\]

Nottingham, M., "The Internet is for End Users", RFC 8890, DOI 10.17487/RFC8890, August 2020, <[https://www.rfc-editor.org/info/rfc8890](https://www.rfc-editor.org/info/rfc8890)\>.  

Nottingham, M.，“互联网是为最终用户服务的”，RFC 8890，DOI 10.17487/RFC8890，2020 年 8 月，< https://www.rfc-editor.org/info/rfc8890>。

\[RFC9230\]

Kinnear, E., McManus, P., Pauly, T., Verma, T., and C.A. Wood, "Oblivious DNS over HTTPS", RFC 9230, DOI 10.17487/RFC9230, June 2022, <[https://www.rfc-editor.org/info/rfc9230](https://www.rfc-editor.org/info/rfc9230)\>.  

Kinnear, E.、McManus, P.、Pauly, T.、Verma, T. 和 C.A. Wood，“HTTPS 上的遗忘 DNS”，RFC 9230，DOI 10.17487/RFC9230，2022 年 6 月，< https://www.rfc-editor.org/info/rfc9230>。

\[RFC9293\]

Eddy, W., Ed., "Transmission Control Protocol (TCP)", STD 7, RFC 9293, DOI 10.17487/RFC9293, August 2022, <[https://www.rfc-editor.org/info/rfc9293](https://www.rfc-editor.org/info/rfc9293)\>.  

Eddy, W., Ed.，“传输控制协议 (TCP)”，STD 7，RFC 9293，DOI 10.17487/RFC9293，2022 年 8 月，< https://www.rfc-editor.org/info/rfc9293>。

\[Schneider1\] \[施耐德1\]

Schneider, N., "What to do once you admit that decentralizing everything never seems to work", Hacker Noon, September 2019, <[https://hackernoon.com/decentralizing-everything-never-seems-to-work-2bb0461bd168](https://hackernoon.com/decentralizing-everything-never-seems-to-work-2bb0461bd168)\>.  

Schneider, N.，“一旦你承认去中心化一切似乎都行不通，该怎么办”，Hacker Noon，2019 年 9 月，< https://hackernoon.com/decentralizing-everything-never-seems-to-work-2bb0461bd168> 。

\[Schneider2\] \[施耐德2\]

Schneider, N., "Decentralization: An Incomplete Ambition", Journal of Cultural Economy, Vol. 12, No. 4, DOI 10.31219/osf.io/m7wyj, April 2019, <[https://osf.io/m7wyj/](https://osf.io/m7wyj/)\>.  

Schneider, N.，“权力下放：一个不完整的野心”，《文化经济杂志》，卷。 12，第 4 期，DOI 10.31219/osf.io/m7wyj，2019 年 4 月，< https://osf.io/m7wyj/>。

\[SOAP\] \[肥皂\]

Mitra, N., Ed. and Y. Lafon, Ed., "SOAP Version 1.2 Part 0: Primer (Second Edition)", W3C Recommendation, 27 April 2007, <[https://www.w3.org/TR/2007/REC-soap12-part0-20070427/](https://www.w3.org/TR/2007/REC-soap12-part0-20070427/)\>. Latest version available at <[https://www.w3.org/TR/soap12-part0/](https://www.w3.org/TR/soap12-part0/)\>.  

米特拉，N.，埃德。和 Y. Lafon，Ed.，“SOAP 版本 1.2 第 0 部分：入门（第二版）”，W3C 建议，2007 年 4 月 27 日，< https://www.w3.org/TR/2007/REC-soap12-part0- 20070427/>。最新版本可在 < https://www.w3.org/TR/soap12-part0/> 获取。

\[Spulber\] \[碎浆机\]

Spulber, D., "Solving The Circular Conundrum: Communication and Coordination in Two-Sided Markets", Northwestern University Law Review, Vol. 104, No. 2, October 2009, <[https://wwws.law.northwestern.edu/research-faculty/clbe/workingpapers/documents/spulber\_circularconundrum.pdf](https://wwws.law.northwestern.edu/research-faculty/clbe/workingpapers/documents/spulber_circularconundrum.pdf)\>.  

Spulber, D.，“解决循环难题：双边市场中的沟通与协调”，西北大学法律评论，卷。 104，第 2 期，2009 年 10 月，< https://wwws.law.northwestern.edu/research-faculty/clbe/workingpapers/documents/spulber\_circularconundrum.pdf>。

\[THOMSON-TMI\] \[汤姆森-TMI\]

Thomson, M., "Principles for the Involvement of Intermediaries in Internet Protocols", Work in Progress, Internet-Draft, draft-thomson-tmi-04, 8 September 2022, <[https://datatracker.ietf.org/doc/html/draft-thomson-tmi-04](https://datatracker.ietf.org/doc/html/draft-thomson-tmi-04)\>.  

Thomson, M.，“中介机构参与互联网协议的原则”，正在进行中，互联网草案，draft-thomson-tmi-04，2022 年 9 月 8 日，< https://datatracker.ietf.org/doc/ html/draft-thomson-tmi-04>.

## [Acknowledgements  致谢](https://www.rfc-editor.org/rfc/rfc9518.html#name-acknowledgements)

This document was born out of early discussions with Brian Trammell during our shared time on the Internet Architecture Board.  

本文档是我们在互联网架构委员会共同讨论期间与 Brian Trammell 进行的早期讨论的结果。

Special thanks to Geoff Huston and Milton Mueller for their well-considered, thoughtful, and helpful reviews.  

特别感谢 Geoff Huston 和 Milton Mueller 深思熟虑、深思熟虑且很有帮助的评论。

Thanks to Jari Arkko, Kristin Berdan, Richard Clayton, Cory Doctorow, Christian Huitema, Eliot Lear, John Levine, Tommy Pauly, and Martin Thomson for their comments and suggestions. Likewise, the [arch-discuss@ietf.org](mailto:arch-discuss@ietf.org) mailing list and Decentralized Internet Infrastructure Research Group provided valuable discussion and feedback.  

感谢 Jari Arkko、Kristin Berdan、Richard Clayton、Cory Doctorow、Christian Huitema、Eliot Lear、John Levine、Tommy Pauly 和 Martin Thomson 的评论和建议。同样，arch-discuss@ietf.org 邮件列表和去中心化互联网基础设施研究小组也提供了有价值的讨论和反馈。

No large language models were used in the production of this document.  

本文档的制作过程中没有使用大型语言模型。

## [Author's Address  作者地址](https://www.rfc-editor.org/rfc/rfc9518.html#name-authors-address)

Mark Nottingham 马克·诺丁汉

Prahran 普拉兰

Australia 澳大利亚
