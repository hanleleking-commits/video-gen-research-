# 数字人、世界模型与互动作品研究日报：2026-07-10

检索窗口：2026-07-04 至 2026-07-10。

## 1. 本日摘要

过去 7 天最明确的变化，是数字人和世界模型开始获得更接近软件组件的输入输出接口：动作流、在线文本提示、路径/关键帧约束、参考资产标签、点云骨架和多 agent 调度，正在取代一次性自然语言 prompt。数字人侧没有新的强 talking-head、lip-sync 或实时对话头像基座，但 7 月 10 日开放的 NVIDIA ARDY 已能将在线文本、鼠标路径、键盘速度和稀疏关节约束转为流式 3D 人体动作，并提供代码、四组权重和浏览器 demo，是本周期最强的“数字人 × runtime”增量。[DreamCharacter-1](https://dreamcharacter-x.github.io/)则把单图 3D 角色生成向动画就绪网格、高分辨率纹理和批量生产推进，但尚未开放代码或产品 API。世界模型侧强度更高：[LingBot-World 2.0](https://arxiv.org/abs/2607.07534)首次把 pilot agent、director agent、动作控制、文本事件和多人接口放入同一生成世界架构，并开放 14B 推理权重；但官方的 60 FPS 部署代码不开放，现有仓库示例仍需 8 GPU，不能把它视为已完成的低成本产品。MIRA 已提供四玩家 Rocket League 实时体验、代码和数据，是本周期可运行证据最强的可玩视频系统。[AlayaWorld](https://alaya-lab.github.io/AlayaWorld/)虽宣称 720p、24 FPS 和 60 秒以上 rollout，但 GitHub 当前只有 README 和发布路线图，需按官方 demo 而非完整开源框架处理。机器人侧 LingBot-VA 2.0 和 WCog-VLA 表明“生成未来画面”正在转向“生成可执行动作与多主体轨迹”。产业信号仍主要来自研究 demo 和开放权重，尚未发现窗口内可核验的客户规模化部署；最接近落地的是游戏原型、角色动画、机器人/驾驶仿真和可控短视频生产。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | ARDY 开放流式人体动作代码、权重与交互 demo；DreamCharacter-1 面向动画就绪 3D 资产 | 从论文/离线人物视频推进到可嵌入运行时的动作生成组件 | 新增 ARDY、DreamCharacter-1、GIRAF、HumanForge；talking-head 仍无强新增 |
| 世界模型 | 很高 | LingBot-World 2.0 引入 agentic harness；MIRA 提供四玩家实时 demo | 从单人相机控制扩展到多人动作、文本事件和 agent 调度 | 新增 LingBot-World 2.0、LingBot-VA 2.0、WCog-VLA；AlayaWorld 开源程度被重新核验并下调 |
| Coding × 互动作品 | 很高 | `action_path`、`autoregressive_step()`、在线约束、KV cache、agent 分工成为明确接口 | ARDY/MIRA/LingBot-World 已有可运行入口，但生产部署和商用许可仍不足 | 从 JSON 批处理和研究框架，进一步走向持续运行、可重规划的生成 runtime |

## 3. 最值得关注的 11 个进展

### 1. LingBot-World 2.0 / Infinite Worlds with Versatile Interactions

- **类型 / 时间 / 主线：** 世界模型、权重、GitHub、demo；2026-07-08，仓库记录 7 月 9 日释放；世界模型 / Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2607.07534)、[GitHub](https://github.com/Robbyant/lingbot-world-v2)。
- **相对上期的新变化：** 全新纳入；是 7 月 8–9 日的核心新增。
- **核心贡献：** 14B 主模型加 1.3B 轻量版本；加入攻击、射箭、施法、射击等动作、文本驱动事件、多人界面，以及 pilot/director 双 agent。作者声称蒸馏版可驱动 720p、60 FPS。
- **Coding 接口：** 仓库提供 Wan2.2 基础上的 `generate.py`、KV-cache 分块因果推理、`action_path` 和 FSDP/Ulysses 多卡命令。agent harness 的完整实现和正式部署代码未开放。
- **互动/工业场景：** 生成式 RPG、互动电影、虚拟展演、游戏 NPC 行为预演。
- **成熟度 / 证据：** 已开放 14B causal-fast 权重和离线推理；实时 Web/移动体验由合作平台提供；部署代码明确不发布。官方声称，尚无第三方性能验证。
- **重要性 / 阅读：** 高 / 必读。许可证为 CC BY-NC-SA 4.0，不可直接商用。

### 2. ARDY：流式可控 3D 人体动作生成

- **类型 / 时间 / 主线：** SIGGRAPH 2026 论文、GitHub、权重、交互 demo；论文 2026-07-09，代码和四组权重 7 月 10 日开放；数字人 / Coding × runtime。
- **官方链接：** [项目页](https://research.nvidia.com/labs/sil/projects/ardy/)、[GitHub](https://github.com/nv-tlabs/ardy)。
- **相对上期的新变化：** 全新纳入。
- **核心贡献：** 用显式 root motion 加 latent body embedding 的混合表示，在自回归两阶段扩散中先生成根节点、再生成身体；支持在线文本、路径、waypoint、全身关键帧和稀疏末端关节约束。
- **Coding 接口：** `Ardy.autoregressive_step()` 接收文本 embedding、`motion_mask` 和 `observed_motion`，输出连续动作帧；浏览器 demo 支持鼠标、键盘和动态重规划，CLI 输出 NPZ 或 Unitree G1 的 MuJoCo qpos CSV。
- **互动/工业场景：** 游戏角色、虚拟人全身表演、预演动画、Unitree G1 动作规划、训练仿真。
- **成熟度 / 证据：** 已开源，Apache-2.0 代码；四组模型权重单独采用 NVIDIA Open Model License。官方在 RTX 4090 上测试，TensorRT 可选。
- **重要性 / 阅读：** 高 / 必读。

### 3. MIRA：四玩家互动世界模型

- **类型 / 时间 / 主线：** 论文、代码、数据、live demo；2026-07-06，v2 7 月 7 日；世界模型 / Coding × 游戏。
- **官方链接：** [论文](https://arxiv.org/abs/2607.05352)、[实时体验](https://mira-wm.com/)。
- **相对上期的新变化：** 上期已提及；本期核验到官方 live demo 可进入，开放状态强于“仅论文声称”。
- **核心贡献：** 5B latent diffusion 同时接收四名玩家的 action streams，学习将视觉变化归因到正确玩家；训练数据为 10,000 小时机器人对局。
- **Coding 接口：** 控制器/键盘动作流进入模型，推理服务器生成下一段视频观测；官方提供代码、数据入口和多人 demo。
- **互动/工业场景：** AI 原生体育游戏、多人对战原型、游戏行为测试和训练数据生成。
- **成熟度 / 证据：** 可运行 demo；作者报告单张 B200 20 FPS、五分钟评测区间内质量稳定。仍缺独立延迟和物理一致性复测。
- **重要性 / 阅读：** 高 / 必读。

### 4. DreamCharacter-1：面向生产的 3D 角色资产

- **类型 / 时间 / 主线：** 技术报告、项目页；2026-07-08；数字人 / 3D 资产基础设施。
- **官方链接：** [项目页](https://dreamcharacter-x.github.io/)、[论文](https://arxiv.org/abs/2607.07817)。
- **相对上期的新变化：** 全新纳入。
- **核心贡献：** 在冻结的 3D foundation backbone 上做几何偏好优化、两阶段纹理生成、稀疏体素补洞和蒸馏，输出单图驱动、动画就绪且身份保持较好的网格与纹理。
- **Coding 接口：** 尚未发布代码、API、SDK 或引擎插件；当前只能按潜在资产管线组件判断。
- **互动/工业场景：** 游戏角色初稿、数字员工形象、UGC avatar、虚拟制作资产。
- **成熟度 / 证据：** 研究原型；VRoid benchmark 指标均为作者实验，暂无第三方验证。
- **重要性 / 阅读：** 高 / 必读。

### 5. AlayaWorld

- **类型 / 时间 / 主线：** 论文、项目页、GitHub 占位仓库；论文 2026-07-07，项目页/仓库 7 月 8 日；世界模型。
- **官方链接：** [项目页](https://alaya-lab.github.io/AlayaWorld/)、[GitHub](https://github.com/AlayaLab/AlayaWorld)。
- **相对上期的新变化：** 重新核验开源状态：仓库只有资产、README 和许可证；推理代码、权重、训练代码和数据仍在 roadmap。
- **核心贡献：** 3D cache 做视角重投影、压缩历史维持时间记忆，以 drifted history 和 error bank 抑制长时误差；DMD 蒸馏支持短 chunk 推理和 prompt switching。
- **Coding 接口：** 演示支持 6-DoF 相机和 chunk 边界事件提示，但开发者 API 尚未开放。
- **互动/工业场景：** 可漫游品牌世界、互动魔法/事件、虚拟制片预演。
- **成熟度 / 证据：** 官方 demo；720p、24 FPS、60 秒以上均为官方声称。
- **重要性 / 阅读：** 高 / 必读。

### 6. LingBot-VA 2.0

- **类型 / 时间 / 主线：** 视频—动作模型；2026-07-09；世界模型 / 机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.08639)。
- **相对上期的新变化：** 全新纳入。
- **核心贡献：** 从内容生成视频模型转向原生 embodiment 预训练；采用语义视觉—动作 tokenizer、严格因果训练、稀疏 MoE 和异步闭环推理。
- **Coding 接口：** 机器人状态和视觉观测进入闭环策略；未来 latent 可在动作执行期间并行预测，再用最新观测重新 grounding。
- **互动/工业场景：** 多机器人操作、远程机器人界面、合成训练和策略评测。
- **成熟度 / 证据：** 论文称已有真实机器人部署，但规模、稳定性和运行成本未披露，按作者实测/研究部署处理。
- **重要性 / 阅读：** 高 / 必读。

### 7. Aura

- **类型 / 时间 / 主线：** 论文、GitHub、推理权重；2026-07-05，v2 7 月 7 日；数字人 / 可控视频。
- **官方链接：** [论文](https://arxiv.org/abs/2607.04311)、[GitHub](https://github.com/Camellia997/Aura)。
- **相对上期的新变化：** 已报道；本期确认仓库是“从完整研究代码抽出的最小推理包”，并非完整训练栈。
- **核心贡献：** 用 `[PERSON_n]`、`[OBJECT_n]`、`[SCENE_n]` 将导演脚本与多参考图绑定，增强人物、物体和场景的一致性。
- **Coding 接口：** JSON 样本、结构化 prompt、单机 CPU offload、多机 FSDP/Ulysses。
- **互动/工业场景：** 品牌短片、虚拟主播素材、批量角色镜头；约 5 秒离线片段，不是实时数字人。
- **成熟度 / 证据：** 已开源推理包，Apache-2.0；质量仍主要是作者演示。
- **重要性 / 阅读：** 中高 / 必读。

### 8. Point as Skeleton

- **类型 / 时间 / 主线：** 论文、GitHub、checkpoint；2026-07-07；世界模型 / 驾驶仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2607.06516)、[GitHub](https://github.com/krauwu/point-as-skeleton)。
- **相对上期的新变化：** 已报道；本期确认 nuPlan 用户自定义轨迹接口、训练配置和 nuScenes/nuPlan 权重入口确已提供。
- **核心贡献：** 将背景点云、actor 点云资产和模拟器状态作为显式 skeleton，减少驾驶世界模型偏离用户轨迹。
- **Coding 接口：** `src/dwm` 负责生成栈，`nuplan` 负责 DB replay、状态更新和用户轨迹。
- **互动/工业场景：** 自动驾驶闭环测试、危险场景合成、感知/规划回归测试。
- **成熟度 / 证据：** 已开放研究代码和权重，依赖数据集与大量点云资产，复现门槛高。
- **重要性 / 阅读：** 高 / 必读。

### 9. HumanForge

- **类型 / 时间 / 主线：** benchmark、dataset/代码预告；2026-07-09；数字人安全。
- **官方链接：** [论文](https://arxiv.org/abs/2607.08705)。
- **相对上期的新变化：** 全新纳入。
- **核心贡献：** 将数字人伪造评测从换脸扩展到人—人、人—物交互和跨模态一致性；以 LangGraph 编排六个 agent，生成超过 18K 视频片段及结构化伪造解释。
- **Coding 接口：** 多 agent 数据构建与闭环验证管线；代码和数据仍是“将发布”。
- **互动/工业场景：** 数字人审核、内容溯源、广告和社交平台风控。
- **成熟度 / 证据：** benchmark 论文；未开放资源，不能视为可部署检测器。
- **重要性 / 阅读：** 中高 / 可读。

### 10. SAGA

- **类型 / 时间 / 主线：** 论文、training-free 推理方法；2026-07-09；世界模型支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.08020)。
- **相对上期的新变化：** 全新纳入。
- **核心贡献：** 用 latent acceleration 的频谱信号检测并抑制 AR 视频中的闪烁、抖动和结构漂移；无需修改或重训 backbone。
- **Coding 接口：** 可作为 chunk-wise autoregressive diffusion 的推理 guidance 层。
- **互动/工业场景：** 可玩视频、持续虚拟摄像机和长时角色画面的稳定器。
- **成熟度 / 证据：** 论文实测；尚未确认代码开放。
- **重要性 / 阅读：** 中 / 可读。

### 11. OpenCoF

- **类型 / 时间 / 主线：** dataset、模型、代码预告；2026-07-09；视频推理 / 世界模型基础设施。
- **官方链接：** [项目页](https://opencof.github.io/)、[论文](https://arxiv.org/abs/2607.08763)。
- **相对上期的新变化：** 全新纳入。
- **核心贡献：** 以 17,312 个视频训练视频模型通过连续帧推演棋局、几何、物理和操作过程，探索 visual/textual reasoning tokens。
- **Coding 接口：** 数据包含程序渲染、规则任务和图形引擎生成；但项目页当前显示模型和数据 Coming Soon。
- **互动/工业场景：** 可视化规划、教学推演、世界模型内部未来状态验证。
- **成熟度 / 证据：** 论文和代码入口存在，完整模型/数据未开放。
- **重要性 / 阅读：** 中高 / 可读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动：** 本周期主增量从脸部驱动转向全身动作和 3D 资产。ARDY 已能在运行时接收连续文本、角色路径和关节约束；GIRAF 则研究角色接近、操作、移动关节物体时的全身协同，但仍是论文原型。talking-head、流式 lip-sync、情感语音和可打断对话本周期暂无高质量新增。

**3D/4D 表示：** DreamCharacter-1 的 SDF latent、mesh、UV texture 路线说明工业角色仍需要拓扑、材质和动画兼容性，纯视频人物不能替代资产系统。ARDY 的 root＋body latent 表示则适合作为可持续驱动的动作层。

**实时交互：** ARDY 是目前证据最完整的新增：20/25 FPS 模型、RTX 4090 测试、TensorRT、浏览器 viewport、自动 replan 都已提供。它生成的是骨骼动作而非照片级人物画面，需要 Unreal、Unity、Omniverse、MuJoCo 或自研渲染器完成角色外观和物理。

**音视频与情感：** 本周期没有新原生音视频数字人基座。在线文本可以改变 ARDY 动作语义，但语音、口型、目光、情绪和 turn-taking 仍需独立模块。

**工程部署：** ARDY 可进入研究和原型管线；Aura 可用于离线五秒镜头；DreamCharacter-1 尚无开发者入口。真正实时数字员工仍需 ASR/TTS、对话 agent、动作/表情融合、WebRTC、审核和降级策略。

**安全与身份治理：** HumanForge 补充了人—物交互伪造评测，但没有解决肖像授权、水印或溯源。本周期未发现新的产品级身份治理闭环。

## 5. 世界模型进展

**架构与训练：** causal pretraining、few-step distillation、KV cache 和短 chunk AR 成为共同路线。LingBot-World 2.0、AlayaWorld、MIRA 分别强调无限/长时 rollout、空间记忆和多人 action attribution。

**可控/可玩生成：** 控制空间从相机键位扩大到多玩家 action streams、攻击/施法事件、动态 prompt 和 agent 规划。模型不再只是“根据图像继续生成”，而是接受结构化运行时事件。

**长时与物理一致性：** MIRA 报告五分钟分布质量稳定，LingBot 宣称无界交互，AlayaWorld 展示 60 秒以上片段；这些仍主要是作者评测。SAGA提供可插拔的漂移抑制，但无法保证隐藏状态、碰撞或游戏规则正确。

**空间表示：** AlayaWorld 使用显式 3D cache；Point as Skeleton 使用点云资产；这比单纯依赖视频 token 更容易接受代码控制、回访旧位置和编辑 actor 状态。

**机器人、游戏与仿真：** 游戏侧 MIRA、LingBot-World 最强；驾驶侧 Point as Skeleton、WCog-VLA；机器人侧 LingBot-VA 2.0。后两类价值不在生成漂亮视频，而在闭环策略训练、状态预测和可执行轨迹。

**实时部署与评测：** 不同 FPS 不可横向比较。MIRA 的 20 FPS 需要单张 B200；LingBot 的 60 FPS 对应未开放部署栈，而公开示例为 8 GPU、480p；AlayaWorld 的 24 FPS 没有开放权重；ARDY 的实时动作可以在 RTX 4090 运行，但输出复杂度远低于视频。

## 6. Coding × 新型互动作品

### 链路一：双 agent → 动作与事件流 → 持续生成世界 → 互动 RPG

LingBot-World 2.0 中，pilot agent 规划角色动作，director agent在运行中引入环境和剧情事件；代码运行时维护玩家输入、任务状态、权限、冷却时间和审核，世界模型生成视觉观测。可形成无需预制所有镜头的互动冒险或品牌世界。已有官方体验和离线权重，但完整 agent harness、服务端部署和规则一致性仍待开放。

### 链路二：在线文本/关节约束 → ARDY → 骨骼与物理跟踪 → 生成式 NPC

开发者可调用 `autoregressive_step()`，持续传入文本、历史动作和路径/关键帧 mask；ARDY 输出动作帧，传统引擎负责 mesh、碰撞、导航和网络同步。由此可实现玩家说“跛行到门边再回头”后即时表演的 NPC。代码和权重已经可运行，是本周期证据最完整的链路。

### 链路三：四人 action streams → MIRA 推理服务器 → 实时视频观测 → 多人可玩视频

游戏服务器同步四名玩家输入，模型预测下一段比赛画面；确定性服务仍需负责计分、回放、身份和反作弊。MIRA 已有 live demo，但视觉生成与权威游戏状态之间如何对齐尚未公开，因此更适合实验性玩法，而非竞技产品。

### 链路四：单图 → DreamCharacter-1 → rig/动作 runtime → UGC 虚拟角色

DreamCharacter-1 生成动画就绪 mesh 和纹理，ARDY或传统 motion graph 生成动作，Unity/Unreal 完成渲染和物理。这能把“角色建模—绑定—动作制作”压缩为半自动管线。DreamCharacter 尚无代码/API，因此组合方案属于本报告推断。

### 链路五：点云 skeleton＋nuPlan 状态 → 生成驾驶观测 → 闭环测试

代码维护车辆、地图、actor 和轨迹真值；Point as Skeleton 把这些状态渲染成相机观测；规划器根据观测继续动作，形成闭环。它比纯视频生成更适合自动驾驶，因为世界的可编辑状态仍掌握在模拟器中。

### 链路六：结构化人物/商品资产 → Aura → 审核与发布 → 品牌互动内容

LLM或脚本生成带 `[PERSON]`、`[OBJECT]`、`[SCENE]` 标签的镜头 JSON；Aura生成身份一致的短片；代码负责资产授权、批处理、质量检测和重试。现阶段适合离线广告和虚拟主播素材，不适合实时直播。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | LingBot-World 2.0、MIRA、AlayaWorld；action stream＋agent＋规则层 | 键鼠、多人动作、动态事件 | 可运行 demo / 研究权重 | MIRA：B200 20 FPS；LingBot 公开复现需 8 GPU；其余未披露 | 权威状态、物理、成本、商用许可 | [LingBot](https://github.com/Robbyant/lingbot-world-v2)、[MIRA](https://mira-wm.com/) |
| 影视/虚拟制作 | DreamCharacter＋ARDY＋Aura＋传统 DCC | 角色资产、导演脚本、路径和关键帧 | ARDY/Aura 可原型验证 | ARDY 测试 RTX 4090；其余未披露 | 多镜头一致、拓扑、版权、人工返修 | [ARDY](https://github.com/nv-tlabs/ardy)、[DreamCharacter](https://dreamcharacter-x.github.io/) |
| 直播与电商 | Aura、SparseCtrl-HOI、TTS/审核系统 | 人物/商品绑定、关键帧动作 | 离线素材生产 | 未披露 | 实时性、手物接触、商品真实性 | [Aura](https://github.com/Camellia997/Aura) |
| 品牌互动 | LingBot/AlayaWorld＋Web runtime | 相机、施法、召唤、文本事件 | 官方 demo | 未披露 | 并发、内容审核、品牌安全 | [AlayaWorld](https://alaya-lab.github.io/AlayaWorld/) |
| 教育培训 | ARDY＋角色渲染＋语音 agent | 指令即时转动作、路径演示 | 研究原型 | RTX 4090 已测试 | 语音/口型、动作正确性、课程审核 | [ARDY](https://research.nvidia.com/labs/sil/projects/ardy/) |
| 企业数字员工 | DreamCharacter/Aura＋ASR/TTS/RAG | 对话驱动预生成或动作层 | 组合推断 | 未披露 | 低延迟、turn-taking、身份授权 | [DreamCharacter](https://dreamcharacter-x.github.io/) |
| 陪伴与社交 | ARDY＋LLM memory＋avatar renderer | 长期记忆、在线动作和表情 | 概念/原型 | 未披露 | 情感稳定、安全、隐私 | [ARDY](https://github.com/nv-tlabs/ardy) |
| 空间计算 | AlayaWorld/LingBot＋6-DoF 控制 | 持续漫游和动态事件 | 官方 demo | 官方声称 24/60 FPS | 视角回环、眩晕、端侧算力 | [AlayaWorld](https://alaya-lab.github.io/AlayaWorld/) |
| 机器人/仿真 | LingBot-VA 2.0、Point as Skeleton | 观测—动作闭环、状态重 grounding | 研究部署/开源代码 | 未披露 | sim-to-real、物理可信、安全认证 | [LingBot-VA](https://arxiv.org/abs/2607.08639)、[PAS](https://github.com/krauwu/point-as-skeleton) |

## 8. 可复现资源与开发者入口

- [nv-tlabs/ardy](https://github.com/nv-tlabs/ardy)：本周最值得复现。Apache-2.0 代码，四组 20/25 FPS 权重；RTX 4090 测试环境。最小路径：运行 `scripts/run_demo.py`，先验证键盘转向和在线 prompt，再调用 `Ardy.autoregressive_step()`。
- [Robbyant/lingbot-world-v2](https://github.com/Robbyant/lingbot-world-v2)：14B causal-fast 权重已放，CC BY-NC-SA，仅限非商业。最小路径是用官方 `run_fast.sh` 生成 361 帧；示例需 8 GPU。实时部署代码不发布，不宜列入短期产品依赖。
- [MIRA](https://mira-wm.com/)：优先先测 live demo 的输入响应、多人同步和长时漂移，再评估本地复现；论文称数据和完整训练/推理代码已发布。
- [AlayaWorld GitHub](https://github.com/AlayaLab/AlayaWorld)：Apache-2.0，但当前只有 README/资产；推理代码、权重和训练数据均未放，不具备复现条件。
- [Aura](https://github.com/Camellia997/Aura)：Apache-2.0 最小推理包。最小路径是替换 JSON 中人物/物体/场景引用，生成约五秒镜头并做身份一致性检查。
- [Point as Skeleton](https://github.com/krauwu/point-as-skeleton)：提供训练配置、nuPlan 接口和多个 checkpoint；数据和点云资产准备复杂，适合自动驾驶团队。
- [DreamCharacter-1](https://dreamcharacter-x.github.io/)：暂无代码、权重、API 和许可证信息，只值得技术跟踪。
- [OpenCoF](https://opencof.github.io/)：代码链接已出现，但模型和 OpenCoF-17K 仍 Coming Soon；等待完整释放后再复现。
- [HumanForge](https://arxiv.org/abs/2607.08705)：代码与数据尚未发布，暂不能接入审核管线。

## 9. 系统架构与技术趋势判断

明显升温的是“显式状态＋生成渲染”：游戏使用多人 action streams，驾驶使用点云 skeleton，数字人使用 root trajectory、joint mask 和结构化角色标签。纯 prompt-to-video 在互动系统中的地位正在下降。

可复用架构逐渐清晰：LLM/agent 负责任务和剧情规划；确定性代码负责状态机、规则、物理、网络同步、安全和日志；世界模型生成未来视觉观测；数字人动作模型生成表演；传统引擎负责 mesh、碰撞、渲染和回退。工业系统不会由单一视频模型包办。

AR diffusion、KV cache、few-step distillation 和 variable history 正成为实时路线；显式 3D cache、点云和 root motion 则承担长期状态锚点。最大未解问题仍是视觉画面与可查询世界状态之间的双向一致性：模型能生成一扇门，不代表运行时能可靠读取门的位置、锁定状态和碰撞体。

需降级处理的表述包括：“无限 rollout”不等于无限状态正确；“开源框架”不等于代码/权重已发布；“实时 60 FPS”不等于公开仓库能复现；“animation-ready”也不等于已通过真实游戏或影视资产验收。本周期没有足够证据证明任何新增已规模化商用。

## 10. 论文精读候选

1. [Infinite Worlds with Versatile Interactions](https://arxiv.org/abs/2607.07534)：重点读 causal pretraining、蒸馏、agentic harness 和多人接口。与上一代差异是动作/事件空间及 agent 编排；风险是实时部署代码不开放。
2. [ARDY](https://arxiv.org/abs/2607.08741)：重点读 motion tokenizer、root/body 两阶段 denoiser、variable history 和 constraint mask。其增量是把高控制度和在线生成统一；复现风险主要是 Llama 3 文本编码权限及 TensorRT环境。
3. [Multiplayer Interactive World Models](https://arxiv.org/abs/2607.05352)：重点读 representation autoencoder、多人条件归因、物理评测和实时 serving。风险是 B200 成本以及视频状态与真实规则状态的偏差。
4. [DreamCharacter-1](https://arxiv.org/abs/2607.07817)：重点读几何偏好优化、Texture-MV、3D inpainting 和加速消融。区别在于从通用 3D 生成转向角色生产约束；风险是无代码、benchmark 由作者单方完成。
5. [Native Video-Action Pretraining](https://arxiv.org/abs/2607.08639)：重点读 visual-action tokenizer、因果预训练、异步推理和真实机器人评测。风险是硬件、数据和部署细节不足。

## 11. 下周跟踪与可行动建议

继续跟踪：

1. LingBot-World 2.0 是否会开放 1.3B、agent harness 或可嵌入服务端接口。
2. 公开的 LingBot 14B 权重能否在非 8 卡配置运行，实际首帧和动作延迟是多少。
3. MIRA 的代码、数据许可证及 B200 以外硬件上的帧率。
4. AlayaWorld 何时真正发布推理代码和 checkpoint。
5. ARDY 接入 Unity/Unreal 后的端到端动作延迟、足滑和碰撞表现。
6. DreamCharacter-1 是否开放 mesh 导出格式、rig、API或商用产品。
7. HumanForge 数据是否包含授权、肖像和生成来源元数据。
8. 本周期缺失的实时 talking-avatar、口型、情感语音方向是否出现新发布。

建议本周动手验证：

- **实验一：ARDY 生成式 NPC。** 目标：用键盘和在线 prompt 控制同一角色连续行走、转向、停下和表演。组件：ARDY、浏览器 demo、RTX 4090。难点：文本编码显存和 prompt 切换过渡。成功判据：连续运行两分钟、不阻塞播放，输入后一个 generation horizon 内出现正确动作。
- **实验二：LingBot 动作条件一致性。** 目标：对同一初始图重复三组 `action_path`。组件：14B causal-fast 权重、8 GPU 或可行的 FSDP 环境。难点：算力和非商用许可。成功判据：动作方向可重复辨识，361 帧内不发生明显场景重置。
- **实验三：Aura 角色—商品批处理。** 目标：自动替换 JSON 中一名角色、两件商品和三个场景。组件：Aura、资产库、简单审核脚本。难点：显存与商品外观漂移。成功判据：至少 80% 镜头保持人物和商品可识别，失败样本能被自动标记重试。
- **实验四：ARDY＋传统引擎边界验证。** 目标：明确模型、代码和物理引擎分工。组件：ARDY NPZ/G1 CSV、MuJoCo或游戏引擎。难点：骨架 retarget 和碰撞。成功判据：模型输出只负责动作意图，碰撞、导航和状态均由确定性 runtime 维护，并可在生成失败时回退到 motion graph。
