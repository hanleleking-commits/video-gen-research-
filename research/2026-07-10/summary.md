# 数字人、世界模型与互动作品研究日报：2026-07-10

检索窗口：2026-07-04 至 2026-07-10。

## 1. 本日摘要

过去 7 天最强信号不是单一视频底座模型升级，而是“可运行的生成式互动系统”开始分化成三类：多人可玩世界模型、低成本实时推理栈、以及可脚本化的角色/主体一致性生成。世界模型侧新增最强：Multiplayer Interactive World Models 把四名玩家的 action streams 作为条件，在 Rocket League 场景中报告 20 FPS 单 B200 实时生成，并声称开放数据、训练/推理代码和 live demo，是本周期与 coding/runtime 连接最强的条目之一([arxiv.org](https://arxiv.org/abs/2607.05352))。AlayaWorld 和 MoWorld 分别从“全栈开源框架”和“NPU 实时部署”切入，但 MoWorld 项目页仍显示代码和体验 Coming Soon，因此成熟度应低于已放代码的项目([arxiv.org](https://arxiv.org/abs/2607.06291))([moxin-tech.github.io](https://moxin-tech.github.io/moworld/))。数字人方向没有看到新的强 talking-head 基座发布，高质量新增主要是 Eurographics 2026 数字人 STAR 综述，以及 Aura、SparseCtrl-HOI 这类“可控人物/人-物交互视频”对虚拟形象生产链的补强([arxiv.org](https://arxiv.org/abs/2607.04341))([arxiv.org](https://arxiv.org/abs/2607.04311))([arxiv.org](https://arxiv.org/abs/2607.05994))。Coding × 互动作品的关键变化是：模型输入正在从自然语言 prompt 变成 JSON schema、action stream、关键帧、点云 skeleton、状态机和引擎接口，代码负责持续状态、约束、事件与审核，生成模型负责视觉/音频/动作补全。产业上最接近落地的仍是可控短视频、电商人货互动、驾驶/机器人仿真和云游戏式可玩视频；完整开放世界和实时数字人直播仍多处在研究原型或 demo 阶段。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | 数字人 STAR 综述系统整理 avatar prior、个性化建模、动画三段式；SparseCtrl-HOI 将人-物交互生成直接指向直播电商 | 研究综述 + 可运行项目页，缺少新 talking-head 产品级发布 | 上期未重点覆盖数字人综述；Aura/SparseCtrl-HOI 从视频生成条目扩展到虚拟形象链路 |
| 世界模型 | 高 | Multiplayer WM、AlayaWorld、MoWorld、Point as Skeleton 同期出现，覆盖游戏、开放世界、实时推理、自动驾驶闭环仿真 | 从“离线 rollout 论文”转向“代码/框架/接口/部署声称” | 上期已覆盖 MoWorld/PAS/AlayaWorld；本报提升 Multiplayer WM 的互动系统优先级 |
| Coding × 互动作品 | 高 | action stream、JSON 推理样本、nuPlan 接口、NPU/云游戏体验成为关键工程接口 | 可运行证据分化：Aura/PAS 有 GitHub；MoWorld 代码未放；AlayaWorld 需继续核验仓库完整性 | 从“模型能力”分析推进到“代码编排持续运行世界”的架构分析 |

## 3. 最值得关注的 11 个进展

| 进展 | 类型 / 时间 | 主线 | 相对上期的新变化 | 核心贡献与 coding 接口 | 成熟度 / 证据 / 优先级 |
|---|---|---|---|---|---|
| [Multiplayer Interactive World Models with Representation Autoencoders](https://arxiv.org/abs/2607.05352) | 论文 / 数据代码 demo 声称；2026-07-06，v2 2026-07-07 | 世界模型 / Coding × 游戏 | 上期只在分类中提及，本报升为核心 | 四玩家 action streams 条件生成 Rocket League；5B latent diffusion；报告单 B200 20 FPS、最长评估 5 分钟稳定，并声称释放数据、训练/推理代码和 live demo([arxiv.org](https://arxiv.org/abs/2607.05352)) | 可运行 demo 待逐项核验 / 官方声称 / 必读 |
| [AlayaWorld](https://arxiv.org/abs/2607.06291) | 论文 / framework；2026-07-07 | 世界模型 / Coding × 互动世界 | 上期已收录；本报强调“框架化接口” | 面向可玩视频世界，统一数据准备、模型架构、训练、推理加速和部署；支持导航、战斗、施法、召唤等用户动作([arxiv.org](https://arxiv.org/abs/2607.06291)) | 研究框架 / 官方声称 / 必读 |
| [MoWorld](https://arxiv.org/abs/2607.06216) / [项目页](https://moxin-tech.github.io/moworld/) | 论文 / 项目页；2026-07-07 | 世界模型 / 实时部署 | 上期已收录；新增核验项目页显示代码和体验 Coming Soon | 3D-native 数据引擎、cross-frame pretraining、denoising-step distillation、混合精度并行推理；项目页强调 NPU、流式输出、相机控制云游戏体验([arxiv.org](https://arxiv.org/abs/2607.06216))([moxin-tech.github.io](https://moxin-tech.github.io/moworld/)) | 研究原型 / 官方声称 / 必读 |
| [Point as Skeleton](https://arxiv.org/abs/2607.06516) / [GitHub](https://github.com/krauwu/point-as-skeleton) | 论文 / GitHub；2026-07-07 | 世界模型 / 自动驾驶仿真 | 上期已收录；本报补充 GitHub 结构 | 用 ego/actor state、地图和点云 skeleton 条件做闭环驾驶视频生成；GitHub 提供 DWM/PAS generation stack 与 nuPlan scene interface([arxiv.org](https://arxiv.org/abs/2607.06516))([github.com](https://github.com/krauwu/point-as-skeleton)) | 已开源代码骨架 / 官方声称 / 必读 |
| [Aura](https://arxiv.org/abs/2607.04311) / [GitHub](https://github.com/Camellia997/Aura) | 论文 / GitHub / 权重；2026-07-05，v2 2026-07-07 | 数字人 / 可控视频 / Coding × 创作工具 | 上期已收录；本报强调 JSON 与结构化导演脚本 | VLM-grounded semantic alignment 绑定人物、物体、场景参考；GitHub 提供 CUDA/PyTorch 安装、权重下载、JSON 推理样本 schema 和结构化 prompt 标签([arxiv.org](https://arxiv.org/abs/2607.04311))([github.com](https://github.com/Camellia997/Aura)) | 已开源推理包 / 官方实验 / 必读 |
| [SparseCtrl-HOI](https://arxiv.org/abs/2607.05994) / [项目页](https://mpi-lab.github.io/SparseCtrl-HOI/) | 论文 / dataset / code；2026-07-07 | 数字人 / 人-物交互 / 电商 | 上期已收录；本报放入数字人互动生产链 | 少量关键帧控制人手与物体交互；Qwen2.5-VL 提取 motion prior，LoRA + Motion Cross-Attention 注入 Wan2.1-DiT；项目页明确面向直播电商视频([arxiv.org](https://arxiv.org/abs/2607.05994))([mpi-lab.github.io](https://mpi-lab.github.io/SparseCtrl-HOI)) | 代码链接已给 / 官方声称 / 必读 |
| [MobileWan](https://arxiv.org/abs/2607.06173) | 论文 / 项目页；2026-07-07 | 支撑基础设施 / 端侧视频 | 上期已收录；本报强调互动 runtime 价值 | 将 Wan2.2-5B 通过 recurrent reformulation、causal linear attention、head pruning、step distillation、VAE decoding 优化部署到商用移动设备；报告 5 秒 480×832、16 FPS、20 秒端到端延迟([arxiv.org](https://arxiv.org/abs/2607.06173)) | 研究原型 / 作者报告 / 必读 |
| [Flowley](https://arxiv.org/abs/2607.06405) | 论文；2026-07-07，ECCV 2026 | 音视频 / 数字人支撑 | 上期已收录；本报强调虚拟角色音效链路 | 单阶段 video-to-audio，用 Progressive Soft-masked Cross-Attention 做同步，SoundCap 生成 sound-aware captions([arxiv.org](https://arxiv.org/abs/2607.06405)) | 论文实测 / 作者报告 / 可读 |
| [How to Build Digital Humans?](https://arxiv.org/abs/2607.04341) | Eurographics 2026 STAR；2026-07-05 | 数字人 / 3D avatar | 本报新增 | 系统整理 3D human avatar 三段式：prior learning、personalized avatar、animation，并按身体部位、priors、表示法分类([arxiv.org](https://arxiv.org/abs/2607.04341)) | 综述 / 学术报告 / 必读 |
| [CrashTwin](https://arxiv.org/abs/2606.28757) | benchmark；v2 2026-07-07 | 世界模型评测 | 上期已收录；本报强调工业安全门槛 | 用 25K 合成和 12K 真实碰撞序列评估多主体碰撞中的时空一致性、动量/动能守恒和 world-dynamics integrity([arxiv.org](https://arxiv.org/abs/2606.28757)) | benchmark / 作者报告 / 必读 |
| [A Definition and Roadmap for World Models](https://arxiv.org/abs/2607.06401) | roadmap；2026-07-07 | 世界模型 / 研究地图 | 上期已收录；本报用于术语统一 | 给出 world model 定义、技术维度和阶段路线图，适合统一 video generation、robotics、physical AI 讨论([arxiv.org](https://arxiv.org/abs/2607.06401)) | 观点报告 / 可读 |

## 4. 数字人 / 虚拟形象能力进展

生成与驱动：本周期没有强新 talking-head 或 lip-sync 大模型发布。实质新增来自 Aura 和 SparseCtrl-HOI：前者把人物、物体、场景参考图与结构化脚本绑定，适合角色一致短片和虚拟主播素材生成；后者将人-物互动从逐帧姿态标注降为少量关键帧，适合商品展示、手部操作教学、直播电商预热视频。

3D/4D 表示：数字人 STAR 综述的价值在于重新整理 avatar pipeline：先学人体外观/运动先验，再创建个性化 avatar，最后动画驱动。它不是新模型，但对工程团队有用，因为它把 full-body、head、hands、hair、garment 等组件化问题拆清楚，提示未来数字人系统不会只靠单一视频扩散模型解决。

实时交互：本周期数字人侧缺少可验证低延迟 talking avatar 新增。MobileWan 的意义是间接的：如果 5B 级视频扩散能进入移动端 20 秒级生成，短视频 avatar 素材生产会更便宜；但它距离实时对话数字人仍有明显延迟差距。

音视频与情感表达：Flowley 对数字人链路的价值是补“画面之外的同步声音”。虚拟角色、商品交互和可玩视频都需要脚步、碰撞、环境声与口型之外的音效同步；Flowley 把同步机制放进 attention，而不是先转文本再配音。

工程部署：Aura 最可复现，提供安装、下载脚本、权重路径和 JSON 推理样本。SparseCtrl-HOI 有项目页和 code 链接，但仍需核验仓库完整度。MoWorld 的实时体验和代码仍未开放，工业判断应降级为官方 demo/声明。

安全与身份治理：本周期没有新强水印、肖像授权或深伪检测条目。数字人进入产品仍需要授权采集、身份验证、审核、可追溯素材库和使用日志；本周期新增主要解决生成质量和控制，不解决治理闭环。

## 5. 世界模型进展

架构与训练：Multiplayer WM 使用 representation autoencoder + latent diffusion，重点解决多主体 action attribution；AlayaWorld 走全栈框架；MoWorld 强调 3D-native 数据引擎和蒸馏；Point as Skeleton 用点云 skeleton 稳定闭环驾驶 rollout。

可控/可玩生成：本周期从单人 camera/action control 扩展到多人 action streams。Multiplayer WM 是游戏侧最强信号；AlayaWorld 的动作空间更像 RPG/开放世界 demo；MoWorld 项目页把连续相机控制和云游戏体验作为应用。

长时与物理一致性：Multiplayer WM 声称短训长推、5 分钟分布质量稳定，但这仍是官方报告。CrashTwin 则提醒：视觉像真不代表物理可信，碰撞、动量、能量和时空一致性需要独立评测。

空间表示：Point as Skeleton 的点云条件和 MoWorld 的 3D-native 数据引擎都说明世界模型正在引入显式几何支架，而不是只靠视频 token 记忆。

机器人/游戏/仿真：驾驶仿真和游戏是本周期最明确落点。机器人本体控制没有强新增，但 world model roadmap 将 embodied AI / physical AI 作为路线图的一部分。

实时部署与评测：MoWorld 报告 NPU 实时，MobileWan 报告移动端视频扩散，Multiplayer WM 报告 B200 20 FPS。三者指标不能横向比较：任务、分辨率、输入条件和开放程度不同。

## 6. Coding × 新型互动作品

1. 多人 action streams → 推理服务器 → 可玩视频比赛 → 游戏原型  
Multiplayer WM 的关键接口不是 prompt，而是多玩家动作序列。代码负责读取控制器/游戏服务器状态、同步四名玩家输入、维护比赛规则和 UI；模型负责生成未来视觉观测。传统引擎仍负责碰撞真值、得分、回放和反作弊。证据：论文声称数据、训练/推理代码和 live demo，但需核验实际仓库。

2. 点云 skeleton + nuPlan 状态 → 闭环生成仿真 → 自动驾驶测试  
Point as Skeleton 的 GitHub 已把 generation stack 和 nuPlan scene interface 分开。代码负责 ego/actor state 更新、地图读取、轨迹偏移和评测；模型把点云资产、深度模板和状态条件渲染成相机观测。工业价值是用更真实的视觉闭环替代只看 CARLA 风格图形的仿真。

3. 结构化导演脚本 + 参考资产 JSON → 角色一致短片 → 虚拟主播/品牌互动  
Aura 的 schema 让开发者用 `[PERSON_1]`、`[OBJECT_1]`、`[SCENE_1]` 绑定素材。代码可生成脚本、批量替换角色/商品/场景、调用推理、做审核和发布。模型负责保持人物与物体身份一致。它适合 5 秒级镜头和短片资产，不是实时 NPC。

4. 稀疏关键帧 + 商品图 + 音频 → 人货互动视频 → 直播电商  
SparseCtrl-HOI 把密集手部/物体轨迹降为少数关键帧。代码负责商品库、关键姿态模板、时间戳、口播音频和品牌规范；模型补全手部动作与物体接触。可落地在商品讲解、教程和短视频广告，但真实直播实时互动还需要低延迟 avatar 和审核系统。

5. 视频片段 → 同步音效 → 可玩世界声画闭环 → 互动电影/训练仿真  
Flowley 可作为视频世界或 avatar 片段后的 V2A 层。代码负责事件标签、音轨混音、响度规范和版权素材过滤；模型生成语义一致、时间同步的环境音。当前是论文阶段，适合离线后处理，不宜宣称实时产品化。

6. 移动端压缩视频生成 → 本地预览 → 创作者工具  
MobileWan 的工程价值在于让创作者在手机或边缘设备上快速预览 5 秒视频。代码负责 prompt/资产管理、设备调度、缓存和上传；模型负责本地生成。它能降低 UGC 工具试错成本，但 20 秒延迟仍不适合逐帧互动游戏。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟指标 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|---|
| 游戏 | Multiplayer WM / AlayaWorld | action stream、latent diffusion、推理服务器、游戏规则层 | 玩家输入驱动下一段视觉观测 | demo/研究框架 | 20 FPS 单 B200为作者报告；AlayaWorld未披露 | 规则一致性、延迟、可控性、反作弊 | ([arxiv.org](https://arxiv.org/abs/2607.05352))([arxiv.org](https://arxiv.org/abs/2607.06291)) |
| 影视/虚拟制作 | Aura / MoWorld | 结构化导演脚本、参考图、相机控制、批量渲染 | 导演脚本控制镜头与角色 | Aura 可复现；MoWorld demo 声称 | Aura未披露成本；MoWorld声称低成本 | 多镜头一致、版权、可编辑性 | ([github.com](https://github.com/Camellia997/Aura))([moxin-tech.github.io](https://moxin-tech.github.io/moworld/)) |
| 直播与电商 | SparseCtrl-HOI | 商品图、关键帧、音频、Wan2.1-DiT、Qwen2.5-VL | 少量关键帧定义人-物交互 | 研究原型 | 未披露 | 手部接触真实度、商品合规、实时性 | ([mpi-lab.github.io](https://mpi-lab.github.io/SparseCtrl-HOI)) |
| 教育培训 | Aura + Flowley | 讲师形象、脚本、字幕、V2A | 生成讲解片段和同步音效 | 离线内容生产 | 未披露 | 身份授权、口型/声音一致、审核 | ([arxiv.org](https://arxiv.org/abs/2607.04311))([arxiv.org](https://arxiv.org/abs/2607.06405)) |
| 企业数字员工 | 数字人 STAR / Aura | avatar pipeline、知识库、TTS/ASR、视频生成 | 预生成或半实时回答视频 | 概念到产品需外部系统 | 未披露 | 低延迟、turn-taking、责任边界 | ([arxiv.org](https://arxiv.org/abs/2607.04341)) |
| 空间计算 | MoWorld | NPU/边缘推理、相机控制、空间重建 | 连续漫游生成世界 | 官方 demo 声称 | 50 FPS为作者/项目声明 | 代码未开放、空间一致性 | ([arxiv.org](https://arxiv.org/abs/2607.06216))([moxin-tech.github.io](https://moxin-tech.github.io/moworld/)) |
| 机器人/自动驾驶 | Point as Skeleton / CrashTwin | nuPlan、点云资产、闭环状态、物理 benchmark | agent 状态驱动视觉仿真 | 研究代码 + benchmark | 未披露 | 物理可信、域迁移、评测标准 | ([arxiv.org](https://arxiv.org/abs/2607.06516))([arxiv.org](https://arxiv.org/abs/2606.28757)) |

## 8. 可复现资源与开发者入口

- [Camellia997/Aura](https://github.com/Camellia997/Aura)：Apache-2.0；值得复现。最小路径是跑 `jobs/install.sh`、`jobs/download.sh`，再用 `metafiles/validation/inference-samples.json` 验证单个多参考样本；硬件需要 CUDA 12.4、PyTorch 2.5、较大显存或 CPU offload。
- [krauwu/point-as-skeleton](https://github.com/krauwu/point-as-skeleton)：值得跟踪。最小路径是先阅读 `src/dwm` 和 `nuplan` 两部分，下载预处理点云资产与 OpenDWM/nuPlan checkpoint；复现成本高，依赖 nuScenes/nuPlan 数据。
- [SparseCtrl-HOI 项目页](https://mpi-lab.github.io/SparseCtrl-HOI/)：可跟踪 code 与 SparseHOI-5K。最小验证是用少量关键帧和商品图生成短片，检查手-物接触和时序。
- [MobileWan](https://arxiv.org/abs/2607.06173)：目前以论文/项目页为主，是否开放移动端部署代码需继续跟踪。适合先复现方法拆解，不适合作为本周工程依赖。
- [MoWorld 项目页](https://moxin-tech.github.io/moworld/)：代码和体验显示 Coming Soon；只适合技术跟踪，不适合复现计划。
- [CrashTwin](https://arxiv.org/abs/2606.28757)：适合做世界模型物理评测候选，关键是核验数据和评测工具是否开放。
- [Flowley](https://arxiv.org/abs/2607.06405)：未确认官方代码；可先精读同步机制和 SoundCap pipeline。

## 9. 系统架构与技术趋势判断

明显升温：多人 world model、闭环驾驶仿真、结构化导演脚本、稀疏关键帧控制、端侧/边缘视频生成。  
正在成为主流：显式状态或结构条件 + 生成模型渲染。具体表现为 action stream、点云 skeleton、参考图 ID、JSON schema、prompt bank、关键帧时间戳。  
可复用架构正在成形：LLM/agent 负责编剧和任务规划，代码运行时负责状态机、输入同步、规则和审核，世界模型负责未来观测，数字人模型负责角色表演，传统引擎负责确定性物理、UI、网络同步和日志。  
仍未解决：实时数字人低延迟、长时身份漂移、多人交互物理一致性、生成世界的可编辑状态、工业审核和版权追溯。  
需要降级处理的项目：只展示视频 demo、没有代码/权重/评测的“实时世界模型”；只包装 prompt expansion 的导演系统；只报告主观质量、没有第三方复现的产品化声称。

## 10. 论文精读候选

1. [Multiplayer Interactive World Models](https://arxiv.org/abs/2607.05352)：重点读 representation autoencoder、multiplayer conditioning、实时推理和物理评测；复现风险是代码/demo 实际开放程度与 B200 成本。
2. [AlayaWorld](https://arxiv.org/abs/2607.06291)：重点读模块化框架、deployment、evaluation tools；复现风险是仓库、权重和文档完整度。
3. [Point as Skeleton](https://arxiv.org/abs/2607.06516)：重点读 Reset-and-Roll、点云 skeleton 条件、nuPlan 接口；复现风险是数据资产和闭环评测配置复杂。
4. [Aura](https://arxiv.org/abs/2607.04311)：重点读 VLM-DiT 两阶段对齐、RoPE-Shift、Memory Tokens 和 JSON 推理流程；风险是 14B 级模型显存与多参考质量稳定性。
5. [How to Build Digital Humans?](https://arxiv.org/abs/2607.04341)：重点读 taxonomy、prior learning、personalized avatar creation；它不是复现论文，但适合建立数字人技术地图。

## 11. 下周跟踪与可行动建议

继续追踪：Multiplayer WM 是否真正放出完整代码、数据和 live demo；AlayaWorld 是否公开仓库和可复现实例；MoWorld 代码/体验何时上线；Aura 第三方复现质量和显存门槛；SparseCtrl-HOI 数据集许可证与商用限制；CrashTwin 是否开放评测工具；MobileWan 是否有真实移动端 demo；数字人方向是否出现新的 streaming talking avatar 开源更新。

小实验 1：用 Aura 做“同一虚拟角色 + 两个商品 + 三个场景”的 5 秒镜头批生成。成功判据：角色脸部和商品外观在 10 条样本中稳定，JSON schema 能被脚本自动替换。

小实验 2：用 Point as Skeleton 跑一个 nuPlan 偏航轨迹样例。成功判据：用户定义轨迹能驱动画面变化，生成结果没有快速崩坏。

小实验 3：复现 SparseCtrl-HOI 的 3 关键帧商品操作。成功判据：手部接触、物体位置和动作阶段与关键帧一致，且不需要逐帧姿态标注。

小实验 4：搭建“规则引擎 + 生成模型”的互动 demo 架构草图。规则引擎维护状态和事件，Aura/视频模型生成镜头，Flowley 生成音效；成功判据是每个生成调用都有明确输入 schema、缓存策略和失败回退。
