# 数字人、世界模型与互动作品研究日报：2026-08-23

检索窗口：2026-08-17 至 2026-08-23。

## 1. 本日摘要

本周期新增重点不在更大的基础模型，而在“论文能力变成可调用、可测试的运行组件”。数字人方向最值得关注的是 HelloWorld：8 月初论文提出按键触发角色转身、注视、挥手或说短句，本周期正式出现推理代码、训练代码和 LoRA，社会化角色由视频演示升级为可修改的工程原型。腾讯云点播则把可灵的数字人、口型同步和动作控制统一放入异步任务 API，说明商业接口正在从单一文生视频向“模型路由＋场景类型＋审核＋任务流”演进，但默认并发仅 1，仍偏离实时数字人。世界模型侧，ForgeWM 增加了完整阶段 checkpoint、训练数据和在线 Space，首次让上一期仍缺交互前端的少步可玩视频链路可以直接试用。AHA-WAM 新增可复现的预训练、RoboTwin 和蒸馏 checkpoint，证明机器人世界模型正在分化为低频世界规划器和高频动作执行器。ImageWAM 的 9B checkpoint进一步提出一个重要反例：工业控制未必需要生成整段未来视频，图像编辑产生的 KV cache 也可充当紧凑世界上下文。TriWorldBench 当前榜单扩展至 24 个模型，并把头部与双腕三视角的一致性、接触、轨迹和任务完成拆成可诊断指标，说明评测开始从“视频好看”转向“多个传感器是否描述同一个物理事件”。整体证据仍以开源组件、在线 demo 和作者实验为主；本周期未发现新增客户试点或规模化部署材料。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 8 月 21 日日报 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | HelloWorld 开放社会交互角色的训练、推理和 LoRA；腾讯云补齐数字人/口型/动作控制 API | 从论文 demo 进入可修改代码和正式云 API | 新增工程入口，但没有端到端实时对话或客户部署证据 |
| 世界模型 | 高 | ForgeWM 可在线试用；AHA-WAM、ImageWAM补齐权重；TriWorldBench扩展到24个模型 | 从“论文＋仓库”转向 checkpoint、Space、评测闭环 | 上期关注 SparsePR、DA-WAM等算法；本期增量主要在发布完整性 |
| Coding × 互动作品 | 高 | 按键事件、动作 schema、异步任务、机器人服务端/客户端和多视角 CI 成为明确接口 | 可运行 demo 和 SDK 增多，但生产级状态持久化、并发与SLA仍不足 | 代码进一步成为输入路由、世界状态、评测和安全治理层 |

## 3. 最值得关注的 8 个进展

### 1. HelloWorld：社会交互角色开放完整训练与推理链路

- **类型 / 主线：** 模型、GitHub、权重、demo；数字人 × 世界模型 × Coding。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05070)、[GitHub](https://github.com/AlayaLab/HelloWorld)、[LoRA](https://huggingface.co/oyly/HelloWorld_V1)。
- **发布或更新时间：** 论文首次提交于 2026-08-05；官方代码和权重在本周期公开并于 8 月 21—23 日被平台收录。仓库未展示精确首次公开时间，故具体小时级时间待核验。
- **相对上期的新变化：** 最近日报未收录；本次纳入原因不是旧论文重述，而是推理、训练、权重和复现配方已实际开放。
- **核心贡献：** 输入首帧、文本、SE(3) 相机轨迹和交互时间窗；按下 `F` 后，通过时间 cross-attention mask 只让交互相关 token 作用于指定帧，使角色面向用户挥手、点头、说短句。400 样本的 HelloWorldBench分别评估动作、时间和视线方向。[论文方法与日期](https://arxiv.org/abs/2608.05070)
- **Coding 接口：** `F` 键和时间窗是明确事件接口；仓库提供单条/批量 shell 推理、prompt 配置、训练脚本和可替换的合成训练数据提示。
- **互动作品 / 工业场景：** 生成式 NPC、互动电影中的角色回应、品牌吉祥物、虚拟导览。
- **成熟度 / 证据：** 已开源研究原型；论文实测＋官方代码。LoRA基于 LTX-2.3 22B，尚不是低延迟流式运行时。
- **重要性 / 阅读：** 高 / 必读。

### 2. ForgeWM：补齐完整 checkpoint 谱系和在线交互 Space

- **类型 / 主线：** 模型、dataset、GitHub、在线 demo；世界模型 × 实时运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2608.14022)、[checkpoint](https://huggingface.co/ForgeWM/ForgeWM)、[在线 Space](https://huggingface.co/spaces/hugging-apps/forgewm-world-model)。
- **更新时间：** Hugging Face 页面显示模型与数据于 2026-08-22 左右更新；论文于 8 月 21 日进入 HF Papers。
- **相对上期的新变化：** 上期已报道论文、仓库和训练资源；本期新增可直接下载的 Stage 0—3、1/2-step、跨 FPS checkpoint，以及正在运行的在线 Space。
- **核心贡献：** 将双向教师逐步转成因果自回归学生；不同 checkpoint 对应领域适配、teacher forcing、一致性蒸馏和 DMD。官方特别提示 Stage 3 必须使用训练时的六帧滑动注意力，否则会发生静默的分布外评测。[模型卡](https://huggingface.co/ForgeWM/ForgeWM)
- **Coding 接口：** `action_type`、键鼠/手柄 schema、配置文件、KV cache、保存草稿后的 replay-time refinement。
- **互动作品 / 工业场景：** 可玩视频、生成式游戏预览、交互轨迹回放与高质量重渲染。
- **成熟度 / 证据：** 已开源、可运行在线 demo；完整端到端延迟和长会话状态仍需独立测试。
- **重要性 / 阅读：** 高 / 必读。

### 3. AHA-WAM：开放预训练、RoboTwin 与蒸馏机器人权重

- **类型 / 主线：** 模型权重、GitHub、机器人运行时；世界模型 × 工业控制。
- **官方链接：** [论文](https://arxiv.org/abs/2606.09811)、[预训练权重](https://huggingface.co/SereneC/AHA-WAM-Pretrained)、[RoboTwin 权重](https://huggingface.co/SereneC/AHA-WAM-RoboTwin2.0)。
- **更新时间：** 两组模型于 2026-08-23 更新。
- **相对上期的新变化：** 旧论文首次提交于 6 月 8 日；本周期开放 13.7 GiB AHA-WAM、11.2 GiB Fast-WAM复现权重，以及 RoboTwin/AHA-WAM-Flash checkpoint，因此符合“旧工作重大开源”纳入条件。
- **核心贡献：** 低频 video DiT保存长期 KV 世界上下文，高频 action DiT按短 chunk闭环执行，通过 Observation-Guided Video-Context Routing读取最新观察而不重跑视频分支。论文报告 24.17 Hz 控制、相对 Fast-WAM 4.59 倍加速；均为作者实验。[论文](https://arxiv.org/abs/2606.09811)
- **Coding 接口：** Hydra配置、`init_checkpoint`微调入口、多 GPU RoboTwin评测器，以及 `wam_policy_server.py`/远程客户端。
- **互动作品 / 工业场景：** 机器人数字孪生、具身 NPC、远程操作与低频规划—高频控制系统。
- **成熟度 / 证据：** 已开源研究组件；不是通用零样本机器人策略。
- **重要性 / 阅读：** 高 / 必读。

### 4. 腾讯云点播 AIGC 生视频 API：数字人、口型与动作控制进入统一任务接口

- **类型 / 主线：** SDK/API、正式云产品；数字人 × Coding。
- **官方链接：** [CreateAigcVideoTask 文档](https://cloud.tencent.com/document/api/266/126239)。
- **更新时间：** 2026-08-21 02:05:40。
- **相对上期的新变化：** 首次收录。
- **核心贡献：** 一个异步任务接口可路由 Kling、Vidu、Hailuo、Hunyuan 等模型；可灵的 `SceneType` 明确支持 `avatar_i2v`、`lip_sync`、`motion_control`。接口还提供输入/输出合规检查、音频生成、任务优先级、幂等去重和后续点播任务流。
- **Coding 接口：** Cloud API 3.0，支持 Python、Java、Go、Node.js、C++ 等 SDK；返回 `TaskId`，适合与回调、队列和审核流水线组合。
- **互动作品 / 工业场景：** 批量数字员工视频、多语言营销内容、直播前素材生成、用户动作模板视频。
- **成熟度 / 证据：** 正式产品文档；默认仅 1 个并发处理，且是异步生成，不能据此声称支持实时数字人。默认接口限频 20 次/秒，但这不等于生成吞吐。
- **重要性 / 阅读：** 高 / 必读。

### 5. TriWorldBench：三视角世界模型榜单扩展至 24 个系统

- **类型 / 主线：** benchmark、dataset、leaderboard、校验工具；世界模型评测。
- **官方链接：** [官网与榜单](https://www.triworldbench.com/)、[数据集](https://huggingface.co/datasets/TriWorldBench/Dataset)、[提交规范](https://www.triworldbench.com/submission)。
- **更新时间：** 当前提交周期于 2026-08-23 19:59:59 北京时间关闭；8 月 23 日页面显示 24 个已发布模型。
- **相对上期的新变化：** 最近日报未收录；本周期榜单、Space和数据集均有更新。
- **核心贡献：** 500 个测试 episode、50 类操作任务，同时生成头部、左腕和右腕视频；19 项信号覆盖任务对齐、三视角一致性、接触/透视、轨迹、时序和视觉质量。当前 BWM 的 TWB-Score 为 65.54，领先 WoVR_Plus 的65.39，但榜单属于组织方自动评测，不代表实体机器人成功率。
- **Coding 接口：** 每个 episode 输出三个 MP4；`submission_check.py`检查分辨率、帧数、目录和校验和，适合接入生成模型 CI。
- **互动作品 / 工业场景：** 机器人仿真验收、多机位虚拟制作、空间计算中的跨视角一致性测试。
- **成熟度 / 证据：** 可运行 benchmark；第三方系统仍需按统一版本复测。
- **重要性 / 阅读：** 高 / 必读。

### 6. ImageWAM 9B：用图像编辑 cache 替代完整未来视频

- **类型 / 主线：** checkpoint、GitHub；世界模型 × 推理架构。
- **官方链接：** [论文](https://arxiv.org/abs/2606.19531)、[9B LIBERO checkpoint](https://huggingface.co/yuyangalin/ImageWAM-FLUX.2-9B-LIBERO)。
- **更新时间：** checkpoint及关联 LIBERO 数据在 2026-08-20—21 日更新。
- **相对上期的新变化：** 6 月论文的 9B checkpoint、配置和归一化统计进入可下载状态，构成产品化/可复现更新。
- **核心贡献：** 不在推理时解码未来图像，而让 flow-matching 动作分支读取图像编辑去噪产生的 KV cache。论文报告相对视频 WAM 将 FLOPs降至约 1/6、延迟降至约 1/4，属作者实测。[论文](https://arxiv.org/abs/2606.19531)
- **Coding 接口：** 提供 `model.pt`、`dataset_stats.json`、配置和八 GPU LIBERO评测脚本。
- **互动作品 / 工业场景：** 机器人控制、低延迟物理 NPC、只需决策而不需展示“想象视频”的后台 agent。
- **成熟度 / 证据：** 已开放研究 checkpoint；需另行准备 FLUX.2 9B 基础权重。
- **重要性 / 阅读：** 中高 / 必读。

### 7. MoVerse 在线世界漫游：旧模型获得本周期可运行入口

- **类型 / 主线：** 在线 demo、模型；世界模型 × 空间交互。
- **官方链接：** [模型与入口](https://huggingface.co/Orange-3DV-Team/MoVerse)、[论文页](https://huggingface.co/papers/2606.13376)。
- **更新时间：** Hugging Apps 的 MoVerse Space 于 2026-08-19 左右上线/更新。
- **相对上期的新变化：** 6 月论文不作为新增；本周期在线漫游与编译 demo 是纳入原因。
- **核心贡献：** 单张窄视野图像先扩展为全景，再提升为持久化 3D Gaussian scaffold，由实时视频模型补充视图；作者报告 RTX 4090 约 8 FPS。
- **Coding 接口：** 360°相机输入、Gaussian场景状态、漫游控制和 AOT 编译 demo。
- **互动作品 / 工业场景：** 沉浸式网页、虚拟看房、空间导览、预演型关卡。
- **成熟度 / 证据：** 可运行 demo；8 FPS仍不足以替代传统引擎的高帧率交互。
- **重要性 / 阅读：** 中 / 可读。

### 8. GigaWorld-1 在线 rollout demo：机器人世界模型更容易被非训练团队验证

- **类型 / 主线：** 在线 demo、checkpoint；世界模型 × 机器人仿真。
- **官方链接：** [GitHub](https://github.com/open-gigaai/giga-world-1)、[模型说明](https://huggingface.co/lh152/gigaworld1)。
- **更新时间：** Hugging Apps demo 于 2026-08-19 左右上线；Stage-1 Nano/Pro权重此前已开放。
- **相对上期的新变化：** 本次只纳入在线推理入口，不重复旧论文和旧权重。
- **核心贡献：** 以动作条件视频 rollout辅助机器人策略评估；开放数据处理、训练、推理、checkpoint转换和 LoRA 合并流程。
- **Coding 接口：** Diffusers格式、场景 LoRA、训练脚本和动作条件视频推理。
- **互动作品 / 工业场景：** 机器人策略回归、合成传感器视频、操作前“如果执行此动作会怎样”的可视预演。
- **成熟度 / 证据：** 开源研究系统；在线 Space当前可能休眠，Stage-2蒸馏仍标记为 coming soon。
- **重要性 / 阅读：** 中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：** HelloWorld把角色响应从环境中的自主运动扩展到用户触发的社会动作，但交互词汇仍是挥手、点头、转身、短语等离散事件，不是开放式持续表演。
- **3D/4D 表示：** 本周期没有新的通用人体4D重建核心论文。MoVerse的 Gaussian scaffold属于场景级空间记忆，可承载角色所在空间，但不解决人体衣物、毛发或关节控制。
- **实时交互：** HelloWorld是按时间窗生成完整片段，不等同于连续流式头像；腾讯云接口也是异步任务。数字人实时性相较上一期 EfficientSync 的组件级高速唇同步没有新突破。
- **音视频与情感：** HelloWorld权重包含联合生成音频的样例，但没有 turn-taking、用户打断、情绪状态或端到端对话时延评测。
- **工程部署：** 腾讯云 API 已提供幂等、优先级、合规检查、存储与任务流，是本周期最接近生产系统的数字人增量；默认并发 1 是明显约束。
- **安全与身份治理：** API包含 `PersonGeneration` 和输入/输出合规开关，但未披露肖像授权、声纹同意、水印和撤回机制。HelloWorld许可证继承 LTX-2 Community License，年收入达到其规定门槛的商业实体需要另行确认商业授权。

## 5. 世界模型进展

- **架构与训练：** ForgeWM代表“视频生成器因果蒸馏”；AHA-WAM代表“低频世界规划＋高频动作”；ImageWAM则反对无条件生成完整视频，改用编辑 cache。
- **可控/可玩生成：** ForgeWM已有键鼠和手柄动作；HelloWorld加入人物交互事件；MoVerse侧重相机漫游。控制信号正在从相机位姿扩展到人物事件和机器人 action chunk。
- **长时与物理一致性：** 本周期没有解决长时漂移的新核心算法；趋势仍是把状态交给 KV memory、Gaussian scaffold、机器人 proprioception或传统运行时，而不是完全依赖像素历史。
- **空间表示：** MoVerse使用全景＋Gaussian；TriWorldBench用三相机共同约束同一物理状态。二者分别从生成和评测侧强化跨视角一致性。
- **机器人/游戏/仿真：** AHA-WAM与ImageWAM直接输出动作，ForgeWM生成可玩像素未来，GigaWorld面向策略评估，形成“控制策略—可视预测—评测”不同分工。
- **实时部署与评测：** 论文中的模型 FPS、控制 Hz 和接口限频不可混用。真实应用必须单独测采集、编码、传输、排队、模型、解码与执行器总延迟。

## 6. Coding × 新型互动作品

### 链路一：会回应观众的生成式角色

**首帧与角色 prompt → `F` 键/事件总线写入交互时间窗 → HelloWorld生成注视、挥手和短句 → Web播放器或互动电影 → 品牌角色、NPC、虚拟展演。**

代码负责事件触发、角色状态、时间窗和播放；模型负责视觉与声音表演；传统引擎负责场景规则。已有开源生成证据，但连续会话属于本报告推断。

### 链路二：在线玩、离线精修的可玩视频

**键鼠/手柄事件 → ForgeWM少步因果模型与滑动 KV cache → 在线低延迟草稿 → replay-time refinement → 生成式游戏和互动回放。**

代码负责动作采样、缓存、session、录制与重放；模型负责像素未来。Space证明前端入口存在，但完整延迟、多人并发和长时状态仍未披露。

### 链路三：低频想象、高频控制的机器人角色

**语言任务＋三相机观察 → AHA-WAM低频世界规划器 → ActionDiT以24 Hz级别输出短动作块 → ROS/机器人客户端执行 → 仓储操作、具身NPC。**

模型负责长期上下文与动作建议；服务端/客户端负责时钟、归一化、限位和紧急停止；物理系统负责真实执行。已有作者实体实验和代码，但生产安全认证不足。

### 链路四：无需渲染想象视频的后台世界模型

**当前图像＋任务指令 → ImageWAM图像编辑去噪 cache → action expert → 控制器 → 机器人或物理角色。**

如果用户不需要观看未来视频，代码可以跳过 VAE解码与视频传输，显著缩短决策链；模型只提供动作相关的世界上下文。已有 LIBERO checkpoint，工业优势仍需第三方复测。

### 链路五：数字人批量内容工厂

**CMS/商品库 → 腾讯云 `CreateAigcVideoTask` → 数字人/口型/动作控制 → 合规检查与点播任务流 → 营销、培训、客服视频。**

代码负责模型路由、幂等、优先级、审核、回调和资产管理；生成模型负责表演。正式 API 是可运行证据，但不是实时直播方案。

### 链路六：三相机生成系统的自动化验收

**机器人轨迹与指令 → 世界模型生成头/左腕/右腕视频 → TriWorldBench检查接触、轨迹和跨视角状态 → CI阻断回归 → 仿真与策略评估。**

代码负责固定输入、版本、打包、校验和阈值；评测模型负责视觉判断；真实仿真/机器人仍是最终验证源。该链路已有数据、脚本和榜单支持。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | ForgeWM、HelloWorld、事件总线、播放器 | 键鼠移动、按键触发角色回应 | 在线/开源 demo | ForgeWM论文模型侧数据不含完整I/O | 长时状态、规则正确性、并发 | [ForgeWM](https://huggingface.co/ForgeWM/ForgeWM)、[HelloWorld](https://github.com/AlayaLab/HelloWorld) |
| 影视/虚拟制作 | HelloWorld、MoVerse、相机轨迹 | 可编程角色动作和虚拟摄影机 | 研究 demo | MoVerse作者报告约8 FPS/4090 | 多镜头连续性、资产导出 | [MoVerse](https://huggingface.co/papers/2606.13376) |
| 直播与电商 | 腾讯云 API、数字人、口型同步 | 异步生成商品讲解与配音视频 | 正式产品接口 | 默认并发1；生成时延未披露 | 不支持实时、成本与SLA未核验 | [API](https://cloud.tencent.com/document/api/266/126239) |
| 品牌互动 | HelloWorld、Web事件、内容审核 | 用户触发吉祥物回应 | 可运行原型 | 未披露 | 品牌一致性、版权、实时性 | [GitHub](https://github.com/AlayaLab/HelloWorld) |
| 教育培训 | 数字人 API、任务流、课程系统 | 按章节批量生成讲师视频 | 正式API/内容层推断 | 未披露 | 事实审核、更新成本 | [腾讯云](https://cloud.tencent.com/document/api/266/126239) |
| 企业数字员工 | 数字人、LLM/TTS、知识库 | 对话后生成视频答复 | 异步可用，实时不足 | 默认并发1 | 打断、端到端延迟、身份授权 | 同上 |
| 陪伴与社交 | HelloWorld、长期记忆、角色状态机 | 角色注视、动作和短句回应 | 研究原型 | 未披露 | 情感安全、长期记忆 | [论文](https://arxiv.org/abs/2608.05070) |
| 空间计算 | MoVerse、Gaussian、Web/原生渲染 | 用户在生成空间中漫游 | 在线 demo | 约8 FPS/4090，作者声称 | 移动端算力、碰撞与编辑 | [模型](https://huggingface.co/Orange-3DV-Team/MoVerse) |
| 机器人/仿真 | AHA-WAM、ImageWAM、TriWorldBench | action chunk闭环、多视角回归 | 开源研究系统 | AHA-WAM作者报告24.17 Hz | 安全验证、版本敏感、真实数据 | [AHA-WAM](https://arxiv.org/abs/2606.09811)、[TriWorldBench](https://www.triworldbench.com/) |

## 8. 可复现资源与开发者入口

| 资源 | 许可证 / 开放状态 | 硬件或成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|---|
| [HelloWorld](https://github.com/AlayaLab/HelloWorld) | 推理、训练、LoRA开放；受LTX-2 Community License约束 | LTX-2.3 22B，预计需高显存GPU | 值得 | 先跑7个官方样例，再移动 `F` 时间窗检查响应是否随之移动 |
| [ForgeWM Space](https://huggingface.co/spaces/hugging-apps/forgewm-world-model) | Space可运行；checkpoint Apache-2.0 | Space可零安装体验；本地模型需GPU | 强烈建议 | 同一首帧分别输入前进/转向，记录动作响应、墙钟延迟和漂移 |
| [AHA-WAM RoboTwin](https://huggingface.co/SereneC/AHA-WAM-RoboTwin2.0) | MIT；权重、统计和配置齐全 | 默认评测管理器示例为8 GPU | 机器人团队建议 | 固定一个任务、10个episode，对比AHA-WAM与Flash成功率及控制频率 |
| [ImageWAM 9B](https://huggingface.co/yuyangalin/ImageWAM-FLUX.2-9B-LIBERO) | MIT checkpoint；需另备FLUX.2权重 | 官方示例为8 GPU评测 | 有算力团队建议 | 在同一LIBERO任务比较视频WAM与ImageWAM端到端延迟 |
| [TriWorldBench](https://huggingface.co/datasets/TriWorldBench/Dataset) | 500测试＋100验证episode；约982 MB | 评测还需VLM/视觉编码器 | 强烈建议 | 先对100个验证episode跑三视角视频和 `submission_check.py` |
| [腾讯云 AIGC API](https://cloud.tencent.com/document/api/266/126239) | 正式付费API | 默认并发1；价格按模型计，本文未完成核验 | 产品团队建议 | 各发起1个 `avatar_i2v`、`lip_sync`、`motion_control` 任务，记录排队与完成时延 |
| [MoVerse](https://huggingface.co/Orange-3DV-Team/MoVerse) | 权重、代码和Space入口 | 作者使用RTX 4090 | 可读/体验 | 单张室内图测试180°回转后重访一致性 |
| [GigaWorld-1](https://github.com/open-gigaai/giga-world-1) | Apache-2.0；Stage-1开放，Stage-2待发布 | Nano 1.3B / Pro 5B | 可跟踪 | 用同一初始观察和两条动作轨迹比较预测分叉是否合理 |
| [MetaPerson 1.37.0](https://docs.metaperson.avatarsdk.com/business-integration/release_notes/desktop/) | 商业SDK；8月17日更新 | 未披露 | 仅跟踪 | 本期仅改善页面加载速度，无新的生成或驱动能力，不列入重点进展 |

## 9. 系统架构与技术趋势判断

明显升温的方向：

1. **事件成为生成模型的正式控制接口。** HelloWorld 的 `F` 键、ForgeWM的动作 schema、腾讯云的 `SceneType` 都比自由文本更适合软件系统调用。
2. **世界模型与动作模型开始异步运行。** AHA-WAM不再要求昂贵的视频分支和高频控制器同速更新。
3. **“世界上下文”不必等于可观看视频。** ImageWAM说明编辑 cache也可承担决策表示，减少无关像素生成。
4. **评测转向多传感器、任务和物理状态。** TriWorldBench明确惩罚错误手臂、错误接触和三视角冲突，而非只打审美分。
5. **在线 demo成为研究发布的关键一层。** ForgeWM、MoVerse和GigaWorld的Space降低了验证门槛，但Space可运行不代表部署SLA。

正在形成的可复用架构为：

`用户/agent意图 → 结构化事件或动作 → 低频世界上下文/状态记忆 → 高频规则或动作执行 → 神经视频/数字人表现 → 多视角、任务、安全与合规评测 → 回放或人工接管`

尚未解决的问题包括：跨分钟身份和世界状态、真实端到端低延迟、多人并发、生成与物理引擎双向同步、可观测性、许可证兼容、肖像/声纹授权及可撤回性。

营销或包装风险主要来自两类说法：把模型内部 FPS 当成完整交互延迟；把在线 Space、实体演示或正式 API 自动推导成规模化部署。本周期没有足够客户或运营数据支持上述推导。

## 10. 论文精读候选

1. **[HelloWorld](https://arxiv.org/abs/2608.05070)**  
   值得读：首次把社会交互时间窗做成视频世界模型接口。重点看第3节训练与 temporal cross-attention mask、第4节交互指标。与普通 talking head 的差异是同时控制角色响应和相机轨迹。复现风险是 LTX-2.3 22B成本及交互词汇有限。

2. **[ForgeWM](https://arxiv.org/abs/2608.14022)**  
   值得读：完整展示双向生成器到少步因果模型的四阶段工程路线。重点看 causal consistency、on-policy distribution matching和 replay refinement。复现风险是训练需多卡H20，且完整端到端延迟尚未验证。

3. **[AHA-WAM](https://arxiv.org/abs/2606.09811)**  
   值得读：低频世界规划与高频动作控制的异步拆分很可能成为机器人运行时范式。重点看 rolling KV、OVCR和部署频率。风险是结果对相机、动作归一化和机器人配置高度敏感。

4. **[ImageWAM](https://arxiv.org/abs/2606.19531)**  
   值得读：直接质疑视频生成是否是WAM的必要条件。重点看编辑 KV cache如何接入 action expert，以及与视频WAM的匹配比较。风险是“更快”可能依赖特定 backbone与评测配置。

5. **[TriWorldBench](https://www.triworldbench.com/)**  
   严格说目前更像公开 benchmark而非已确认 arXiv论文，但值得精读指标实现：VLM三种一致性判断、VQA、STATE校正和轨迹DTW。风险是复合分数的权重、VLM偏差和排行榜版本变化。

## 11. 下周跟踪与可行动建议

需要持续追踪：

1. HelloWorld能否扩展为音频流驱动、可打断、带长期角色状态的持续会话。
2. ForgeWM Space加入VAE解码和网络传输后的真实输入响应时间。
3. ForgeWM在不同动作分布下能否保持一分钟以上状态与场景一致。
4. AHA-WAM checkpoint能否由第三方复现论文的24.17 Hz和实体任务成功率。
5. ImageWAM的延迟优势在单GPU、相同精度和相同控制频率下是否仍成立。
6. TriWorldBench 8 月 28 日新一轮榜单是否改变当前模型排序。
7. 腾讯云数字人任务的实际单条价格、排队时延、并发扩容与回调可靠性。
8. 数字人 API 是否补充可验证的肖像授权、水印、溯源和删除机制。

本周适合动手的小实验：

1. **HelloWorld交互时间窗回归测试**  
   目标：确认事件控制而非随机动作。组件：官方LoRA、同一首帧和prompt、三个不同 `F` 时间窗。难点：随机种子与GPU显存。成功判据：动作类型不变，响应起止时间随窗口稳定移动，窗口外角色不提前动作。

2. **ForgeWM端到端延迟审计**  
   目标：测出真实“按键到可见帧”延迟。组件：1-step checkpoint、VAE、输入监听、播放器。难点：区分模型、解码、编码和前端缓冲。成功判据：连续100次输入报告P50/P95延迟、动作正确率和显存峰值，而非只引用模型FPS。

3. **三视角世界模型 CI**  
   目标：把跨相机冲突转成发布阻断条件。组件：TriWorldBench验证集、任一开放机器人世界模型、`submission_check.py`。难点：评测模型成本和版本固定。成功判据：能自动定位“头部视图成功但腕部接触失败”的episode，并在模型更新后给出可比较回归结果。

4. **视频WAM与ImageWAM等条件对比**  
   目标：判断产品是否真的需要输出未来视频。组件：ImageWAM、一个视频WAM、同一LIBERO任务和GPU。难点：统一预处理、动作频率和基础模型。成功判据：同时报告任务成功率、P95动作延迟、FLOPs、显存；若ImageWAM保持任务性能且延迟显著下降，则后台控制链可取消视频解码。
