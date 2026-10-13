# 数字人、世界模型与互动作品研究日报：2026-10-13

检索窗口：2026-10-07 至 2026-10-13。  
信息截点：2026-10-13 09:09（Asia/Shanghai）。已与 10 月 9—12 日日报去重。arXiv 本批论文多于 10 月 9 日提交、10 月 12 日进入公开列表；以下采用实际提交日期，不把列表更新时间误作论文发布日期。

## 1. 本日摘要

本周期数字人的主要新增不是更逼真的单张脸，而是两类可持续运行的动作组件：PAGE 同时约束嘴、眼、手和全身语音手势，Sonic-AMG 则把空间音频实时转换为角色反应动作。

Sonic-AMG 在 RTX A6000 上报告 184 ms 首帧延迟，并能依据生成角色当前姿态重新计算声源相对方向，说明数字角色开始从“按音频跳舞”走向“在空间中听见并反应”。但代码仍未开放，其 Unitree G1 结果也只是离线 MuJoCo rollout。

PAGE 对中文播报场景补充了 CAS 数据集和部位级 guidance，适合讲师、主播和导览角色；不过未披露帧率、显存和流式接口，不能视为实时数字人产品。

世界模型最明确的工程进展是 ForcingBooster：它将去噪预算从整帧固定分配改为 patch 级路由，作者报告减少约 50% patch 去噪步骤、端到端额外加速约 1.3 倍，并已开放 Apache-2.0 代码和部分权重。

SimVTA 与此前 Grounded-Dreamer、DreamTrue 形成同一趋势：生成模型不再自行证明未来合理，而由 3DGS 模拟器为候选动作渲染匹配视频，再用代码选择高回报、高误差样本进行后训练。

Artisan 是本日 Coding × 互动资产方向最完整的新增：设计、编码、审查三个 agent 生成可动 3D 资产，MuJoCo 自动检测碰撞和稳定性并反馈修复；代码、benchmark 和交互 viewer 均已开放。

但 Artisan 的物理结论主要来自与生成反馈相同的 MuJoCo 环境，仍需跨引擎或真实物体验证；“可进模拟器”尚不等于“工业级数字孪生资产”。

产业侧未发现本窗口内可独立核验的新客户试点或规模化部署。今日证据集中在论文实测、作者 demo 和研究代码，商业成熟度没有同步跃升。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | 空间音频驱动实时全身反应；语音手势细化到嘴、眼、手 | Sonic-AMG 有实时延迟数据但未开源；PAGE 仍是离线论文系统 | 从上期的场景 affordance、动作夸张控制，推进到在线听觉反应和局部表演质量 |
| 世界模型 | 高 | patch 级算力路由；模拟器为反事实轨迹提供视频监督 | ForcingBooster 已开源；SimVTA 仅有项目页和占位仓库 | 从“物理引擎裁决生成结果”推进到“模拟器直接参与后训练监督” |
| Coding × 互动作品 | 高 | agent 编写程序化资产、物理验证器自动反馈、benchmark 可回归 | Artisan 已具 CLI、viewer、SDK contract 和评测代码 | 相比上期的记忆与状态管理，本期新增“代码生成资产—编译—仿真—修复”闭环 |
| 实时运行时 | 中高 | 184 ms 动作首帧、patch 去噪减少约 50% | 单模块达到交互级；端到端角色与世界服务仍缺 p95、并发和恢复指标 | 算力调度由 chunk/control 级进一步下沉到空间 patch 级 |
| 安全与治理 | 中 | LETHE 主动保护照片不被 I2V 模型可靠动画化 | 仅论文与 benchmark，未见公开工具 | 从去身份评测转向发布前主动防护 |

## 3. 最值得关注的 10 个进展

### 1. Artisan：多 agent 生成可动、可验证的 3D 资产

- **类型 / 日期 / 主线：**论文、GitHub、benchmark、交互 demo；2026-10-09；Coding × 互动作品、世界模型基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13947)、[项目页](https://physicalvision.github.io/artisan/)、[Apache-2.0 代码](https://github.com/physicalvision/Artisan)、[2.97 GB benchmark](https://huggingface.co/datasets/mastercai/Artisan-Benchmark)。
- **相对上期的新变化：**首次收录；比上期 MAMHOI 的 affordance 预测更进一步，直接输出包含零件、关节、质量、材料和惯量的可运行资产。
- **核心贡献：**Design、Coding、Review agent 分工完成规划、程序化建模和独立审查；MuJoCo 检查碰撞与重力稳定性，再把失败反馈给生成流程。
- **Coding 接口：**`run.sh run` 接受文本和参考图；提供 TOML 配置、预算控制、本地 viewer、SDK model contract 和 benchmark CLI。
- **关键结果：**作者报告单关节碰撞率由 Articulate-Anything 的 31.2% 降至 1.7%，静置稳定率由 Codex 基线的 86% 提至 98%。
- **场景：**游戏道具、机器人训练资产、数字孪生、可交互商品展示、虚拟制作预演。
- **成熟度 / 证据：**已开源研究组件；论文实测与作者 demo。默认闭源模型预算约 15 美元/资产。
- **边界：**生成与主要评测共享 MuJoCo，跨 PhysX、Bullet、真实物体的可靠性未证实。
- **重要性 / 阅读：**高 / 必读。

### 2. ForcingBooster：为每个视频 patch 动态分配去噪预算

- **类型 / 日期 / 主线：**论文、GitHub、权重、推理加速；2026-10-08；世界模型、实时运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2610.12625)、[项目页](https://igl-hkust.github.io/ForcingBooster/)、[Apache-2.0 代码](https://github.com/IGL-HKUST/ForcingBooster)、[权重](https://huggingface.co/EXCAI/ForcingBooster)。
- **相对上期：**首次收录；相较 CtrlCache 的控制事件级缓存，本项在单个视频 chunk 内按空间区域调度计算。
- **核心贡献：**轻量 router 让静态背景提前停止，运动主体、新出现区域和纹理细节接受更多去噪。
- **Coding 接口：**支持 Wan 2.1 1.3B/14B、LingBot World 等后端；CLI 可覆盖模型、checkpoint、输出和 NFE 成本权重。
- **关键结果：**作者报告 patch 总去噪步骤减少约 50%，相对四步基线额外获得约 1.3× 端到端加速。
- **复现要求：**Wan 1.3B 推理可单 GPU；默认输出 81 帧、480×832、16 fps。训练配置使用八张 A800 80 GB。
- **成熟度 / 证据：**已开源、部分 checkpoint 可用；性能仍是作者实测。
- **重要性 / 阅读：**高 / 必读。

### 3. SimVTA：让模拟器为反事实驾驶轨迹生成匹配视频

- **类型 / 日期 / 主线：**论文、项目页、占位 GitHub；2026-10-09；世界模型、自动驾驶。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13781)、[项目与排行榜](https://opendrivelab.com/SimVTA/)、[GitHub](https://github.com/OpenDriveLab/SimVTA)。
- **相对上期：**首次收录；延续 Grounded-Dreamer 的模拟器裁决，但将模拟结果进一步用作视频监督。
- **核心贡献：**采样候选轨迹，在 3DGS 重建的真实场景中用 World Engine 渲染对应未来；优先选择高回报且模型误差大的未来，联合执行轨迹 GRPO 和视频 flow-matching。
- **Coding 接口：**候选轨迹、驾驶 reward、渲染预算和错误优先级均为结构化软件接口；推理时直接预测轨迹，不调用模拟器。
- **关键结果：**作者报告 NAVSIM v1 PDMS 由 86.2 提升至 91.3；NAVSIM v2 EPDMS 为 88.2；HUGSIM 闭环平均 HD-Score 为 38.8。
- **成熟度 / 证据：**论文与可视化；仓库明确将代码、checkpoint 列为 TODO，不能视为已开源。
- **重要性 / 阅读：**高 / 必读。

### 4. Sonic-AMG：从空间音频实时生成角色全身反应

- **类型 / 日期 / 主线：**论文、demo；2026-10-09；数字人、实时交互。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13300)、[项目页与动作序列](https://jingchengni.com/sonic-amg/)。
- **相对上期：**首次收录；补足数字角色对环境声音方向和事件类型的在线反应。
- **核心贡献：**分别编码声音内容和相对声源几何；因果 transformer 输出连续动作 latent，并依据生成后的 root pose 更新声源相对坐标。
- **Coding 接口：**运行时输入连续双耳音频、世界坐标声源位置和动作历史，输出下一动作块；可接 Unity/Unreal 音频事件系统和角色控制器。
- **关键结果：**作者报告 RTX A6000 首帧 184 ms，约为离线 MOSPA 的 1/59；在线空间条件下 FID 为 8.066。
- **场景：**VR 角色、恐怖游戏 NPC、空间导览、声学训练、声音驱动机器人。
- **成熟度 / 证据：**可观看 demo；代码和模型承诺录用后发布。Unitree G1 仅为离线 MuJoCo rollout。
- **重要性 / 阅读：**高 / 必读。

### 5. PAGE：嘴、眼、手与全身共同受语音约束

- **类型 / 日期 / 主线：**论文、dataset、demo；2026-10-09；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13301)、[项目页](https://aigc-explorer.github.io/PAGE/)。
- **相对上期：**首次收录；相比 TalkLikeYou 主要控制个体化口部习惯，PAGE 扩展到嘴、眼、手和身体的联合质量。
- **核心贡献：**Parts-Specific Reweighting 加强关键部位训练；音频条件 classifier 在推理时纠正音视频不一致；Differential Guidance 分开细化面部和身体。
- **Coding 接口：**从单张参考图和语音生成 portrait video；理论上可作为数字人渲染后端，但未提供正式 API、CLI 或服务封装。
- **场景：**中文虚拟主播、教育讲师、唱歌角色、营销视频。
- **成熟度 / 证据：**TCSVT 论文和视频 demo；项目页的“Code”目前指向论文，实际代码未核验到。
- **重要性 / 阅读：**中高 / 必读。

### 6. LAPT-Scale：从 2.33 万小时人类视频学习连续潜动作

- **类型 / 日期 / 主线：**论文；2026-10-09；世界模型、机器人数据基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13732)。
- **相对上期：**首次收录；为此前 latent-action 世界模型补充大规模人类视频预训练证据。
- **核心贡献：**用连续 latent action 解释相邻帧变化，并联合感知、语义和光流信号；再执行机器人 embodiment alignment 和任务后训练。
- **关键结果：**作者报告人类视频规模由 1,000 小时增至 5,000 小时时，实机成功率由 63.3% 增至 85.0%。
- **Coding 接口：**latent action 可作为人类视频、数字角色动作和机器人控制之间的中间协议；未见公开代码或权重。
- **成熟度 / 证据：**CoRL 论文实测，包含实机结果；可复现性不足。
- **重要性 / 阅读：**高 / 必读。

### 7. Dual-Stream Memory：执行进度与历史视觉证据分开存储

- **类型 / 日期 / 主线：**论文；2026-10-09；世界模型记忆、具身 agent。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13262)。
- **相对上期：**首次收录；相较 LEDGER 的对象账本和 GAMMA 的文本日志，本项直接服务 VLA 策略。
- **核心贡献：**Gated DeltaNet 将执行历史压为一个时间 token，retriever 则保留与任务相关的旧帧；一个回答“进行到哪”，另一个回答“之前看见了什么”。
- **关键结果：**作者报告 RoboMME 平均成功率由 44.51% 提至 49.22%，四项实机任务由 37.5% 提至 62.5%。
- **Coding 接口：**检索阈值、历史帧缓存和阶段状态可由运行时监控；换杯测试仍暴露对象身份追踪失败。
- **成熟度 / 证据：**论文与实机原型；未见代码。
- **重要性 / 阅读：**中高 / 必读。

### 8. EmbodiedCLBench：测试 coding agent 能否积累并重组技能

- **类型 / 日期 / 主线：**benchmark、GitHub、dataset；2026-10-09；Coding × 互动、agent 评测。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13021)、[代码](https://github.com/PKU-VaLuE-Lab/EmbodiedCLBench)、[数据](https://huggingface.co/datasets/PKU-VaLuE-Lab/EmbodiedCLBench)。
- **相对上期：**首次收录；为“持续运行的生成世界由 coding agent 管理”增加可回归评测。
- **核心贡献：**agent 先完成基础任务，再以完整轨迹或压缩 skill 解决组合式高级任务；覆盖 HermesAgent、Codex、Claude Code、OpenClaw。
- **关键结果：**零样本高级任务成功率仅约 3%—12%；基础任务经验可显著提升结果，但增加更多样本没有稳定收益。
- **Coding 接口：**提供 smoke、20% pilot、完整实验、断点续跑、并发控制、轨迹保存和报告生成脚本；默认通过兼容 API 调用模型，不要求本地 GPU。
- **成熟度 / 证据：**已开源 benchmark；论文实测，尚不是生产 agent 平台。
- **重要性 / 阅读：**高 / 必读。

### 9. LETHE：在照片发布前主动阻止未授权 I2V 动画

- **类型 / 日期 / 主线：**论文、benchmark；2026-10-09；数字人安全。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13333)。
- **相对上期：**首次收录；从 HiFD 的生成后去身份评测转向素材发布前主动防护。
- **核心贡献：**通过压缩参考图 value vector 的多样性、吸引后续帧注意力到受破坏 token，使 I2V 输出失真或偏离人物。
- **Coding 接口：**可作为图片上传、社交发布或数字资产管理系统的预处理步骤。
- **边界：**仅作者在若干现代 I2V 模型上的实验；对裁剪、重编码、截图、未知模型和打印翻拍的鲁棒性仍需验证。
- **成熟度 / 证据：**预印本研究原型；未见代码。
- **重要性 / 阅读：**中高 / 必读。

### 10. Make It Flow：让静态 3DGS 场景中的水实时循环流动

- **类型 / 日期 / 主线：**论文、demo；2026-10-09；互动场景基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2610.13860)。
- **相对上期：**首次收录；与 OuroWorld 的整体动态环境互补，聚焦高频流体效果。
- **核心贡献：**从参考图或生成结果估计二维流场，将其提升为三维 Eulerian motion field，再变形 Gaussian primitives 生成无缝循环。
- **Coding 接口：**用户提供水体 mask 和流动提示；运行时只播放预计算的 Gaussian 动画，保留自由视点和实时渲染。
- **场景：**VR 景观、游戏背景、数字展览、虚拟制片环境。
- **成熟度 / 证据：**论文与视频 demo；无代码、引擎插件或具体帧率。
- **重要性 / 阅读：**中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**PAGE 将单图加语音的驱动范围扩展至面部和手势；Sonic-AMG 则从语音语义扩展到任意环境声音和空间方向。
- **3D/4D 表示：**本日没有新的高质量人头或全身 4D avatar 表示。Artisan 和 Make It Flow 的增量分别面向可动物体和动态场景，而非人物身份建模。
- **实时交互：**Sonic-AMG 的 184 ms 首帧已进入交互级，但尚未包括蒙皮渲染、碰撞修正、网络传输和语音对话，因此不是完整数字人端到端延迟。
- **音视频与情感：**PAGE 改善局部动作和音视频一致性；Sonic-AMG 可生成遮耳、转身、躲避等环境反应。明确的情绪参数、眼神目标、倾听状态和 turn-taking 仍未统一。
- **工程部署：**PAGE 没有可调用代码；Sonic-AMG 代码 coming soon。当前最现实的方案仍是把它们输出的动作或视频接入已有角色状态机，而非直接替换商用数字人栈。
- **安全与身份治理：**LETHE 提供主动照片保护思路，但不替代授权记录、水印、生成内容标记、撤回机制和深伪检测。

结论：本周期数字人能力开始感知“声音来自哪里、角色此刻面对哪里”，但工程可用性弱于研究新颖性。没有新的正式产品或客户部署证据。

## 5. 世界模型进展

- **架构与训练：**SimVTA 使用模拟器生成动作匹配的视频监督；LAPT-Scale 从人类视频提取连续潜动作；二者都在解决真实日志只覆盖已执行动作、缺少反事实未来的问题。
- **可控 / 可玩生成：**本日没有新的通用可玩视频世界。控制增量主要来自 ForcingBooster 的算力路由，以及 Artisan 对资产关节和物理参数的显式控制。
- **长时与物理一致性：**Dual-Stream Memory 分离压缩状态和原始证据；Artisan 把碰撞、稳定性变成可执行测试。对象 ID 持久化仍是明显短板。
- **空间表示：**3DGS 继续充当“生成画面”和“可执行状态”之间的桥梁：SimVTA 用其重建驾驶场景，Make It Flow 在其上增加三维流场。
- **机器人 / 游戏 / 仿真：**机器人和自动驾驶证据最强。游戏方向更接近资产与运行时组件，没有新的完整生成式游戏产品。
- **实时部署：**ForcingBooster 已具单 GPU 推理入口，但训练成本高；Sonic-AMG 展示动作级实时性；SimVTA 没有公开推理延迟。
- **评测：**Artisan 同时测空间、碰撞、稳定性、质量和下游机器人任务；EmbodiedCLBench 衡量经验是否真的转化为组合技能。评测正从视觉相似度走向运行结果。

## 6. Coding × 新型互动作品

### 链路一：能听见空间事件并即时反应的 NPC

**游戏音频事件与声源坐标 → Sonic-AMG → 动作块 → 引擎 IK/碰撞 → 角色反应**

代码负责声源 ID、世界坐标、状态机和动作安全；模型负责生成非预制的转头、遮耳、躲避与手势；传统引擎负责骨骼、接触和导航。已有实时动作 demo，但 Unity/Unreal 插件和在线物理执行属于**本报告推断**。

### 链路二：物理验证的 agentic 资产工厂

**文本/参考图 → Design agent → Coding agent 写程序化资产 → MuJoCo 测试 → Review agent 修复 → viewer/模拟任务**

Artisan 已提供端到端代码。可用于批量生成游戏道具和机器人训练对象，但商业上线前仍需跨引擎验证、人工美术审核和许可证检查。

### 链路三：按画面难度调度 GPU 的可玩视频运行时

**键盘/相机输入 → 自回归视频模型 → ForcingBooster patch router → 选择性去噪 → 视频流**

代码负责输入事件、延迟预算和质量降级策略；router 负责给运动主体与新区域增加计算；模型负责视觉未来。核心推理已经开源，但多用户 serving、动态 batching 和 p95 延迟尚未披露。

### 链路四：模拟器监督的自动驾驶世界—动作闭环

**候选轨迹 → 规则 reward → 3DGS 模拟器渲染匹配未来 → 高回报/高误差采样 → GRPO＋视频损失 → 直接轨迹推理**

代码和模拟器持有道路规则、车辆状态和物理约束；生成模型学习视觉后果与规划表示。SimVTA 有多项 benchmark 结果，但代码、checkpoint 尚未发布。

### 链路五：持续学习的互动 agent 回归测试

**基础任务轨迹 → ICL 或 skill distillation → 高级组合任务 → EmbodiedCLBench → 保存轨迹与失败原因**

适合验证剧情 agent、机器人助理或生成式 NPC 是否真正积累经验。benchmark 已可运行；把其中技能机制迁移到商业互动角色属于**本报告推断**。

### 链路六：动态 3D 场景资产

**静态 3DGS → 水体分割和流向提示 → 三维 motion field → 循环 Gaussian 动画 → Web/VR/引擎渲染**

生成模型负责从有限视图推断流动外观，代码负责 mask、播放周期、碰撞区域和 LOD。适合景观和背景效果，不适合需要真实流体交互的玩法。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Artisan＋ForcingBooster＋引擎状态机 | 生成可动物体、按输入生成画面 | 开源研究组件 | Artisan 默认约 15 美元/资产；运行时 p95 未披露 | 跨引擎物理一致性、并发、确定性 | [Artisan](https://github.com/physicalvision/Artisan)、[ForcingBooster](https://github.com/IGL-HKUST/ForcingBooster) |
| 影视/虚拟制作 | PAGE、Make It Flow、3DGS | 语音表演与自由视点动态背景 | 论文 demo | 未披露 | 高分辨率、编辑性、资产交接 | [PAGE](https://aigc-explorer.github.io/PAGE/)、[Make It Flow](https://arxiv.org/abs/2610.13860) |
| 直播与电商 | PAGE＋现有 TTS/RTC | 中文播报、手势和局部表演 | 研究 demo | 未披露 | 双工延迟、长时身份、审核 | [论文](https://arxiv.org/abs/2610.13301) |
| 品牌互动 | Artisan＋Web 3D viewer | 用户操作生成商品或装置 | 可运行原型 | 约 15 美元/生成案例 | 品牌资产准确性、人工审核 | [交互项目页](https://physicalvision.github.io/artisan/) |
| 教育培训 | PAGE＋Sonic-AMG＋课程状态机 | 讲解、手势、对环境声音反应 | 组件级 demo | Sonic-AMG 首帧 184 ms | 事实可靠性、对话打断、无障碍 | [Sonic-AMG](https://jingchengni.com/sonic-amg/) |
| 企业数字员工 | Dual-Stream Memory＋数字人前端 | 保留执行阶段和历史证据 | 机器人原型；跨域待验证 | 未披露 | 权限、对象身份、审计 | [论文](https://arxiv.org/abs/2610.13262) |
| 陪伴与社交 | Sonic-AMG＋对话 agent | 对用户和环境声音做身体反应 | 研究 demo | 动作首帧 184 ms | 情绪安全、接触、隐私 | [项目页](https://jingchengni.com/sonic-amg/) |
| 空间计算 | Make It Flow＋3DGS＋WebXR | 自由视点动态场景 | 论文 demo | 渲染帧率未量化 | 移动端显存、交互物理 | [论文](https://arxiv.org/abs/2610.13860) |
| 机器人/仿真 | Artisan、LAPT-Scale、SimVTA | 资产生成、潜动作学习、反事实监督 | 开源研究组件 / 实机论文原型 | 训练与仿真成本未完整披露 | sim-to-real、数据许可、安全验证 | [LAPT-Scale](https://arxiv.org/abs/2610.13732)、[SimVTA](https://opendrivelab.com/SimVTA/) |

## 8. 可复现资源与开发者入口

| 资源 | 许可与要求 | 最小验证路径 | 复现建议 |
|---|---|---|---|
| [Artisan GitHub](https://github.com/physicalvision/Artisan) | Apache-2.0；Linux x86_64；需模型 API；默认每案例预算 15 美元 | 用示例微波炉运行 `run.sh run`，在本地 viewer 检查关节，再跑 collision evaluator | **优先复现**；系统链路最完整 |
| [Artisan Benchmark](https://huggingface.co/datasets/mastercai/Artisan-Benchmark) | 2.97 GB、混合许可证，商用须逐数据源核验 | 下载一个 ArtVIP 子集，比较开启/关闭物理反馈的碰撞率 | 适合验证闭环收益 |
| [ForcingBooster](https://github.com/IGL-HKUST/ForcingBooster) | Apache-2.0；FlashAttention；Wan 1.3B 推理可单 GPU | 用十条内置 prompt 生成 81 帧视频，记录 router 的 patch NFE 与总耗时 | **优先复现**；需自行测端到端速度 |
| [ForcingBooster 权重](https://huggingface.co/EXCAI/ForcingBooster) | Apache-2.0；当前模型卡信息较少 | 先验证 Wan 1.3B router；不要等待 H3/LongLive 未发布权重 | 值得复现，但需核验文件完整性 |
| [EmbodiedCLBench](https://github.com/PKU-VaLuE-Lab/EmbodiedCLBench) | 许可证未明确展示；Linux、Python 3.10、模型 API；不需本地 GPU | 先运行 dry-run 和 Stage 0 smoke，再比较 zero-shot 与 skill setting | **优先复现**；适合 coding agent 团队 |
| [SimVTA GitHub](https://github.com/OpenDriveLab/SimVTA) | 页面内容 Apache-2.0；当前只有说明和结果 | 暂只能复核排行榜、World Engine 接口和样例未来 | 等代码/checkpoint 后再投入 |
| [Sonic-AMG 动作序列](https://jingchengni.com/sonic-amg/) | 代码 coming soon | 下载公开动作序列，接一个角色 retarget/IK 管线测试 | 可先验证集成，不可复现模型 |
| [PAGE 项目页](https://aigc-explorer.github.io/PAGE/) | 网页 CC BY-SA；模型代码未开放 | 目前仅能人工比较嘴、眼、手的同步和局部质量 | 仅跟踪 |
| [EmbodiedCLBench 数据](https://huggingface.co/datasets/PKU-VaLuE-Lab/EmbodiedCLBench) | 数据条款需逐卡核验 | 跑单个基础任务与对应高级组合任务 | 适合低成本 smoke test |

## 9. 系统架构与技术趋势判断

- **明显升温：**模拟器提供反事实监督、agent 编写结构化资产、生成结果自动进入物理测试、局部动态算力调度。
- **正在成为主流的架构：**`结构化输入/状态 → 生成模型提出候选 → 代码或模拟器执行 → reward/verifier 评分 → 修复或后训练 → 权威状态回写`。
- **数字人与世界模型的汇合点：**角色动作不再只由语音时间轴决定，而开始消费世界坐标、空间音频、对象身份和执行阶段。
- **代码负责：**状态、预算、权限、约束、回放、选择和审计；**生成模型负责：**外观、动作先验和候选未来；**传统引擎负责：**碰撞、物理、骨骼、导航和确定性多人状态。
- **关键未解问题：**跨引擎物理迁移、对象 ID 持久化、端到端打断延迟、多人并发、模型升级回归、GPU 成本、资产版权和生成过程可审计性。
- **降温信号：**单纯宣称“生成更长视频”已不足以构成世界模型进展；本周期高价值工作几乎都提供了结构化控制、物理验证、运行时调度或行为 benchmark。
- **营销风险：**SimVTA 有 GitHub 不等于代码已发布；Sonic-AMG 的机器人视频不等于实机在线控制；Artisan 的 MuJoCo 通过率不等于跨引擎或真实物理可靠。
- **产业判断：**当前最容易落地的是资产生产和离线训练数据闭环，其次是单角色动作组件；端到端生成式多人世界仍处于研究阶段。

## 10. 论文精读候选

1. **[Artisan](https://arxiv.org/abs/2610.13947)**  
   值得读：少见地把 coding agent、程序化建模、物理仿真和下游机器人训练放进同一系统。重点看方法中的 agent 分工、物理反馈循环和 benchmark protocol。与 USDCraft 的差异在于增加独立 Review agent、质量/稳定性验证和完整运行入口。复现风险是闭源模型成本及 MuJoCo 内部验证偏差。

2. **[ForcingBooster](https://arxiv.org/abs/2610.12625)**  
   值得读：将视频推理调度由模型级、chunk 级下沉到 patch 级。重点看 router 监督、随机退出 DMD、NFE cost loss 和端到端 speedup 分解。复现风险是八卡训练资源，以及 patch NFE 降低不必然等比例降低 wall-clock。

3. **[SimVTA](https://arxiv.org/abs/2610.13781)**  
   值得读：清楚展示“轨迹 reward 正确但视觉未来没有监督”的问题。重点看高回报候选筛选、高误差优先分配渲染预算、共享表示更新。与 Grounded-Dreamer 的差异是模拟器画面直接成为训练目标，而不只作为过滤器。风险是代码未发布。

4. **[Sonic-AMG](https://arxiv.org/abs/2610.13300)**  
   值得读：把声音内容、声源几何和生成身体姿态组成闭环。重点看 rehearsal training、spatial feedback、在线与 GT 空间条件差异。风险是缺少公开代码、真实角色引擎和实机在线验证。

5. **[LAPT-Scale](https://arxiv.org/abs/2610.13732)**  
   值得读：提供大规模人类视频转机器人监督的明确 scaling evidence。重点看连续 latent action、三类 alignment loss 和 embodiment alignment。风险是 2.33 万小时数据、标注流程及权重均难以等规模复现。

## 11. 下周跟踪与可行动建议

### 持续跟踪

1. ForcingBooster 的 checkpoint 是否完整，单卡真实 p50/p95 加速是否接近作者的 1.3×。
2. Artisan 生成资产导入 PhysX、Bullet、Unity 或 Unreal 后，碰撞与稳定性收益是否保留。
3. SimVTA 何时开放代码、checkpoint，以及渲染预算对收益和训练成本的曲线。
4. Sonic-AMG 是否发布模型；184 ms 是否包含音频编码、动作解码和数据传输。
5. PAGE 是否真正开放 CAS 数据和推理实现，以及长时间生成中的身份与手部稳定性。
6. LAPT-Scale 的人类视频数据许可、latent action 权重和跨机器人本体迁移效果。
7. LETHE 面对裁剪、JPEG、截图、未知 I2V 模型和对抗净化时是否仍有效。
8. EmbodiedCLBench 的 skill learning 能否跨场景、跨模型版本持续复用，而不只是同一 benchmark 内压缩轨迹。

### 本周可动手的小实验

#### 实验一：Artisan 跨引擎资产烟雾测试

- **目标：**验证 MuJoCo 合格资产进入另一物理引擎后是否仍稳定。
- **组件：**Artisan、一个生成微波炉或抽屉资产、PyBullet 或 Unity PhysX。
- **难点：**关节轴、质量、碰撞 mesh 和摩擦参数映射。
- **成功判据：**随机 100 次关节与重力 rollout 中，无穿透、爆炸或明显漂移；结果与 MuJoCo 差异可量化。

#### 实验二：ForcingBooster 交互输入延迟测试

- **目标：**确认 patch 路由是否改善可玩视频的真实响应，而非只减少理论 NFE。
- **组件：**Wan 1.3B、ForcingBooster、键盘相机事件记录器、GPU profiler。
- **难点：**编译预热、显存峰值和输入转折时的质量抖动。
- **成功判据：**相同分辨率下 p95 chunk 延迟降低至少 20%，快速转向时控制一致性不低于四步基线。

#### 实验三：空间音频驱动 NPC 原型

- **目标：**验证 Sonic-AMG 类动作输出能否接入实时角色系统。
- **组件：**项目页公开动作序列、Unity/Unreal、retarget、IK、空间音频事件。
- **难点：**动作骨架映射、脚滑、碰撞和状态机抢占。
- **成功判据：**角色能在声音方向变化后 300 ms 内开始可辨识反应，且连续事件切换不发生明显姿态跳变。

#### 实验四：coding agent 技能积累回归

- **目标：**检查轨迹压缩为 skill 是否优于直接塞入完整历史。
- **组件：**EmbodiedCLBench Stage 0/20% pilot、一个兼容 API 模型、Codex 或另一 harness。
- **难点：**API 成本、随机性、任务泄漏和失败归因。
- **成功判据：**至少三次独立运行中，skill setting 的高级任务成功率稳定高于 zero-shot，并在 token 成本低于完整 ICL 的情况下保持接近或更好的表现。
