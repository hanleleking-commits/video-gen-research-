# 数字人、世界模型与互动作品研究日报：2026-10-16

检索窗口：2026-10-10 至 2026-10-16。  
信息截点：2026-10-16 09:09（Asia/Shanghai）。论文日期以 arXiv 提交记录为准；已与 10 月 9—15 日日报去重。

## 1. 本日摘要

本日最重要的变化不是单个视频模型画质提升，而是生成系统开始明确分离“权威世界状态”和“观察者看到的生成体验”。ATAS 用场景记忆与事件记忆维护跨镜头世界，DAO/JING 则进一步将可计算的共享状态引擎与第一人称音视频生成器分开，形成更接近持续互动作品的架构。

实时世界模型的“时间”也开始成为可编程参数：RAR 用物理时间坐标支持运行中切换生成帧率，而不清空历史缓存，解决硬件吞吐变化导致世界运行速度失真的问题。

游戏侧，Player-1 证明程序化合成环境、可验证奖励和键鼠复合动作能显著改善跨游戏控制；但训练环境、权重和完整代码尚未发布，当前仍是论文与展示阶段。

数字人侧最强新增是 Kandinsky S2V 1.0：覆盖 2B/21B、基础版与蒸馏版，并把嘴形、表情、姿态和手势放进同一 speech-to-video 模型；不过论文所称完整权重和代码截至截点尚未全部可访问，应按“发布承诺”而非“已可复现”处理。

OmniPoser 与 MimicX 分别补齐实时人体动作恢复和视频到人形机器人策略闭环：前者报告单步流匹配超过 700 FPS，后者已开放九组策略、动作资产及执行反馈驱动的训练代码。

工业世界模型正在从“生成未来”转向“生成可供决策检查的后果”：SafeDriveVLA 让模型先判断自然语言驾驶命令是否与场景冲突，WEDGE 则从 agent 的失败日志反向生成针对性训练环境。

Coding 的角色因此更清晰：代码持有对象状态、物理时间、规则、安全模式、奖励和版本门禁；生成模型负责补全视觉、音频、动作或潜在未来；传统引擎继续承担碰撞、确定性逻辑和硬实时控制。

除 SafeDriveVLA、MimicX、ATAS、WEDGE 和 JING 的部分组件外，多数新增仍缺第三方复现、并发压力、p95 延迟或真实客户部署，本周期没有可独立核验的规模化产业案例。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | 开放 speech-to-video 模型族、灵活传感器动捕、视频到机器人策略同时出现 | MimicX 已开放策略和代码；Kandinsky、OmniPoser 的完整可复现性仍待确认 | 从单纯实时 talking head 扩展到全身动作、手势和物理执行 |
| 世界模型 | 很高 | 权威状态、事件记忆、物理时间和安全模式成为显式接口 | ATAS、JING、SafeDriveVLA 有代码入口；RAR 仍为占位仓库 | 从机器人 WAM 集中爆发，转向持续世界和互动运行时 |
| Coding × 互动作品 | 很高 | 状态编译器、Prompt skills、可验证奖励、模式 token、执行门禁进入系统设计 | 出现可运行 CLI、数据管线和训练脚本，但实时 causal 模型仍不足 | 从“调用视频模型”推进到“代码维护世界、模型渲染体验” |
| 工业应用 | 中 | 驾驶冲突检查、失败定向合成、视频到机器人策略具备闭环证据 | 仿真/研究闭环增强，真实生产部署未新增 | 相较上期机器人预测控制，本期更强调安全、诊断和数据闭环 |
| 安全与治理 | 中 | SafeDriveVLA 显式输出 STRICT/CAUTIOUS/FALLBACK；SyncReward提供音画同步门禁 | 有 benchmark 和模式接口，无肖像授权新机制 | 新增运行时行为安全，但数字人身份治理无实质增量 |

## 3. 最值得关注的 12 个进展

### 1. ATAS：把长视频的跨镜头一致性改造成世界状态管理问题

- **类型 / 日期 / 主线：**论文、GitHub、CLI；2026-10-14；世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2610.20571)、[MIT 代码](https://github.com/ziplab/ATAS)、[项目页](https://lhmd.top/atas/)。
- **相对上期：**首次收录；比上期 Happy Oyster 的对象状态更偏影视生产和多镜头回访。
- **核心贡献：**将剧本编译成带场景、相机、事件时间的 shot request；静态几何进入 scene memory，动态观察进入 event memory。新镜头只有通过审片、注册和状态检查后，才可更新共享世界。
- **Coding 接口：**`atas run --screenplay ...`；支持无记忆消融、只规划、限制镜头数、断点续跑，并输出逐镜头 manifest 与报告。
- **互动/工业场景：**互动电影、虚拟制作、可回访的品牌叙事、自动分镜与补拍预演。
- **成熟度 / 证据：**已开源研究工具；作者实测，无第三方复现。几何服务约需 20 GB GPU 显存，视频生成依赖外部服务。
- **重要性 / 阅读：**高 / 必读。

### 2. DAO + JING：共享世界由代码计算，每个观察者的体验由模型生成

- **类型 / 日期 / 主线：**技术报告、模型、代码、demo；2026-10-14；世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2610.19726)、[JING 代码](https://github.com/XGEN-Labs/XGEN-JING)、[模型入口](https://huggingface.co/XGENlabs/XGEN-JING)。
- **相对上期：**延续 AgentGarten、LingBot World 2 的“代码保存世界”路线，但新增共享状态引擎 DAO 与观察者特定音视频生成器 JING 的明确分层。
- **核心贡献：**DAO 保存人物、物体、空间、关系与世界时间；JING 根据这些状态、参考图、观察历史和动作生成第一人称音视频。不同观察者共享事实，却获得不同视角。
- **Coding 接口：**已提供四步双向推理、相机控制、参考图、音视频联合生成示例及 Prompt skills；真正用于持续交互的 causal 模型仍标为 coming soon。
- **场景：**生成式 NPC 世界、多人互动叙事、虚拟展览、训练模拟器。
- **成熟度 / 证据：**可运行离线 demo；六张 H100 验证，实时因果闭环尚未开放。代码和权重受 MiniMax H3 Community License 约束。
- **重要性 / 阅读：**高 / 必读。

### 3. Player-1：用程序化游戏环境训练可迁移的视觉控制 agent

- **类型 / 日期 / 主线：**论文、数据/环境发布计划；2026-10-14；Coding × 互动作品、游戏。
- **官方链接：**[论文](https://arxiv.org/abs/2610.20584)、[GitHub](https://github.com/TencentARC/Player-1)、[项目页](https://player-1-project.github.io/)。
- **相对上期：**不同于 Summer Engine 的“agent 造游戏”，Player-1 研究“agent 如何在可编程合成游戏中学习操作能力”。
- **核心贡献：**在计算机操作动作空间中增加相对指针移动、持续按键和键鼠复合动作；用 6,044 条、覆盖 172 款游戏的轨迹初始化，再在 16 个可验证合成环境中做 PPO。
- **关键结果：**作者报告 Player-1-9B 在 MineDojo-300 达到 38.33%，最强对照为 12.63%；9B/35B 相对各自 SFT 策略提升 27.8/17.7 个百分点。
- **Coding 接口：**环境直接从内部游戏状态计算奖励，适合自动生成关卡、扰动控制和回归测试。
- **成熟度 / 证据：**论文与视频 demo；数据、Gym、权重、训练代码均仍在 release plan 中。
- **重要性 / 阅读：**高 / 必读。

### 4. Kandinsky S2V 1.0：开放式全身 speech-to-video 数字人模型族

- **类型 / 日期 / 主线：**论文、模型发布声明；2026-10-13；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.19290)、[项目页](https://kandinskyavatar.github.io/)、[实验性 Diffusers 管线](https://huggingface.co/kandinskylab/tiny-kandinsky-s2v-modular-pipe)。
- **相对上期：**此前日报未收录；本次纳入原因是窗口内论文与 Diffusers 组件出现，可核验实际发布状态。
- **核心贡献：**以单张参考图、语音和文本生成 25 FPS 长视频，同时驱动嘴形、表情、姿态和手势；提供 2B Lite、21B Pro 及 π-Flow 蒸馏版本。
- **关键结果：**论文报告蒸馏 Pro 以 16 步、无 CFG 推理，端到端加速 5.4 倍；指标与偏好均为作者测试。
- **Coding 接口：**设计为 Diffusers pipeline，可将参考图、音频和 prompt 作为结构化输入。
- **边界：**论文称 MIT 代码和 checkpoint 已发布，但截至截点仅核验到随机权重测试管线，完整模型应标记为**待核验**。
- **成熟度 / 证据：**论文 demo / 发布承诺。
- **重要性 / 阅读：**高 / 必读。

### 5. What Matters for World Modeling in VLAs：系统比较四种世界模型接入方式

- **类型 / 日期 / 主线：**论文、系统评测；2026-10-14；世界模型、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.19825)、[项目页](https://dylanmqs.github.io/WM-in-VLAs/)。
- **相对上期：**相较密集出现的单点 WAM，本项首次在统一训练协议下比较 Implicit Future Alignment、Explicit Future Query、MoT Future Expert 和 External World Predictor。
- **核心发现：**作者报告 DINOv3、SigLIP 等 2D 语义表示跨范式最稳定；解耦的 MoT Future Expert 在 OOD 和预训练 VLA 适配上更稳；世界建模收益主要出现在长时和动态任务。
- **Coding 接口：**为工程选型提供决策：已有 VLA 优先外挂未来专家，而不是侵入式改写主干；未来预测可作为独立服务和回归模块。
- **成熟度 / 证据：**论文实测；尚无工业部署。
- **重要性 / 阅读：**高 / 必读。

### 6. RAR：让世界模型按物理时间而不是帧序号运行

- **类型 / 日期 / 主线：**论文、demo；2026-10-12；世界模型、实时运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2610.16302)、[GitHub](https://github.com/HKUST-LongGroup/RAR)。
- **相对上期：**从 CtrlCache 的“控制变化决定缓存”推进到“硬件吞吐决定 rollout rate，但不改变世界物理速度”。
- **核心贡献：**Stride-RoPE 把不同采样率的帧对齐到同一物理时间轴；支持未见过的中间速率和运行中切换速率，且无需清空 KV cache。
- **Coding 接口：**运行时可根据 GPU 负载、网络抖动或交互紧迫度改变 stride。
- **成熟度 / 证据：**论文与对比视频；仓库明确把 inference code 列为 TODO，无许可证声明。
- **重要性 / 阅读：**高 / 必读。

### 7. SafeDriveVLA：先想象“照做会怎样”，再决定是否服从自然语言命令

- **类型 / 日期 / 主线：**论文、benchmark、GitHub；2026-10-14；世界模型、自动驾驶、安全。
- **官方链接：**[论文](https://arxiv.org/abs/2610.20609)、[Apache-2.0 代码](https://github.com/ASGuard-UCI/safedrive-vla)。
- **相对上期：**从 SimVTA 的轨迹—视频对齐推进到语言命令与场景冲突的显式安全判断。
- **核心贡献：**VLA 在动作前输出 `<STRICT>`、`<CAUTIOUS>` 或 `<FALLBACK>`；冻结的 V-JEPA 2 预测器按命令锚定动作，想象严格服从后的未来。
- **关键结果：**作者报告 Bench2Drive DS 88.16、成功率 68.64%；B2D-C 中碰撞由最佳 SimLingo 变体的 60 降至 38。
- **Coding 接口：**模式 token 可直接映射到速度限制、人工接管或拒绝执行；提供 CARLA-F、B2D-C、B2D-Adv 路线与指标。
- **成熟度 / 证据：**仓库已有实质代码，但 README 同时把部分训练代码和 checkpoint 列作 TODO，按**部分开源**处理。
- **重要性 / 阅读：**高 / 必读。

### 8. OmniPoser：任意稀疏传感器组合驱动实时全身 SMPL

- **类型 / 日期 / 主线：**论文、项目 demo；2026-10-14；数字人、动作捕捉。
- **官方链接：**[论文](https://arxiv.org/abs/2610.19857)、[项目页](https://jamesbridgewater51.github.io/omniposer-eccv/)。
- **相对上期：**从固定设备配置推进到 3D 关键点、IMU、头显 6-DoF 或其任意子集共用一个模型。
- **核心贡献：**跨模态掩码和 Masked Conditioning Encoder 统一缺失、异构传感器；单步 Conditional Flow Matching 恢复全身 SMPL。
- **关键结果：**作者报告在七个 benchmark 上达到同类水平，并超过 700 FPS；硬件、端到端采集延迟和多人性能仍需核验。
- **Coding 接口：**统一的 masked sensor tensor 适合接入 XR、动捕、虚拟主播和游戏角色 retarget 管线。
- **成熟度 / 证据：**研究 demo；未核验到公开代码或权重。
- **重要性 / 阅读：**中高 / 必读。

### 9. MimicX：让机器人执行失败反向修改视频动作监督

- **类型 / 日期 / 主线：**论文、GitHub、策略与动作资产；论文 2026-10-06，重大开源更新 2026-10-14；数字人动作、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.09055)、[Apache-2.0 代码](https://github.com/NEBULIS-Lab/MimicX)、[策略](https://huggingface.co/Shuaijun/MimicX-Policies)、[资产](https://huggingface.co/datasets/Shuaijun/MimicX-Assets)。
- **纳入原因：**论文早于窗口，但窗口内首次开放九组 PPO 策略、Parkour 混合控制器、数据和完整复现说明。
- **核心贡献：**执行 rollout 定位困难时间段和身体部位，再调整 tracking objective 与 reset curriculum；候选更新只有通过重复执行门禁才替换旧策略。
- **关键结果：**作者报告平均身体跟踪误差下降 25.7%，Robust Execution Horizon 增长 255.6%。
- **Coding 接口：**GVHMR/SMPL-X → GMR retarget → MuJoCo/MjLab → PPO → AutoRefine。
- **成熟度 / 证据：**已开源研究栈；无真实人形机器人部署证据。
- **重要性 / 阅读：**中高 / 必读。

### 10. WEDGE：从 agent 失败模式反向生成训练环境

- **类型 / 日期 / 主线：**论文、MIT GitHub、数据；2026-10-14；世界模型、合成环境。
- **官方链接：**[论文](https://arxiv.org/abs/2610.19420)、[代码与 episode corpora](https://github.com/zhangyuejoslin/WEDGE)。
- **相对上期：**延续 LoopWAM 的失败数据回流，但把失败用于选择“下一批应生成什么环境”。
- **核心贡献：**LLM 比较成功/失败轨迹，提出可执行弱点特征，经诊断集验证后，将弱点映射到 ProcTHOR 环境参数。
- **Coding 接口：**开放 `discover.py`、验证 gate、sandbox、episode summary 和 mock 模式；可替换 agent 描述与日志语料。
- **成熟度 / 证据：**弱点发现部分可运行；完整环境生成和 agent 适配仍依赖外部栈。
- **重要性 / 阅读：**中高 / 可读。

### 11. SyncReward：以人类标注把音画同步变成训练和发布指标

- **类型 / 日期 / 主线：**dataset、benchmark、reward model；2026-10-12；数字人支撑基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2610.16338)、[GitHub](https://github.com/WeChatCV/SyncReward)。
- **相对上期：**为 LTX-Avatar、Kandinsky S2V 和联合音视频模型补充可训练的同步门禁。
- **核心贡献：**80K 专业同步评分、1K held-out 视频，覆盖语音、瞬态声、乐器和环境声；reward 可用于 best-of-N 或 RL 后训练。
- **Coding 接口：**可嵌入生成服务的候选排序、A/B 测试和 CI 回归。
- **成熟度 / 证据：**论文实测；仓库称将发布，当前按待开放处理。
- **重要性 / 阅读：**中 / 可读。

### 12. Glance Forcing：把实时视频模型蒸馏成本从数千 GPU 小时压至一次轨迹

- **类型 / 日期 / 主线：**论文、项目占位；2026-10-14；视频基础设施、世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2610.20220)、[GitHub](https://github.com/zhuobaidong/Glance-Forcing)。
- **相对上期：**相比 ForcingBooster 的推理算力分配，本项解决双向扩散模型转因果少步模型的训练成本。
- **核心贡献：**高噪声慢专家负责语义和运动，低噪声快专家负责纹理；分别拟合不同曲率的 PF-ODE 区域。
- **关键结果：**作者称只采集一条 PF-ODE 轨迹、约一 A100-GPU 小时，即超过完整训练基线的 VBench 总分 84.04→84.22。
- **Coding 接口：**适合作为将离线视频模型转换为因果/流式运行时的训练 recipe。
- **成熟度 / 证据：**论文实测；未见可直接复现的完整训练资产。
- **重要性 / 阅读：**中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**Kandinsky S2V 将语音、文本和参考图统一为全身人物视频条件；相较传统 talking head，覆盖表情、姿态和手势，但完整开放状态仍未兑现。
- **3D/4D 表示：**OmniPoser 输出 SMPL 动作而非外观资产；本周期没有新的高质量单图 3DGS/NeRF 全身 avatar 进入可复现阶段。
- **实时交互：**OmniPoser 的 700+ FPS 是动作恢复模块速度，不等于包含传感、渲染、语音和网络的端到端 avatar 延迟。Kandinsky 蒸馏版虽减少采样步骤，也没有达到双工通话级实时证据。
- **音视频与情感：**Kandinsky 同时建模嘴形、姿态与手势；SyncReward开始对语音、声音事件和画面时序统一评分。视线、倾听姿态、打断恢复和情绪可控性仍未在开放栈中统一。
- **工程部署：**MimicX 的资产、策略、复现文档最完整；OmniPoser适合作为 XR/动捕前端；Kandinsky需等待真实 checkpoint、显存与吞吐说明。
- **安全与身份治理：**本日没有新的授权、撤回、水印、C2PA 或肖像权机制。开源 speech-to-video 能力增强，但治理接口没有同步增强。

结论：人体动作与表演生成正在走向统一和实时，但“可打断对话大脑＋低延迟语音＋全身 avatar＋身份授权”的产品级闭环仍未出现。

## 5. 世界模型进展

- **架构与训练：**主流路线正在分化为显式状态引擎＋生成观察（DAO/JING）、外置场景/事件记忆（ATAS）、VLA 外挂未来专家，以及以视觉运动构造 action token。
- **可控/可玩生成：**JING 已支持键盘相机控制、文本交互和参考图，但当前公开模型是双向生成，真正连续 causal 版本尚未发布。
- **长时与物理一致性：**ATAS 依靠注册和准入门禁避免错误镜头污染记忆；RAR 解决物理时间一致性；二者仍不能保证生成内容遵守完整物理规律。
- **空间表示：**静态几何、事件时间、场景关系和观察者状态逐渐取代“仅保留最近几帧”的隐式记忆。
- **机器人/游戏/仿真：**Player-1、WEDGE、MimicX 都采用程序化环境或物理执行产生可验证反馈；SafeDriveVLA 把想象结果用于安全模式选择。
- **实时部署：**RAR 的动态 stride 和 Glance Forcing 的低成本因果蒸馏具有运行时价值；但除论文指标外，仍缺 p95、并发、显存抖动和故障恢复数据。
- **评测：**世界模型的评测对象正在从画质扩展为跨镜头状态、命令冲突、物理时间、执行时长和失败后的改进能力。

## 6. Coding × 新型互动作品

### 链路一：持久世界中的多人互动叙事

**剧情/用户动作 → DAO 权威状态与规则 → 观察者条件 → JING 音视频 → 玩家输入 → 状态更新**

代码负责对象、关系、世界时间、权限和冲突裁决；JING 负责把同一状态渲染成每个观察者的第一人称体验；传统服务器负责多人同步。JING 离线推理可运行，连续 causal 多人世界仍属未完成能力。

### 链路二：可回访、可重拍的生成式电影

**剧本 → ATAS world-state compiler → 场景/事件检索 → 视频生成 → 几何注册与状态检查 → 镜头时间线**

代码决定镜头发生在何地、何时以及哪些生成结果有资格写回记忆；模型负责生成画面；传统剪辑系统负责成片。这一链路已有 CLI 和示例，适合先用于预演、分镜和素材辅助，而非无人工监督的成片生产。

### 链路三：随硬件负载调速、但物理时间不变的可玩视频

**键鼠事件＋GPU telemetry → RAR stride policy → KV 历史连续 rollout → WebRTC/播放器**

代码根据负载选择生成速率；世界模型维持相同物理时钟；RTC 控制播放缓冲。RAR 推理代码尚未发布，因此是有论文证据的运行时设计，而非现成 SDK。

### 链路四：从合成小游戏学会真实游戏控制

**coding agent 生成环境/任务 → 程序状态计算奖励 → Player-1 PPO → 键鼠复合动作 → 外部游戏评测**

代码和游戏引擎维护确定状态与奖励；视觉 agent 从截图决定动作；RL 扩展未覆盖状态。论文结果较强，但环境和模型尚未开放，当前不能独立复现。

### 链路五：具备“拒绝危险指令”能力的驾驶 agent

**自然语言导航 → world-model dreaming → STRICT/CAUTIOUS/FALLBACK → VLA 轨迹 → CARLA/车辆控制器**

模型预测服从命令后的后果；代码把模式 token 映射为速度上限、最小安全距离、人工接管或拒绝执行；传统控制器负责硬实时。这是本周期最清晰的“生成预测 → 代码安全策略 → 执行”链路，但证据仍主要来自 CARLA。

### 链路六：视频表演转为可执行人形机器人技能

**人物视频 → SMPL-X/GVHMR → retarget → MuJoCo/MjLab PPO → 执行诊断 → MimicX AutoRefine → 验证门禁**

生成/恢复模型提供动作目标；仿真和控制器负责动力学；代码根据失败窗口调整训练目标，并只有在重复执行改善时接受新版本。已有开放代码与策略，真实机器人安全迁移属于后续工作。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | DAO/JING、Player-1、规则服务器 | 键鼠/文本改变共享状态与观察 | demo / 发布计划 | JING 验证需 6×H100；Player-1 未披露 | causal 速度、多人权威状态、内容安全 | [JING](https://github.com/XGEN-Labs/XGEN-JING)、[Player-1](https://github.com/TencentARC/Player-1) |
| 影视/虚拟制作 | ATAS＋视频生成器＋几何注册 | 跨镜头回访、事件重拍 | 已开源研究工具 | 几何服务约 20 GB 显存；生成费用未披露 | 人物/服装一致性、人工审批、资产版权 | [ATAS](https://github.com/ziplab/ATAS) |
| 直播与电商 | Kandinsky S2V＋RTC＋对话 agent | 语音驱动全身表演 | 论文 demo | 完整推理成本未核验 | 首帧延迟、打断、并发、肖像授权 | [Kandinsky S2V](https://arxiv.org/abs/2610.19290) |
| 品牌互动 | DAO/JING＋状态机＋参考资产 | 角色、物体与场景持续响应 | 可运行离线 demo | 6×H100 参考配置 | 品牌一致性、审核、成本 | [JING](https://github.com/XGEN-Labs/XGEN-JING) |
| 教育培训 | WEDGE＋ProcTHOR＋任务评分器 | 按学员/agent 弱点生成训练环境 | 研究原型 | 未披露 | 环境真实性、教学目标验证 | [WEDGE](https://github.com/zhangyuejoslin/WEDGE) |
| 企业数字员工 | avatar＋agent＋业务状态机 | 对话、工具调用、表演 | 本周期无新部署 | 未披露 | 权限、审计、打断、身份安全 | [SyncReward](https://arxiv.org/abs/2610.16338) |
| 陪伴与社交 | JING＋角色记忆＋音视频生成 | 多轮对话与共享事件 | 概念/离线 demo | 未披露 | 长期记忆隐私、情感依赖、实时成本 | [DAO/JING](https://arxiv.org/abs/2610.19726) |
| 空间计算 | OmniPoser＋XR 传感器＋Unity/Unreal | 任意传感器组合驱动全身 | 研究 demo | 作者称动作恢复 700+ FPS | 端到端延迟、遮挡、设备标定 | [OmniPoser](https://arxiv.org/abs/2610.19857) |
| 机器人/仿真 | MimicX、SafeDriveVLA、CARLA/MuJoCo | 失败回流、安全模式和策略门禁 | 已开源研究闭环 | 生产成本未披露 | sim-to-real、安全认证、尾部事件 | [MimicX](https://github.com/NEBULIS-Lab/MimicX)、[SafeDriveVLA](https://github.com/ASGuard-UCI/safedrive-vla) |

## 8. 可复现资源与开发者入口

| 资源 | 状态与许可证 | 硬件/成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|---|
| [ATAS](https://github.com/ziplab/ATAS) | 实质代码；MIT | 几何服务约 20 GB GPU；视频 API 另计 | 值得 | 使用 bookstore 示例，分别以 `--memory none` 和默认模式生成 2—3 镜头并比较 manifest |
| [XGEN-JING](https://github.com/XGEN-Labs/XGEN-JING) | 四步双向推理、Prompt skills；MiniMax H3 社区许可证 | 官方验证 6×H100 | 有集群时值得 | 跑单个相机控制示例；先验证动作条件是否改变视频和音频 |
| [Player-1](https://github.com/TencentARC/Player-1) | MIT 仓库，但数据、Gym、权重和训练代码待发布 | 未披露 | 暂仅跟踪 | 检查 release plan 是否转为已完成，再复现一个合成环境 |
| [Kandinsky S2V 测试管线](https://huggingface.co/kandinskylab/tiny-kandinsky-s2v-modular-pipe) | 随机权重，仅用于 Diffusers 集成测试 | 不能评估实际质量 | 暂不做质量复现 | 验证 pipeline API 和依赖；等待真实 Lite checkpoint |
| [RAR](https://github.com/HKUST-LongGroup/RAR) | 仅项目页；推理代码 TODO；许可证未披露 | 未披露 | 仅跟踪 | 等代码后比较固定帧率和运行中切换 stride 的同一物理时刻 |
| [SafeDriveVLA](https://github.com/ASGuard-UCI/safedrive-vla) | Apache-2.0；代码与 benchmark 已出现，checkpoint 状态不一致 | CARLA 训练成本未披露 | 值得先跑评测 | 先执行 B2D-C 指标脚本，检查模式 token 与碰撞变化 |
| [WEDGE](https://github.com/zhangyuejoslin/WEDGE) | 弱点发现代码和语料；MIT | 需 Gemini API；提供 mock 模式 | 值得 | 用 mock 跑一轮，再对 Uni-NaVid 655 条 episode 做弱点发现 |
| [MimicX](https://github.com/NEBULIS-Lab/MimicX) | 代码、九组策略、动作资产；Apache-2.0 | MuJoCo/MjLab＋GPU，具体显存未披露 | 值得 | 下载单一 Tennis 策略，复现固定参考与 AutoRefine 的执行时长 |
| [VITAL](https://github.com/worldbench/VITAL) | Apache-2.0；当前仓库已有评分/数据工具，核心推理权重仍列为 TODO | NAVSIM 数据占用较大 | 暂仅验证数据工具 | 跑公开 NAVSIM 下载与 scoring，不把仓库存在误报为完整模型开源 |
| [SyncReward](https://github.com/WeChatCV/SyncReward) | 论文承诺发布，当前资源待补齐 | 未披露 | 等模型发布 | 对同一数字人视频人工制造 80—200 ms 音频偏移，检查排序单调性 |

## 9. 系统架构与技术趋势判断

- **明显升温：**显式世界状态、事件时间、失败驱动训练、外置未来专家、可验证执行门禁。
- **仍在升温但证据偏弱：**联合音视频数字人、可玩生成视频、通用全身 avatar。帧率或采样步数改善尚未转化为端到端双工体验。
- **趋于主流的架构：**

  1. `权威状态/规则引擎 → 观察条件 → 生成式渲染 → 用户动作 → 状态更新`；
  2. `现有 VLA → 独立未来预测器 → 动作/安全模式`；
  3. `人类视频 → 动作恢复/重定向 → 物理仿真 → 策略训练 → 执行反馈`；
  4. `失败日志 → 弱点诊断 → 定向环境生成 → 再训练 → 回归门禁`。

- **关键未解问题：**连续生成的 p95 延迟、多人并发、状态与画面的双向一致性、模型生成失败时的事务回滚、跨版本记忆迁移、肖像授权、可撤回训练数据和长时间身份稳定。
- **工程包装风险：**只开放项目页或空仓库的项目不应视为开源；“实时”若只指单个动作恢复模块或离线生成吞吐，也不能等同于可打断对话或可玩运行时。
- **产业判断：**短期最可落地的不是完全生成的开放世界，而是让生成模型服务于传统软件中的局部环节：预演镜头、合成训练环境、动作恢复、未来风险检查和候选排序。

## 10. 论文精读候选

1. **[Any Time, Any Space](https://arxiv.org/abs/2610.20571)**  
   值得读：把生成视频明确建模为世界状态事务。重点看 world-state compiler、memory admission、registration 和 runtime 分析。相较普通多镜头 agent，新增的是“生成成功”与“允许写回状态”分离。复现风险在外部视频 API 和 Pi3X/SAM3 依赖。

2. **[Generative World Simulation with DAO and JING](https://arxiv.org/abs/2610.19726)**  
   值得读：代表“规则持有事实、模型生成体验”的系统路线。重点看 DAO 状态模式、observer conditioning、连续交互循环。与 Happy Oyster/AgentGarten 的差异是联合音视频和观察者特定生成。风险是 causal 模型未开放且需六张 H100。

3. **[What Matters for World Modeling in VLAs?](https://arxiv.org/abs/2610.19825)**  
   值得读：少见的跨表示、跨接入范式控制实验。重点看四类 paradigm、representation ablation、OOD 与 π0.5 适配。风险是结论依赖冻结 encoder 和统一但有限的损失设定。

4. **[RAR](https://arxiv.org/abs/2610.16302)**  
   值得读：把“生成帧率”和“世界物理时间”解耦。重点看 Stride-RoPE、多 stride 训练、unseen-rate 与在线切换实验。复现风险是推理代码未发布。

5. **[Kandinsky S2V 1.0](https://arxiv.org/abs/2610.19290)**  
   值得读：开放全身 speech-to-video 的模型规模、数据筛选和 π-Flow 蒸馏较完整。重点看三尺度 reference memory、area-adaptive face loss、蒸馏和长视频实验。风险是论文声称的完整开源资源尚未全部可访问。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. JING causal 模型是否按计划开放，以及在单流、多人和长时运行下的真实帧率。
2. Kandinsky S2V Lite/Pro 权重、MIT 代码和 Diffusers 集成是否真正上线，论文与仓库状态是否一致。
3. RAR 推理代码是否发布；在线切换速率时控制响应和 KV 一致性是否可复现。
4. Player-1-Gym、6,044 条轨迹、9B/35B 权重是否开放，合成训练是否迁移到未见商业游戏。
5. SafeDriveVLA 的训练代码和 checkpoint 是否补齐，README 中“已发布”和 TODO 的矛盾是否消除。
6. ATAS 在人物、服装和可动物体上的状态注册失败率，以及错误状态的回滚成本。
7. OmniPoser 700+ FPS 的硬件、batch size、传感器同步成本和 Unity/Unreal 插件情况。
8. SyncReward 对嘴形同步、环境声和复杂多人说话的相关性是否经第三方验证。

### 本周可做的小实验

#### 实验一：为生成式短片加入“状态写回门禁”

- **目标：**验证显式状态是否减少跨镜头物体位置和事件时间错误。
- **组件：**ATAS、任一视频 API、两场景六镜头剧本。
- **难点：**几何注册失败与 API 随机性。
- **成功判据：**同一地点回访时关键物体位置错误率下降；被拒绝镜头不污染后续生成。

#### 实验二：构建音画同步 CI

- **目标：**建立数字人模型版本升级的同步回归测试。
- **组件：**TalkLikeYou 或现有 avatar、人工音频偏移脚本、SyncReward 发布后模型。
- **难点：**奖励模型可能只对特定声音类别可靠。
- **成功判据：**0、80、160、240 ms 偏移的得分基本单调下降，并与五人盲评排序一致。

#### 实验三：视频动作到仿真角色的失败闭环

- **目标：**验证执行反馈是否优于固定动作参考。
- **组件：**MimicX、单个公开策略、MuJoCo/MjLab。
- **难点：**SMPL-X 授权、依赖版本和接触参数。
- **成功判据：**在相同随机种子和预算下，AutoRefine 提升连续执行时长，同时不恶化身体跟踪误差门限。

#### 实验四：实现负载感知的物理时间调度原型

- **目标：**在 RAR 代码发布前验证运行时接口设计。
- **组件：**固定视频或简单模拟器、GPU telemetry、WebSocket 播放器、可变 stride 调度器。
- **难点：**区分生成吞吐、显示帧率与仿真 tick。
- **成功判据：**GPU 负载变化时画面帧率可降级，但角色移动一秒所覆盖的物理距离保持稳定。
