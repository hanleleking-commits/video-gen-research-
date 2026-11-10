# 数字人、世界模型与互动作品研究日报：2026-11-10

检索窗口：2026-11-04 至 2026-11-10。已对照 11 月 4、5、6、9 日日报去重；日期以论文提交记录和官方公告为准。

## 1. 本日摘要

本周期最强的新增来自 Coding 与生成模型的结合：Video-Coder 不再让视频模型直接猜测复杂运动，而是先由 agent 编写并执行 3D 场景代码，用可运行的“草模视频”约束后续生成。  
Frontier3DBench 则首次系统测量 coding agent 从单个物体、完整场景到可交互 Unreal 世界的能力，结果显示局部开关、门、抽屉等机制已经相对可靠，但路径净空和跨步骤状态保持仍明显不足。  
数字人侧最有产业价值的信号是 Runway 公开了 57B 实时视频角色的生产 serving 数据：单会话 GPU 数从 8 张 H100 降到 4 张，视频链路已经稳定超过 24 FPS，但完整对话的中位响应仍为约 2.1 秒，主要瓶颈转向语音 agent。  
Meta Muse Realtime Avatar 进一步证明“可打断语音 agent＋有表情的实时形象”已进入消费产品，但没有开放 SDK、成本或底层模型指标。  
世界模型侧，EvoWorld 将“离开镜头后世界仍继续变化”变成独立问题，通过 VLM 世界脑维护事件后果，再由视频模型负责渲染；这比单纯扩大视频上下文更接近游戏状态机。  
WoVR 的窗口内重大修订把世界模型从视频预测器推进为 VLA 强化学习模拟器，并明确处理 policy exploitation、幻觉成功和策略分布漂移。  
SplatAnything 改进了跨尺度 3D Gaussian 重建，为数字人、世界记忆和空间运行时提供更稳定的可渲染中间表示。  
产业侧新增主要集中在创作管线和产品集成，而非规模化生成世界：Adobe 将 Kling、Seedance 接入企业 Firefly 管理面，Dreamina 把脚本、角色记忆和分集生产组合成 Drama Studio。  
总体判断是：代码正在承担几何、状态、约束、验证和资产可编辑性，生成模型则集中承担视觉实现与角色表演；这是比“端到端生成所有内容”更可靠的近期路线。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 11 月 9 日 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | Runway 公布真实 serving 遥测；Meta 上线可打断实时 Avatar | 从论文 FPS 进入生产会话、消费产品 | 上期重点是手部与 Gaussian 资产，本期转向端到端实时服务 |
| 世界模型 | 中高 | EvoWorld 显式推理离屏事件后果；WoVR 控制模拟器幻觉 | 研究原型开始服务策略训练，但仍缺独立复现 | 从粒子/视频状态预测扩展到因果状态与 policy-model 共演化 |
| Coding × 互动作品 | 高 | Video-Coder 以代码草模约束视频；Frontier3DBench 评测可执行世界 | 开源模型、数据、harness 与 UE5 benchmark 同期出现 | 相比 ManimCode 的二维动画闭环，进一步进入 3D 场景与游戏运行时 |
| 产业应用 | 中 | Adobe 企业接入、Drama Studio、Muse Avatar | 出现正式产品与企业管理入口 | 仍缺并发、留存和客户 ROI 数据 |

## 3. 最值得关注的 10 个进展

### 1. Video-Coder：让 coding agent 先搭 3D 动态草模，再生成视频

- **类型／主线：**论文、模型、dataset、GitHub；Coding × 互动作品。
- **官方链接／日期：**[论文](https://arxiv.org/abs/2611.09894)、[代码](https://github.com/CYWang735/Video-Coder)，2026-11-06。
- **相对上期的新变化：**首次收录；论文声明模型、数据和代码完整开放。
- **核心贡献：**agent 编写并执行 3D 场景程序，渲染保留几何、遮挡、相机和运动的 Draft Video，再交给视频生成器视觉化；训练使用执行奖励和视频奖励，并逐步降低执行奖励权重。作者报告在 Minimax-H3 上令 PhyGenBench 提升约 7 分、VBench-2.0 提升约 24 分。
- **Coding 接口：**Video-Coder Harness 集成代码执行、3D 渲染、错误反馈和生成模型调用，可作为 agent 训练及推理环境。
- **场景：**精确镜头预演、物理事件短片、产品动画、互动电影分支片段。
- **成熟度／证据：**已开源研究栈；指标为论文实测，尚无第三方复现。
- **重要性／阅读：**高／必读。

### 2. Frontier3DBench：coding agent 构建可执行 3D 世界的首个完整压力测试

- **类型／主线：**benchmark、GitHub、leaderboard；Coding × 互动作品。
- **官方链接／日期：**[论文](https://arxiv.org/abs/2611.09017)、[项目页](https://lab.einsia.ai/frontier3dbench/)、[代码](https://github.com/Einsia/Frontier3D)，2026-11-06。
- **核心贡献：**145 个任务覆盖 Blender 物体、场景和 UE5 互动开放世界，要求交付可编辑 `.blend` 或 `.uproject`，并同时评测外观、结构和运行行为。11 组 model-harness 中最高总分为 66.02/100；跨所有 UE5 提交，局部机制平均 83.08，但路径净空仅 20.82、状态化工作流仅 30.26。
- **Coding 接口：**支持 Codex、Claude Code、Kimi CLI 等 harness；通过 Docker、Blender、UE5 和自动运行测试组成 CI。
- **场景：**关卡白盒、互动展览、数字孪生原型、游戏资产生产。
- **成熟度／证据：**Apache-2.0 benchmark；作者实测，尚未独立复现。
- **重要性／阅读：**高／必读。

### 3. Runway Characters：57B 实时视频角色的生产 serving 公开

- **类型／主线：**技术论文、正式产品、API；数字人＋运行时。
- **官方链接／日期：**[论文](https://arxiv.org/abs/2611.05642) 11 月 3 日提交；[技术博客](https://runway.com/news/research/real-time-video-at-batch-size-one) 11 月 5 日发布；[开发文档](https://docs.dev.runwayml.com/characters/)。
- **相对上期的新变化：**产品此前已存在，本次首次公开模型规模、单会话算力、生产遥测和端到端延迟构成。
- **核心贡献：**约 57B 自回归视频扩散模型以 704×1088、24 FPS 输出。缓存写时处理、FP8 和通信融合把四卡 H100 的扩散步从 240.9 ms 降至 159.3 ms，融合内核进一步降至 147.4 ms；99.5% 的有效生产会话平均超过 23.5 FPS。
- **Coding 接口：**Characters API、网页嵌入组件、LiveKit Agents、ElevenLabs Agents；开发者可自带 STT、LLM、TTS 和工具调用。
- **场景：**数字客服、实时讲师、品牌角色、游戏主持人。
- **成熟度／证据：**正式产品；生产遥测为官方实测。中位对话响应约 2.1 秒，其中视频管线约 0.49 秒，仍明显慢于自然人对话节奏。
- **重要性／阅读：**高／必读。

### 4. EvoWorld：让离开镜头的实体继续发生因果变化

- **类型／主线：**论文、模型权重、benchmark；世界模型。
- **官方链接／日期：**[论文](https://arxiv.org/abs/2611.06748)、[权重](https://huggingface.co/Tommy-Bie/EvoWorld)，11 月 4 日提交、11 月 5 日修订。
- **核心贡献：**VLM “世界脑”观察生成片段、记录实体和事件、推断离屏后果；自回归视频 renderer 再表现其结果。EvoWorld-Bench 用事件实现、因果反应、重访一致性等指标评测 120 个案例。
- **Coding 接口：**结构化状态、事件、reaction prompt、实体锚点和相机重访可被剧情系统或状态机编排。
- **场景：**互动叙事、离屏 NPC 行为、持续变化的虚拟场景。
- **成熟度／证据：**权重已发布，依赖 LTX-2.5 22B 组件及其社区许可证；结论主要为作者实测。
- **重要性／阅读：**高／必读。

### 5. WoVR v3：显式约束世界模型中的“幻觉成功”

- **类型／主线：**论文重大修订、机器人训练框架；世界模型。
- **官方链接／日期：**[论文](https://arxiv.org/abs/2602.13977)、[项目页](https://wovr-corl.github.io/)，v3 更新于 2026-11-05。
- **本次纳入原因：**旧工作在窗口内加入更强初始化、多机器人平台和不同 VLA backbone 的结果。
- **核心贡献：**Keyframe-Initialized Rollout 缩短关键交互前的预测深度；PACE 周期性用新策略数据刷新模拟器，防止策略学会利用世界模型错误。新版作者报告 WoVR 在 512 帧 rollout 下达到 23 FPS，并在 Franka、AgileX Piper 和 π0.5 上获得现实执行增益。
- **Coding 接口：**世界模型作为 RL 环境，策略、reward proxy、rollout 调度器和数据回灌器组成持续训练循环。
- **成熟度／证据：**研究系统；现实机器人结果仍是作者实验，未形成通用工业 SDK。
- **重要性／阅读：**高／必读。

### 6. Meta Muse Realtime Avatar：可打断数字人进入消费级 agent

- **类型／主线：**正式产品；数字人＋互动应用。
- **官方链接／日期：**[Meta 官方公告](https://about.fb.com/news/2026/11/introducing-muse-ai-voice-expressive-avatars/)，2026-11-05。
- **核心贡献：**Avatar 在聆听和说话时表现与语气匹配的表情和手势，并支持用户打断；首批覆盖美国的移动端、Web 和 Mac。
- **Coding 接口：**消费者产品未开放 Avatar SDK；Muse 自身可连接外部应用并执行任务，但头像层与工具调用层的接口未披露。
- **场景：**面试演练、陪伴、个人助理、口语交流。
- **成熟度／证据：**正式产品、官方声称；模型、延迟、并发和成本均未披露。
- **重要性／阅读：**中高／可读。

### 7. SplatAnything：统一不同场景尺度下的 3D Gaussian 间距

- **类型／主线：**论文、GitHub、demo；3D/4D 支撑基础设施。
- **官方链接／日期：**[论文](https://arxiv.org/abs/2611.08264)、[代码系列](https://github.com/zju3dv/InfiniSplat)，2026-11-05。
- **核心贡献：**GSNorm 以场景级 Gaussian 邻域间距为归一化基准，减少物体、室内和室外场景在 Gaussian scale 监督上的系统偏移。
- **Coding 接口：**多视图图片输入，输出可渲染 3DGS/PLY，可接 Web splat viewer、游戏引擎或空间记忆后端。
- **场景：**数字人背景重建、虚拟片场、AR 场景、世界模型显式记忆。
- **成熟度／证据：**研究代码；大部分指标为论文实测，极稀疏输入和不可见区域仍会产生空洞。
- **重要性／阅读：**中高／可读。

### 8. Adobe 将 Kling、Seedance 纳入企业 Firefly 管理面

- **类型／主线：**产品集成；创作工具。
- **官方链接／日期：**[Adobe 公告](https://community.adobe.com/announcements-623/manage-access-to-kling-and-seedance-partner-models-for-creative-cloud-teams-and-enterprise-1654590)，2026-11-06。
- **核心贡献：**团队和企业可在 Firefly/Firefly Boards 中使用两类视频模型，管理员可以独立开关，并沿用现有生成额度和“不用于训练”的企业条款。
- **Coding 接口：**本次重点是治理与资产管线，不是新 API；可与 Firefly 编辑、审批和企业权限结合。
- **成熟度／证据：**正式产品；缺少具体客户产量、单镜头成本和 SLA。
- **重要性／阅读：**中／可读。

### 9. Dreamina Drama Studio：脚本、角色记忆与分集生产合并

- **类型／主线：**产品、创作工作流；互动内容支撑。
- **官方链接／日期：**[官方产品页](https://dreamina.capcut.com/tools/drama-studio)；海外上线于 11 月 9 日由行业报道确认，官方页面未标注发布时间。
- **核心贡献：**Drama Agent 将剧本转成多集短剧，Canvas 管理角色、场景和剧情流，Seedance 2.5 支持 30 秒长镜头、时间戳和多模态参考。
- **Coding 接口：**当前以可视化工作流为主，未见公开 SDK、状态机导出或引擎插件。
- **场景：**连载短剧、漫画频道、本地化改编、低成本预演。
- **成熟度／证据：**正式产品；功能为官方声称，跨集身份和剧情记忆缺少公开评测。
- **重要性／阅读：**中／仅跟踪。

### 10. After Effects 2027：自然语言 agent 进入专业后期工程

- **类型／主线：**产品更新；Coding × 创作工具。
- **官方链接／日期：**[27.0 滚动发布公告](https://community.adobe.com/announcements-527/after-effects-2027-27-0-is-on-its-way-1654670)，2026-11-07；[AI Assistant 文档](https://helpx.adobe.com/after-effects/desktop/work-with-after-effects-ai-assistant/after-effects-ai-assistant-overview.html)。
- **相对上期的新变化：**Assistant 并非本周首次出现；本次是进入 27.0 发布周期并开始按生成操作计费。
- **核心贡献：**自然语言操作项目、图层、表达式、Motion Graphics 模板、渲染和生成模型；传统 AE 工程仍是可编辑权威资产。
- **Coding 接口：**可编写及调试 AE expressions；当前文档称可生成脚本，但自动执行脚本能力仍受限。
- **成熟度／证据：**Beta/滚动发布；适合生产辅助，不应视为全自动成片系统。
- **重要性／阅读：**中／可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**Runway 已证明单图角色可以由大型视频扩散模型持续驱动；Meta 则把聆听表情、语音和打断组合成消费体验。
- **3D/4D 表示：**本周期没有新的完整 4D 人体方案。SplatAnything 改善的是跨尺度场景 3DGS，可为虚拟片场和显式世界记忆提供底层表示。
- **实时交互：**Runway 的视频生成环节已经达到真实生产帧率，但完整对话仍约 2.1 秒；“实时渲染”不能等同于“自然 turn-taking”。
- **音视频与情感：**Meta 展示了表情、手势与会话语气协同，但没有公开可量化情感控制接口。
- **工程部署：**成熟度最高的是封装式云 API 和消费产品；开源数字人仍多受多卡推理、许可证和端到端延迟限制。
- **安全与身份治理：**本周期没有新的肖像授权协议、水印或深伪检测研究。Runway、Meta 的平台审核不能替代可携带的身份授权与内容溯源。

## 5. 世界模型进展

架构正在从“预测下一段视频”分化为三个层次：

1. **状态推理层：**EvoWorld 的 VLM 世界脑维护事件、实体和离屏变化。
2. **生成渲染层：**自回归视频模型负责把状态转换成可观看结果。
3. **策略学习层：**WoVR 把模型变成 RL 模拟器，并用 keyframe、reward mask 和共演化限制模拟器偏差。

长时一致性因而不再只依赖 KV cache：实体锚点、显式事件状态、关键帧初始化和 policy-model 数据回灌正成为更通用的方案。当前证据仍主要来自作者实验；EvoWorld 的因果合理性不是物理正确性，WoVR 的机器人增益也不能自动外推到开放世界游戏。

## 6. Coding × 新型互动作品

### 链路一：代码约束的生成电影

自然语言镜头 → Video-Coder 编写 3D 场景代码 → sandbox 执行并渲染 Draft Video → 视频模型完成材质、人物和光影 → 互动电影、产品演示、物理事件短片。

代码负责几何、相机、遮挡和运动；生成模型负责视觉实现。已有开源证据，但尚非实时互动。

### 链路二：agent 生成可编辑游戏原型

设计 brief/参考图 → coding agent 生成 Blender/UE5 工程 → 引擎运行行为测试 → agent 根据错误修订 → 人工设计师继续编辑。

Frontier3DBench 证明单个机制较容易，完整活动仍会被路径和状态问题击穿。近期适合白盒与局部机制，不适合无人监督生成完整游戏。

### 链路三：持续演化的互动叙事

玩家事件 → 传统状态机记录权威事实 → EvoWorld 世界脑推断离屏角色反应 → renderer 生成重访画面 → 游戏引擎校验位置、库存和任务条件。

世界模型提供开放式表演，代码维护不可违背的事实。当前属于本报告推断的组合架构，尚无完整产品证据。

### 链路四：可调用工具的实时数字角色

麦克风/摄像头 → LiveKit 或自有 agent 执行 ASR、LLM、记忆和工具调用 → Runway Characters 生成持续视频流 → WebRTC 播放。

已具备正式 API 和生产遥测，可构建客服、讲师和品牌角色；主要限制是单会话多卡成本和约两秒的完整响应。

### 链路五：世界模型驱动机器人训练

VLA 策略产生动作 → WoVR 生成未来并计算 reward → 幻觉片段被 mask 或截短 → PACE 用新策略数据刷新模拟器 → 策略部署到真实机器人。

代码负责 rollout、奖励、数据版本和回归测试；世界模型负责低成本想象。已有真机作者实验，尚缺安全认证和独立复现。

### 链路六：脚本化连续内容工厂

剧本/商品数据 → Drama Agent 或 AE Assistant 生成分集结构、表达式和镜头资产 → Seedance/Kling/Firefly 生成画面 → AE/人工审片维护最终工程。

已存在正式创作工具，但“剧情记忆”仍是平台内部能力，未形成开发者可调用的持久世界状态 API。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展／技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Frontier3DBench＋UE5＋coding agent | 生成工程并运行状态测试 | benchmark 可运行 | API 成本随模型变化 | 路径净空、持久状态、多人确定性 | [项目页](https://lab.einsia.ai/frontier3dbench/) |
| 影视/虚拟制作 | Video-Coder＋视频生成器 | 代码草模约束最终镜头 | 开源研究原型 | 未披露 | 复杂人物、资产版权、渲染时间 | [论文](https://arxiv.org/abs/2611.09894) |
| 直播与电商 | Runway Characters＋LiveKit | 实时问答、工具调用、视频流 | 正式产品 | 4×H100；中位约 2.1 秒 | 单会话成本、并发 | [技术报告](https://runway.com/news/research/real-time-video-at-batch-size-one) |
| 品牌互动 | Muse／Runway Characters | 可打断会话、情感形象 | 正式产品 | 未披露 | 品牌安全、角色一致性 | [Meta](https://about.fb.com/news/2026/11/introducing-muse-ai-voice-expressive-avatars/) |
| 教育培训 | 实时角色＋工具调用 | 讲解、演练、共享内容 | 可开发产品 | 未披露 | 事实核验、延迟和学习效果 | [Runway 文档](https://docs.dev.runwayml.com/characters/) |
| 企业数字员工 | Runway API＋业务状态机 | 对话、知识库、业务动作 | 正式 API | $0.20/分钟为当前公开角色价格口径 | SLA、权限审计、隐私 | [定价](https://dev.runwayml.com/pricing) |
| 陪伴与社交 | Muse Realtime Avatar | 聆听表情、打断、长期 agent | 美国首发产品 | 未披露 | 安全、依赖、记忆治理 | [Meta](https://about.fb.com/news/2026/11/introducing-muse-ai-voice-expressive-avatars/) |
| 空间计算 | SplatAnything＋3DGS viewer | 多视图重建、自由视角 | 研究代码 | 未披露 | 不可见区域空洞、移动端性能 | [论文](https://arxiv.org/abs/2611.08264) |
| 机器人/仿真 | WoVR＋VLA＋RL | imagined rollout 与真机迁移 | 研究原型 | 23 FPS 为作者世界模型指标 | 幻觉 reward、安全认证 | [项目页](https://wovr-corl.github.io/) |

## 8. 可复现资源与开发者入口

| 资源 | 开放情况 | 硬件/成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---:|---|
| [Video-Coder](https://github.com/CYWang735/Video-Coder) | 代码、9B 模型、SFT/RL 数据 | 需 GPU 与 Blender/生成模型 | 高 | 先运行单个物理事件 prompt，保存代码、草模和最终视频 |
| [Frontier3DBench](https://github.com/Einsia/Frontier3D) | Apache-2.0 代码；资产各自授权 | Git LFS 约 8–10GB；L3 需 UE5 | 高 | 先跑一个 Blender L1 和一个 UE5 L3 任务 |
| [EvoWorld 权重](https://huggingface.co/Tommy-Bie/EvoWorld) | 完整组件与 LoRA；LTX-2.x 社区许可 | 22B backbone，显存要求较高 | 中高 | 复现“事件发生—镜头离开—返回”案例 |
| [SplatAnything/InfiniSplat](https://github.com/zju3dv/InfiniSplat) | 研究/教育免费，组织使用需注册 | CUDA GPU | 中高 | 用同一场景的 4/8/16 张图比较空洞和模糊 |
| [Runway Characters](https://docs.dev.runwayml.com/characters/) | 托管 API、Widget、LiveKit/ElevenLabs 接口 | 按量付费 | 高 | 单图角色＋LiveKit agent，记录 turn latency 分解 |
| [WoVR](https://wovr-corl.github.io/) | 论文、项目材料；完整复现链较重 | 多 GPU＋机器人数据 | 仅研究团队 | 先在 LIBERO 比较普通 rollout 与 keyframe rollout |

## 9. 系统架构与技术趋势判断

明显升温的方向是“代码先验＋生成渲染”“显式世界脑＋视觉 renderer”以及“世界模型训练期使用、运行期轻量执行”。纯视频模型仍擅长表现，却难以独立承担精确几何、长期事实、碰撞和规则。

正在形成的可复用架构是：

`用户意图 → coding/剧情 agent → 权威状态机或 3D 草模 → 世界模型推演 → 视频/数字人渲染 → 引擎与视觉测试 → agent 修订`

其中代码维护可编辑资产、规则、权限、日志和验收；生成模型负责开放式外观、动作与表演；传统引擎继续负责碰撞、时间、多人同步和最终真值。

尚未解决的问题包括：完整对话低于 500 ms、世界模型被策略利用、跨镜头身份漂移、离屏物理真实性、生成式 UE5 世界的路径可达性、单会话多卡成本，以及肖像授权和可验证水印。

应降级看待的项目包括：只展示长视频但不报告持续吞吐的“实时”模型；只生成漂亮画面而没有状态接口的“世界模型”；以及宣称跨集记忆、却没有公开评测或导出接口的短剧平台。

## 10. 论文精读候选

1. [Video-Coder](https://arxiv.org/abs/2611.09894)：重点读 Harness、草模表示、执行/视频双奖励和 reward annealing。与 ManimCode 的差异是进入 3D 动态场景并服务下游生成器；风险是最终指标依赖生成模型和自动评测器。
2. [Frontier3DBench](https://arxiv.org/abs/2611.09017)：重点读 L3 行为评分、路径净空和状态化工作流。价值在于交付物是可运行工程；风险是 UE5 仅 15 个任务且结果依赖 harness。
3. [EvoWorld](https://arxiv.org/abs/2611.06748)：重点读世界脑状态记录、Identity-Anchored Attention 和自 rollout 训练。差异是显式评测离屏因果变化；风险是“合理反应”仍由生成数据和模型 judge 定义。
4. [Runway Characters serving](https://arxiv.org/abs/2611.05642)：重点读 KV cache 写时处理、FP8 GEMM/通信融合、解码并行和生产故障案例。它提供少见的真实 serving 证据；风险是无法独立复现专有模型。
5. [WoVR v3](https://arxiv.org/abs/2602.13977)：重点读 KIR、masked GRPO、PACE 和真机协议。差异是承认世界模型不可靠并限制 RL 利用错误；风险是缺少理论界限，真机任务规模仍小。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. Video-Coder 的 9B 权重、训练数据和 Blender harness 能否完整一键复现。
2. Frontier3DBench 的 stateful workflow 失败究竟来自代码推理、空间规划还是缺少自动 playtest。
3. EvoWorld 是否公开数据、推理代码和事件状态的机器可读格式。
4. Runway Characters 能否把语音 agent 的 1.42 秒瓶颈降到可自然打断的水平。
5. WoVR v3 是否开放统一训练配置及全部机器人实验资产。
6. SplatAnything 在人物、毛发和动态区域上的表现是否稳定。
7. Muse Realtime Avatar 是否开放开发者接口、地区扩展和安全评测。
8. Dreamina 的跨集剧情记忆能否导出、人工编辑并接受外部状态机控制。

### 本周可动手的小实验

1. **代码草模约束视频实验**  
   目标：验证 Video-Coder 是否减少复杂运动错误。组件：Blender、Video-Coder、同一视频生成模型。难点：固定 prompt 与随机种子。成功判据：10 个物理事件中，草模条件版本在动作顺序和相机约束上至少 7 个优于纯文本版本。

2. **Agent 生成 UE5 状态机实验**  
   目标：复现 Frontier3DBench 暴露的“局部机制可用、完整流程失败”。组件：UE5、coding agent、自动 playtest。难点：把玩家轨迹和状态断言写成机器测试。成功判据：门、抽屉、拾取分别通过，且“取钥匙—开门—进入区域”连续执行成功。

3. **实时数字人延迟预算实验**  
   目标：定位完整对话延迟。组件：LiveKit、ASR、LLM/TTS、Runway Characters。难点：统一时间戳和处理中断。成功判据：分别得到语音端点、LLM 首 token、TTS 首音频、Avatar 首帧及总延迟 P50/P95。

4. **离屏世界状态原型**  
   目标：验证“代码真值＋生成表演”的组合。组件：简单 JSON 状态机、VLM、EvoWorld 或普通视频模型。难点：防止 renderer 改写权威事实。成功判据：角色离屏后发生三个状态变化，镜头返回时物品、角色状态和事件后果全部与 JSON 一致。
