# 数字人、世界模型与互动作品研究日报：2026-10-26

检索窗口：2026-10-20 至 2026-10-26。  
信息截点：2026-10-26 09:00（Asia/Shanghai）。已与 10 月 20—25 日日报去重。由于截点对应北美周日晚间，今日尚无新的 arXiv 工作日发布批次；本期排序反映滚动七日的重要性，而不是用旧闻填充“今日新增”。

## 1. 本日摘要

本周期最强信号并非单一模型画质跃升，而是“显式状态、生成视觉、代码运行时”开始形成清晰分层：WorldStream 用可版本化 world record 保存世界事实，AnyGameRenderer 让 coding agent 只构建可玩的灰盒，再由视频模型负责视觉表现。

世界模型的控制接口也从单主体按键扩展至多主体异构动作。LiveAct 将按钮、车辆控制量和机器人关节指令统一为“主体描述＋控制 schema＋数值流”，但其 17.70 FPS 仍不等于已经证明低动作延迟或生产级并发。

SolarWM-H3 Realtime、CADRE 和 Direct Forcing 分别从部署框架、视频 autoencoder 和训练 recipe 压缩生成链路；只有 SolarWM 与 CADRE 提供了较完整的代码、模型或 demo 入口。

数字人侧没有新的 talking-head 基础模型突破。更有价值的增量转向角色“身体”：RealMe降低2K体积人物的捕获门槛，RLMuscle开放实时肌肉形变，PiHand让手—物交互进入物理策略，bitHuman则修复了对话数字人的空闲闭嘴与开口过渡。

Coding × 互动作品的关键变化是可测试性上升：GenXR-Bench在真实XR运行时测试 coding agent，PhysTrace定位视频中的物理违规区间，AnyGameRenderer提供实体级一致性指标。这些组件让生成系统开始具备软件工程意义上的测试、诊断与回归门禁。

产业证据仍弱于研究证据。窗口内没有新的规模化客户部署、P95端到端延迟、并发、SLA或单位会话成本披露；“实时论文结果”“可运行开源 demo”和“正式产品”仍须严格区分。

身份治理继续落后于能力发展：本周期没有高质量新增覆盖可撤回身份授权、跨供应商授权凭证、输出水印和肖像滥用追踪闭环。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 10 月 25 日日报 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | 2K体积人物、实时肌肉、物理手物交互形成身体级组件链 | RLMuscle已开源；bitHuman为正式SDK；RealMe、PiHand仍属研究阶段 | 今日无实质新增 |
| 世界模型 | 高 | 多主体动作、持久状态、专项codec、实时部署同时推进 | 出现开源运行时、权重和可执行评测，但生产指标仍缺失 | 今日无实质新增 |
| Coding × 互动作品 | 高 | 灰盒游戏生成渲染、XR运行时测试、物理CI门禁 | 从“生成代码/内容”推进到“持续运行、验证和修复” | 今日无实质新增 |
| 工业应用 | 中低 | 正式数字人SDK与离线头像产品可用 | 尚无新增规模化部署证明 | 相对上期无成熟度跃迁 |
| 安全与治理 | 低 | 无高质量新增 | 未改善 | 风险继续随单图角色和自动内容管线扩大 |

## 3. 最值得关注的 10 个进展

### 1. WorldStream：以持久状态和确定性代码生成开放3D世界

- **类型 / 日期 / 主线：**论文、系统原型；2026-10-20；世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2610.28827)。
- **相对上期的新变化：**今日无更新；作为滚动窗口内最完整的“模型规划＋程序世界”架构保留。
- **核心贡献：**Macro/Micro planner输出结构化决策，确定性代码编译地形、道路、植被和实体；world record持久保存seed、对象身份、边界约束、编辑和NPC状态，回访时重新编译。
- **Coding接口：**typed decision state、tile compiler、属性编辑及可版本化世界记录。
- **场景：**开放世界、长期品牌空间、NPC社区、数字孪生导览。
- **成熟度 / 证据：**研究原型；论文与作者演示。未公开代码、帧率和多人测试。
- **重要性 / 阅读：**高 / 必读。

### 2. LiveAct：实时多主体、异构动作世界模型

- **类型 / 日期 / 主线：**论文、demo、dataset；2026-10-21；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2610.29408)、[项目页](https://liveact-ai.github.io/)。
- **相对上期的新变化：**今日无更新。
- **核心贡献：**以语言定义每个主体和控制槽，再用主体感知空间调制及逐主体注意力，将不同动作流绑定到正确实体。训练覆盖四款游戏与两个多机器人数据源，共119.54小时。
- **Coding接口：**`主体描述＋动作schema＋逐帧数值流`，可由游戏服务器或机器人中间件生成。
- **场景：**多人生成式游戏、协作机器人仿真、多角色表演。
- **成熟度 / 证据：**官方可运行demo；论文实测。作者报告512×448下17.70 FPS，但未披露GPU、P95动作延迟和代码。
- **重要性 / 阅读：**高 / 必读。

### 3. SolarWM-H3 Realtime：开放的实时相机控制世界模型运行时

- **类型 / 日期 / 主线：**模型、GitHub、部署框架；2026-10-22；世界模型、Coding。
- **官方链接：**[GitHub](https://github.com/Junchao-cs/SolarWM)。
- **相对上期的新变化：**今日未见新的release或权重。
- **核心贡献：**以统一训练和推理框架覆盖5B—33B视频模型，通过因果适配、分布匹配蒸馏、加速DiT与流式VAE构建持续生成器。
- **Coding接口：**Python CLI、配置系统、训练/推理模块、网页运行时及生成过程中的相机输入。
- **场景：**可玩视频、生成式漫游、虚拟摄影、机器人视觉仿真。
- **成熟度 / 证据：**已开源、可运行demo；论文实测。代码为Apache-2.0，衍生权重许可证需分别核查。
- **重要性 / 阅读：**高 / 必读。

### 4. AnyGameRenderer：coding agent做玩法灰盒，生成模型做最终视觉

- **类型 / 日期 / 主线：**论文、GitHub、LoRA、dataset、benchmark；2026-10-22；Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2610.31466)、[代码](https://github.com/AlayaLab/AnyGameRenderer)。
- **相对上期的新变化：**今日无更新；仍是本周期 coding 与生成媒体结合最直接的开源实例。
- **核心贡献：**将灰盒、线框或语义视频重绘为游戏画面，同时尝试保留实体位置、数量和出现时机。作者报告实体时间匹配由50.9%升至62.9%，数量变化漏检由34.7%降至21.3%。
- **Coding接口：**`infer.py`、JSONL队列、颜色—实体caption协议，以及供coding agent使用的`SKILL.md`。
- **场景：**生成式游戏皮肤、品牌小游戏、互动电影预演。
- **成熟度 / 证据：**已开源；论文实测。约123 GB基础权重、至少80 GB显存，H100生成124帧约17分钟，明确不是实时运行时。
- **重要性 / 阅读：**高 / 必读。

### 5. CADRE：为互动延迟重新设计视频autoencoder

- **类型 / 日期 / 主线：**论文、模型、live demo；2026-10-21/22；世界模型基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2610.30129)、[Kyutai发布页](https://kyutai.org/blog/2026-10-22-cadre/)。
- **相对上期的新变化：**今日无更新。
- **核心贡献：**研究不同空间和时间压缩率如何影响动作等待时间。结果表明：固定相机操作更适合时间压缩，移动相机导航更依赖空间分辨率；粗空间token可能先破坏运动而非静态画质。
- **Coding接口：**可替换的因果codec层和一至数步扩散decoder。
- **场景：**机器人遥操作、低延迟世界模型服务、可玩视频。
- **成熟度 / 证据：**模型与demo开放；论文实测。
- **重要性 / 阅读：**高 / 必读。

### 6. GenXR-Bench：在真实空间运行时验收coding agent

- **类型 / 日期 / 主线：**论文、benchmark、GitHub；2026-10-21，10月22日修订；Coding × XR。
- **官方链接：**[论文](https://arxiv.org/abs/2610.29014)、[仓库](https://github.com/whuang37/genxr-bench)。
- **相对上期的新变化：**今日无新版本。
- **核心贡献：**112项任务涵盖功能实现、bug修复和完整XR应用生成，同时使用交互脚本、程序断言和视觉断言；测试器与专家判断的一致率为96.5%，最强模型总体Pass@1仅50.9%。
- **Coding接口：**XR Blocks SDK、Harbor、Docker和自动verifier。
- **场景：**空间培训、XR导览、互动原型CI。
- **成熟度 / 证据：**benchmark已开源；论文实测。
- **重要性 / 阅读：**高 / 必读。

### 7. PhysTrace：把物理错误变成可定位的CI门禁

- **类型 / 日期 / 主线：**论文、benchmark、GitHub；2026-10-22；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2610.31429)、[MIT代码](https://github.com/PhysTrace/PhysTrace)。
- **相对上期的新变化：**今日无更新。
- **核心贡献：**对solidity、permanence、immutability和continuity等违规给出时间区间及解释，而非只返回整体物理分数；IntPhys2-Timing最高Hit Rate为70.7%。
- **Coding接口：**批处理、可恢复任务、评分脚本和Gradio检查页，可接nightly evaluation。
- **场景：**世界模型回归测试、镜头质检、机器人预测筛选。
- **成熟度 / 证据：**已开源；论文实测。默认依赖大型视觉模型和A100级环境。
- **重要性 / 阅读：**高 / 必读。

### 8. RealMe：从稀疏低清拍摄生成2K体积人物

- **类型 / 日期 / 主线：**论文、demo；2026-10-21；数字人、4D表示。
- **官方链接：**[论文](https://arxiv.org/abs/2610.30023)、[项目页](https://zju3dv.github.io/realme/)、[仓库](https://github.com/zju3dv/RealMe)。
- **相对上期的新变化：**代码仍处于内部审核，未形成新的可复现增量。
- **核心贡献：**从720p稀疏多视角捕获构建辅助4D表示，再用高分辨率视频扩散生成2K、多视角和时间一致的增强素材。
- **Coding接口：**预期包含捕获、辅助视图生成、扩散增强和4D优化管线，目前尚不可完整运行。
- **场景：**体积人像、自由视角表演、影视数字替身。
- **成熟度 / 证据：**论文和demo；作者实测，代码未发布。
- **重要性 / 阅读：**高 / 必读。

### 9. RLMuscle：开放的实时肌肉驱动角色形变

- **类型 / 日期 / 主线：**论文、GitHub、dataset、物理引擎；2026-10-21；数字人运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2610.30033)、[代码](https://github.com/chunleili/rlmuscleV2)。
- **相对上期的新变化：**今日无更新。
- **核心贡献：**RL控制器在轻量肌肉代理中学习激活，再将同一激活交给XPBD体积求解器生成形变；EMG先验约束募集模式。
- **Coding接口：**Python、JSON配置、NVIDIA Warp、USD/PLY/Houdini导出。
- **场景：**游戏角色、体育训练、数字替身、影视肌肉形变。
- **成熟度 / 证据：**已开源研究组件；作者在RTX 4090上报告194肌肉、18 ms/帧。
- **重要性 / 阅读：**高 / 必读。

### 10. PiHand与bitHuman：角色的物理交互和会话状态分别成熟一步

- **类型 / 日期 / 主线：**论文、SDK；PiHand 2026-10-22，bitHuman 2.12.7于2026-10-20；数字人、Coding。
- **官方链接：**[PiHand论文](https://arxiv.org/abs/2610.31299)、[PiHand项目页](https://virtualhumans.mpi-inf.mpg.de/PiHand/)、[bitHuman changelog](https://docs.bithuman.ai/changelog)。
- **相对上期的新变化：**今日无更新。
- **核心贡献：**PiHand学习手与物体的联合运动及接触先验，可迁移至抓取、接球和双手交接；bitHuman修复约半秒空闲闭嘴恢复及开口前两帧过渡。
- **Coding接口：**PiHand适合作为物理策略模块；bitHuman提供Python、移动端、WebGPU、LiveKit、CLI/MCP和REST入口。
- **场景：**VR角色交互、产品演示、数字员工、陪伴和导览。
- **成熟度 / 证据：**PiHand为论文原型，checkpoint尚待发布；bitHuman为正式SDK、证据属于官方发布说明。
- **重要性 / 阅读：**中高 / 必读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期没有新的高质量 talking-head、lip-sync或视线控制基础模型。P-Video-2-Pro虽增加最长三分钟肖像视频与agent时间线编辑，但属于离线产品能力，不能视为实时对话突破。[官方发布](https://tellers.ai/blog/p_video_2_pro_avatar_2026-10-22)

- **3D/4D表示：**RealMe提升稀疏捕获下的体积人物分辨率；RLMuscle补上可编程的体积肌肉形变；PiHand补上受物理约束的手物接触。三者共同把数字人从“会说话的脸”推进到“可捕获、可变形、可操作的身体”。

- **实时交互：**RLMuscle的18 ms/帧是局部物理组件指标，不包含角色大脑、语音、网络和渲染。bitHuman已进入对话状态机细节，但未公开端到端首帧、打断恢复P95和并发数据。

- **音视频与情感：**未发现情绪、凝视、倾听表情、多语言和可打断对话同时获得显著提升的新增工作。

- **工程部署：**bitHuman可进入产品验证；RLMuscle可进入角色资产研发；RealMe和PiHand仍需等待完整代码、资产规范与引擎插件。

- **安全与身份治理：**没有新的授权、水印或可撤回身份机制。单图或少量捕获生成角色时，仍需由产品层实施身份凭证、素材访问控制、输出标识和删除传播。

## 5. 世界模型进展

- **架构与训练：**Direct Forcing把因果延续和自身rollout分布对齐压缩为单阶段训练；CADRE说明codec本身决定每个动作需要等待多少帧，不能继续照搬离线T2V的autoencoder。

- **可控/可玩生成：**LiveAct扩展为多主体异构动作；SolarWM开放相机运行时；WorldStream则用语言操作持久化实体。三者分别解决动作协议、视觉流和权威状态。

- **长时与物理一致性：**WorldStream依靠外部world record避免只在视频上下文中“记忆”；PhysTrace负责发现何时违反物理。长时系统正在从扩大上下文转向显式状态加自动验收。

- **空间表示：**持久世界不能只保留像素latent。位姿图、mesh、Gaussian、对象数据库、地形tile和物理属性正成为生成模型与传统引擎之间的中间协议。

- **机器人/游戏/仿真：**LiveAct、CADRE和PiHand均横跨游戏或机器人；但窗口内没有证明开放域世界模型可以稳定替代高保真工程仿真器。

- **实时部署与评测：**少步生成、17.70 FPS或“Realtime”标签都不足以证明交互实时。仍需报告输入事件到可见结果的P50/P95、抖动、连续运行时长、并发和降级策略。

## 6. Coding × 新型互动作品

### 链路一：生成式开放世界

**自然语言需求 → WorldStream规划器 → typed world record → tile compiler/规则引擎 → 可探索、可编辑、可回访世界**

模型负责语义规划，代码负责身份、边界、存档和确定性重建，传统引擎负责碰撞与权威规则。已有论文demo，缺公开运行时。

### 链路二：灰盒玩法到高质视觉

**coding agent生成游戏逻辑与灰盒 → 自动试玩录像 → AnyGameRenderer重绘 → 实体一致性测试 → 品牌游戏或互动短片**

代码负责玩法真实性，模型负责视觉风格，实体级benchmark负责阻止漏怪、数量错误和时机漂移。已有开源证据，但只能离线渲染。

### 链路三：实时多角色生成世界

**游戏服务器/机器人中间件 → 主体schema与动作流 → LiveAct → 联合视觉未来 → 玩家或控制器继续输入**

代码负责主体ID、动作权限和网络同步，模型负责下一段视觉。官方demo证明可运行，尚无多人权威状态、网络抖动和长期稳定性证据。

### 链路四：能拿取物体的数字角色

**剧情或用户目标 → PiHand潜空间策略 → 物理引擎接触求解 → RLMuscle体积形变 → VR角色或产品演示**

策略模型负责自然动作分布，物理引擎负责接触约束，肌肉求解器负责可信形变。各组件有研究证据，端到端组合属于本报告推断。

### 链路五：可测试的XR作品生成

**自然语言需求 → coding agent修改XR项目 → 容器构建 → 脚本交互＋程序/视觉断言 → 修复循环**

GenXR-Bench证明XR代码不能只测试能否编译，还需测试传感器生命周期、物理落点和交互语义。benchmark已运行，自动修复成效仍待进一步验证。

### 链路六：世界模型CI

**模型rollout → PhysTrace滑窗检测 → 时间区间级违规 → 阻断release/重采样/人工复核**

代码负责批任务、阈值、版本对比和回滚；评测模型负责异常定位。这是本周期最容易直接落地的工程链路之一。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | WorldStream、LiveAct、SolarWM | 持久状态、多人动作、相机控制 | demo / 开源原型 | LiveAct 17.70 FPS；端到端未披露 | 权威状态、长时漂移、并发 | [LiveAct](https://arxiv.org/abs/2610.29408) |
| 影视/虚拟制作 | RealMe、RLMuscle | 自由视角人物、肌肉形变 | 论文demo / 开源组件 | RLMuscle 18 ms/帧@4090 | 资产交接、导演控制、捕获成本 | [RLMuscle](https://arxiv.org/abs/2610.30033) |
| 直播与电商 | bitHuman、P-Video | 实时音频驱动或离线头像视频 | 正式SDK / 正式产品 | P-Video官方称1–2.2秒处理/秒成片 | 打断、审核、肖像授权 | [bitHuman](https://docs.bithuman.ai/changelog) |
| 品牌互动 | AnyGameRenderer | 灰盒玩法换视觉风格 | 开源离线原型 | 124帧约17分钟/H100 | 幻觉实体率、GPU成本 | [代码](https://github.com/AlayaLab/AnyGameRenderer) |
| 教育培训 | GenXR、bitHuman | 空间交互、数字讲师 | benchmark / SDK | 未披露 | 内容正确性、硬件碎片化 | [GenXR](https://github.com/whuang37/genxr-bench) |
| 企业数字员工 | bitHuman＋LLM/RTC | listening/thinking/speaking状态机 | 正式SDK | P95与并发未披露 | SLA、审计、真人接管 | [文档](https://docs.bithuman.ai/start/on-device-avatar-sdk) |
| 陪伴与社交 | bitHuman、PiHand | 实时对话、身体交互 | SDK可用，完整系统早期 | 未披露 | 情感安全、隐私、长期记忆 | [PiHand](https://arxiv.org/abs/2610.31299) |
| 空间计算 | RealMe、GenXR | 6DoF观看、手物操作 | 研究原型 | 未披露 | 移动端算力、遮挡、漂移 | [RealMe](https://zju3dv.github.io/realme/) |
| 机器人/仿真 | CADRE、PhysTrace、LiveAct | 动作预测、物理诊断 | 开源研究组件 | CADRE依配置；PhysTrace在A100测试 | sim-to-real、闭环安全 | [CADRE](https://arxiv.org/abs/2610.30129) |

## 8. 可复现资源与开发者入口

| 资源 | 许可证/要求 | 是否值得复现 | 最小验证路径 |
|---|---|---:|---|
| [SolarWM](https://github.com/Junchao-cs/SolarWM) | 代码Apache-2.0；权重许可另查 | 高 | 启动单用户网页，测键盘输入到可见帧P50/P95 |
| [AnyGameRenderer](https://github.com/AlayaLab/AnyGameRenderer) | Apache-2.0；≥80 GB显存 | 中高 | 用一个灰盒小游戏生成32—64帧，统计实体漏失和视觉增益 |
| [PhysTrace](https://github.com/PhysTrace/PhysTrace) | MIT；默认需大型视觉模型/A100 | 高 | 对20段已知失败视频跑时间定位并人工复核 |
| [GenXR-Bench](https://github.com/whuang37/genxr-bench) | CC BY 4.0；Docker/Harbor | 高 | 选5项bug修复任务建立agent回归基线 |
| [RLMuscle](https://github.com/chunleili/rlmuscleV2) | 研究代码；NVIDIA Warp，资产约3.32 GB | 中高 | 跑单个动作并导出USD，检查引擎/DCC可接性 |
| [FlexPart](https://github.com/Gucci233/FlexPart) | MIT代码；权重和数据另查 | 中高 | 由点或mask生成部件GLB，检查碰撞体和装配缝隙 |
| [bitHuman SDK](https://docs.bithuman.ai/start/on-device-avatar-sdk) | 商业SDK | 高 | 跑30轮对话，记录idle→speech、打断及断网降级 |
| [RealMe仓库](https://github.com/zju3dv/RealMe) | 当前仍待完整代码 | 暂缓 | 等实现开放后再复现，不把项目页demo视为代码证据 |

## 9. 系统架构与技术趋势判断

明显升温的是三层架构：

1. **权威状态层：**world record、对象数据库、位姿图、物理属性和传统规则维护可检查事实。
2. **生成表现层：**视频模型、avatar renderer和生成式渲染器负责外观、运动细节及风格。
3. **agent与工程层：**coding agent、规划器、状态机、verifier和CI负责编排、测试、诊断、回滚及人工接管。

趋于主流的技术路线包括少步因果生成、rollout分布对齐、专项视频codec、显式动作schema以及生成结果的时间区间级验收。单纯扩大上下文或只优化单段FVD的重要性相对下降。

尚未解决的问题包括动态对象长期身份、多人权威同步、物理因果、动作到显示延迟、服务成本、长会话恢复、跨引擎资产规范和肖像授权。

需要降级看待的材料包括：只公布采样步数却称“实时”、只展示视频却无可执行控制接口、把离线处理速度包装为会话延迟，以及只承诺开源但仓库仍为空的项目。

## 10. 论文精读候选

1. [WorldStream](https://arxiv.org/abs/2610.28827)：重点读typed decision model、world record和tile recompilation；它与纯视频世界模型的根本差异是权威状态外置。风险是缺代码和规模测试。

2. [LiveAct](https://arxiv.org/abs/2610.29408)：重点读HAI、Agent-Aware Spatial Modulation和rollout-aware distillation；核心差异是统一多主体异构控制。风险是实时硬件与延迟定义不完整。

3. [CADRE](https://arxiv.org/abs/2610.30129)：重点读不同任务的时空压缩消融和latency-quality frontier；适合指导世界模型运行时选型。风险是结果对任务和骨干模型敏感。

4. [AnyGameRenderer](https://arxiv.org/abs/2610.31466)：重点读实体级benchmark、数据构建和失败案例；它把游戏生成渲染从主观画质推进到对象时序验收。风险是幻觉率仍高且资源要求巨大。

5. [GenXR-Bench](https://arxiv.org/abs/2610.29014)：重点读任务构成、交互测试器和失败分类；适合研究coding agent如何进入空间软件。风险是对XR Blocks SDK及测试环境存在依赖。

## 11. 下周跟踪与可行动建议

继续追踪：

1. SolarWM是否发布可比较的动作到显示P50/P95及长时漂移曲线。
2. LiveAct是否开放代码、权重、GPU配置与多人长期rollout。
3. WorldStream是否开放world-record schema、编译器和存档兼容机制。
4. RealMe代码何时结束内部审核，实际捕获相机数量和训练成本是多少。
5. PiHand的processed data与checkpoint能否按承诺发布。
6. AnyGameRenderer能否降低46%左右的幻觉实体问题并支持流式推理。
7. bitHuman是否披露端到端延迟、并发、断网降级和真实客户指标。
8. 数字人供应商是否提供可机器验证的身份授权、撤回和输出溯源协议。

本周可动手实验：

- **世界模型延迟基线：**部署SolarWM单用户demo；记录输入事件、服务接收、首个新latent和首个可见帧时间。难点是跨端时间同步；成功判据是得到P50/P95及连续十分钟漂移数据。

- **生成游戏实体回归：**让coding agent生成一个包含增删实体的灰盒小游戏，再用AnyGameRenderer重绘。难点是80 GB显存和实体标注；成功判据是所有关键实体的出现时机误差不超过两帧且无新增幻觉实体。

- **物理CI门禁：**收集20段团队已有的生成视频失败样本接入PhysTrace。难点是阈值校准；成功判据是至少70%的已知违规被定位到人工标注区间附近，并生成可用于重采样的错误类别。

- **数字员工状态机：**用LLM、TTS、LiveKit和bitHuman实现`listening → thinking → speaking → interrupted`四状态原型。难点是打断时音频、嘴型和业务工具的一致取消；成功判据是30轮对话无持续张嘴、重复音频或失控工具调用。
