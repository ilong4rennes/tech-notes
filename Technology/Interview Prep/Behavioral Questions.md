### 自我介绍

我认为重要的方向（对我个人发展而言）：

1. CS基础 - 计算机网络知识、算法
    
2. 项目实践经验 - 为何要做这个项目？为项目选择合适的工具（技术栈）--为什么要用这个技术栈/语言？它们有什么特性？有什么优缺点？有没有需要注意的点？用xxx工具是为了解决什么问题？怎么确保系统的稳定性（不出意外问题）？虽然现在vibe code发展得特别迅速，但是这些元架构方面的知识还是需要掌握的。
    
3. 对于AI与最新技术趋势的理解/应用能力 - 技术太多太杂，我是否知道最前沿的；我是否能将最合适的工具技术投入实践中。对于AI新技术的看法与判断 见下。
    
4. 学习新知识的SOP/认知框架/结构学习的能力 - 目前技术发展太快了…如何快速补齐认知，形成判断。目前在用obsidian记录学习过程，放在下面的section详细展开。
    

  
面试官您好，我是陆一晨，目前在卡内基梅隆大学读信息系统大四，辅修了人工智能，今年五月毕业。

我这几年的经历主要围绕全栈开发，从实习到开源项目再到自己动手做系统，最近也做了AI 项目的落地。

先说实习，大三暑假我在北美最大的体育用品公司 Dick's Sporting Goods 实习，在计算机视觉组。他们有一个叫 GameChanger 的 app，主要是给棒球、垒球这些运动用的，教练可以用它来记录比赛、自动计分、追踪运动员的数据表现。要实现这些功能，背后就需要计算机视觉模型来识别球场上发生的事情。

我在这个组做的是一个模型评估平台。当时团队的痛点是，模型研究和实际业务之间有一道很明显的鸿沟——工程师看得懂模型指标，但产品和业务那边完全不知道这个模型到底好不好用、能不能上线。我做的这个平台，就是把模型的表现可视化出来，让不同背景的人都能看懂，大家在同一个信息基础上做决策，迭代速度快了很多。

然后是我自己做的一个 AI 项目 DreamScope，一个 AI 梦境分析平台。这个项目对我影响挺大，因为我从零开始，把一个 AI 功能做成了一个真正意义上的服务，认真考虑了异步处理、缓存、云部署这一整套东西。做完之后我对"AI 产品"的理解变了很多，我觉得模型本身只是其中一小块，更难的是怎么让它稳定、可控、低成本地跑在真实业务里。

我申请这个岗位，是因为我想继续往 AI 加云这个方向深耕。谢谢。

  

Hi, my name is Yichen Lu. I’m currently studying Information Systems at Carnegie Mellon University, with a minor in Artificial Intelligence.

Over the past few years, I’ve mainly focused on building full-stack systems, and more importantly, turning AI capabilities into something that can actually be used in real-world scenarios.

I previously interned at Dick’s Sporting Goods, where I worked on a platform for evaluating computer vision models in sports video analysis. The goal was to take models that were originally used in research settings and turn them into a system where people could easily visualize results and compare different models.

At CMU, I’ve also worked on a high-traffic course planning platform through Scotty Labs, where I focused on improving performance and ensuring the system could stay responsive under heavy concurrent usage.

More recently, I built an AI project called Dream Scope. Instead of just calling a model API, I designed it as a more complete system, thinking about things like request handling, reliability, output validation, and deployment. That experience really shaped how I think about AI — not just as a model, but as a service that needs to be stable, scalable, and controllable.

Overall, I’m particularly interested in the intersection of AI and cloud systems, and I enjoy working on problems where I can take new technologies and turn them into something practical and reliable.


### 学习新知识SOP

现在的流程：

1. 信息input渠道：各类社交媒体，如推特、小红书等，听闻某项技术
    
2. 了解：用AI工具做初步认识，快速理解：解决什么问题，和已有方案有什么区别
    
3. 让AI给我一个最简单的use case，去实践，让它跑起来
    
4. 在整个过程中，用obsidian记录
    
5. 一定程度入门后，查官方文档，“一手信息”是十分重要的


#### 最希望从工作中获得什么？

我对“AI + Cloud”这个交叉点特别感兴趣。

我不太想做那种只盯着一个模型或者一个功能的工作，我更想理解一整套系统怎么被搭起来，怎么稳定、怎么扩展、怎么真正让企业用起来。阿里云现在刚好就在做这件事，而且它不是只讲概念，产品层已经有很完整的云服务和 AI 平台。我觉得这个环境和我想走的方向是很一致的。

#### 如何快速接手完全陌生的任务？

我一般会分几个步骤来接手一个完全陌生的任务。

第一步，我会先快速建立一个整体认知，比如这个任务的目标是什么，它大概涉及哪些模块，不会一开始就陷入细节。

第二步，我会借助一些工具，比如文档或者AI，快速了解核心概念，但不会停留在理解层面。

第三步，我会尽快找到一个最小可运行的路径，把系统跑起来，哪怕只是一个简化版本，这样我可以更快建立直觉。

第四步，在跑通之后，我会再回到官方文档或者源码，去补齐对原理和边界的理解。

最后，我会总结这个技术或系统解决的本质问题，这样下次遇到类似问题，可以更快迁移。

#### 个人在项目中的价值

第一，我能比较快地把复杂需求转成清晰的系统方案。

第二，我不只看功能实现，也会同时看性能、可维护性和扩展性。

第三，我在团队里比较像连接器，既能下沉到技术细节，也能把技术问题讲清楚，让协作推进得更顺。

#### 未来的职业规划

我对未来的规划，其实是比较分阶段的。短期来说，我希望先把自己的工程基础打扎实，尤其是在云计算和 AI 结合这一块。我现在已经在做一些相关的项目，但我更希望能在真实业务场景里去理解，比如系统怎么设计更稳定、怎么做扩展、怎么控制成本这些。

中期的话，我希望可以成长为一个对系统有整体理解的工程师，不只是写某一块代码，而是能从架构的角度去看问题，比如怎么把不同服务组织起来，怎么让系统在高并发下也能稳定运行。

长期来说，我其实比较希望能往更偏技术+业务结合的方向走，比如参与到一些平台或者产品层面的设计里，把技术能力真正转化成业务价值。

#### 性格特质

1. 自主学习能力强：新技术出来，不是跟风，而是迅速判断它解决什么问题、代价是什么、适合什么场景。

2. 对业务有感觉：知道技术不是炫技，是服务效率、成本、增长、体验。

3. 能做复杂系统：会做架构分层、会做权衡、会做稳定性。

4. 有 ownership：看到问题会主动补位，不会只等分工。

5. 表达清晰：能把复杂技术说成人能听懂的话。

在你求学这么多年，让你自己受益最大的一种学习方式和方法是什么？ 
分享一下你自学某一个知识的例子？自学过程中最艰难的地方？ 
分享一个你压力很大的故事？最后如何克服的？ 
有没有跨团队合作的经历？遇到意见不一致如何处理？这个过程中你学到了什么？ 
分享一个就是其他周围的人给到你负面反馈的例子吗？ 
在整个 AIGC 浪潮过程中，你错失了哪些机遇？你有做对什么事情，有做错什么事情？ 
你认为优秀的工程师所需要具备的三个能力和素质？
  