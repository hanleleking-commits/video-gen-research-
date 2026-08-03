# 数字人、世界模型与互动作品研究日报：2026-08-03

检索窗口：2026-07-28 至 2026-08-03。  
检索截止：2026-08-03 18:01（UTC+8）。

## 1. 本日摘要

本日新增主要来自 8 月 3 日公开的 arXiv 批次；论文实际提交时间集中在 7 月 30—31 日，以下均以官方提交时间为准。数字人方向没有新的实时 talking-head、口型同步、全双工对话或端侧部署突破，增量转向单图可动画 3D 资产：S-Avatar解决头部新视角与表情控制，Forwardrobe把宽松服装从身体层中分离，OASIS则集中处理高遮挡手部。世界模型方向出现两条值得重视的路线：PhiZero将未来变化压缩为离散“物理语言”，从直接生成像素转向“先推理状态变化、再渲染”；WCM则把未来状态预测嵌入机器人强化学习的 critic，而不是把世界模型只当视频生成器。BWM的新技术报告把已有开源推理栈、WorldArena排名和实机实验整合成完整方法，但论文声称开放训练代码，仓库目前仍显示训练部分“Coming soon”，应降级为部分开源。ACE-Data-0提供同时对齐视频、人体、手部、物体6-DoF、音频和触觉的长时数据，为数字人动作、机器人世界模型和接触评测建立共同数据层，但尚无公开下载及许可证入口。Coding × 互动作品方面，最明确的新接口不是通用产品API，而是结构化中间表示：物理语言token、轨迹、分离式服装层、FLAME控制、机器人动作序列和结构化prompt JSON。总体趋势是代码管理权威状态、轨迹和资产绑定，生成模型负责外观、动作细节与难以手写的未来响应；真正达到正式产品或规模化部署的新增仍为空。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | 单图头部、手部、宽松服装分别获得3DGS专用表示 | 从整人重建转向可独立控制的身体组件；均未进入产品 | 上期偏实时音视频，本日偏3D资产与动画管线 |
| 世界模型 | 高 | PhiZero离散化状态变化；WCM把未来预测写入critic；BWM补齐技术报告 | BWM已有权重和推理代码；其余主要为论文demo | 从持续视频生成转向显式中间状态和策略评估 |
| Coding × 互动作品 | 中高 | 轨迹、物理token、FLAME、动作序列和结构化JSON成为模型接口 | 出现可组合组件，但缺Unity/Unreal插件和线上SLA | 相比上期正式NPC平台，本日更偏底层生产管线 |

## 3. 最值得关注的 10 个进展

### 1. PhiZero：用离散“物理语言”表达世界状态变化

- **类型 / 时间 / 主线：** 论文、项目demo；2026-07-30；世界模型、Coding × 运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2607.28624)、[项目页](https://phi-zero.github.io/)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 将相邻视频状态的变化编码为离散token，采用“reason then render”：Qwen3-VL-4B初始化的reasoner先预测变化序列，Wan2.2-5B扩散解码器再生成视频。项目页称33帧、4秒视频只需256个物理token，对比Wan VAE的44,800个连续token，压缩约175倍。
- **Coding接口：** 文本动作意图、初始帧和物理token可以分别由agent、状态机和生成器处理；token可保存、重放、替换初始外观或用于搜索候选未来。
- **互动/工业场景：** 机器人预执行、可编辑物理动画、跨角色动作迁移、生成式仿真。
- **成熟度 / 证据：** 作者demo；代码仍为“Soon”，指标均为论文作者评测，无第三方验证。
- **重要性 / 阅读优先级：** 高 / 必读。

### 2. WCM：把未来预测能力加入机器人强化学习critic

- **类型 / 时间 / 主线：** 论文、实机实验；2026-07-31；世界模型、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29613)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 指出单帧value estimator不能充分表示部分可观测机器人状态；WCM以轻量LeJEPA同时预测未来latent和估计价值，使critic通过世界建模目标学习跨时序动态。
- **Coding接口：** 可接入on-policy和off-policy RL，并与π0、π0.5、OpenVLA-OFT组合；代码层面对应`history → latent predictor/value head → advantage/replay update`。
- **互动/工业场景：** 机器人策略后训练、失败预判、低样本真实任务适配。
- **成熟度 / 证据：** 作者报告149项仿真任务及7项实机任务；未确认公共代码、延迟或算力。
- **重要性 / 阅读优先级：** 高 / 必读。

### 3. BWM：动作条件机器人世界模拟器发布完整技术报告

- **类型 / 时间 / 主线：** 论文、GitHub、权重、benchmark；论文2026-07-31；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29302)、[GitHub](https://github.com/boundless-large-model/boundless-world-model)、[权重](https://huggingface.co/BLM-Lab/Boundless-World-Model)、[WorldArena](https://github.com/tsinghua-fib-lab/WorldArena)。
- **相对上期的新变化：** 仓库和推理权重早于本窗口存在；本次因完整技术报告、实机及数据引擎/策略评估实验公开而纳入。
- **核心贡献：** 基于Wan2.2-TI2V-5B，以初始环境、动态视觉历史和时间对齐机器人动作自回归预测后续观测；既生成训练rollout，也评价候选策略。
- **Coding接口：** Apache-2.0仓库包含checkpoint、示例动作、归一化统计及推理脚本；输入协议比自然语言更适合机器人控制栈。
- **限制：** 摘要声称开放训练代码，但当前README仍标注训练部分“Coming soon”；不能称为完整可复现。
- **成熟度 / 证据：** 推理代码与权重已开源、作者实机实验；未披露单次rollout延迟和GPU需求。
- **重要性 / 阅读优先级：** 高 / 必读。

### 4. ACE-Data-0：统一视频、人体、物体、触觉和音频的长时数据层

- **类型 / 时间 / 主线：** dataset、benchmark、采集系统；2026-07-30；世界模型、数字人基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.28625)、[项目页](https://ace-data-engine.github.io/ACE-Data-0/)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 150小时、1,700万帧、200类任务、50名参与者和75,000段交互；同步记录ego/exo视频、全身与手部动作、对象mesh与6-DoF、60Hz运动状态、音频和手掌触觉。
- **Coding接口：** 共享时钟、世界坐标、相机内外参、接触标签和语言时间线可直接形成world-state schema，连接动作学习、数字人重定向与世界模型训练。
- **互动/工业场景：** 家务机器人、训练模拟、全身数字人、手物交互、教育动作捕捉。
- **成熟度 / 证据：** 数据采集与benchmark论文；项目页未提供下载、GitHub或许可证，暂属“宣布发布、实际入口待核验”。
- **重要性 / 阅读优先级：** 高 / 必读。

### 5. Forwardrobe：将宽松服装变成独立可控的Gaussian资产

- **类型 / 时间 / 主线：** 论文、3D avatar；2026-07-31；数字人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29106)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 从单图前馈重建人体，并在canonical Gaussian空间显式分离身体和服装；服装层使用连续性几何、skinning初始化、姿态条件非刚性变形和外观适配，重点改善裙装。
- **Coding接口：** 独立服装层可被资产ID、骨骼姿态和编辑参数控制，理论上可导入试衣或角色装备系统。
- **互动/工业场景：** 虚拟试衣、可换装NPC、数字模特、服装预演。
- **成熟度 / 证据：** 论文作者实验；未确认代码、标准资产导出或实时帧率。
- **重要性 / 阅读优先级：** 高 / 必读。

### 6. S-Avatar：单图头部3DGS接入FLAME表情控制

- **类型 / 时间 / 主线：** 论文、demo；2026-07-30；数字人、3D/4D表示。
- **官方链接：** [论文](https://arxiv.org/abs/2607.28164)、[项目入口](https://github.com/Hailsong/S-Avatar)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 先由扩散模型生成高分辨率canonical 3DGS，再对齐FLAME，并以binding template将Gaussian形变绑定到FLAME参数。
- **Coding接口：** FLAME expression、pose和shape参数可由音频驱动器、动捕或状态机持续更新；确定性rig负责控制，3DGS负责外观渲染。
- **互动/工业场景：** VR/AR头像、游戏对话角色、数字员工前端。
- **成熟度 / 证据：** 作者称可实时渲染，但未披露设备、FPS、首帧构建时间；仓库当前以项目展示为主。
- **重要性 / 阅读优先级：** 中高 / 必读。

### 7. FlexComposer：以程序轨迹统一静态和动态素材的视频合成

- **类型 / 时间 / 主线：** 论文、视频创作组件；2026-07-31；Coding × 互动内容。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29627)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 将静态图片或带自身动作的视频素材规范化到居中latent，再利用VAE latent的平移等变性沿用户轨迹注入目标视频；无需显式3D重建或额外可学习adapter。
- **Coding接口：** 轨迹可由鼠标路径、关键帧、物理引擎或agent输出；适合表示为时间戳化二维路径和缩放/遮挡参数。
- **互动/工业场景：** 商品演示、互动广告、虚拟制片、用户拖放角色后生成合成片段。
- **成熟度 / 证据：** 论文实测；未确认代码、API和生成时延。
- **重要性 / 阅读优先级：** 中高 / 必读。

### 8. MoRoute：让每个视频DiT block动态选择VLM语义层

- **类型 / 时间 / 主线：** 论文、项目demo；2026-07-31；视频基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29545)、[项目页](https://orange-3dv-team.github.io/MoRoute/)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 冻结VLM与预训练视频DiT保持异构，由轻量router为每个DiT block选择最相关的VLM层；图片和源视频作为in-context token统一进入生成序列。
- **Coding接口：** 同一模型可通过不同条件组合执行生成、参考保持和编辑，减少工作流中多模型切换。
- **互动/工业场景：** 角色/商品参考驱动视频、分支叙事素材、批量视频编辑。
- **成熟度 / 证据：** 项目demo；作者报告三个benchmark平均提升0.15、0.18和0.34分（1—5量表），无第三方验证和公开代码。
- **重要性 / 阅读优先级：** 中 / 可读。

### 9. OASIS：针对自遮挡手部的单图Gaussian avatar

- **类型 / 时间 / 主线：** 论文、项目demo；2026-07-31；数字人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29633)、[项目页](https://mova-hand.github.io/MOVA/)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 以可见性感知的point-image attention把图像证据传给3D手部token，并用Feature-on-Mesh处理关节运动造成的局部拉伸。
- **Coding接口：** 可由MANO/手部骨骼姿态驱动，后续可接动捕、手势识别和商品交互系统。
- **互动/工业场景：** VR手部、数字人手势、虚拟试戴、精细商品展示。
- **成熟度 / 证据：** 作者demo；项目页声称论文接收后开放代码，但链接当前404，尽管论文已标注ACM MM 2026接收，属于待兑现资源。
- **重要性 / 阅读优先级：** 中 / 可读。

### 10. Context Scaling：结构化prompt成为可调用的视觉生成协议

- **类型 / 时间 / 主线：** 论文、GitHub、权重、FastAPI demo；2026-07-31；Coding × 生成基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.29679)、[GitHub](https://github.com/heheyas/context-scaling)、[模型集合](https://huggingface.co/collections/heheyas/context-scaling)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 发现生成损失与prompt中的结构化信息量关系更强，而非简单与文本长度相关；引入带语义、几何和bounding box字段的JSON prompt，并训练LLM prompter。
- **Coding接口：** 已提供`/api/generate_sp`、`/api/caption_image`和`/api/generate_image`三个JSON端点，可作为agent到视觉模型的中间协议。
- **互动/工业场景：** 可编辑角色/场景描述、品牌素材生成、生成任务CI、视觉agent工具调用。
- **成熟度 / 证据：** Apache-2.0代码、模型和demo已开放；本地demo需两张GPU，首次约下载110GB。
- **重要性 / 阅读优先级：** 中高 / 必读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** 本周期新增长在3D组件，不在音频驱动视频。S-Avatar用FLAME控制头部，OASIS处理手部，Forwardrobe分离服装；三者可共同指向模块化全身角色，但当前没有统一骨骼、坐标、材质和导出协议。

**3D/4D表示。** 3DGS继续成为单图avatar的主流表示，但研究开始弥补其结构性弱点：用FLAME提供头部rig，用mesh特征处理手部表面拉伸，用独立Gaussian层表达宽松衣物。新增的[FillGS](https://arxiv.org/abs/2607.29284)则主动选择视角—时间位置，让生成模型补足4DGS观测缺口，但仍可能引入虚构纹理或错误几何。

**实时交互。** S-Avatar声称实时渲染，不能等同于实时创建、流式语音驱动或多人并发。最稳妥的产品架构仍是传统rig和状态机负责实时控制，神经表示只承担渲染。

**音视频与情感表达。** 相对上期暂无新的高质量口型、情绪、视线、active listening或可打断对话模型；不应以3D重建论文替代这部分增量。

**工程部署。** 当前最可能先产品化的是服装层、FLAME参数和手部骨骼等离线资产接口。欠缺项包括Unity/Unreal插件、GLB/FBX导出、移动端性能、LOD、碰撞体和失败回退。

**安全与身份治理。** 本日没有新的高质量授权、水印、深伪检测或肖像治理进展。单图avatar降低建模门槛，也同步提高了参考图授权、身份冒用和资产来源记录的重要性。

## 5. 世界模型进展

**架构与训练。** PhiZero把状态变化离散化；WCM把未来预测作为critic辅助目标；BWM仍采用动作条件视频生成。三者分别对应“显式变化token”“隐式预测表征”和“像素级rollout”三类路线。

**可控/可玩生成。** 本日最强控制来自机器人动作和物理token，而非键鼠式游戏控制。PhiZero的token理论上更适合程序检索、编辑和复用，但尚无公开tokenizer或交互API。

**长时与物理一致性。** ACE提供长达分钟级连续活动及权威物体状态；BWM以历史帧保持状态；但没有新工作证明开放世界中对象库存、任务变量和因果状态长期正确。

**空间表示。** 相机、对象6-DoF、mesh、手部关节、触觉和物理token正在形成像素之外的状态协议。相比单纯追加历史帧，这些接口更适合代码验证。

**机器人/仿真。** WCM服务策略训练，BWM服务数据生成与策略评估，ACE服务训练和评测数据，已形成相对完整的“数据—模拟—critic—实机”链路。

**实时部署与评测。** BWM默认配置基于5B模型并使用50步推理，尚无实时证据；WCM未披露critic推理开销；PhiZero代码未开源。世界模型目前更适合离线预执行和训练，而不是机器人低层安全控制。

## 6. Coding × 新型互动作品

1. **文本/agent动作 → PhiZero物理token → 视频解码器 → 可编辑的生成式物理片段**  
   代码负责任务状态、token保存、候选分支和回滚；模型负责推断变化与渲染。已有作者demo，公共API缺失。

2. **机器人动作序列 → BWM rollout → WorldArena/任务评分 → 策略选择 → 实机执行**  
   控制器保留安全边界，世界模型只提供视觉预测，评价器排序候选策略。已有权重、推理脚本和作者实机证据；非实时。

3. **交互历史 → WCM未来latent/value → RL更新 → 更稳健的VLA策略**  
   代码负责replay buffer、advantage和训练循环；WCM学习跨时序状态；机器人控制器执行最终动作。论文有149项任务与7项实机结果，代码待开放。

4. **单图人物 → 身体/服装Gaussian分层 → 骨骼动作或MoRAE动作 → 可换装角色**  
   [MoRAE](https://arxiv.org/abs/2607.29180)可生成动作latent，Forwardrobe提供服装层，传统引擎负责骨骼、碰撞和网络同步。组合方案为本报告推断，各组件尚无统一导出标准。

5. **用户拖拽轨迹 → FlexComposer latent注入 → 商品或角色合成视频 → 互动广告**  
   前端将鼠标或触控轨迹转换为时间化路径；生成器完成阴影、光照和背景融合；播放器管理分支。已有论文证据，无实时API。

6. **自然语言/图片 → Context Scaling JSON → 生成端点 → 自动验收与版本化素材**  
   agent负责生成或修改结构化字段；视觉模型负责渲染；CI比较seed、bbox和品牌规则。代码、权重和FastAPI已开放，是本日最可立即验证的Coding链路。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | S-Avatar＋Forwardrobe＋骨骼引擎 | 表情、动作、换装 | 研究组件 | 未披露 | 引擎插件、LOD、碰撞 | [S-Avatar](https://arxiv.org/abs/2607.28164) |
| 影视/虚拟制作 | FlexComposer＋轨迹编辑器 | 关键帧或路径控制素材 | 论文原型 | 未披露 | 生成时延、遮挡、精修 | [论文](https://arxiv.org/abs/2607.29627) |
| 直播与电商 | Forwardrobe＋商品/服装资产 | 换装、试穿、展示 | 研究原型 | 未披露 | 服装真实性、体型泛化 | [论文](https://arxiv.org/abs/2607.29106) |
| 品牌互动 | Context Scaling＋FastAPI | 修改JSON字段生成版本 | 可运行原型 | 两张GPU；约110GB下载 | 审核、品牌一致性 | [GitHub](https://github.com/heheyas/context-scaling) |
| 教育培训 | ACE数据＋动作生成＋3D角色 | 示范和动作重放 | 数据/研究原型 | 未披露 | 数据尚无下载入口 | [ACE](https://ace-data-engine.github.io/ACE-Data-0/) |
| 企业数字员工 | S-Avatar＋传统ASR/LLM/TTS | FLAME参数驱动表情 | 机会判断 | 未披露 | 口型、插话、并发 | [论文](https://arxiv.org/abs/2607.28164) |
| 陪伴与社交 | 3D头部＋服装＋动作状态机 | 长时角色表演 | 机会判断 | 未披露 | 记忆、审核、身份授权 | [Forwardrobe](https://arxiv.org/abs/2607.29106) |
| 空间计算 | 3DGS avatar＋FLAME | 头手实时渲染 | 作者demo | FPS/设备未披露 | 端侧预算、视角伪影 | [OASIS](https://mova-hand.github.io/MOVA/) |
| 机器人/仿真 | ACE＋BWM＋WCM＋VLA | 动作想象、评分、训练 | 开源推理＋实机研究 | BWM默认50步；其余未披露 | 非实时、安全与标定 | [BWM](https://github.com/boundless-large-model/boundless-world-model) |

## 8. 可复现资源与开发者入口

- [BWM GitHub](https://github.com/boundless-large-model/boundless-world-model)：Apache-2.0；已提供Wan2.2-5B适配、checkpoint、动作示例与推理脚本。最小路径是使用仓库`demo/`动作和初始帧生成一段81帧rollout，再用WorldArena视频质量脚本评分。训练代码仍未开放。
- [BWM权重](https://huggingface.co/BLM-Lab/Boundless-World-Model)：配置为640×480、81帧、9帧历史、30 FPS编码和50步推理；这些是生成配置，不代表实时吞吐量。
- [Context Scaling](https://github.com/heheyas/context-scaling)：Apache-2.0；包含训练、评测、prompt强化学习、FastAPI和浏览器demo。最小路径可不运行DiT，只调用`generate_sp`比较普通prompt与结构化JSON的可编辑性。
- [Context Scaling模型](https://huggingface.co/collections/heheyas/context-scaling)：已开放Qwen-Image-SP与35B-A3B prompter；完整本地demo下载量约110GB。
- [OASIS项目页](https://mova-hand.github.io/MOVA/)：有动画对比，但代码链接404；暂不建议搭建复现环境。
- [PhiZero](https://phi-zero.github.io/)、[ACE-Data-0](https://ace-data-engine.github.io/ACE-Data-0/)、[MoRoute](https://orange-3dv-team.github.io/MoRoute/)仅确认项目页或demo，代码/数据/许可证仍待开放。
- Forwardrobe、FlexComposer、WCM、MoRAE和FillGS未确认公共代码、权重或引擎插件，当前适合精读，不适合立即投入完整复现。

## 9. 系统架构与技术趋势判断

明显升温的方向是“结构化中间状态”：

`用户/agent意图 → 权威状态与轨迹 → 物理token/姿态/对象状态 → 生成或预测模型 → critic/规则检查 → 引擎或机器人执行`

数字人侧的结构化接口是FLAME、手部骨骼和独立服装层；世界模型侧是物理token、机器人动作、未来latent和对象6-DoF；创作侧是轨迹及结构化prompt JSON。它们共同降低了端到端黑盒模型与软件系统之间的耦合。

代码应负责身份与资产ID、任务状态、权限、物理约束、网络同步、版本、日志和回滚；生成模型负责开放式外观、动作细节与未来观测；传统引擎负责碰撞、rig、实时渲染和确定性规则；critic或评价模型只负责筛选，不应拥有最终安全权限。

仍未解决的问题包括：单图avatar背面与遮挡区域的真实性；服装—身体碰撞；跨头、手、衣物的统一坐标和资产格式；物理token的可解释性与错误检测；世界模型的实时成本；生成rollout与真实安全结果之间的校准。

需要降级看待的表述包括：S-Avatar的“实时”缺少硬件和FPS；BWM论文所称训练代码开放与仓库现状不一致；ACE称“release”但没有下载或许可证；PhiZero所有指标为作者自报且代码未开放；OASIS代码链接失效。Context Scaling和BWM推理栈的工程证据相对最强。

## 10. 论文精读候选

1. [PhiZero](https://arxiv.org/abs/2607.28624)：重点读物理tokenizer、FSQ、reasoner训练和跨embodiment迁移。与像素级世界模型的差异最大；风险是token是否真实表达因果状态仍缺外部验证。
2. [WCM](https://arxiv.org/abs/2607.29613)：重点读state approximation论证、LeJEPA辅助目标、on/off-policy集成及实机协议。价值在于把世界模型变成critic表征学习器。
3. [BWM](https://arxiv.org/abs/2607.29302)：重点读动作时间对齐、overlapping clip、history机制及WorldArena功能评测。复现风险是训练代码缺失、50步推理成本较高。
4. [ACE-Data-0](https://arxiv.org/abs/2607.28625)：重点读同步标定、长时任务协议、触觉监督和benchmark拆分。风险是数据访问与许可未明确。
5. [Forwardrobe](https://arxiv.org/abs/2607.29106)：重点读服装—人体分层、skinning初始化和非刚性形变。风险是尚无碰撞、复杂材质和标准引擎导出证据。

## 11. 下周跟踪与可行动建议

继续跟踪：

1. PhiZero是否开放tokenizer、reasoner、物理token词表及实时rollout接口。
2. BWM训练代码是否真正发布，并披露单段生成显存、时延和并发能力。
3. WCM是否开放RL集成代码，以及未来预测目标带来的实际训练开销。
4. ACE-Data-0何时提供下载、文件schema、许可证和隐私/肖像授权说明。
5. S-Avatar是否公布构建耗时、渲染FPS和Unity/Unreal导出。
6. Forwardrobe能否处理身体—服装碰撞、多层衣物、鞋帽和动态拓扑。
7. OASIS失效代码链接是否修复，以及是否提供MANO或引擎适配。
8. FlexComposer与MoRoute是否开放推理代码，并支持可编辑轨迹JSON。

本周可做的小实验：

1. **BWM动作敏感性测试**：固定初始帧，修改一段动作序列的速度、方向或末端位姿；检查对象结果是否随动作单调变化，而非只生成视觉上合理的视频。成功判据是动作变化与结果变化可重复对应。
2. **结构化prompt资产协议**：运行Context Scaling的`generate_sp`，将人物、服装、镜头和空间位置转成内部JSON schema；成功判据是修改单一字段后，其余角色与构图保持稳定。
3. **模块化avatar接口设计**：不训练模型，只定义`head/hand/garment/skeleton/material`资产清单和坐标协议，将S-Avatar、OASIS、Forwardrobe的输出需求映射到Unity或Unreal组件。成功判据是同一动作时间线能同时驱动头、手和服装占位资产。
4. **世界模型critic原型**：用现有机器人视频embedding训练一个“未来latent预测＋成功价值”双头模型，对比单帧value baseline。成功判据是在遮挡或动作前缀相同的任务中，双头模型的策略排序与真实结果相关性更高。
