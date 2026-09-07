# L3 巡检记忆巩固：文献证据与迁移边界

> 检索日期：2026-09-07。用途：支持 [当前方案](../ideas/l3-memory-consolidation-agent.md)，不是系统综述，也不是技术选型冻结。
>
> 主输入为用户当前条件：固定路线/时间的人控巡检、无显式检查点、路过为主、RGB-D，后期有 LiDAR/IMU/轨迹。既有采集设计中的具体数量和设置答案不视为当前数据事实。

## 1. 检索与证据口径

围绕四组问题检索原始论文、出版社/会议页面、作者项目和官方代码：

- 长期机器人时空记忆与开放词汇场景图；
- 动态环境中的周期状态、活动发现与长期对象建模；
- LLM Agent 的记忆关联、反思和离线处理；
- 不规则时间采样、变化点以及可用感知/定位组件。

代表检索式包括 `robot long term spatio temporal memory FreMEn`、`Khronos spatio temporal metric semantic`、`robot unsupervised routine activities long term observations`、`agent memory consolidation sleep time compute`、`Understanding Lomb Scargle Periodogram`。

下表严格区分论文/官方资料中可确认的机制与本项目的迁移判断。没有复制不同数据集上的准确率进行排名；“有开源代码”也不等于已在本机复现。2026 年预印本单独标注，未用搜索引擎摘要中的宽泛性能宣传作为本项目能力承诺。

查阅深度：Khronos、RAVEN、A-MEM 查阅正文相关方法/实验或局限段落；FreMEn 查阅作者项目摘要和论文文本入口；其余主要核对原始摘要、论文页面或官方实现说明。此表支撑机制比较，不能替代后续复现时的完整精读。

## 2. 与本任务最相关的机制

| 文献 | 可核查事实 | 本项目可借鉴部分（推断） | 必须保留的边界 |
|---|---|---|---|
| **Krajník, Fentanes, Santos & Duckett：FreMEn**，IEEE T-RO 2017 | 以频率表示动态环境状态的不确定性，并将其转换回时间域预测；研究包含长时间环境数据。[论文/作者项目](https://iliad-project.eu/publications/2017-2/fremen-frequency-map-enhancement-for-long-term-mobile-robot-autonomy-in-changing-environments/) | 设施状态和事件存在的周期模型基线 | 周期建模以状态观测为前提；不完成开放对象/活动发现，也不会解决完全未覆盖时段的不可识别性 |
| **Schmid, Abate, Chang & Carlone：Khronos**，RSS 2024 | 将局部活动窗口、全局优化和片段关联结合，构建时空度量语义地图。原文局限包括局部可见/遮挡、几何关联无法关联已移动片段，以及片段规模增长。[正文](https://arxiv.org/html/2402.13817v2) | encounter/fragment 到历史实体的组织；区分短时运动和长期变化 | 不能作为开箱即用的跨日任意对象重识别器，更未直接学习巡检事件周期 |
| **Gu et al.：ConceptGraphs**，2023 预印本/2024 工作 | 把二维基础模型输出经多视角关联融合成开放词汇三维对象与关系图。[论文](https://arxiv.org/abs/2309.16650) | 不依赖人工设施清单的对象级空间骨架 | 场景图中有对象和关系，不等于形成了跨次统计规律；状态/身份误差仍需处理 |
| **Anwar et al.：ReMEmbR**，2024 预印本/ICRA 2025 | 通过带时空信息的记忆构建与检索支持机器人长视频问答和导航，并提供 NaVQA。[论文](https://arxiv.org/abs/2409.13682)、[官方实现](https://github.com/NVIDIA-AI-IOT/remembr) | Agent 可调用的时间、地点、语义检索；直接实现合理基线 | 主要验证检索问答，不是从不完整多日观察自动检验常态和周期；caption 会遗漏未描述细节 |
| **Hu et al.：RAVEN**，2026-06，预印本 v1 | 保存视觉嵌入、原图、位姿与时间，以工具调用进行多轮检索；实验含视频问答和 Go1 导航。[正文](https://arxiv.org/html/2606.25206v1) | 保留图像/片段作为可重看的证据通路，与 caption 基线比较 | 本文的 semantic memory 命名不能直接等同本项目 L3 规律巩固；未验证本任务的观测机会修正与周期归纳 |
| **Duckworth, Hogg & Cohn：Unsupervised human activity analysis for intelligent mobile robots**，Artificial Intelligence 2019 | 用人–物的定性时空关系表达观测，以概率潜变量方法发现活动概念，并增量更新。[出版社原文](https://doi.org/10.1016/j.artint.2018.12.005) | 用关系与局部时序模式聚类事件，而非只按整段文本相似性分组 | 感知、人体/关键对象表示有结构前提；不是任意开放视频中零假设发现所有活动 |
| **Patel & Chernova：Proactive Robot Assistance via Spatio-Temporal Object Modeling**，CoRL 2022/PMLR 2023 | 研究基于日常规律预测对象位置以支持主动协助；贡献模拟家庭的长期对象移动数据 HOMER。[会议页](https://proceedings.mlr.press/v205/patel23a.html) | 通过后续对象状态预测检验记忆是否有用 | 模拟家庭/对象移动设定不同于真实巡检路过的视频感知；不可直接迁移性能 |
| **Xu et al.：A-MEM**，2025 | 构建带属性的记忆笔记、动态连接和记忆演化，主要评测长期对话问答。[论文](https://arxiv.org/abs/2502.12110)、[正文 v11](https://arxiv.org/html/2502.12110v11) | 候选记忆关联、按新信息触发修订 | 动态改写记忆不等于物理事实经过检验；本项目需追加版本、原始证据及统计检查 |
| **Park et al.：Generative Agents**，2023 | 架构含观察、记忆、反思和规划，评测重点是模拟人物行为的可信程度。[论文](https://arxiv.org/abs/2304.03442) | 从多条经历提出高层候选解释的流程 | 行为可信并不代表规律客观为真；不能用反思文字取代反例与独立日期验证 |
| **Lin et al.：Sleep-time Compute**，2025 | 在用户查询之前离线处理上下文，在数学推理任务和 SWE 案例中研究计算分配。[论文](https://arxiv.org/abs/2504.13171) | 巡检后集中聚合、预计算和检验的调度思路 | 不是机器人周期事件的验证，也不能据此给出本任务成本收益数字 |
| **VanderPlas：Understanding the Lomb–Scargle Periodogram**，2017 预印本/ApJS 2018 | 解释不均匀采样的周期估计、窗口函数、混叠与实践局限。[论文](https://arxiv.org/abs/1703.09824)、[期刊原文](https://doi.org/10.3847/1538-4365/aab766) | 检查时间相位覆盖、采样窗和备选周期 | 不是针对二元事件与删失视频窗口的完整模型；“允许不规则采样”不意味着可修复无覆盖 |
| **Adams & MacKay：Bayesian Online Changepoint Detection**，2007 | 在线维护最近变化点及其后持续长度的概率分布。[论文](https://arxiv.org/abs/0710.3742) | 作为稳定常态与持续漂移的统计比较工具 | 原模型有分段独立等假设；不区分感知错误、实体混淆和环境变化，需上游诊断 |

## 3. 可替换的实现组件

以下用于构建候选工具，不作为整套系统已经成熟的证据。

| 组件 | 原始/官方入口 | 能力与调用边界 |
|---|---|---|
| RTAB-Map | [官方仓库](https://github.com/introlab/rtabmap)、[官方多会话教程](https://github.com/introlab/rtabmap/wiki/Multi-Session-Mapping-with-RTAB-Map-Tango) | RGB-D 等输入的建图与多会话处理候选；是否适用于实际室外深度、运动和传感器配置需要试跑 |
| SAM 2，Ravi et al.，2024 | [论文](https://arxiv.org/abs/2408.00714)、[官方实现](https://github.com/facebookresearch/sam2) | 提示式图像/视频分割、掩码传播；需要候选初始化，不提供活动含义或长期身份保证 |
| Grounding DINO，Liu et al.，2023/2024 | [论文](https://arxiv.org/abs/2303.05499)、[官方实现](https://github.com/IDEA-Research/GroundingDINO) | 以文本定位开放类别；文字可以由 Agent 从观测提出，但文本条件本身仍是先验 |
| DINOv2，Oquab et al.，2023 | [论文](https://arxiv.org/abs/2304.07193)、[作者项目](https://dinov2.metademolab.com/) | 通用视觉特征，可测试空间/实例相似性及状态聚类；必须实测视角和细微状态敏感性 |

未在本次工作中安装、执行或测速这些组件。实施时记录精确模型权重、代码提交、依赖版本、硬件、输入参数和可控随机种子。

## 4. 综合判断与反证要求

**综合判断 1：检索记忆与 L3 巩固应分开验收。** ReMEmbR/RAVEN 主要说明历史证据可以被检索用于问答或导航；本项目还必须验证从多次经历抽象出的分布/规律，以及对新日期的适用性。不能只凭“检索出几张类似图片”判定学到了周期。

**综合判断 2：无检查点首先改变观察组织方式。** 对象和地点要由数据关联得到，且对象“该不该被看见”是单独的推断。几何和视觉记忆提供实现基础，但只有对特定属性的可判断性经过校准，缺席统计才有意义。

**综合判断 3：Agent 的候选解释必须经过外部检查。** 反思和关联机制可帮助提出问题，是否为真仍取决于原始片段、去重统计、覆盖条件、备选解释和时间外验证。用更多语言推理代替新增证据会形成自我强化。

**综合判断 4：现有数据的因果/全天解释存在边界。** 固定路线与固定时刻下，时间、地点、操作员行为可能无法分离。本方案优先报告观测条件下的模式，并将不可区分的解释保留为待验证；不把“规律是人为设置的”当成可见证据。

本次选读支持这些模块作为研究起点，但不足以宣称“本领域尚无相关方法”或证明整体方案新颖。后续需要针对最接近的时空模式归纳方法进行复现和系统比较。

## 5. 后续精读顺序

1. FreMEn：状态输入、模型阶数、采样方式和评测协议，落实周期基线。
2. Khronos：片段生命周期、可见性/射线证据、关联局限；决定借鉴表示还是集成实现。
3. ReMEmbR 与 RAVEN：搭建等预算 caption/视觉检索基线。
4. Duckworth et al.：分析不完整活动片段、关系表示和增量聚类的适用性。
5. VanderPlas 与变化点方法：冻结时间候选、覆盖检查和漂移评测。
6. A-MEM 与离线处理工作：借鉴任务调度和关联机制，保留物理证据与统计判据。
