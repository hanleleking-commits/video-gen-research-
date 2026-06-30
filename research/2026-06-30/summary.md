# 过去 7 天视频生成研究进展日报：2026-06-30

检索窗口：2026-06-23 至 2026-06-30。优先使用 arXiv、项目页、GitHub/官方仓库；未纳入只在二手媒体出现、或发布时间早于窗口且无明确本周更新的内容。

## 1. 本日摘要

过去 7 天没有看到 Veo / Sora / Seedance / Wan / HunyuanVideo 级别的新公开视频底座发布，主线集中在“把视频生成推向 world model”和“让生成可控、可评测、可复现”。最明显升温的是物理一致性与具身场景：PhysisForcing、PhysRAG、MemoBench 都在追问视频模型是否真的理解动态世界，而不是只生成视觉上合理的片段。长程与交互控制方面，Directing the World 用自回归长视频框架组合人类动作与相机控制，是本周期最值得跟的 world-model 方向论文之一。视频编辑方向出现更强几何显式控制，GIVE 把对象级 3D 状态变化作为编辑条件，目标是减少编辑后的阴影、反射、时序不一致。工业系统方面，Recommendation-as-Generation 说明“按用户兴趣即时生成视频”开始进入推荐/广告闭环，但研究可复现性较弱。评测方面，MemoBench 补上了动态遮挡后记忆一致性的空白，值得和 EntityBench、WorldReasonBench 一起跟踪。Tokenizer/VAE、原生音视频一体化、低 VRAM 部署这周没有看到高质量新增主线。

## 2. 最值得关注的 7 个进展

1. **Directing the World: Fast Autoregressive Video Generation with Compositional Human-Camera Control**  
类型：论文 / 项目页  
链接：[arXiv](https://arxiv.org/abs/2606.27964)；项目页见论文摘要中的链接  
发布时间：2026-06-26  
所属方向：7 交互式世界模型；6 长视频；8 控制与相机  
贡献：提出自回归长视频 world model 框架，同时组合 SMPL 人体动作控制和相机轨迹控制，并用 Fast-Slow Memory 稳定长程 rollout。重点在“人是可控 agent，而不是被动视觉元素”。([arxiv.org](https://arxiv.org/abs/2606.27964))  
重要性：高  
推荐阅读优先级：必读

2. **PhysisForcing: Physics Reinforced World Simulator for Robotic Manipulation**  
类型：论文  
链接：[arXiv](https://arxiv.org/abs/2606.28128)  
发布时间：2026-06-26  
所属方向：7 world model；10 评测可靠性；3 训练 recipe  
贡献：面向机器人操作视频生成，引入像素轨迹对齐和语义关系对齐来强化物理一致性；官方声称可提升 Wan2.2-I2V-A14B、Cosmos3-Nano 在 R-Bench 上的表现，并提升 closed-loop success rate。([arxiv.org](https://arxiv.org/abs/2606.28128?utm_source=openai))  
重要性：高  
推荐阅读优先级：必读

3. **PhysRAG: Enhancing Physics-Awareness in Video Generation via Retrieval-Augmented Generation**  
类型：论文 / GitHub / 数据 / 模型  
链接：[arXiv](https://arxiv.org/abs/2606.26916)，[GitHub](https://github.com/sediment1024/PhysRAG)  
发布时间：2026-06-25  
所属方向：3 数据与 recipe；7 world model；10 评测  
贡献：用物理视频检索库和 learnable query 把物理参考注入视频扩散模型；仓库已给出代码、数据、模型、Wan2.2 复现路径和 PhyGenBench/VideoPhy2/VBench wrappers。([arxiv.org](https://arxiv.org/abs/2606.26916?utm_source=openai)) ([github.com](https://github.com/sediment1024/PhysRAG))  
重要性：高  
推荐阅读优先级：必读

4. **MemoBench: Benchmarking World Modeling in Dynamically Changing Environments**  
类型：benchmark / 论文  
链接：[arXiv](https://arxiv.org/abs/2606.27537)  
发布时间：2026-06-25  
所属方向：10 评测；7 world model  
贡献：提出 disappear-and-reappear 动态记忆评测：目标物体在不可见期间发生物理变化，模型需要在重现时恢复正确状态。该 benchmark 直接测试视频模型的动态记忆，而不只是可见帧一致性。([arxiv.org](https://arxiv.org/abs/2606.27537?utm_source=openai))  
重要性：高  
推荐阅读优先级：必读

5. **Geometry-Instructed Video Editing / GIVE**  
类型：论文 / 项目页  
链接：[arXiv](https://arxiv.org/abs/2606.24225)，[项目页](https://geometry-instructed-video-editing.github.io/give/)  
发布时间：2026-06-23  
所属方向：8 控制、编辑、相机  
贡献：用 depth-box 和 orientation-box 表达对象编辑前后的 3D 状态，支持移动、旋转、缩放、复制、移除等对象级视频编辑，并用图形引擎生成 paired supervision。([arxiv.org](https://arxiv.org/abs/2606.24225?utm_source=openai))  
重要性：中高  
推荐阅读优先级：必读

6. **Recommendation as Generation: Unifying Personalized Video Generation and Recommendation at Industrial Scale**  
类型：论文 / 工业系统  
链接：[arXiv](https://arxiv.org/abs/2606.25496)  
发布时间：2026-06-24  
所属方向：3 训练数据与 recipe；5 音视频生成；6 多镜头/系统化生成  
贡献：把推荐系统从“匹配已有视频池”改成“按用户兴趣生成视频”，用 semantic IDs 连接推荐表征与视频生成 agent，并在广告场景做线上 A/B。可复现性较弱，但代表工业闭环方向。([arxiv.org](https://arxiv.org/abs/2606.25496?utm_source=openai))  
重要性：中  
推荐阅读优先级：可读

7. **EO-WM: A Physically Informed World Model for Probabilistic Earth Observation Forecasting**  
类型：论文 / benchmark  
链接：[arXiv](https://arxiv.org/abs/2606.27277)  
发布时间：2026-06-25  
所属方向：7 world model；10 评测  
贡献：把多光谱地球观测预测视作 weather-conditioned world modeling，引入气候基线、天气异常和累积物理压力信号，并设计极端夏季与季节 matched-pair 评测。更偏遥感，但对“物理条件驱动的视频扩散 world model”有参考价值。([arxiv.org](https://arxiv.org/abs/2606.27277?utm_source=openai))  
重要性：中  
推荐阅读优先级：可读

## 3. 按方向分类总结

### 1. 基础视频底座 / Video Foundation Models
本周期暂无高质量新增。没有看到新的 Wan / HunyuanVideo / LTX / Seedance / Veo / Sora 类公开视频 foundation model 发布。现有新增更多是基于 Wan2.2、Cosmos3-Nano 或自回归框架做控制、物理一致性和评测增强。

### 2. 视频表征层：VAE / tokenizer / latent representation
本周期暂无高质量新增。TivTok 是近期重要 tokenizer 工作，但发布时间为 2026-06-16，超出本次 7 天窗口；本日报不纳入主列表。

### 3. 训练数据与训练 recipe
PhysRAG 的两阶段物理视频过滤、物理检索库和可复现训练流程最值得关注。PhysisForcing 用轨迹与语义关系监督强化物理区域，也属于更“任务化”的后训练 recipe。RaG 则展示工业侧把用户反馈、生成质量和兴趣对齐放入统一 reward learning 闭环。

### 4. 生成范式
Directing the World 是本周最明确的自回归视频生成进展，强调长 horizon 与条件组合。PhysRAG、PhysisForcing 仍主要建立在视频 diffusion / DiT 生态上，但关注点从视觉质量转向物理约束和可控未来预测。

### 5. 原生音视频一体化
本周期暂无高质量新增。RaG 提到 video generation agents 包含 audio alignment，但它不是原生联合音视频生成模型论文，更多是工业生成系统。

### 6. 长视频、多镜头与叙事一致性
Directing the World 直接处理长程 rollout、人类动作和相机轨迹组合。RaG 与长视频 agent 编排相关，但核心目标是推荐/广告个性化，不是叙事一致性研究。多镜头实体一致性本周没有新的 benchmark 级发布。

### 7. 交互式世界模型 / Video World Models
这是本周期最热方向。Directing the World 关注可控人-相机 world exploration；PhysisForcing 关注机器人操作中的物理一致性；MemoBench 把“动态环境中的记忆恢复”变成诊断 benchmark；EO-WM 把遥感预测纳入 world model 范式。

### 8. 控制、编辑、相机与实时 V2V
GIVE 是本周控制/编辑方向最清晰的新增，使用显式几何状态变化来约束视频编辑。Directing the World 也属于相机控制与人体控制结合。实时 V2V 本周期暂无高质量新增。

### 9. 推理加速与部署工程
本周期暂无高质量新增。Directing the World 使用“fast autoregressive”表述，但主要贡献不在低 VRAM、量化、蒸馏或 serving pipeline。

### 10. 评测、安全与可靠性
MemoBench 是本周最值得跟的评测新增，补动态遮挡记忆。PhysRAG 使用 PhyGenBench、VideoPhy2、VBench wrappers，说明物理一致性评测正在进入复现实验链路。PhysisForcing 引入 R-Bench、PAI-Bench、EZS-Bench 与 WorldArena protocol，值得观察这些评测是否会形成共识。

## 4. 技术趋势判断

明显升温：world model、物理一致性、动态记忆、机器人/具身视频模拟、几何显式控制。

正在成为主流的路线：以公开视频 foundation model 为底座，通过后训练、adapter、检索增强、显式几何/轨迹条件注入来补齐可控性和可靠性；评测从 FVD/VBench 式整体质量，转向状态记忆、物理关系、行动后果和 closed-loop utility。

仍未解决的问题：长程自回归的误差积累；遮挡后对象状态更新；接触、碰撞、形变等物理过程；跨镜头角色/物体/地点一致性；真实交互式 world model 的低延迟；评测指标与人类/下游任务成功率之间的稳定相关性。

可能只是工程包装、研究增量有限的方向：单纯把现有视频模型接进 agent pipeline、推荐系统或多步骤工作流，如果没有公开数据、消融和可复现代码，研究价值应谨慎看待。RaG 的工业结果有参考意义，但短期更适合作为系统趋势跟踪，而不是可复现研究基线。

## 5. 论文精读候选

1. [Directing the World](https://arxiv.org/abs/2606.27964)  
为什么读：自回归长视频 + 人体动作 + 相机控制的组合问题很关键。  
重点 section：Method 中 MMPL backbone、Fast-Slow Memory、SMPL 条件注入、causal camera control。  
新意：不是单一 ControlNet 或短视频 motion transfer，而是把人作为可控 agent 放进长程 world rollout。

2. [PhysisForcing](https://arxiv.org/abs/2606.28128)  
为什么读：把物理一致性监督落到机器人操作场景，并报告 downstream policy / closed-loop 改善。  
重点 section：physics-informative regions、pixel trajectory alignment、semantic relational alignment、WorldArena protocol。  
新意：相比纯视觉 fine-tuning，更强调接触关系和操作物理。

3. [PhysRAG](https://arxiv.org/abs/2606.26916)  
为什么读：代码、数据、模型路径相对完整，适合复现。  
重点 section：data filtering、physical video database、learnable-query injection、VBench/PhyGenBench evaluation。  
新意：把 RAG 从文本/图像知识扩展到物理视频参考检索，用于视频生成的物理规律补强。

4. [MemoBench](https://arxiv.org/abs/2606.27537)  
为什么读：评测问题定义清楚，针对动态遮挡后的状态记忆。  
重点 section：disappear-and-reappear task design、four diagnostic pillars、model failure analysis。  
新意：不只测“实体是否一致”，而是测实体不可见期间发生变化后能否正确恢复。

5. [GIVE](https://arxiv.org/abs/2606.24225)  
为什么读：对象级几何编辑是视频编辑落地的核心能力。  
重点 section：object-state formulation、depth-box/orientation-box、graphics-engine paired data pipeline。  
新意：把编辑指令变成前后 3D 状态约束，减少自然语言编辑的不确定性。

## 6. 开源与复现实用资源

- [PhysRAG GitHub](https://github.com/sediment1024/PhysRAG)：值得复现。仓库包含代码、数据/模型下载入口、Wan2.2 base model 依赖、训练和推理说明，并说明已 smoke-test 49 帧 704×480 推理。([github.com](https://github.com/sediment1024/PhysRAG))  
- [PhysRAG Hugging Face dataset/model](https://github.com/sediment1024/PhysRAG)：值得跟。README 链出 dataset 和 model，适合先跑单 prompt inference，再评估训练成本。([github.com](https://github.com/sediment1024/PhysRAG))  
- [GIVE 项目页](https://geometry-instructed-video-editing.github.io/give/)：建议跟踪代码是否释放。当前从 arXiv 摘要可见项目页，但未确认完整开源。([arxiv.org](https://arxiv.org/abs/2606.24225?utm_source=openai))  
- Directing the World 项目页：建议跟踪数据和代码释放。论文称会 release curated datasets，但本次检索未确认 GitHub/weights 可用。([arxiv.org](https://arxiv.org/abs/2606.27964))  
- MemoBench：建议跟踪 benchmark 数据和 evaluator 是否开放。论文定义有价值，但本次检索未确认仓库可用。([arxiv.org](https://arxiv.org/abs/2606.27537?utm_source=openai))  

## 7. 与长期研究地图的关系

- 视频底座平台化：本周没有新底座，更多是围绕 Wan2.2、Cosmos3-Nano 等底座做增强。
- tokenizer / VAE 基础设施：本周空档；近期 TivTok 值得下周继续观察代码释放。
- 原生音视频：本周没有强新增。
- 长视频与叙事一致性：Directing the World 推进长程可控 rollout；RaG 体现工业多步骤视频生成闭环。
- world model 化：本周主线最强，PhysisForcing、PhysRAG、MemoBench、EO-WM 都在把视频生成转向物理/状态/行动后果。
- 实时与低成本部署：本周没有主线新增。
- 可靠评测与安全：MemoBench 和 PhysRAG/PhysisForcing 的评测链路值得持续跟踪；安全、水印、版权治理本周没有高质量新增技术工作。

## 8. 下周值得继续跟踪的问题

1. Directing the World 是否会放出代码、数据过滤 pipeline 或模型权重？
2. PhysisForcing 是否会开源训练代码，以及 Wan2.2/Cosmos3-Nano 改进是否能被第三方复现？
3. PhysRAG 的 Hugging Face 数据和 checkpoint 是否稳定可用，社区能否复现 PhyGenBench / VBench 提升？
4. MemoBench 是否发布数据、评测脚本和主流模型榜单，是否会成为 WorldReasonBench/EntityBench 之后的新标准？
5. GIVE 是否开源图形引擎数据生成 pipeline；如果开源，它可能成为对象级视频编辑训练数据的重要来源。
