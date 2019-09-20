## RDMA 实习

---

- **时间**：2019.7-2019.9
- **指导老师**：微软
- **实习内容**：我们首先自适应地根据深度学习任务和数据中心集群的网络拓扑结构来分析vm-pair间的流量情况，然后当用户申请创建虚拟机时基于蒙特卡洛搜索树确定虚拟机放置位置，保证用户申请的带宽在物理资源的层面上得到保证。具体的用户进行任务训练的时候，我们基于RoCE(RDMA over Converged Ethernet)包的dscp头来自适应地为VM-pair间的流量指定traffic class，从而根据RDMA网卡的硬件基础功能让用户带宽得到保证。
- **实习挑战**：1. 基于TCP的网络流量负载均衡技术非常成熟，但基于RDMA提供带宽保证目前(可能)还是全球首创。2.数据中心网络中的集群拓扑多种多样(Fat tree, VL2, Bcube等)，我们的系统需要自适应任意拓扑。 3. 常规的虚拟机放置算法仅考虑计算资源，我们也考虑带宽资源的合理分配。
- **实习意义**：近年来数据中心网络越来越多的应用RDMA技术来减少延迟，提高带宽，但是RDMA的QoS技术相比TCP还非常缺乏，因此如果利用机器集群来训练神经网络时，因为网络服务的质量不能保证，模型训练的效率是不能预测的。我们的工作可以自适应任意拓扑，任意分布式神经网络训练流程，可以为多租户提供带宽保证。
- **实习成果**：PARFAIT: PredictAble RDMA For AI Training 一个基于RDMA的多租户带宽保证分布式深度学习训练原型
- **参与部分**：parfait的中央逻辑控制器（brain）实现(另一个主要部分是agent，每个服务器都运行agent，agent和brain交互来控制系统运行)
- **个人能力**：这段科实习历反映了我很多可贵的能力和品质。（~~我些能力我也不知道该叫啥好~~）⚠️**这个部分是有逻辑的！逻辑是：品质背景、具体事例、反应品质。**其中事例用黑体标出：每一个事例都有论文中对应的章节，有一个解决困难的事例（效用函数设计那个）从两个角度分析出我的两个优秀品质。
  - 1.***学以致用能力***：我能在实习遇到困难时，根据专业课学过的知识制定有效的解决方案。**For example，系统的中央逻辑控制器存在单点故障问题，我提出了增加了运行时数据的冗余备份的方案，在agent端也做了相应处理。这样，在brain宕机时，agent可以自动读取备份的数据，并选择另一节点充当brain的功能。**这个解决方案的灵感来自于我大三的专业课《数据库导论》和《计算机网络导论》。没有扎实的专业课基础，是很难提出这样的想法的。
  - 2.***自学能力***：实习不同于平时作业，很多情况下没有现成的资料，需要从各种网站查阅我需要的资料：**一开始在ethereum上部署RDMA环境时，我常常去mallenox官网查看官方文档（document）和API**。**除此之外，China国内的论坛甚少包含和RDMA相关的问题，实现过程中遇到的代码问题的解决思路大部分是从Segment Fault或者一些Github Issue中获取的。**这个过程不仅需要我提取出来有用资源，有时候文档还要用我的非母语（英语）阅读，所以能够迅速找到问题的solution，体现出了我较强的自学能力。
  - 3.***沟通能力***：实习是个社交的过程，比起在校学习，提出了更高的交流要求。良好的沟通能力在工作中give me an edge：我能快速理解客户的需求；听懂上级的安排，清晰汇报工作；并且，和小组组员的沟通也十分顺利。这大大提升了组里工作的效率。（*这个要是写不下就不写了*）
- **实习收获**：学习了RDMA的相关知识，怎样在代码上进行QoS的设置和处理。另外就是看了多篇sigcomm，nsdi，nips， sosp， sysml的论文，学习了多个网络拓扑、各类分布式深度学习训练的算法（mesh based， PS， ring allreduce， 2D-torus， Hierarchical synchronization等）和基于流水等训练加速的方法。

---

>  **Abstract**:  There has been a growing trend to migrate distributed machine learning (DML) training tasks to the cloud. While cloud  providers offer guarantees for server resources, they do not offer any guarantees for network resources. The lack of bandwidth guarantees prevents tenants from predicting lower bounds on the performance of their DML training tasks. The research community has recognized this problem, but, unfortunately, prior research focused on traditional TCP/IP networks. Given the trend to build cloud network infrastructures with Remote Direct Memory Access (RDMA) technology, how to provide bandwidth guarantees for DML training tasks in RDMA networks is becoming an increasingly important but unsolved problem.
> 
> In this paper, we present our first-step efforts to solve the above problem. Our primary contribution  is the design of a virtual network abstraction, called Parfait. It accurately captures the bandwidth requirements of existing DML training tasks, enabling the physical network to support more tenants. Furthermore, we also developed an efficient enforcement scheme that can realize the proposed Parfait~abstraction in physical networks using a limited number of hardware queues.
