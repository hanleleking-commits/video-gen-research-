# 数字人、世界模型与互动作品研究日报：2026-10-09

检索窗口：2026-10-03 至 2026-10-09。  
信息截点：2026-10-09 09:15（Asia/Shanghai）。已与 9 月 28—30 日日报去重；窗口外工作仅在本周期发生实质更新时纳入。

## 1. 本日摘要

本周期最明显的进展，是数字人从“生成一段讲话视频”进一步转向可持续运行的流式组件：PixReenact 报告 239 ms 平均发射延迟，TalkLikeYou 将个人发音习惯变成可由预设 ID 或参考视频控制的参数，并开放了推理代码、权重和浏览器实时 demo。

联合音视频生成也逼近实时播放速率。Parallel Adapter Composition 在 MiniMax-H3 上通过并行训练、推理时相加的因果与少步 adapter，达到论文声称的约 26 fps；但需要六张 GPU、首帧仍需 4.2 秒，且 30 秒后声音身份明显漂移，距离实时对话产品仍有差距。

世界模型侧没有出现新的通用可玩世界产品，真正的增量来自运行时：CtrlCache 根据控制输入是否变化决定缓存复用，使三种交互世界模型的 DiT 主干加速 1.21—1.41 倍；SPW-Nav 则把自然语言方向指令变成可中途切换的一分钟 2K 全景流。

Ctrl-CWM 展示了一条更接近工业仿真的路线：模型学习人群动力学，代码以可插拔 cost function 在运行中加入避让或吸引目标，规划调用约 10 ms。这里代码持有目标和约束，模型提供动态先验。

LEDGER 与 InteractionBench 分别补齐了持续互动系统的两块短板：前者保存离屏对象的三维位置与历史，后者衡量系统是否能在正确时刻发言并在无事发生时保持沉默。现有系统在这两方面仍远未可靠。

UltraWorld 把世界模型引入超声扫描，通过程序化探头轨迹合成动作—视频训练对；DIGHT 则用仿真中的摩擦、漂浮、接触位置与作用力反向对齐双人动作生成器。二者说明工业价值越来越来自“生成—仿真—评分—再训练”闭环，而不只是画面质量。

本周期没有可独立验证的新客户试点或规模化部署。除 TalkLikeYou、Ctrl-CWM、LEDGER、InteractionBench 和 HiFD 外，多数项目仍只有论文或 demo；所有性能数字均为作者实测，尚缺第三方压力测试。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | 流式 reenactment、个性化发音动作、联合音视频生成同时推进 | 从离线动作约束推进到可运行实时 demo；通用 turn-taking 仍未解决 | 上期偏全身动作与接触，本期回到头部、声音和流式生成 |
| 世界模型 | 高 | 控制感知缓存、全景导航、人群运行时控制、三维对象记忆 | 开源规划与记忆组件增加；通用可玩世界仍是研究原型 | 从编辑与触觉条件，推进到运行时控制、缓存和持续记忆 |
| Coding × 互动作品 | 高 | 控制事件、cost function、动作轨迹、对象账本成为正式软件接口 | 可组合性明显增强，部分已有 CLI、Python、浏览器或 OpenAI-compatible 接口 | “代码持有状态”进一步扩展为“代码调度算力、验证发言时机和物理执行” |
| 实时运行时 | 高 | 239 ms 发射、26 fps 联合音视频、控制感知 1.21—1.41× 加速 | 吞吐进步明显，但首帧、GPU 数、尾延迟和多人并发证据不足 | 新增面向控制转折的缓存策略与完整实时交互评测 |
| 安全与治理 | 中 | HiFD 同时衡量去身份、表达/生理信号保留和画质 | 从单一人脸相似度推进到分层可回归评测 | 新增公开 benchmark、toolkit 和 CI leaderboard |

## 3. 最值得关注的 12 个进展

### 1. Parallel Adapter Composition：实时联合音视频生成

- **类型 / 日期 / 主线：**论文、demo；2026-10-07，10 月 8 日修订；数字人、实时基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2610.10343)、[项目页](https://pac-demo-2027.github.io/demo/)。
- **相对上期：**首次收录；比上期分离式数字人管线更接近原生音视频角色。
- **核心贡献：**分别训练因果 adapter 与少步 adapter，推理时执行 `W + Δd + Δc`，避免串行蒸馏互相破坏能力。
- **Coding 接口：**adapter 可离线融合；运行时按 chunk 管理 KV 历史、文本脚本、首帧条件和音视频输出。
- **关键边界：**作者报告 480×832、约 26 fps、四步采样、连续 30 秒；但需六张 GPU，首帧约 4.2 秒，尾段声音相似度下降 41%。
- **场景：**实时虚拟主播、互动角色、边生成边播放的剧情对话。
- **成熟度 / 证据：**可观看研究 demo；论文实测，代码仅承诺后续开放。
- **重要性 / 阅读：**高 / 必读。

### 2. CtrlCache：让控制事件决定世界模型何时复用计算

- **类型 / 日期 / 主线：**论文、推理加速；2026-10-06；世界模型、运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2610.08777)、[项目页](https://wrecklong.github.io/CtrlCache/)、[占位仓库](https://github.com/wrecklong/CtrlCache)。
- **相对上期：**首次收录；相较上期通用 KV 压缩，本项直接利用键盘或相机控制信号。
- **核心贡献：**将 chunk 分类为初始、转折、转向和稳定状态；动作变化时全量刷新，稳定期复用 transformer residual，并继承低频历史结构。
- **Coding 接口：**控制事件流直接驱动 cache policy，可插入 Matrix-Game 2.0、LingBot-World v1/v2 推理循环。
- **关键结果：**作者报告 DiT 主干加速 1.21—1.41 倍，三种模型的 WBench Overall 均略有提高。
- **场景：**低延迟可玩视频、云端世界模型 serving、按事件自适应分配算力。
- **成熟度 / 证据：**论文与对比视频；仓库尚无实现代码。
- **重要性 / 阅读：**高 / 必读。

### 3. PixReenact：直接用像素条件驱动流式头部替演

- **类型 / 日期 / 主线：**论文；2026-10-04；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.05233)。
- **相对上期：**首次收录；从语音驱动转向实时视频动作迁移。
- **核心贡献：**参考图和 driving frame 直接经 VAE 编码进入因果视频扩散，不依赖外部 landmark 或 3DMM；通过长自回归 rollout 和冷启动/稳态双教师蒸馏抑制漂移。
- **Coding 接口：**输入为持续到达的 driving frame，输出每次四帧；适合接摄像头、WebRTC 或动捕流。
- **关键结果：**4-NFE rolling student 的作者平均发射延迟为 239 ms。
- **场景：**直播替身、远程表演、虚拟主持、低带宽角色视频。
- **成熟度 / 证据：**论文研究原型；未核验到代码、GPU 配置或并发数据。
- **重要性 / 阅读：**高 / 必读。

### 4. TalkLikeYou：把“说话习惯”变成可编程数字人参数

- **类型 / 日期 / 主线：**论文、GitHub、权重、浏览器 demo；2026-10-05；数字人、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2610.06658)、[MIT 代码](https://github.com/BQ-Wang0511/TalkLikeYou)、[权重](https://huggingface.co/doubi-killer/TalkLikeYou)、[流式 demo](https://github.com/BQ-Wang0511/TalkLikeYou-Streaming-RealTime-Demo)。
- **相对上期：**首次收录；新增的不只是嘴形同步，而是同一句话的个体化发音动作。
- **核心贡献：**在 18 维嘴部运动空间中使用单步 flow matching，以 preset person ID 或参考视频指定说话习惯，并提出 PLAD 指标。
- **Coding 接口：**提供 CLI、采样步数、motion scale、guidance scale、习惯 ID 和参考视频参数；浏览器 demo 支持麦克风、配音与对话模式。
- **性能：**仓库声称 RTX 3090 上超过 30 fps、生成部分显存低于 4 GB；完整环境建议至少 12 GB 显存。
- **场景：**个性化数字员工、角色配音、粉丝虚拟形象、语言教学。
- **成熟度 / 证据：**已开源可运行；性能为作者自测，权重另受第三方条款约束。
- **重要性 / 阅读：**高 / 必读。

### 5. InteractionBench：实时系统不仅要答对，还要知道何时沉默

- **类型 / 日期 / 主线：**benchmark、dataset、GitHub；2026-10-05；Coding × 互动、评测。
- **官方链接：**[论文](https://arxiv.org/abs/2610.05775)、[代码](https://github.com/Espere-1119-Song/InteractionBench)、[数据](https://huggingface.co/datasets/InteractionBench/InteractionBench)。
- **相对上期：**首次收录；补齐此前实时 benchmark 只测吞吐、不测打断和误触发的问题。
- **核心贡献：**1,060 次交互、812 个视频，分别评估内容准确率、视频时钟上的响应时机和 silence compliance。
- **Coding 接口：**`ibench run/eval`、本地 Hugging Face adapter、OpenAI-compatible API adapter、九类流式系统和 tool-using agent runner。
- **关键结果：**最佳开放方案仍无法通过三分之一的近似干扰套件；原生实时系统在 89.9% 的负样本流上错误发言。
- **场景：**数字人助手、直播讲解、看护系统、实时视频 agent 的发布门禁。
- **成熟度 / 证据：**完整评测栈已开放；约 54 GB 视频，许可证未明确披露。
- **重要性 / 阅读：**高 / 必读。

### 6. SPW-Nav：语言可控的一分钟 2K 全景生成世界

- **类型 / 日期 / 主线：**论文、项目 demo、dataset 方法；2026-10-06；世界模型、空间计算。
- **官方链接：**[论文](https://arxiv.org/abs/2610.08941)。
- **相对上期：**首次收录；从透视相机控制扩展到 360° 全景和自然语言导航。
- **核心贡献：**球面旋转解耦、pose-aligned translation condition 和多项记忆支持一边生成一边切换移动指令；作者声称可从单张全景图实时流式生成一分钟 2K 360° 视频。
- **Coding 接口：**上层 agent 输出自然语言移动指令，运行时将其转换为球面旋转和平移条件。
- **场景：**VR 导览、沉浸式网页、远程空间探索、具身训练数据。
- **成熟度 / 证据：**论文与项目 demo；代码、权重、GPU 数和端到端延迟未披露。
- **重要性 / 阅读：**高 / 必读。

### 7. Ctrl-CWM：用可插拔目标函数控制正在运行的人群

- **类型 / 日期 / 主线：**论文、MIT GitHub；2026-10-07；世界模型、工业仿真。
- **官方链接：**[论文](https://arxiv.org/abs/2610.09438)、[代码](https://github.com/jungyu0413/Ctrl-CWM)、[交互项目页](https://jungyu0413.github.io/Ctrl-CWM/)。
- **相对上期：**首次收录。
- **核心贡献：**encoder 学习真实人群动力学，actor 提议位移，critic 评价想象轨迹，CEM planner 在运行时叠加用户定义的避让或吸引成本，无需重新训练。
- **Coding 接口：**公开 `CtrlCWMSimulator`、`CEMPlanner`、`UserCost`、训练和评测脚本；命令行可传避让区域或吸引点。
- **关键结果：**作者报告规划调用约 10 ms，多区域避让 compliance 为 0.950，对照 CrowdES 为 0.579。
- **场景：**游戏群众、机器人导航测试、车站疏散、城市规划和数字孪生。
- **成熟度 / 证据：**代码已开放，预训练权重尚待发布；论文仿真实测。
- **重要性 / 阅读：**高 / 必读。

### 8. LEDGER：为持续世界建立离屏对象的三维账本

- **类型 / 日期 / 主线：**论文、MIT GitHub；2026-10-07；世界模型记忆、空间 agent。
- **官方链接：**[论文](https://arxiv.org/abs/2610.10538)、[代码](https://github.com/LEDGER-3D/LEDGER)、[项目页](https://ledger-3d.github.io/)。
- **相对上期：**首次收录；回应上期 Beyond3D 暴露的离屏对象记忆不足。
- **核心贡献：**关联重复观察，保存对象三维位置、静置位置、移动历史、容器内容和上下文描述；对象离开视野后仍保留。
- **Coding 接口：**内含 memory builder、检索器、Qwen3.5-9B reader、HTTP server 和三类 benchmark 示例。
- **关键结果：**作者报告 HD-EPIC 由 29.7% 提升至 42.6%，Ego4D 返回对象中位定位误差为 0.99 m。
- **场景：**AR 助手、仓储盘点、机器人长期任务、持续游戏世界。
- **成熟度 / 证据：**最小工作示例已开放；回答侧需约 24 GB GPU，完整构建依赖多种视觉模型。
- **重要性 / 阅读：**高 / 必读。

### 9. UltraWorld：从无位姿临床视频学习超声世界模型

- **类型 / 日期 / 主线：**论文、demo；2026-10-07；世界模型、机器人/医疗仿真。
- **官方链接：**[论文](https://arxiv.org/abs/2610.09785)、[项目页](https://ultraworld-project.github.io/)。
- **相对上期：**首次收录。
- **核心贡献：**先用程序化探头轨迹和三维解剖 mask 合成动作—视频对，再自蒸馏为推理时只需当前观察和动作的世界模型；AsMap 显式编码采样位置、声束方向和深度。
- **Coding 接口：**探头位姿、成像深度和轨迹成为结构化控制；CEM 对候选未来进行 MPC 规划。
- **关键结果：**九次模拟闭环任务中，作者报告最终距离和方向误差较视觉伺服分别降低 29% 和 38%。
- **场景：**机器人超声、医疗培训、稀缺动作数据合成。
- **成熟度 / 证据：**研究 demo；代码明确为评审后开放，且尚无真实机器人闭环。
- **重要性 / 阅读：**高 / 必读。

### 10. DIGHT：让数字双人互动通过仿真反馈变成机器人技能

- **类型 / 日期 / 主线：**论文；2026-10-07；数字人动作、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.10322)。
- **相对上期：**首次收录；延伸上期 HOI-Retarget 的“接触保持”路线。
- **核心贡献：**在仿真中执行多个文本条件双人互动候选，以跟踪误差、摩擦、漂浮、接触位置、持续时间和作用力构造偏好，再用分解式 diffusion DPO 对齐生成器。
- **Coding 接口：**`生成候选 → 仿真 rollout → 多维评分 → DPO → tracker 微调`。
- **场景：**双人表演、格斗 NPC、协作机器人和安全动作数据生成。
- **成熟度 / 证据：**论文仿真原型；未见代码、实机结果或实时性数据。
- **重要性 / 阅读：**中高 / 必读。

### 11. Counterfactual Route Optimization：动态调整 Gaussian 头部 Avatar 的训练目标

- **类型 / 日期 / 主线：**论文；2026-10-07；3D 数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.09791)。
- **相对上期：**首次收录。
- **核心贡献：**从同一训练状态分别短程尝试几何、外观和联合更新，根据实际收益选择路线，再训练轻量 controller 替代固定 loss 权重。
- **Coding 接口：**controller 可嵌入 Gaussian avatar 优化循环，动态调节几何和外观目标。
- **场景：**高保真数字替身、虚拟制作资产和远程化身。
- **成熟度 / 证据：**NeRSemble 上的论文实验；无代码、帧率或资产导出证据。
- **重要性 / 阅读：**中 / 可读。

### 12. HiFD / UtilFace：数字人去身份能力的分层评测

- **类型 / 日期 / 主线：**benchmark、dataset、toolkit、leaderboard；2026-10-07；安全与治理。
- **官方链接：**[论文](https://arxiv.org/abs/2610.10334)、[项目与 leaderboard](https://cv-ac.github.io/hifd/)。
- **相对上期：**首次收录。
- **核心贡献：**同时衡量身份抑制、宏观表情、微观动作、生理信号和画质；UtilFace 含 99,928 张图、2,069 个身份。
- **Coding 接口：**提交 JSON 后由 CI 验证 schema 和调和平均一致性，再自动更新 leaderboard。
- **关键结果：**作者报告 12 种方法中最佳 HiFD 仅 0.607，说明“看不出是谁”不等于隐私与下游效用同时合格。
- **场景：**数字人素材匿名化、医疗/客服录像、肖像治理和上线前隐私回归。
- **成熟度 / 证据：**研究工具和榜单已开放；资源限定研究使用。
- **重要性 / 阅读：**中高 / 必读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**PixReenact 用实时视频驱动，TalkLikeYou 用音频加习惯 ID 或参考视频驱动，PAC 则直接从文本联合生成语音和人物视频。三者分别覆盖动作迁移、个性化表演和原生音视频。
- **3D/4D 表示：**本周期只有 Counterfactual Route Optimization 的 Gaussian head avatar 训练策略较突出，没有新的单图全身 4D avatar 或商用资产管线。
- **实时交互：**TalkLikeYou 已有浏览器和麦克风入口；PixReenact 报告 239 ms 发射延迟。PAC 虽达到播放帧率，但 4.2 秒首帧和六 GPU 成本不适合直接称为低延迟对话。
- **音视频与情感表达：**个性化发音动作是明确增量；但情绪、视线、全身手势、倾听状态和用户打断仍没有在一个开放系统中统一。
- **工程部署：**TalkLikeYou 是本周期最接近开发者可用组件的项目。其他模型普遍缺服务 API、batching、并发、故障恢复和 SLA。
- **安全与身份治理：**HiFD 可用于验证匿名化输出是否同时保留表情与生理信号；但它不解决肖像授权、撤回、水印和生成内容溯源。

结论：数字人生成已具备单模块实时性，但“感知用户—决定何时回应—生成语音和动作—处理打断—保持身份”的端到端实时角色仍未成熟。

## 5. 世界模型进展

- **架构与训练：**世界模型开始更积极地使用现成视频先验，再通过程序化动作条件自蒸馏到垂直领域，UltraWorld 是代表。
- **可控 / 可玩：**SPW-Nav 使用语言导航；Ctrl-CWM 使用 runtime cost；CtrlCache 使用控制事件决定算力。这些控制均可被代码记录、回放和测试。
- **长时与物理一致性：**LEDGER 将离屏对象外置到显式三维记忆；它没有解决生成视频本身的物理漂移，但为持续 agent 提供了更可靠的权威状态。
- **空间表示：**球面全景、对象三维账本、超声采样场和人群轨迹表明，“空间”正从视频像素扩展为任务相关的结构化表示。
- **机器人 / 游戏 / 仿真：**UltraWorld、DIGHT 和 Ctrl-CWM 均采用模型想象、代码评分、仿真执行的闭环。游戏侧本周期没有超过上期 GameDirector、CoDeR 的新完整系统。
- **实时部署：**CtrlCache 优化单模型推理成本，Ctrl-CWM 的 planner 达到毫秒级；但前者代码未开放，后者只生成轨迹而非高保真画面。
- **评测：**InteractionBench 将时机和沉默纳入系统指标；LEDGER 验证离屏记忆；世界模型发布门禁正在从画质指标转向在线行为正确性。

## 6. Coding × 新型互动作品

### 链路一：可插入业务逻辑的实时数字人

**麦克风/对话 agent → TTS 或音频流 → TalkLikeYou 习惯参数 → 流式视频 → WebRTC**

代码负责 turn-taking、工具调用、授权和习惯 ID；模型负责嘴形、头部动作和外观；RTC 负责播放与打断。浏览器 demo 已可运行，但真正的双工对话与打断延迟尚未验证。

### 链路二：按输入变化自适应分配算力的生成世界

**键盘/相机事件 → 控制状态分类 → CtrlCache 刷新或复用 → 视频世界模型 → 播放缓冲**

代码知道动作何时转折，因此能在转折处保证计算、稳定移动时节省计算。论文有三个模型的对照结果；实现尚未开放。

### 链路三：自然语言控制的沉浸式全景导览

**用户说“向左进入走廊” → agent 解析导航意图 → SPW-Nav 球面运动条件 → 2K 360° 视频 → WebXR 播放器**

代码负责导航状态、热点、权限和剧情；模型生成未预制的观察；WebXR 管理头显姿态和交互。全景生成有 demo，完整 WebXR 产品链路属于**本报告推断**。

### 链路四：运行时可改变目标的人群数字孪生

**场景事件 → `UserCost` → CEM imagined rollout → 轨迹选择 → Unity/Unreal 人群角色**

世界模型提供人类运动先验，代码定义禁区、出口或吸引点，引擎执行寻路、碰撞和渲染。Ctrl-CWM 的规划代码已开放；引擎插件与大规模并发尚未证明。

### 链路五：拥有对象永久记忆的空间助手

**摄像头视频 → LEDGER 检测/三维定位 → 对象历史账本 → LLM 检索 → 数字人或 AR 回答**

代码账本保存位置和移动历史，VLM/LLM 负责感知描述与问答，数字人负责呈现。可形成“杯子最后放在哪里”的家庭、仓储或维修助手；已有最小工作示例。

### 链路六：仿真反馈驱动的双人动作创作

**剧情 agent 生成互动意图 → 动作模型生成候选 → 物理引擎 rollout → 接触/摩擦评分 → 选择或再训练 → 角色/机器人执行**

生成模型负责动作多样性，代码和物理引擎负责可执行性及安全约束。DIGHT 已证明仿真训练闭环，实时游戏和实机机器人结果仍属后续工作。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 技术栈与互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | CtrlCache、Ctrl-CWM | 控制事件、cache policy、cost function、引擎渲染 | 开源组件/论文原型 | 1.21—1.41×；planner 约 10 ms | 高保真渲染、多人状态与尾延迟 | [Ctrl-CWM](https://github.com/jungyu0413/Ctrl-CWM) |
| 影视/虚拟制作 | PixReenact、Gaussian head | driving video、因果扩散、Gaussian asset | 研究原型 | 239 ms；其余未披露 | 4K、色彩管理、可编辑资产导出 | [PixReenact](https://arxiv.org/abs/2610.05233) |
| 直播与电商 | TalkLikeYou、PAC | RTC、TTS/联合音视频、商品状态与审核 | 可运行 demo | 30+ fps/3090；PAC 六 GPU | 打断、并发、口播事实性和肖像授权 | [TalkLikeYou](https://github.com/BQ-Wang0511/TalkLikeYou) |
| 品牌互动 | SPW-Nav | agent、360° 生成、WebXR、活动状态机 | demo | 论文称 2K 实时；硬件未披露 | 品牌资产一致性和内容审核 | [SPW-Nav](https://arxiv.org/abs/2610.08941) |
| 教育培训 | DIGHT、InteractionBench | 动作生成、仿真评分、实时讲解控制器 | 研究原型/benchmark | 未披露 | 教学正确性、误触发和安全 | [InteractionBench](https://github.com/Espere-1119-Song/InteractionBench) |
| 企业数字员工 | TalkLikeYou、LEDGER | 工具 agent、长期记忆、语音与数字人 | 组件可运行 | 回答侧约 24 GB GPU | 双工对话、权限、审计和 SLA | [LEDGER](https://github.com/LEDGER-3D/LEDGER) |
| 陪伴与社交 | PAC、TalkLikeYou | 人设记忆、音视频生成、RTC | demo | 首帧 4.2 秒至实时吞吐 | 声音漂移、误发言和安全治理 | [PAC](https://pac-demo-2027.github.io/demo/) |
| 空间计算 | SPW-Nav、LEDGER | 360° 视频、3D object memory、WebXR | 研究 demo | 未披露 | 坐标漂移、端侧成本和隐私 | [LEDGER 项目页](https://ledger-3d.github.io/) |
| 机器人/仿真 | UltraWorld、DIGHT、Ctrl-CWM | MPC/CEM、物理引擎、动作条件世界模型 | 模拟闭环 | Ctrl-CWM 约 10 ms；其余未披露 | sim-to-real、故障边界与认证 | [UltraWorld](https://ultraworld-project.github.io/) |

## 8. 可复现资源与开发者入口

| 资源 | 开放状态 / 许可证 | 要求 | 最小验证路径 | 建议 |
|---|---|---|---|---|
| [TalkLikeYou](https://github.com/BQ-Wang0511/TalkLikeYou) | 代码 MIT；权重另有第三方条款 | Linux、CUDA 12.1；建议 ≥12 GB GPU | 单图 + WAV + preset ID，运行单步推理 | 本期数字人首选复现 |
| [TalkLikeYou 实时 demo](https://github.com/BQ-Wang0511/TalkLikeYou-Streaming-RealTime-Demo) | 浏览器前端与流式服务代码 | 作者基准 RTX 3090 | 麦克风输入，记录首帧、持续 fps 和打断恢复 | 值得做产品接口验证 |
| [Ctrl-CWM](https://github.com/jungyu0413/Ctrl-CWM) | MIT，训练和生成代码齐全；权重待发布 | Python ≥3.9、PyTorch ≥2.0、CrowdES 数据 | 先训练/加载单场景模型，再运行一个 avoidance cost | 本期世界模型首选复现 |
| [InteractionBench](https://github.com/Espere-1119-Song/InteractionBench) | 代码和约 54 GB 数据开放；许可证未明确 | Python ≥3.10、FFmpeg | 用 OpenAI-compatible adapter 跑负样本流 | 适合作为上线前 CI |
| [LEDGER](https://github.com/LEDGER-3D/LEDGER) | MIT，含成品 memory 示例 | 回答侧 24 GB GPU、约 19 GB Qwen 权重 | 先跳过建库，复现 examples 问答 | 适合验证显式长期记忆 |
| [HiFD / UtilFace](https://cv-ac.github.io/hifd/) | 研究使用，toolkit 和 CI 榜单开放 | 视评测模型而定 | 对现有匿名化输出生成 JSON 并跑分项指标 | 安全团队值得复现 |
| [CtrlCache](https://github.com/wrecklong/CtrlCache) | 仅占位仓库 | 未披露 | 暂只能复核论文伪代码和视频 | 等代码后再安排 GPU |
| PAC、PixReenact、SPW-Nav、UltraWorld、DIGHT | 论文/demo；未见可运行代码 | 未披露或高端多 GPU | 只能阅读方法、复核 demo | 暂不适合完整复现 |

## 9. 系统架构与技术趋势判断

明显升温的是“流式模型之外的运行时”。缓存调度、对象账本、响应控制器、cost function、仿真评分器都在决定系统是否真正可交互。

主流架构正在收敛为五层：

1. **输入与事件层：**语音、摄像头、键盘、传感器和自然语言指令；
2. **权威状态层：**对象账本、角色状态、权限、剧情、空间坐标；
3. **规划与规则层：**agent、状态机、CEM/MPC、cost function；
4. **生成表现层：**数字人、视频世界模型、动作生成和神经渲染；
5. **验证与运行层：**物理引擎、InteractionBench、安全评测、缓存、RTC 与可观测性。

数字人路线正在从统一平均嘴形转向个体化表演；世界模型路线则从“一个大模型记住一切”转向显式状态和外部记忆。生成模型负责逼真的感知与表现，代码负责持续世界的事实、规则、资源和责任边界。

仍未解决的问题包括：稳定低首帧延迟、p95/p99 chunk 延迟、多会话 batching、长时声音与身份保持、真正双工的打断恢复、跨模态审核、资产授权、世界状态与生成画面的冲突处理，以及从模拟闭环到真实设备的安全保证。

需要降级看待的宣传包括：只报告平均 fps 而不报告首帧和 GPU 数；把“轨迹实时”称为“高保真世界实时”；把项目页视频当作开放 demo；以及把仿真中的九次规划实验描述成医疗或机器人部署。

## 10. 论文精读候选

1. [Real-Time Joint Audio-Video Generation](https://arxiv.org/abs/2610.10343)：重点读 adapter 更新方向、并行组合条件、长时声音漂移和成本表。它提供了比串行多阶段蒸馏更模块化的路线；风险是单一 MiniMax-H3、六 GPU 且未开源。
2. [CtrlCache](https://arxiv.org/abs/2610.08777)：重点读动作状态分类、refresh policy、frequency-mixed history prior 和逐模型消融。它最适合迁移到现有世界模型 runtime；风险是主干加速不等于端到端加速。
3. [PixReenact](https://arxiv.org/abs/2610.05233)：重点读交叉身份伪监督、双教师蒸馏和长 rollout benchmark。与 landmark/3DMM 路线的差异明确；风险是缺代码和硬件披露。
4. [InteractionBench](https://arxiv.org/abs/2610.05775)：重点读视频时钟评分、negative streams、near-miss suites 和 controller 分析。它可直接转化为数字人系统 CI；风险是外部 API judge 和数据许可需进一步核验。
5. [UltraWorld](https://arxiv.org/abs/2610.09785)：重点读程序化动作—视频合成、AsMap、self-distillation 和模拟闭环实验。它展示世界模型进入专业设备控制的完整因果链；风险是真实临床数据没有成对未来真值，代码尚未开放。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. PAC 是否开放 adapter、推理代码，以及单 H200、量化和多人 batch 结果。
2. PixReenact 的 GPU 配置、首帧时间、长时身份曲线及公开实现。
3. TalkLikeYou 的端到端音频缓冲延迟、打断恢复和权重商业许可。
4. CtrlCache 代码何时开放，端到端 VAE、传输和渲染加速是否仍成立。
5. SPW-Nav 的真实硬件、帧率定义、SPW-NavSet 和权重开放计划。
6. Ctrl-CWM 预训练权重、大规模人群复杂度和 Unity/Unreal 接入。
7. LEDGER 在多房间、多用户和数小时视频中的构建成本及错误纠正机制。
8. UltraWorld、DIGHT 是否提供真实机器人结果和安全失败分析。

### 本周可动手实验

1. **个性化数字人双工原型**
   - 目标：验证说话习惯控制是否能在真实对话中保持。
   - 组件：TalkLikeYou 实时 demo、流式 ASR/LLM/TTS、WebRTC。
   - 难点：音频分块、首帧、用户打断和重新同步。
   - 成功判据：连续 20 轮对话中无明显音画累积漂移，打断后在团队设定阈值内恢复。

2. **控制感知缓存模拟器**
   - 目标：在现有世界模型外先验证 CtrlCache 的调度思想。
   - 组件：动作事件日志、Matrix-Game/LingBot 推理脚本、缓存命中与画质记录器。
   - 难点：区分转折动作与连续动作，避免错误复用。
   - 成功判据：固定控制序列下端到端耗时下降，转折帧的控制服从率不低于原始推理。

3. **实时数字人“沉默能力”CI**
   - 目标：检测系统是否对无关动作、相似事件和环境噪声误响应。
   - 组件：InteractionBench、现有数字人 agent、OpenAI-compatible adapter。
   - 难点：对齐视频时钟、区分 agent 决策和 avatar 播放延迟。
   - 成功判据：分别报告内容、时机、沉默和端到端播放延迟，并为负样本设发布门槛。

4. **带显式对象账本的互动空间**
   - 目标：比较视频上下文记忆与 LEDGER 式外部状态。
   - 组件：Unity/Godot 小场景、对象事件日志、LEDGER/Qwen reader、简单 NPC。
   - 难点：对象 ID 合并、移动误报和代码状态与视觉观察冲突。
   - 成功判据：在遮挡、跨房间和再次返回三类测试中，对象位置及容器内容问答显著优于纯视频上下文基线。
