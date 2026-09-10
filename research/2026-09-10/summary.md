# 数字人、世界模型与互动作品研究日报：2026-09-10

检索窗口：2026-09-04 至 2026-09-10。  
发布日期以 arXiv 提交历史、官方项目页和仓库状态为准。已与 9 月 3—9 日日报去重；“相对上期”指相对 2026-09-09 日报。

## 1. 本日摘要

本日新增主要来自 9 月 8 日提交、随后开放代码或权重的一批项目。最重要的变化是：世界模型的“动作接口”正从固定机器人坐标或键鼠指令，扩展为可通过视觉校准、历史上下文和代码协议解释的运行时接口。

SyncWorld 用一段配对的“动作—画面”校准视频描述新相机、新环境和新机器人，使同一个 checkpoint 无需微调即可解释不同具身系统的数值动作；这比单纯提高生成画质更接近通用仿真服务。

GE-Act 2.0 则把世界预测和动作生成联合起来，并在真实机器人上给出从 300 小时到 3 万小时的零样本扩展曲线，但最大规模成功率仍低于 45%，说明视觉规划尚不能替代可靠控制。

实时交互侧，Gander 开放了模型、代码和编排运行时，将连续音视频感知、可打断对话、工具调用和异步长任务放进同一系统；它不是数字人渲染器，却补上了数字人长期缺失的“听、看、插话、委派与反馈”控制层。

数字人生成本身仍没有新的高质量 talking-head、3D/4D avatar 或身份保持模型。近期增量集中于交互大脑、全双工音频以及世界中的角色控制，而非形象生成质量。

Mask Forcing、MovieGrid 和 VidaForge 分别推进少步自回归视频的训练稳定性、跨镜头角色一致性和可审计数据管线，为实时世界、互动电影与数字人内容生产提供底层支撑。

Coding 的职责边界更加清晰：代码维护权威状态、权限、任务生命周期、动作协议和回归测试；生成模型负责视觉、语音和局部动态；传统引擎或机器人控制器继续负责确定性物理与安全执行。

本周期新增大多属于论文实测或开源研究系统，尚无新的客户规模、并发、单位会话成本或正式生产 SLA 证据。产业成熟度仍显著落后于研究 demo。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中低 | Gander 开放全双工音视频交互与异步 agent 运行时 | 交互大脑进入可复现阶段；渲染能力无跃迁 | 新增 Gander；无新专用 avatar 模型 |
| 世界模型 | 很高 | SyncWorld 用视觉校准适配未知具身环境；GE-Act 给出真机 scaling 曲线 | 从视频预测扩展到可调用的机器人仿真、候选动作评估 | 新增 SyncWorld、GE-Act 2.0 |
| Coding × 互动作品 | 高 | 工具调用、任务生命周期、视觉校准协议和 outcome-adaptive 控制 | 可编排运行时证据增强 | 新增 Gander runtime 与 agentic generation 分级框架 |
| 支撑基础设施 | 高 | Mask Forcing、MovieGrid、VidaForge 均开放代码或资源 | 训练、长视频与数据管线的复现性提高 | 上期漏报，本期补录并核对仓库 |
| 工业应用 | 中 | 世界模型开始用于零样本机器人 rollout 和策略候选重排 | 真机研究验证增加，生产证据仍缺 | GE-Act、SyncWorld 提供新实测 |
| 安全与身份治理 | 低 | 无新增肖像授权、水印或身份撤回机制 | 无明显变化 | 本周期暂无高质量新增 |

## 3. 最值得关注的 11 个进展

### 1. SyncWorld：视觉校准把世界模型变成跨环境零样本模拟器

- **类型 / 日期 / 主线：**论文、模型、GitHub、评测集；2026-09-08；世界模型、机器人、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.09155)、[项目页](https://umass-embodied-agi.github.io/SyncWorld/)、[代码](https://github.com/UMass-Embodied-AGI/SyncWorld)、[权重](https://huggingface.co/yyuncong/SyncWorld)。
- **相对上期：**新增；上期未收录。
- **核心贡献：**通过一段覆盖各控制自由度的配对视频和动作序列，在上下文中建立当前部署环境的 Action–Visual Mapping，解决相机、机器人位置和具身形态变化导致相同数值动作产生不同像素运动的问题。
- **Coding 接口：**提供训练、16 帧分块自回归评测、PSNR/SSIM/LPIPS、FSDP、TOML 配置和 Hugging Face checkpoint。可让 VLA 生成候选动作，SyncWorld rollout，VLM 打分后选择执行。
- **场景：**机器人策略预演、跨机器人仿真、合成数据和安全动作筛选。
- **成熟度 / 证据：**已开源研究系统；论文实测。代码基于 Cosmos3-Nano，采用 OpenMDW-1.1；要求 Ampere 及以上 GPU、CUDA 12.8+，显存未披露。
- **重要性 / 阅读：**高 / 必读。

### 2. Gander：把可打断数字人交互与异步 agent 任务放进同一运行时

- **类型 / 日期 / 主线：**模型、代码、运行时、demo；2026-09-08；数字人支撑、Coding × 互动。
- **官方链接：**[论文](https://arxiv.org/abs/2609.08977)、[项目页](https://omni-interaction-gander.github.io/)、[GitHub](https://github.com/Omni-Interaction-Gander/Omni-Interaction-Agent)、[模型卡](https://huggingface.co/Gander-Omni/Gander)。
- **相对上期：**新增。
- **核心贡献：**“前小脑”持续处理音频、视频、说话/倾听/打断决策；“后大脑”处理复杂推理和工具任务；中间运行时维护异步任务状态。
- **Coding 接口：**公开 `task_start`、`task_send`、`task_resolve` 等结构化任务操作和可替换 worker；示例配置将 Thinker 与 Talker 分布在两张 GPU 上。
- **场景：**可插话数字员工、屏幕陪伴角色、教育助手、直播副驾和支持长任务的虚拟讲解员。
- **成熟度 / 证据：**模型、代码和 demo 已开放，Apache-2.0；论文称数据开放，但当前模型卡仍标记 dataset “Coming soon”，数据状态需继续核验。1 秒是处理单元而非端到端延迟。
- **重要性 / 阅读：**高 / 必读。

### 3. GE-Act 2.0：从世界预测直接生成机器人动作

- **类型 / 日期 / 主线：**论文、真机实验、项目 demo；2026-09-04；世界模型、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2609.05588)、[项目解读与实验数据](https://declare-lab.github.io/projects/ge-act-2/)。
- **相对上期：**上期漏报，本期补录。
- **核心贡献：**以控制导向自编码器、单步视觉规划器和逆动力学模型组成 world-action model；KASO 从多个预测未来中选择与记录动作相容的样本，减少视觉未来与动作标签错配。
- **关键结果：**论文报告联合训练数据从 300 增至 30,000 小时后，G1-OP 成功率由 17.1% 增至 44.1%，G2-90D 由 13.4% 增至 31.1%；覆盖 100 项真实机器人任务且不做逐任务微调。
- **Coding 接口：**运行时为“观察 → 单步未来状态 → 动作 chunk → 执行 → 重规划”；尚无公共 SDK、代码或 checkpoint。
- **场景：**机器人零样本操作、异常条件重规划、动作数据扩充。
- **成熟度 / 证据：**研究原型、作者真机实验；不等同于规模化机器人产品。
- **重要性 / 阅读：**高 / 必读。

### 4. Mask Forcing：降低实时自回归视频蒸馏的模式坍缩

- **类型 / 日期 / 主线：**论文、GitHub、权重；2026-09-08；实时视频与世界模型基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.09123)、[项目页](https://alicezrzhao.github.io/mask_forcing/)、[代码](https://github.com/delaprada/Mask-Forcing)。
- **相对上期：**新增。
- **核心贡献：**在自 rollout 的空间和时间位置随机注入较低噪声 token，使学生模型覆盖更多教师分布模式，同时为较噪 token 提供局部锚点；不增加推理步骤或网络结构。
- **关键结果：**在多种 chunk-wise、frame-wise 基线上普遍改善 HPSv3、指令跟随和 VBench，但部分配置存在视觉质量与动态幅度的权衡。
- **Coding 接口：**Apache-2.0 仓库已提供 Wan2.1 1.3B 推理脚本与 checkpoint；训练代码仍待发布。
- **场景：**实时世界渲染、流式数字人背景、可玩视频的低步数生成后端。
- **成熟度 / 证据：**推理可复现，训练链路不完整；论文实测。
- **重要性 / 阅读：**高 / 必读。

### 5. MovieGrid：空间网格联合建模长篇多镜头叙事

- **类型 / 日期 / 主线：**论文、模型、GitHub、数据管线；2026-09-06；视频基础设施、互动电影。
- **官方链接：**[论文](https://arxiv.org/abs/2609.06373)、[项目页](https://jwmao1.github.io/moviegrid_web/)、[代码](https://github.com/jwmao1/moviegrid)、[权重](https://huggingface.co/JiaMao/MovieGrid)。
- **相对上期：**上期漏报，本期确认代码与 16/64-grid 权重已开放。
- **核心贡献：**把时间顺序明确的短视频块排列成空间网格，联合生成以交换全局信息；MGLV 从 1,000 部长视频构造 54K 个网格样本。
- **关键结果：**论文称在 1,616 帧、相同 token 预算下生成的镜头数为 temporal packing 的 6.05 倍；跨镜头一致性 0.5914，对比 StoryMem 的 0.5384。
- **Coding 接口：**Wan2.2-TI2V-5B 基座、LoRA、16/64-grid 训练与推理脚本、数据构建 pipeline；训练示例为 8 GPU。
- **场景：**分支电影素材、连续广告、跨镜头角色故事板和互动剧集预生成。
- **成熟度 / 证据：**已开源研究原型；许可证在仓库首页未明确显示，商业使用前需核验。
- **重要性 / 阅读：**高 / 必读。

### 6. VidaForge：把视频数据 recipe 变成可执行、可追溯工作流

- **类型 / 日期 / 主线：**论文、GitHub、dataset；2026-09-06；数据基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.06652)、[MIT 许可代码](https://github.com/GAIR-NLP/VidaForge)。
- **相对上期：**新增补录。
- **核心贡献：**将采集、分镜、筛选、标注和打包建模为五阶段工作流，保留每个样本的处理来源；VIDAFORGE-3M 含 314 万个场景级片段、约 6,475 小时。
- **关键结果：**Wan 2.1 与 V-JEPA 2.1 的从零预训练实验均显示，更广覆盖的数据 recipe 在下游评测上优于仅追求高质量过滤的 recipe；训练 loss 并不能可靠选择最佳数据配方。
- **Coding 接口：**Ray、FFmpeg、去重、质量过滤、相机与 caption 标注，以及面向 Wan/V-JEPA 的打包接口。
- **场景：**企业视频模型数据治理、数字人动作数据清洗、世界模型训练集版本化。
- **成熟度 / 证据：**代码已开源；论文实测；完整数据下载成本和版权清单仍需核验。
- **重要性 / 阅读：**中高 / 必读。

### 7. Agentic Visual Generation：给“生成式 agent”建立可执行分级

- **类型 / 日期 / 主线：**survey、benchmark 框架、GitHub 索引；2026-09-06；Coding × 视觉生成。
- **官方链接：**[论文](https://arxiv.org/abs/2609.06758)、[资源库](https://github.com/YinmingHuang/Awesome-agentic-visual-generation-model)。
- **相对上期：**新增补录。
- **核心贡献：**以控制器能直接改变的生成决策范围划分 L0—L4：固定组件、条件控制、执行控制、结果自适应、跨任务经验自适应。
- **Coding 接口：**强调工具选择、结果观察、重试和经验存储必须表现为可执行控制回路，不能仅凭“使用多个 agent”判定为 agentic。
- **场景：**互动作品制作 agent、自动镜头修复、世界模型 CI、跨任务资产生成。
- **成熟度 / 证据：**综述与评测方法，不是新产品；适合作为系统验收框架。
- **重要性 / 阅读：**中高 / 可读。

### 8. GWM Worlds 2：持续原生音视频世界仍是实时交互上限信号

- **类型 / 日期 / 主线：**模型、内部 demo；2026-09-03；世界模型、数字角色。
- **官方链接：**[Runway 技术发布](https://runway.com/research/introducing-gwm-worlds-2)。
- **相对上期：**无新 API、SDK、价格或开放测试信息。
- **核心贡献：**WorldPrompt 分离持久世界设定与时间戳动作、对白、声音和相机控制，持续生成 720p/24fps 视频与 48kHz 音频。
- **Coding 接口：**目前仅见内部键鼠映射、LLM 提示生成和 LiveKit 多人分发设计。
- **成熟度 / 证据：**官方研究 demo；性能和可控性为官方声称。
- **重要性 / 阅读：**高 / 持续跟踪。

### 9. WorldReward：生成世界具备可服务化回归测试

- **类型 / 日期 / 主线：**reward model、benchmark、GitHub；论文 9 月 3 日，27B 权重 9 月 7 日；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2609.03952)、[代码与模型](https://github.com/CodeGoat24/WorldReward)。
- **相对上期：**无进一步更新；9B、27B、760 对人工比较 benchmark 均已确认。
- **核心贡献：**分别评分动作执行、外观和运动，可接候选重排、RL 和 nightly regression。
- **成熟度 / 证据：**已开源、论文实测；尚缺独立复现与单位推理成本。
- **重要性 / 阅读：**高 / 必读。

### 10. Text-Audiobox：数字人交互继续从口型转向轮次和重叠语音

- **类型 / 日期 / 主线：**论文、音频模型；2026-09-03；数字人基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.03992)。
- **相对上期：**无代码、权重或 API 新增。
- **核心贡献：**统一生成重叠说话、backchannel、情感和跨语言配音。
- **Coding 接口：**论文描述 multi-diffusion 和候选重排，但没有公共流式协议。
- **成熟度 / 证据：**论文原型；尚未证明首包延迟、打断恢复和并发能力。
- **重要性 / 阅读：**中高 / 可读。

### 11. SolarWM：开放世界模型训练栈仍是本周期复现基线

- **类型 / 日期 / 主线：**论文、数据、训练框架、权重；9 月 2—3 日；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2609.02886)、[Apache-2.0 仓库](https://github.com/Junchao-cs/SolarWM)。
- **相对上期：**无新增；14B 与部分基础模型训练阶段仍未发布。
- **核心贡献：**统一 143 万个片段的数据合同，并提供双向到因果、teacher forcing 和分布匹配蒸馏流程。
- **成熟度 / 证据：**部分权重和完整工程骨架已开放；大规模训练成本较高。
- **重要性 / 阅读：**高 / 持续跟踪。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期暂无高质量专用 talking-head、单图 avatar、全身人物生成或身份保持新模型。GWM Worlds 2 能联合生成角色动作、对白和口型，但没有专用身份与表情指标。
- **3D/4D 表示：**暂无数字人专用新增。SyncWorld 的跨视角几何一致性面向机器人场景，不能直接等同于可驱动 3D/4D 人体。
- **实时交互：**Gander 是本日最重要增量：它把持续观看、倾听、插话、工具委派和任务反馈做成开放运行时。其价值在“角色控制面”，不是脸部渲染。
- **音视频与情感：**Gander 已提供流式 Thinker–Talker；Text-Audiobox 提供更丰富的重叠语音和情感建模。前者更可复现，后者生成质量研究更强但缺接口。
- **工程部署：**可落地结构是 `VAP/ASR → 交互控制器 → agent 工具 → 动作DSL → TTS/avatar → WebRTC`。Gander 可以承担前三层，外观仍需独立 2D/3D avatar 后端。
- **安全与身份治理：**本周期无新授权、撤回、水印、检测或肖像来源登记机制。任何真人数字员工仍需另建身份授权、声音克隆许可和输出溯源层。

结论：数字人的新增是“更像实时协作者”，而不是“更逼真的脸”。

## 5. 世界模型进展

- **架构与训练：**因果视频的主线仍是双向模型蒸馏。Mask Forcing表明，解决反向 KL 的模式坍缩与中间 rollout 误差，可能比继续压缩采样步数更重要。
- **可控 / 可玩生成：**SyncWorld 将动作解释本身变为上下文学习问题；GWM Worlds 2 则把自然语言、角色事件和相机输入统一为时间化控制。
- **长时与物理一致性：**MovieGrid改善跨镜头语义一致性，但它是离线叙事生成，不维护可玩世界的库存或任务状态。物理与长期实体状态仍需显式数据库、ECS 或传统模拟器。
- **空间表示：**SyncWorld 用多视角预测验证共享三维几何；SolarWM、Matrix-Game 和 Puffin-World 则分别强调相机合同、显式记忆与 RGB-D 状态。
- **机器人 / 仿真：**SyncWorld适合候选动作的“想象—排序”，GE-Act 直接将视觉未来解码成动作。前者模块化且可接现有 VLA，后者端到端程度更高。
- **实时部署与评测：**公开项目仍极少报告动作输入到首个可见响应的 p50/p95、连续运行失败率、恢复时间和每小时 GPU 成本。

## 6. Coding × 新型互动作品

### 链路一：实时感知 → 任务生命周期 → 可打断数字员工

摄像头和麦克风进入 Gander 前小脑；运行时将请求编译为 `task_start/task_send/task_resolve`；后端 agent 调用业务 API；Talker 在任务运行期间反馈进度；独立 avatar 渲染器执行口型和表情。

代码负责权限、任务状态和工具调用，模型负责轮次、语音及交互判断。Gander 已有可运行代码；视觉化身集成属于本报告推断。

### 链路二：VLA 候选动作 → SyncWorld rollout → VLM 排序 → 机器人执行

控制程序从 VLA 采样多个动作 chunk；SyncWorld 根据校准视频生成未来画面；VLM 按任务指令排序；安全层检查碰撞和工作空间后执行最佳候选。

这是本周期最完整的“模型作为代码可调用模拟器”链路，已有论文实验和开放代码，但尚无工业控制器 SLA。

### 链路三：剧情图 → MovieGrid → 镜头拆分 → 互动电影运行时

coding agent 将故事图编译成角色、地点、镜头和分支节点；MovieGrid 生成相互一致的镜头组；传统视频管线切分网格；运行时根据用户选择播放或预取下一分支。

MovieGrid 已开放生成代码与权重，但“用户操作后实时生成分支”尚无证据；近期更适合预生成内容树。

### 链路四：自回归世界模型 → Mask Forcing 后训练 → 实时视觉后端

开发团队以 SolarWM 或同类因果模型为基础，使用 Mask Forcing 进行 DMD 后训练；WorldReward 检查动作执行和运动；Principia 类物理规则作为 CI 门禁；通过 WebRTC 输出。

代码负责训练编排、版本对比和回滚，模型负责连续画面。组件大多可获取，但端到端集成仍需自行实现。

### 链路五：可执行数据 recipe → 模型版本 → 自动回归

VidaForge 将来源、分镜、过滤、caption、相机信息和打包规则写成可版本化任务；训练系统生成不同数据配方的模型；WorldReward 与内部人工集比较版本。

这使“为什么新模型退化”能够追溯到具体数据决策，适合企业视频、数字人和世界模型团队。VidaForge 已提供代码，数据版权治理仍需另建。

### 链路六：真机观察 → 视觉未来 → 逆动力学 → 反应式角色身体

GE-Act 的 visual planner 生成目标状态，逆动力学输出动作 chunk，执行短段后重新观察和规划。若将机器人替换为游戏角色，可把逆动力学输出约束为动画状态机或动作 DSL。

机器人部分已有真机证据；用于游戏 NPC、数字角色或虚拟制作属于本报告推断。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 技术栈 / 互动机制 | 当前成熟度 | 成本或延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | GWM Worlds 2、Mask Forcing | ECS、事件流、因果视频、流媒体 | 内部 demo / 研究原型 | 未披露 | 状态持久、多玩家同步、成本 | [GWM](https://runway.com/research/introducing-gwm-worlds-2) |
| 影视/虚拟制作 | MovieGrid | 剧情图、网格视频、镜头切分、编辑器 | 开源研究原型 | 训练示例 8 GPU；推理未披露 | 精细修改、版权、生产分辨率 | [代码](https://github.com/jwmao1/moviegrid) |
| 直播与电商 | Gander + avatar 后端 | 全双工音视频、工具调用、WebRTC | 组件可运行 | 双 GPU 示例；端到端延迟未披露 | 并发、审核、声音授权 | [模型卡](https://huggingface.co/Gander-Omni/Gander) |
| 品牌互动 | GWM Worlds 2 | 观众事件、导演权限、实时生成 | 官方 demo | 未披露 | 品牌一致性、实时审核 | [官方发布](https://runway.com/research/introducing-gwm-worlds-2) |
| 教育培训 | Gander、MovieGrid | 课程状态机、可打断讲解、分支视频 | 原型可组装 | 未披露 | 事实可靠性、学习效果评估 | [Gander](https://omni-interaction-gander.github.io/) |
| 企业数字员工 | Gander | 连续感知、异步 agent、进度反馈 | 开源研究系统 | 未披露 | 权限、审计、隐私、降级 | [GitHub](https://github.com/Omni-Interaction-Gander/Omni-Interaction-Agent) |
| 陪伴与社交 | Gander、Text-Audiobox | backchannel、打断、长期记忆、角色状态 | 研究原型 | 未披露 | 情感安全、长期一致性 | [论文](https://arxiv.org/abs/2609.08977) |
| 空间计算 | SyncWorld | 校准视频、跨视角 rollout、空间输入 | 开源研究原型 | Ampere+；显存未披露 | 端侧算力、尺度与动态几何 | [项目页](https://umass-embodied-agi.github.io/SyncWorld/) |
| 机器人/仿真 | SyncWorld、GE-Act 2.0 | VLA、世界预测、候选排序、逆动力学 | 开源模拟组件 / 真机研究验证 | 未披露 | sim-to-real、安全认证、长尾失败 | [GE-Act](https://arxiv.org/abs/2609.05588) |

没有项目在本周期提供足以证明“规模化部署”的客户数、并发或生产 SLA。

## 8. 可复现资源与开发者入口

| 资源 | 状态、许可与要求 | 最小验证路径 | 建议 |
|---|---|---|---|
| [SyncWorld](https://github.com/UMass-Embodied-AGI/SyncWorld) | 代码、模型、100 个模拟评测 episode；OpenMDW-1.1；Ampere+、CUDA 12.8+ | 跑 ManiSkill 50 episodes，对比有/无 calibration 的 rollout 指标 | 最高优先 |
| [Gander](https://github.com/Omni-Interaction-Gander/Omni-Interaction-Agent) | 模型和 runtime；Apache-2.0；完整配置示例使用两张 GPU | 运行全双工服务，测打断、任务追加与取消 | 数字人团队优先 |
| [Mask Forcing](https://github.com/delaprada/Mask-Forcing) | Apache-2.0；推理代码和 checkpoint 已开；训练代码待发布 | 在相同 prompt 上比较 Self Forcing 与 checkpoint 的动态和质量 | 可立即验证 |
| [MovieGrid](https://github.com/jwmao1/moviegrid) | 16/64-grid LoRA、训练/推理与数据 pipeline；基座 Wan2.2-5B | 运行 16-grid 示例，切分后检查角色和场景一致性 | 影视团队优先 |
| [VidaForge](https://github.com/GAIR-NLP/VidaForge) | MIT；Python 3.11、Ray、FFmpeg，依赖较重 | 用小型视频集跑完整五阶段 pipeline，并保留 provenance | 数据团队优先 |
| [WorldReward](https://github.com/CodeGoat24/WorldReward) | 9B/27B、benchmark、vLLM 服务 | 评测 20—50 对内部视频，与人工排序比较 | 适合接 CI |
| [SolarWM](https://github.com/Junchao-cs/SolarWM) | Apache-2.0；部分 5B 权重 | 记录长 rollout 的显存、FPS、重访漂移 | 高价值、成本较高 |
| GE-Act 2.0 | 无公共代码或权重 | 当前只能复核论文、项目数据和 demo | 暂不进入生产依赖 |
| GWM Worlds 2 / Text-Audiobox | 无公共 SDK 或模型 | 只能进行接口设计和官方 demo 审查 | 仅跟踪 |

## 9. 系统架构与技术趋势判断

明显升温的方向包括：视觉校准式动作解释、世界—动作联合模型、少步因果视频蒸馏、跨镜头联合建模、全双工 agent 运行时，以及可追溯数据 recipe。

正在形成的通用架构是：

`权威状态 / 任务图 → agent 规划 → 类型化动作或事件 → 世界/数字人生成器 → 奖励与物理验证 → 执行或流媒体 → 日志、回滚与经验库`

世界模型不再适合被视为“新的游戏引擎”。更现实的分工是：传统引擎、数据库或控制器保存确定性事实；世界模型预测感知结果、生成外观或比较候选未来；agent 决定何时调用、重试和降级。

Mask Forcing 和 MovieGrid 说明两类一致性问题正在分化：流式世界关注自回归误差和动作响应，影视内容关注跨镜头角色与叙事一致性。二者不应只用同一套视频质量指标评估。

关键未解问题仍是毫秒级动作响应、小时级状态持久性、多人确定性同步、真实物理可靠性、消费级硬件、实时审核、商业许可证和身份治理。

需要降级看待的说法包括：把一秒处理 chunk 当作一秒延迟；把可生成长视频等同于保持长期状态；把真机平均成功率当作通用机器人产品能力；把有项目页和 demo 当作公共 API；把视觉逼真等同于物理正确。

## 10. 论文精读候选

1. [SyncWorld](https://arxiv.org/abs/2609.09155)  
   **原因：**提出了跨具身动作接口的新范式。重点读视觉校准构造、action conditioning、跨视角一致性和 test-time policy improvement。复现风险是 Cosmos 环境复杂、真实机器人评测数据有限。

2. [Omni Interaction Agent / Gander](https://arxiv.org/abs/2609.08977)  
   **原因：**把全双工交互与异步工具执行统一。重点读 Cerebellum–Brain、Thinker–Talker、任务协议和 interaction benchmark。风险是官方内部人工评测占比较高，端到端延迟未报告。

3. [GE-Act 2.0](https://arxiv.org/abs/2609.05588)  
   **原因：**罕见地给出 world-action model 的真机预训练扩展曲线。重点读 CoAE、SVP、IDM、KASO 和 100-task 协议。风险是无代码、数据规模难复现且最大成功率仍有限。

4. [Mask Forcing](https://arxiv.org/abs/2609.09123)  
   **原因：**直接处理实时视频蒸馏中的模式坍缩和误差累积。重点读 dual-noise rollout、分布解释及质量—动态权衡。风险是训练代码尚未开放。

5. [MovieGrid](https://arxiv.org/abs/2609.06373)  
   **原因：**把长篇视频从纯时间建模改写为空间—时间联合建模。重点读 MGLV、Noise-Free Random-Grid、Grid Embedding 和跨镜头评测。风险是网格拆分后的实际编辑成本、分辨率和许可证尚需核验。

## 11. 下周跟踪与可行动建议

### 继续追踪

1. SyncWorld 在真实机器人、较长 rollout 和无显式校准条件下的独立复现结果。
2. Gander 数据集是否正式开放，以及音频输入到可听响应的 p50/p95 延迟。
3. Gander 与现有 2D/3D avatar、Unity、Unreal 或 WebRTC 前端的社区集成。
4. GE-Act 2.0 是否发布代码、checkpoint、机器人接口和失败日志。
5. Mask Forcing 训练代码能否复现论文中的动态—质量收益。
6. MovieGrid 的许可证、显存、单次生成时间及 storyboard conditioning。
7. GWM Worlds 2 是否开放 WorldPrompt schema、API、访问计划和定价。
8. 数字人身份授权、水印和声音克隆治理是否出现与实时生成匹配的更新。

### 本周可做的小实验

1. **可打断数字员工闭环。**  
   目标：验证 Gander 是否能在持续对话中委派、追加和取消后台任务。组件：Gander、一个模拟业务 API、WebSocket/WebRTC、简单 avatar。难点：音画同步和任务失败降级。成功判据：用户插话后 1 个交互周期内停止旧回复，任务状态不丢失且操作日志可审计。

2. **跨环境动作校准 A/B。**  
   目标：量化 SyncWorld 校准上下文的价值。组件：公开 ManiSkill/LIBERO 集、SyncWorld checkpoint、评测脚本。难点：CUDA 环境和相机动作格式。成功判据：有校准版本在长 rollout 的 LPIPS、动作方向正确率及失败恢复上稳定优于 history-only。

3. **跨镜头角色一致性验证。**  
   目标：判断 MovieGrid 是否适合互动短剧素材。组件：Wan2.2-5B、MovieGrid-16、三个重复角色的剧情 prompt、角色识别与人工评分。难点：网格拆分、角色遮挡和相邻镜头顺序。成功判据：切分后的角色身份、服装、场景和故事状态一致性显著优于逐镜头独立生成。

4. **生成世界 CI 最小栈。**  
   目标：形成“生成—评分—阻断—回滚”工程闭环。组件：任一因果视频模型、固定动作测试集、WorldReward 9B、两条物理规则和 GitHub Actions/内部 CI。难点：奖励偏差和 GPU 调度。成功判据：已知退化版本能被稳定阻断，误报率可接受，且每次测试的模型、数据、prompt 与指标均可追溯。
