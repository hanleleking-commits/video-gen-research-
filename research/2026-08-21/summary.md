# 数字人、世界模型与互动作品研究日报：2026-08-21

检索窗口：2026-08-15 至 2026-08-21。

## 1. 本日摘要

本周期最值得注意的变化，不是出现了更大的通用视频模型，而是“生成结果如何进入可执行系统”变得更具体。数字人侧，EfficientSync 放弃整张下半脸的扩散重绘，改用参考纹理变形与混合；作者在单张 A100、416×320 面部裁剪下报告 166 FPS 和 718 MB 峰值显存，但唇形同步分数仍落后于部分扩散方案，说明它更接近低延迟产品组件，而非全面质量领先。精确鼓手动作生成进一步表明，音频驱动虚拟人正在从“动作看起来合理”走向“末端执行器在厘米级位置和节拍上正确”。

世界模型侧出现两种互补路线：DA-WAM 为每条候选驾驶轨迹分别预测未来 latent，并让该 latent 直接参与轨迹评分；GigaBrain-WBC-0.5 则让同一因果 Transformer 同时预测动作、状态和下一行为分布，以便在运行时识别不可能完成的指令。RoomWright 是 Coding 主线最强新增：它把房间、资产、关节以及跨物体行为全部表示为代码，并将互动编译为“触发—条件—效果”规则，在 Isaac Sim/OmniGibson 中执行。其结果具备可编辑性和仿真证据，但作者报告平均生成一个场景消耗约 4300 万 token、耗时约 9 小时，距离在线生成式游戏或实时创作仍很远。

基础设施方面，SparsePR 已开源训练免除的稀疏注意力实现，在四类视频/世界模型上报告 1.48–2.61 倍端到端加速；VA-Judger 同日开放奖励模型、benchmark、推理代码和 LTX-2 RL LoRA，使原生音视频生成首次出现较完整的人类偏好后训练入口。相对 8 月 20 日日报，本期没有重复收录 Hydra-0、DynaForcing、FireRedTTS3 等项目，因为未发现其实质更新。整体证据仍以论文实测、开源研究代码和实体 demo 为主，未发现新的规模化商业部署。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | 唇同步从扩散式重绘转向低成本真实纹理变形；音频动作开始强调末端空间精度 | EfficientSync 有视频 demo、无代码；尚非 SDK/产品 | 从昨日“防止长时动态坍缩”推进到单帧低延迟与纹理保真 |
| 世界模型 | 高 | 预测必须与候选决策一一对应；行为世界模型开始在部署时拒绝不合理命令 | SparsePR、VA-Judger 可运行；DA-WAM、GigaBrain 多数实现未开放 | 从通用未来预测转向“预测是否真正改变决策” |
| Coding × 互动作品 | 高 | RoomWright 将完整 3D 场景、跨物体因果行为和物理属性编译为代码 | 有 Isaac Sim/OmniGibson 实验，但系统未开源且生成成本高 | 从搜索关卡生成程序进一步扩展到生成持续运行的交互场景程序 |

## 3. 最值得关注的 11 个进展

### 1. RoomWright：把互动房间、资产与跨物体行为全部编译成代码

- **类型 / 时间 / 主线：** 论文、agent 系统、仿真 demo；2026-08-19 UTC 提交；Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18840)、[HTML 全文](https://arxiv.org/html/2608.18840v1)。
- **相对上期：** 首次收录。
- **核心贡献：** 从房间用途推断对象、支撑关系和 interaction graph；将开关、旋钮、接触等行为编译成可组合的 trigger–condition–effect 规则。关节、质量、惯量、摩擦、碰撞体和材质也保存在程序中。
- **Coding 接口：** 输出参数化资产程序、命名部件、状态与规则；通过统一 runtime 绑定至 Isaac Sim/OmniGibson。代码文本可 diff、审查和由 agent 二次修改。
- **互动与工业场景：** 可操作数字孪生、机器人训练环境、可编程密室/教育空间、游戏机制原型。
- **成熟度 / 证据：** 研究 demo；论文展示机器人触发灯、微波炉、风扇等行为。未发现公开仓库。作者报告每场景平均约 4300 万 token、9 小时 10 分钟，且暂不支持软体布料和真实挂钩力学。
- **重要性 / 阅读：** 高 / 必读。

### 2. EfficientSync：以真实参考纹理变形实现高速唇同步

- **类型 / 时间 / 主线：** 论文、视频 demo；2026-08-19；数字人。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18832)、[项目 demo](https://alunaticat.github.io/EfficientSync/index.html)。
- **相对上期：** 首次收录。
- **核心贡献：** Dynamic Texture Mixer 从多参考帧选择和混合真实嘴部纹理，STAR Sampling 挑选清晰且拓扑多样的参考帧，避免扩散模型重新“幻想”牙齿和唇纹。
- **Coding 接口：** 工程形态接近 `video + audio + reference frames → lip-synced video` 单次前向服务；依赖 MediaPipe 468 点、16 kHz 音频和参考帧缓存，但未开放代码/API。
- **场景：** 实时配音、直播数字人、游戏对话过场、低成本企业数字员工。
- **成熟度 / 证据：** 可观看 demo、作者实测。单 A100 报告 166.01 FPS、718 MB 峰值显存；但 HDTF 跨音频 Sync_conf 为 7.59，低于 LatentSync 的 9.04，不能只看速度。
- **重要性 / 阅读：** 高 / 必读。

### 3. DA-WAM：让每条候选轨迹拥有自己的未来

- **类型 / 时间 / 主线：** 论文、GitHub 占位仓库；2026-08-19，8 月 20 日更新 v2；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2608.19085)、[GitHub](https://github.com/LeapWM/da-wam)。
- **相对上期：** 首次收录。
- **核心贡献：** V-JEPA 2.1 在线编码器为32条候选轨迹分别预测0.5秒后的 latent，评分器同时读取当前状态、动作和对应未来；安全临界 hard negative 用于区分几何相近但结果不同的轨迹。
- **Coding 接口：** 输入轨迹候选数组，输出 NC、可行驶区域、进度、TTC、舒适度和总效用，可嵌入 candidate-based planner。
- **场景：** 自动驾驶轨迹选择、机器人 MPC、安全关键互动代理。
- **成熟度 / 证据：** 论文实测；NAVSIM-v2 EPDMS 87.7，比列出的最强对比高0.2点。GitHub 目前只有“coming soon”，不可复现。
- **重要性 / 阅读：** 高 / 必读。

### 4. GigaBrain-WBC-0.5：部署时识别“不可能的动作指令”

- **类型 / 时间 / 主线：** 行为世界模型、实体机器人 demo；2026-08-18；世界模型 / 工业运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18234)、[项目页](https://shepherd1226.github.io/gigabrain-wbc-0.5/)。
- **相对上期：** 首次收录。
- **核心贡献：** 因果 Transformer 联合预测下一动作、下一状态和下一行为 latent 分布；预测分布在运行时充当可行性边界，将不合理命令收缩回学到的行为流形。
- **Coding 接口：** 上游 teleoperation、VLM 或规划器提供粗粒度动作意图，低层策略完成平衡、环境接触与跌倒恢复。
- **场景：** 人形机器人遥操作、工业搬运、具身角色控制、安全降级执行。
- **成熟度 / 证据：** 实体 demo、作者实测；报告地形交互成功率81.3%、不合理命令83.1%、跌倒恢复99.3%。未开放代码或 checkpoint。
- **重要性 / 阅读：** 高 / 必读。

### 5. SparsePR：把视频世界模型稀疏注意力变成可执行内核

- **类型 / 时间 / 主线：** 论文、GitHub、推理组件；2026-08-19；实时基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18484)、[GitHub](https://github.com/PardisTaghavi/SparsePR)、[项目页](https://pardistaghavi.github.io/SparsePR-website/)。
- **相对上期：** 首次收录。
- **核心贡献：** 先按当前 attention response 将查询与 K/V 分组，再用少量精确 probe row 拟合被稀疏计算遗漏的残差。
- **Coding 接口：** 提供统一 `sparsepr-infer` CLI、Triton/CUDA 内核和模型 adapter；已接 HunyuanVideo、Wan2.2、Cosmos-Predict2.5、Cosmos3-Nano。
- **场景：** 世界模型 serving、数字人离线生成、互动预览和批量 rollout。
- **成熟度 / 证据：** 已开源、可运行；在21.9%–26.0% executed-pair density 下报告1.48–2.61倍端到端加速。推荐 H100，许可证需结合仓库 `LICENSES` 与各上游模型条款逐项确认。
- **重要性 / 阅读：** 高 / 必读。

### 6. VA-Judger：原生音视频生成的人类偏好奖励模型

- **类型 / 时间 / 主线：** 论文、GitHub、权重、benchmark、RL LoRA；论文2026-08-19，资源于8月20日开放；数字人音视频基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18607)、[GitHub](https://github.com/ShareLab-SII/VA-Judger)。
- **相对上期：** 首次收录；8月20日开放代码和权重构成本期明确增量。
- **核心贡献：** 用人类偏好共同评估提示遵循、声音、画面、音画一致性和内容完整度，降低分别优化单项指标造成的 reward hacking。
- **Coding 接口：** 提供 Qwen3-Omni reward-model 推理、VA-Judger-Bench、LTX-2 19B RL LoRA、MS-Swift SFT 和 dimension-wise GRPO 流程。
- **场景：** 数字人音画质检、原生音视频后训练、生成内容 CI。
- **成熟度 / 证据：** 基础代码、checkpoint 和1150对 benchmark 已开放；VAPref-10K 原始训练集仍待发布。LTX-2 路径需要大型 GPU 和 gated upstream 权重。
- **重要性 / 阅读：** 高 / 必读。

### 7. CamWorldQA：专门评估相机控制世界视频

- **类型 / 时间 / 主线：** benchmark、质量评测模型；2026-08-19；世界模型评测。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18710)。
- **相对上期：** 首次收录。
- **核心贡献：** 720个生成视频覆盖6种方法、20个源视频和6类相机轨迹，并由人类给出 MOS；CWQA联合空间、时序和光流分支预测质量。
- **Coding 接口：** 可作为相机控制生成服务的无参考质量门和回归测试器。
- **场景：** 可玩视频、虚拟摄影机、互动电影镜头、空间浏览。
- **成熟度 / 证据：** 论文 benchmark；CWQA 报告 SRCC 0.7804。未发现代码和数据下载入口。
- **重要性 / 阅读：** 中高 / 必读。

### 8. Decision-Metric Alignment：latent 看起来好，不代表适合规划

- **类型 / 时间 / 主线：** 论文、诊断指标；2026-08-19；世界模型 / Coding。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18746)。
- **相对上期：** 首次收录。
- **核心贡献：** Plan-Real Spearman 与 CEM-stage Spearman 检查 latent 距离排序是否与真实代价排序一致；DA-LeWM 通过 inverse dynamics 和 goal-action head 改善用于 MPC 的几何。
- **Coding 接口：** 可嵌入 CEM/MPC 调试流水线，监测搜索越深入、latent 排序是否越偏离真实环境。
- **场景：** 机器人规划、游戏代理、自动驾驶和生成式数字孪生验收。
- **成熟度 / 证据：** 论文实测；未见代码。
- **重要性 / 阅读：** 中高 / 必读。

### 9. DyG²T：以3D Gaussian 粒子图预测对象动力学

- **类型 / 时间 / 主线：** 论文、对象动力学模型；2026-08-19；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2608.18498)。
- **相对上期：** 首次收录。
- **核心贡献：** 从原始粒子补回 key point 丢失的局部细节，以时间解耦网络放大跨帧变化，再用全局粒子图 Transformer 建模远程力传播。
- **Coding 接口：** 可作为 `observed particles → future trajectories` 仿真模块；尚无 SDK 或引擎插件。
- **场景：** 软物体/颗粒交互、机器人操作预演、物理型生成世界。
- **成熟度 / 证据：** 论文原型，无公开代码；跨对象与真实数据结果均为作者实测。
- **重要性 / 阅读：** 中高 / 可读。

### 10. USR-Drive：联合生成3D Gaussian 几何和对象框

- **类型 / 时间 / 主线：** 论文、3D场景表示；2026-08-19；世界模型 / 仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2608.19036)。
- **相对上期：** 首次收录。
- **核心贡献：** 将稠密 Gaussian 与稀疏3D box 编码成对齐 token 流，以统一 MMDiT 联合去噪；几何为检测提供度量依据，box 为动态重建提供实例约束。
- **Coding 接口：** box 可进入规则、碰撞和交通 agent；Gaussian 负责渲染和连续几何。
- **场景：** 自动驾驶数字孪生、可编辑交通仿真、合成传感器数据。
- **成熟度 / 证据：** 论文实测，nuScenes/VKitti；无代码。
- **重要性 / 阅读：** 中高 / 可读。

### 11. 精确鼓手动作：音频驱动角色开始追求可接触的末端轨迹

- **类型 / 时间 / 主线：** 论文、动作生成 demo；2026-08-19；数字人 / 动作生成。
- **官方链接：** [论文](https://arxiv.org/abs/2608.19055)、[Disney Research 补充视频](https://studios.disneyresearch.com/)。
- **相对上期：** 首次收录。
- **核心贡献：** 双目标损失分离骨骼自然度与鼓槌精度，并提出 impact-to-target distance 和 audio-motion correlation；作者声称达到厘米级鼓槌落点。
- **Coding 接口：** 可将音轨转为骨骼及鼓槌轨迹，再由 Unity/Unreal 动画系统、IK与碰撞器执行。
- **场景：** 虚拟演出、音乐游戏、数字偶像、动作合成数据。
- **成熟度 / 证据：** SCA 2026 最佳论文、视频 demo；未发现公开实现。
- **重要性 / 阅读：** 中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：** EfficientSync 证明，面向已知身份的视频改口型不一定需要重型扩散模型。复用真人纹理可以同时降低成本与身份漂移，但动作范围受已有参考帧覆盖限制。
- **3D/4D 表示：** 本日没有新的通用4D人体重建高质量新增。DyG²T 更偏对象动力学，尚未证明适用于衣物、毛发或完整人体。
- **实时交互：** EfficientSync 的166 FPS是模型帧处理速度，不包括音频识别、TTS、网络、编码和播放器缓冲；端到端对话时延仍未披露。
- **音视频与情感：** 鼓手动作说明音频驱动角色可加入明确空间目标，而非只优化视觉自然度。VA-Judger 则为联合音视频提供整体偏好奖励，但不等同于情感表达或 turn-taking。
- **工程部署：** EfficientSync 的算力指标具产品潜力，却没有代码、量化结果和消费级 GPU 测试。VA-Judger 可立即用于研究评测，但30B reward model和19B生成模型不适合轻量服务。
- **安全与身份治理：** 本周期没有新的肖像授权、声纹许可、水印或撤回机制。提升唇同步速度会同时降低深伪制作成本，生产部署仍需授权凭证、内容标记和输出审计。

## 5. 世界模型进展

- **架构与训练：** DA-WAM 和 Decision-Metric Alignment 共同说明，世界模型的 latent 不应只追求可预测，还必须保持与实际决策代价相同的排序。
- **可控/可玩生成：** 本日没有新的通用 action-conditioned 可玩视频模型。新增更集中于轨迹控制、机器人行为和代码场景。
- **长时与物理一致性：** RoomWright 用权威程序状态避免像素 rollout 漂移；DyG²T 用粒子图处理对象动力学。两者都把状态持久性放在像素生成之外。
- **空间表示：** USR-Drive 的 Gaussian + box 双流与 RoomWright 的 object/state graph 都指向“稠密外观表示 + 稀疏可编程对象状态”。
- **机器人/游戏/仿真：** GigaBrain 已有实体硬件证据；RoomWright 有模拟器机器人互动；DA-WAM 仍是 NAVSIM 离线评测。
- **实时部署与评测：** SparsePR 解决 Transformer 计算成本；CamWorldQA 解决相机运动质量；Decision-Metric Alignment 检查规划 latent。三者分别覆盖运行速度、视觉质量和决策正确性。

## 6. Coding × 新型互动作品

### 链路一：可编程、可持续运行的生成房间

**自然语言用途 → RoomWright scene/object/interaction graph → trigger–condition–effect runtime → Isaac Sim/OmniGibson 互动房间 → 机器人训练、游戏关卡、数字孪生。**

LLM负责用途和部件语义；代码负责权威状态、关节、碰撞和因果规则；传统物理引擎负责实时执行；生成模型只负责部分纹理。已有论文运行证据，但生成耗时过长且系统未开源。

### 链路二：低延迟对话数字人

**对话 agent/TTS → 音频流与参考帧缓存 → EfficientSync → WebRTC/直播播放器 → 客服、虚拟主播、互动教育。**

代码负责对话、打断、帧缓存、媒体同步和身份授权；模型仅编辑嘴部。166 FPS 为组件证据，完整双向实时链路为本报告推断。

### 链路三：每个动作先想象自己的后果

**候选轨迹生成器 → DA-WAM逐候选未来 latent → factorized scorer → 控制器执行 → 自动驾驶/机器人/游戏代理。**

代码负责生成候选、设置安全指标和最终执行；世界模型负责候选特定后果；规则系统提供碰撞、TTC和可行驶区域监督。已有 benchmark 证据，代码仍为空壳。

### 链路四：不会盲从不可能指令的具身角色

**VLM/遥操作输入 → GigaBrain行为分布检查 → 命令投影到可行行为 → 全身控制器 → 人形机器人或物理 NPC。**

上游模型负责意图，行为世界模型判断可行性，低层控制器保证平衡和接触。已有实体机器人 demo，但跨机器人泛化和开放实现不足。

### 链路五：生成内容的多层 CI

**提示与动作规范 → 视频/世界模型 → SparsePR加速生成 → CamWorldQA检查相机质量 + VA-Judger检查音画偏好 + 规则检查任务结果 → 自动重试或人工审核。**

这条链路把“生成一次”改成可观测、可回归的服务。SparsePR和VA-Judger已有代码；CamWorldQA尚未开放，因此端到端 CI 仍需自行复刻。

### 链路六：可精确击中物体的音乐角色

**音乐音频 → 精确鼓手动作模型 → 骨骼/鼓槌轨迹 → IK、碰撞和音游事件系统 → 虚拟演出与音乐游戏。**

模型负责自然动作和时序；代码负责鼓面位置、碰撞判定、分数和演出状态。论文有动作精度证据，引擎集成属于本报告推断。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | RoomWright、规则 runtime、物理引擎 | 玩家触发跨物体状态变化 | 仿真 demo | 平均约9.2小时/场景 | 生成成本、资产美术、软体物理 | [论文](https://arxiv.org/html/2608.18840v1) |
| 影视/虚拟制作 | 精确鼓手动作、IK、DCC | 音乐驱动可接触的角色表演 | 论文/demo | 未披露 | 角色重定向、手指与器械碰撞 | [论文](https://arxiv.org/abs/2608.19055) |
| 直播与电商 | EfficientSync、TTS、WebRTC | 实时改口型与多语言配音 | 组件 demo | 166 FPS、718 MB/A100；端到端未披露 | 代码未开放、打断和并发 | [论文](https://arxiv.org/html/2608.18832v1) |
| 品牌互动 | VA-Judger、音视频生成、审核 | 用户提示触发带声音视频 | 开源研究组件 | 30B reward、19B生成模型 | 成本、品牌一致性、版权 | [GitHub](https://github.com/ShareLab-SII/VA-Judger) |
| 教育培训 | RoomWright、数字人、模拟器 | 操作设备与虚拟讲师反馈 | 概念链路/局部 demo | 未披露 | 内容正确性、制作周期 | [RoomWright](https://arxiv.org/abs/2608.18840) |
| 企业数字员工 | EfficientSync、对话 agent、TTS | 语音对话与口型反馈 | 组件级原型 | 模型侧约6 ms/帧；全链未披露 | 肖像授权、SLA、知识安全 | [EfficientSync](https://arxiv.org/abs/2608.18832) |
| 陪伴与社交 | 数字人、长期记忆、互动规则 | 表情、语音与持久状态 | 概念判断 | 未披露 | 情感安全、长期一致性 | 本周期无直接部署证据 |
| 空间计算 | RoomWright、USR-Drive、Gaussian/scene graph | 查询和操纵空间对象 | 研究原型 | 未披露 | 移动端预算、动态重建 | [USR-Drive](https://arxiv.org/abs/2608.19036) |
| 机器人/仿真 | GigaBrain、DA-WAM、Isaac Sim | 候选后果预测、指令降级、物理执行 | 实体demo/benchmark | 运行频率未披露 | 安全认证、代码缺失、sim-to-real | [GigaBrain](https://shepherd1226.github.io/gigabrain-wbc-0.5/)、[DA-WAM](https://arxiv.org/abs/2608.19085) |

## 8. 可复现资源与开发者入口

| 资源 | 开放状态 / 许可证 | 硬件或成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|---|
| [SparsePR](https://github.com/PardisTaghavi/SparsePR) | 完整代码、CLI、Triton/CUDA、测试；根许可证需核验，上游模型各自受限 | Linux，推荐 H100 | 强烈建议 | 先用同一 prompt 对比 `dense` 与 `sparsepr`，记录完整生成时间和质量 |
| [VA-Judger](https://github.com/ShareLab-SII/VA-Judger) | 基础训练/推理代码、reward checkpoint、benchmark、LTX-2 LoRA；VAPref-10K 待发布 | Qwen3-Omni 30B、LTX-2 19B，多GPU更实际 | 建议有算力团队复现 | 先跑1150对 benchmark，再对10组音视频做人工一致性校验 |
| [DA-WAM](https://github.com/LeapWM/da-wam) | 仅 README“coming soon” | 论文训练为8 GPU、20 epoch | 暂不值得 | 等待模型、数据配置和NAVSIM评测脚本 |
| [EfficientSync demo](https://alunaticat.github.io/EfficientSync/index.html) | 仅项目视频，无代码/权重 | 作者测试为单 A100 | 先跟踪 | 下载 demo，独立测嘴部纹理、SyncNet和跨身份泄漏 |
| [GigaBrain 项目页](https://shepherd1226.github.io/gigabrain-wbc-0.5/) | 论文和实体视频，无代码/checkpoint | 机器人与训练成本未披露 | 等开放 | 先复刻“行为分布拒绝不合理命令”的小型仿真版本 |
| [RoomWright 论文](https://arxiv.org/html/2608.18840v1) | 方法、规则结构和评测提示公开；实现未开放 | 平均4300万 token、约9.2小时/场景 | 适合小规模复刻 | 在 Isaac Sim 手写5种 trigger–condition–effect 行为，验证命名契约 |
| [CamWorldQA](https://arxiv.org/abs/2608.18710) | 论文；数据与模型入口未见 | 训练成本未披露 | 可复刻小集 | 采集30段相机控制视频，建立人工MOS并比较光流异常 |
| [精确鼓手动作](https://arxiv.org/abs/2608.19055) | 论文和补充视频 | 未披露 | 动画团队可跟踪 | 用现有动作扩散模型增加鼓面末端距离损失 |

## 9. 系统架构与技术趋势判断

明显升温的方向：

1. **程序成为权威世界状态。** RoomWright 不让视频或网格承担全部语义，而用对象、命名部件、状态和规则保存可执行事实。
2. **预测与决策强绑定。** DA-WAM 和 Decision-Metric Alignment 都反对“预测准但规划不用”的松耦合世界模型。
3. **模型开始主动拒绝不可行动作。** GigaBrain 的行为分布既是预测结果，也是运行时命令过滤器。
4. **生成基础设施从单指标优化转向整体运行质量。** SparsePR管速度，CamWorldQA管相机体验，VA-Judger管音画整体偏好。
5. **局部编辑重新获得工程价值。** EfficientSync 表明，明确局部任务不必全部升级为重型扩散生成。

正在形成的可复用架构是：

`用户/agent 意图 → 结构化动作、对象或候选计划 → 世界模型预测局部后果 → 程序状态机保存权威状态 → 物理/规则引擎执行 → 神经渲染或数字人表现 → 速度、任务、音画和安全评测器验收`

尚未解决的问题包括：跨小时身份与状态持久性、端到端打断时延、多人互动、软体与接触物理、反事实未来缺少真实监督、模型生成代码的沙箱安全、生产并发、资产版权及肖像授权。

需要降级看待的表述：

- “实时”若只报告单 A100 模型 FPS，而不包含TTS、网络和编码；
- “开源”若 GitHub 只有 coming soon；
- “世界模型用于规划”若未来 latent 只是训练辅助、部署时被丢弃；
- “可生成完整互动场景”若平均耗时9小时且没有公开 runtime；
- “人类偏好对齐”若原始偏好数据尚未开放，只有作者 benchmark。

## 10. 论文精读候选

1. **[RoomWright](https://arxiv.org/html/2608.18840v1)**  
   值得读：代码如何成为完整互动世界表示。重点看3.2–3.4、附录D–G。与静态 Blender 脚本的差异是加入跨对象因果规则和统一 runtime。复现风险是依赖闭源大模型、平均成本极高、无代码。

2. **[EfficientSync](https://arxiv.org/html/2608.18832v1)**  
   值得读：局部纹理变形如何换取速度与身份保真。重点看 Dynamic Texture Mixer、STAR Sampling 和效率表。复现风险是无权重，且唇形同步不如最强扩散基线。

3. **[DA-WAM](https://arxiv.org/html/2608.19085v2)**  
   值得读：候选轨迹与未来 latent 的一一对应。重点看3.3–3.5和无未来/共享未来消融。复现风险是仓库为空、离线数据只有专家轨迹的真实未来。

4. **[SparsePR](https://arxiv.org/abs/2608.18484)**  
   值得读：如何把统计稀疏性转成硬件可执行路由。重点看 response-coupled partition、probe residual、不同模型端到端分解。风险是推荐 H100，模型 adapter 和内核版本敏感。

5. **[GigaBrain-WBC-0.5](https://arxiv.org/abs/2608.18234)**  
   值得读：行为世界模型如何兼任策略和安全边界。重点看联合预测目标、terrain annotation、command projection 与硬件实验。风险是无代码，训练数据和控制频率未充分披露。

## 11. 下周跟踪与可行动建议

### 持续跟踪

1. RoomWright 是否开放资产程序、interaction runtime、Isaac Sim/OmniGibson 导入器和完整生成日志。
2. EfficientSync 是否发布权重、消费级 GPU/端侧数据及流式音频接口。
3. DA-WAM 仓库何时从占位页变为可运行代码，并披露推理延迟。
4. GigaBrain 是否开放 Unitree G1 checkpoint、控制频率和安全限幅机制。
5. VA-Judger 是否发布 VAPref-10K、完整许可证及人工标注一致性。
6. SparsePR 在多会话、批处理、低端 GPU 和长 rollout 下是否仍有加速。
7. CamWorldQA 是否开放 MOS 数据，以及模型是否能跨未见世界模型泛化。
8. RoomWright 的规则 runtime 能否与 Unity、Unreal、Godot 或 WebGPU 互操作。

### 本周可动手的小实验

1. **Trigger–Condition–Effect 微型互动房间**  
   目标：验证“代码作为世界状态”的最小架构。组件：Isaac Sim、5个带关节资产、JSON/YAML规则引擎。难点：命名部件与物理 link 的稳定绑定。成功判据：开灯、旋钮调速、容器放置三类跨对象行为可重放、可测试、可由文本修改。

2. **低延迟唇同步端到端预算**  
   目标：判断166 FPS模型是否真的改善对话体验。组件：任一轻量 lipsync、流式TTS、WebRTC、时间戳日志。难点：同步音频 chunk、视频编码和首帧。成功判据：首个可用视频帧低于300 ms、连续10分钟音画漂移低于80 ms。

3. **候选专属未来 vs 共享未来**  
   目标：复刻 DA-WAM 的核心因果假设。组件：CARLA或简单2D驾驶模拟、候选轨迹生成器、轻量 latent predictor。难点：为未执行候选构造可信监督。成功判据：在几何相近但碰撞结果不同的候选上，专属未来排序准确率显著高于共享未来。

4. **世界模型生成 CI 原型**  
   目标：把速度、相机运动和音画一致性统一进自动验收。组件：SparsePR、10–20条视频提示、光流异常检测、VA-Judger、人工复核表。难点：避免自动裁判偏差。成功判据：加速后人工质量无显著下降，自动综合评分与人工排序一致率达到80%以上。
