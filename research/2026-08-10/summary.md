# 数字人、世界模型与互动作品研究日报：2026-08-10

检索窗口：2026-08-04 至 2026-08-10。  
检索截止：2026-08-10 10:08（UTC+8）。8 月 8—10 日为周末至周一早间，arXiv 最近一次正式批次为 8 月 7 日；因此今日高质量新增主要是 8 月 6 日提交、8 月 7 日公开的论文，以及随后开放的代码、权重和 ComfyUI 集成。

## 1. 本日摘要

本周期最重要的工程信号不是单纯画质提升，而是生成模型开始接受软件系统熟悉的状态、事件、条件窗口和缓存协议。数字人侧，Wan-Animate-2 同时开放 14B 权重、推理代码、Diffusers 管线，并在一天内进入 ComfyUI 主线，使人物动画从论文 demo 迅速变成可编排节点；但论文中的“实时 Lite”与公开的重型 14B 离线版本仍需严格区分。Vorch-Streamer 则把原生语音—视频数字人推进到因果流式生成，以语言模型产生 25 Hz speech-planning token，作者报告 27.12 FPS，是本周期实时性最强的研究信号，但代码未开放。世界模型侧，MASS 借鉴多人游戏服务器，以唯一权威 typed state 分离世界逻辑与按需视图渲染，首次把多人一致性、并发和长时同步放在世界模型架构中心。UA-NWM 进一步显示，工业控制未必需要生成大量像素未来：以 DINO latent、单次前向和不确定性子空间即可完成低延迟无人机轨迹评分，并已开放代码、数据、权重和部署适配。GAUGE 则揭示现阶段世界模型“运动形态看似正确、物理参数却错误”，物理可玩性仍不能由视觉质量代替。Coding 的角色因此更清楚：维护权威状态、事件时序、权限、缓存、回滚和安全约束；模型负责外观、语义动作、感情场与不确定未来；传统引擎和控制器负责确定性执行。本周期没有新增的正式客户部署、SLA 或规模化并发案例，产业证据仍集中在开源研究栈和作者实机实验。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | Wan-Animate-2 开放权重、代码并合入 ComfyUI；Vorch-Streamer 声称原生音视频 27.12 FPS | 从项目 demo 推进到可执行节点与 Diffusers API；真正实时模型仍未开源 | 上期 EmpaAva 强在多 worker 编排，本期补上更强的生成器与创作工具入口 |
| 世界模型 | 高 | MASS 引入权威共享状态；UA-NWM 提供低延迟不确定性规划；GAUGE量化物理误差 | 出现完整代码/数据/部署适配，但多人世界与物理评测代码仍缺失 | 从单角色事件和反事实控制，推进到多人同步、并发和实机低延迟规划 |
| Coding × 互动作品 | 高 | ComfyUI 状态窗口、K/V 缓存、LLM speech token、typed state 和双层 worker 架构 | 可立即复现的开发入口显著增加 | 相比上期 JSON worker，本期进一步进入节点图、缓存策略、服务端权威状态和异步长期记忆 |

## 3. 最值得关注的 12 个进展

### 1. Wan-Animate-2：开放权重，并在一天内进入 ComfyUI 主线

- **类型 / 时间 / 主线：** 模型、论文、GitHub、权重、节点集成；论文 2026-08-06 提交，代码与权重 08-07 发布；数字人、Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2608.06009)、[项目页](https://humanaigc.github.io/wan-animate-2/)、[GitHub](https://github.com/Wan-Video/Wan-Animate-2)、[ComfyUI 合并记录](https://github.com/Comfy-Org/ComfyUI/pull/15362)。
- **相对上期的新变化：** 全新条目；相较 EmpaAva 的组件编排，本项目提供更强的端到端角色动画生成器和成熟节点入口。
- **核心贡献：** 直接把驱动视频送入双分支 DiT，避免显式姿态提取误差；支持多人驱动和视角解耦。公开蒸馏版为 10 步、无 CFG。
- **Coding 接口：** Python CLI、`WanAnimate2Pipeline`、Gradio；ComfyUI 新增 `WanAnimate2ToVideo` 和 `WanAnimate2Cache`，支持条件强度、起止窗口、动作续接、帧偏移、上下文窗口以及 RAM/VRAM、int8/int4 缓存。
- **互动/工业场景：** 摄像头驱动的虚拟主播、直播换角、角色预演、短视频批量制作。
- **成熟度 / 证据：** Apache-2.0、代码和 14B 权重已开放；默认 720p 配置需 8×A800，480p 测试需 2×A800。论文所称实时 Lite 未作为独立轻量权重开放，不能将公开版本称为低成本实时产品。
- **重要性 / 优先级：** 高 / 必读。

### 2. Vorch-Streamer：以语言模型规划 token 驱动原生音视频流

- **类型 / 时间 / 主线：** 论文、项目 demo；2026-08-06；数字人、实时运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05663)、[项目页](https://vorch-project.github.io/Vorch-Streamer-project/)。
- **新变化 / 技术：** 以混合 Teacher/Diffusion Forcing、长时 Self-Forcing 和四步 DMD 蒸馏，将 LTX2.3 改造成因果块式 T2AV 模型；外部 LLM 输出 25 Hz speech-planning token，决定每个生成块此刻应说什么。
- **Coding 接口：** 最自然的协议是“文本计划 → 定时 speech token → 因果 A/V block”；可被对话 agent 的取消、插话和轮次调度器控制，但官方尚未发布 API 或代码。
- **场景 / 成熟度 / 证据：** 虚拟主持、商品讲解、访谈和对话角色；作者 demo。作者报告 22.8B 模型达到 27.12 FPS、WER 7.92%，但硬件、首帧延迟和多人并发未披露。
- **重要性 / 优先级：** 高 / 必读。

### 3. MASS：多人世界模型采用唯一权威共享状态

- **类型 / 时间 / 主线：** 论文、世界模型架构；2026-08-06；世界模型、Coding × 多人互动。
- **官方链接：** [论文](https://arxiv.org/abs/2608.06257)。
- **新变化 / 技术：** Logic Engine 根据联合动作推进唯一 typed state，Rendering Engine 再从该状态按相机请求生成不同玩家视图，避免各视图分别携带世界记忆。
- **Coding 接口：** 可映射为服务器权威 tick：`joint actions → typed state transition → per-client camera render`，天然兼容事件日志、重放、冲突解决和反作弊。
- **场景 / 成熟度 / 证据：** 多人生成式游戏、协作训练场、共享虚拟空间。作者在 Snake benchmark 上报告 1,024 个并发玩家、10,000 个递归步骤；无代码、写实场景或网络抖动实验。
- **重要性 / 优先级：** 高 / 必读。

### 4. UA-NWM：单次前向的不确定性世界模型进入无人机闭环

- **类型 / 时间 / 主线：** 论文、GitHub、权重、dataset、实机 demo；2026-08-06；世界模型、机器人/仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05597)、[项目页](https://duryi.github.io/UA-NWM-Project-Page/)、[代码](https://github.com/DurYi/UA-NWM)、[AirGoal-10k](https://huggingface.co/datasets/DurYi/AirGoal-10k)。
- **技术 / Coding：** action-conditioned Transformer 预测 DINOv3 latent 和不确定性子空间，只以无法被子空间解释的目标差异评分；仓库覆盖训练、CEM/MPC、评测、可视化和部署 server adapter。
- **场景 / 成熟度 / 证据：** 已开源研究栈＋作者实机；项目页报告 32 候选时 8.47 ms/帧、AirSim 闭环 76.0% SR，并在 MacBook Air 连接自制四旋翼上演示零样本 sim-to-real。数据集 10.9 GB，许可证标为“other”，商用前需核查上游数据条款。
- **重要性 / 优先级：** 高 / 必读。

### 5. Vorch 系列：一个基础模型分化出编排、长故事和身份替换能力

- **类型 / 时间 / 主线：** 三篇论文、项目 demo；2026-08-06；数字人、视频基础设施、互动叙事。
- **官方链接：** [Vorch-Omni](https://arxiv.org/abs/2608.05803)、[Vorch-Director](https://arxiv.org/abs/2608.05776)、[Vorch-IR](https://arxiv.org/abs/2608.05648)。
- **技术 / Coding：** Omni 用 `TaskConfig(video,audio,image,text)`、condition mask、task ID 和 position type 统一十余种任务；Director 用带噪声级标签的 residual 和一秒 clean sink 延长多镜头故事；IR 由指令指定多人身份与背景参考。
- **场景 / 成熟度 / 证据：** agent 可把剧本拆成镜头、角色和 A/V 条件，模型持续生成或替换身份；项目页有分钟级 demo，但三个项目代码均为 Coming Soon。
- **重要性 / 优先级：** 高 / 必读。

### 6. GAUGE：把“物理上像”变成可测的参数误差

- **类型 / 时间 / 主线：** benchmark、论文；2026-08-06；世界模型、评测。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05948)。
- **技术 / Coding：** 22 类任务覆盖刚体、绳索、织物和体积形变；用真实轨迹、物理元数据、观测量和不确定性同时测试 Isaac Sim、Genesis、Newton 及六个 I2V 模型。
- **场景 / 成熟度 / 证据：** 可作为仿真和生成世界的发布门：碰撞、摩擦、动量、振荡和形变误差超阈值即拒绝。论文发现视频可呈现正确方程形态，却恢复出错误加速度和动量；暂无确认的公共工具链。
- **重要性 / 优先级：** 高 / 必读。

### 7. Robust-WAM：让视频生成预训练服务机器人动作，而不被外观变化绑架

- **类型 / 时间 / 主线：** 论文、实机实验；08-06 提交、08-07 修订；世界模型、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05903)。
- **技术 / Coding：** 保留 VAE latent 的生成路径，在动作流加入带时间位置编码的 query token，与未来帧语义表征对齐；适合以 post-training adapter 接入已有 WAM。
- **场景 / 成熟度 / 证据：** 光照和外观变化下的机器人控制；作者仿真及实机结果，无代码、时延或安全控制接口。
- **重要性 / 优先级：** 高 / 必读。

### 8. PhyLatent：JEPA 不塌缩，不代表保留了物理因果

- **类型 / 时间 / 主线：** 论文、训练目标；2026-08-06；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05720)。
- **技术 / Coding：** 针对物理不变性、可辨识性和反事实动力学三种 collapse，加入物理状态 grounding、反事实分支分离和 latent 去噪。适合作为 world-model CI 的表征级测试。
- **成熟度 / 证据：** 作者报告 OGBench-Cube MPC 成功率由 70.0% 升至 78.1%，TwoRooms 由 81.0% 升至 98.0%；未发现代码。
- **重要性 / 优先级：** 高 / 必读。

### 9. StreamArena / StreamMind：互动角色需要前台实时 worker 与后台长期记忆分离

- **类型 / 时间 / 主线：** benchmark、agent 架构；2026-08-06；Coding × 互动运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05703)。
- **技术 / Coding：** 243 个平均 88.8 分钟的视频、3,646 个开放问答；StreamMind 将主动监测和低延迟响应交给独立前台 worker，后台异步构建多模态记忆、检索历史并调用外部搜索。
- **场景 / 成熟度 / 证据：** 长时虚拟陪伴、直播助手、监控与培训角色；论文原型。它不生成数字人，但补上了长时运行时的“感知与记忆侧”。
- **重要性 / 优先级：** 中高 / 必读。

### 10. EffectLearner / EffectWorld：删除对象时同时推理其物理后果

- **类型 / 时间 / 主线：** 论文、GitHub、dataset、Unreal 数据管线；2026-08-06；支撑基础设施、Coding × 虚拟制作。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05565)、[代码](https://github.com/MorleyOlsen/EffectLearner-Official)、[项目页](https://morleyolsen.github.io/EffectLearner/)、[数据集](https://huggingface.co/datasets/jenniferwuuu/EffectWorld)。
- **技术 / Coding：** VLM 用结构化 prompt 推理目标引起的阴影、反射、形变或远端效应，DiT 再完成视频擦除；EffectWorld 由 Unreal Engine 生成对齐 source/target/mask 三元组。
- **成熟度 / 证据：** 代码和数据入口已开放，适合虚拟制作清理、事故重演和合成数据；尚无实时编辑或 DCC 插件。
- **重要性 / 优先级：** 中高 / 可读。

### 11. EmoWorld：把氛围、语义情绪线索和时间变化拆成独立控制轴

- **类型 / 时间 / 主线：** 论文；2026-08-06；数字人表演、视频基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2608.06231)。
- **技术 / Coding：** 对冻结 Video DiT 注入 Visual、Semantic、Temporal 三类 affect field；程序可分别控制场景氛围、情绪物件和情绪转折曲线。
- **成熟度 / 证据：** 作者在 27 类情绪上测试并报告情绪对齐和转变单调性提升；无代码、人物表情 rig 或实时证据。
- **重要性 / 优先级：** 中 / 可读。

### 12. UniVVT：虚拟试衣从解析—姿态—形变流水线转向语义条件

- **类型 / 时间 / 主线：** 论文、工业研究原型；2026-08-06；数字人、直播电商。
- **官方链接：** [论文](https://arxiv.org/abs/2608.05745)。
- **技术 / Coding：** MLLM scene-task perceiver 将人物视频、服装图和任务指令压缩为 task-aware token，推理时不再依赖人体 mask、姿态和 garment warping。
- **场景 / 成熟度 / 证据：** 视频试衣、商品展示和数字模特；仅论文实测，没有代码、成本、服装物理或商品真实性保证。
- **重要性 / 优先级：** 中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** Wan-Animate-2 的关键增量是驱动视频直入 DiT、多人动画和可独立控制相机；Vorch-Streamer 则从“语音驱动画面”推进到“文本同时生成语音与人物视频”。前者已可运行，后者的实时效果仍只能通过作者 demo 验证。

**3D/4D 表示。** 本批次没有新的高质量 3DGS、NeRF、mesh 或单图 4D avatar 突破。新增主要发生在 2D 视频生成与流式音视频，不能以人物视频 demo 替代真正空间化资产。

**实时交互。** Vorch-Streamer 的 27.12 FPS 是明确吞吐量，但缺首帧、端到端轮次、打断和硬件指标。Wan-Animate-2 公开版需要多张 A800；其“实时 Lite”尚未形成可复现证据。

**音视频与情感。** LLM speech token 为音频、口型和语义进度提供统一时间轴；EmoWorld 又增加可脚本化的情绪曲线。仍缺少 active listening、视线反馈、可随时插话及多语言评测。

**工程部署。** 当前最成熟的是 Wan-Animate-2 的 Diffusers、CLI、ComfyUI 和缓存控制。Vorch 系列拥有清晰条件协议，却没有 SDK。生产系统仍应把 RTC、会话状态、取消、审核和降级放在生成模型之外。

**安全与身份治理。** Vorch-IR 与 Wan-Animate-2 显著降低多人身份替换门槛，但本周期没有新的授权、数字水印、C2PA 或深伪检测机制。身份引用、同意记录、输出水印和审计日志应成为默认资产管线。

## 5. 世界模型进展

- **架构与训练：** MASS 把权威 typed state 与视图生成分离；Robust-WAM 把 VAE 外观能力与语义未来对齐；PhyLatent直接约束 latent 是否保留动作因果。
- **可控/可玩：** 控制正从“相机和单角色事件”扩展到多主体联合动作及多人视图，但 MASS 当前只在 Snake 环境验证。
- **长时与物理一致性：** 10,000 步共享状态说明结构化世界可长时递归；GAUGE 同时提醒，长时稳定不等于物理参数正确。
- **空间表示：** typed state、DINO latent、不确定性子空间和物理观测量正在取代纯 RGB，成为代码可检查的中间协议。
- **机器人/仿真：** UA-NWM 的单次前向和部署 adapter 是最完整新增；Robust-WAM、PhyLatent仍是模型训练方法。
- **实时部署与评测：** UA-NWM 的 8.47 ms/帧具备在线候选评分意义；视频生成式世界仍缺可复现的端到端交互延迟。

## 6. Coding × 新型互动作品

1. **摄像头/动捕视频 → ComfyUI `WanAnimate2ToVideo` → K/V 缓存与动作续接 → 虚拟主播或角色预演。**  
   代码负责窗口、帧偏移、缓存和任务队列；模型负责身份外观与动作迁移；传统播放器/RTC 负责同步与中断。已有可运行代码，公开版不满足低成本实时。

2. **对话 agent 文本 → 25 Hz speech-planning token → Vorch 因果 A/V block → 持续说话的数字角色。**  
   agent 负责轮次、工具调用与要说的事实；规划 token 决定时间进度；生成器合成语音、口型和画面。已有作者实时 demo，代码缺失。

3. **多玩家输入 → MASS Logic Engine 权威 typed state → 按相机生成玩家视图 → 多人生成世界。**  
   服务器负责动作收集、tick、权限和重放；学习逻辑预测状态转移；渲染模型生成个性化视图。当前是研究 benchmark，不是可部署游戏服务器。

4. **剧本 agent → Vorch `TaskConfig`/角色参考/镜头列表 → Director 长故事生成 → 互动电影分支。**  
   代码保存角色 ID、剧情图、镜头版本和分支条件；模型负责跨镜头音视频；传统编辑系统负责确定性时间线。仅项目 demo。

5. **无人机观测＋候选轨迹 → UA-NWM 单次 latent 评分 → CEM/MPC → 安全控制器执行首个 waypoint。**  
   世界模型只负责候选排序，传统控制器保留飞行边界和急停。代码、数据和部署 adapter 已开放，是本周期证据最完整的工业链路。

6. **真实轨迹 → GAUGE 物理观测量 → 自动回归测试 → 仿真或世界模型发布门。**  
   CI 可对碰撞、动量、振荡和形变分别设阈值，避免“视觉很好看”掩盖物理错误。链路为本报告推断，benchmark 工具尚未确认开放。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | MASS＋权威服务器＋生成渲染 | 多人联合动作、独立视角 | 论文原型 | 1,024 玩家/10,000 步；耗时未披露 | 写实扩展、网络延迟、确定性 | [论文](https://arxiv.org/abs/2608.06257) |
| 影视/虚拟制作 | Vorch-Director/IR＋镜头图 | 剧本分支、多人身份替换 | 作者 demo | 未披露 | 代码、精修、版权 | [项目页](https://vorch-project.github.io/Vorch-Director-project/) |
| 直播与电商 | Vorch-Streamer＋RTC | 文本持续生成同步音视频 | 作者 demo | 27.12 FPS；硬件未披露 | 首帧、插话、并发、审核 | [项目页](https://vorch-project.github.io/Vorch-Streamer-project/) |
| 品牌互动 | Wan-Animate-2＋ComfyUI | 用户视频驱动品牌角色 | 已开源原型 | 720p 8×A800；480p 2×A800 | 成本、授权、实时性 | [GitHub](https://github.com/Wan-Video/Wan-Animate-2) |
| 教育培训 | StreamMind＋数字人前端 | 主动监测、历史追问 | 论文原型 | 查询延迟具体值未披露 | 隐私、长期记忆误差 | [论文](https://arxiv.org/abs/2608.05703) |
| 企业数字员工 | Vorch-Streamer＋业务 agent | 工具结果转为连续 A/V 回复 | 机会判断 | 未披露 | SLA、事实性、取消与回退 | [论文](https://arxiv.org/abs/2608.05663) |
| 陪伴与社交 | StreamMind＋EmoWorld＋avatar | 长时记忆与情绪演进 | 机会判断 | 未披露 | 心理安全、隐私、依赖风险 | [EmoWorld](https://arxiv.org/abs/2608.06231) |
| 空间计算 | Wan-Animate-2 多视角＋传统引擎 | 独立相机观察角色动作 | 研究 demo | 未披露 | 缺 3D 几何、立体一致性 | [项目页](https://humanaigc.github.io/wan-animate-2/) |
| 机器人/仿真 | UA-NWM＋CEM/MPC＋UAV | 每步重规划并执行首动作 | 开源＋作者实机 | 8.47 ms/帧 | 安全认证、天气和传感失效 | [项目页](https://duryi.github.io/UA-NWM-Project-Page/) |

## 8. 可复现资源与开发者入口

- [Wan-Animate-2](https://github.com/Wan-Video/Wan-Animate-2)：Apache-2.0，含 Base、10 步蒸馏推理、Diffusers 和 Gradio。最小路径是使用官方 demo 素材运行蒸馏版；资源门槛很高，先不要承诺实时。
- [ComfyUI Wan-Animate-2](https://github.com/Comfy-Org/ComfyUI/pull/15362)：已于 08-07 合并；最小验证是比较缓存开关的耗时、显存及动作一致性，并测试 int8/int4 缓存误差。
- [UA-NWM](https://github.com/DurYi/UA-NWM)：MIT；训练、评测、CEM/MPC 和部署代码齐全。最小路径是下载 checkpoint 与 AirGoal-10k 测试集，对 32 个候选比较 HEP 和 cosine baseline。
- [AirGoal-10k](https://huggingface.co/datasets/DurYi/AirGoal-10k)：11,000 条轨迹、10.9 GB；数据许可证为“other”，研究可复现性高，商业使用需核查来源。
- [EffectLearner](https://github.com/MorleyOlsen/EffectLearner-Official) 与 [EffectWorld](https://huggingface.co/datasets/jenniferwuuu/EffectWorld)：适合验证结构化 object-effect prompt 和 Unreal 配对数据管线。
- Vorch-Streamer、Vorch-Omni、Vorch-Director、Vorch-IR 均只有论文和项目 demo，代码为 Coming Soon；暂不投入完整集成。
- MASS、GAUGE、Robust-WAM、PhyLatent、EmoWorld、UniVVT 未确认公共代码或权重，当前适合精读和接口设计。

## 9. 系统架构与技术趋势判断

正在升温的通用架构是：

`用户/agent 意图 → 权威状态与时间化计划 → 生成/预测模型 → 物理或规则校验 → 按需渲染/执行 → 持久日志与记忆`

数字人侧的时间协议是 speech token、动作窗口和 frame offset；世界模型侧是 typed state、joint action、DINO latent 和不确定性子空间；创作工具侧是 ComfyUI 节点、缓存及 `TaskConfig`。这说明“coding 生成一次内容”正在转向“coding 维护一个持续运行、可观测和可回滚的生成系统”。

主流分工逐渐稳定：代码持有身份、权限、任务状态、取消、缓存和审核；生成模型承担开放式外观、动作与音视频；传统引擎持有碰撞、网络同步和安全边界；世界模型用于候选预测和评分，而非直接绕过控制器。

尚未解决的问题包括：真正可打断的低首帧数字人；多人写实世界的状态—像素一致性；生成视图能否可靠反映权威状态；物理参数校准；跨小时身份和对象记忆；GPU 成本、并发、内容授权及输出溯源。

需要降级看待的表述包括：Wan-Animate-2 项目页称“实时”，但开放仓库默认需多张 A800；Vorch-Streamer 的实时指标缺硬件和首帧信息；MASS 的 1,024 玩家来自简单 Snake benchmark；UniVVT 的端到端语义方案没有服装物理与商品真实性保证。

## 10. 论文精读候选

1. [MASS](https://arxiv.org/abs/2608.06257)：重点读 typed state schema、Logic/Rendering Engine 解耦、多人同步和复杂度分析。差异在于把游戏服务器架构引入生成世界；风险是简单环境结论难以扩展到写实视频。
2. [Vorch-Streamer](https://arxiv.org/abs/2608.05663)：重点读 25 Hz speech token、因果块边界、Self-Forcing 和 FPS 测量协议。风险是代码与硬件缺失。
3. [Wan-Animate-2](https://arxiv.org/abs/2608.06009)：重点读 Time-Align RoPE、Sparse-Ref Attention、蒸馏和多相机控制；结合 ComfyUI 缓存实现阅读。风险是论文 Lite 与公开 14B 版本不完全对应。
4. [GAUGE](https://arxiv.org/abs/2608.05948)：重点读真实测量、参数反演、任务误差定义及跨物理引擎比较。价值在于建立视觉质量之外的工程验收标准。
5. [UA-NWM](https://arxiv.org/abs/2608.05597)：重点读 Hierarchical Error Projection、单次前向评分、CEM/MPC 和实机协议。复现风险主要是 DINOv3 gated 权重及数据许可证。

## 11. 下周跟踪与可行动建议

继续跟踪：

1. Wan-Animate-2 Lite 是否开放，以及单卡 FPS、首帧、显存和输入到输出延迟。
2. ComfyUI 的 int8/int4 pose cache 在长视频中是否造成身份或动作漂移。
3. Vorch-Streamer 是否开放代码、流式 API、硬件配置和可取消 block。
4. MASS 是否公布状态 schema、benchmark 和写实多主体实验。
5. GAUGE 是否开放真实轨迹、标定工具和自动评分脚本。
6. UA-NWM 在风、低照度、目标图过时及通信中断下的失效模式。
7. Vorch-IR、Wan-Animate-2 对身份授权、水印和审计元数据的支持。
8. StreamMind 是否开放 worker 调度、持久多模态记忆和评测集。

本周可做的小实验：

1. **Wan-Animate-2 节点缓存实验**：固定人物和驱动视频，对比无缓存、FP16、int8、int4 缓存。成功判据是生成耗时下降且身份、动作和边界帧差异保持在预设阈值内。
2. **权威状态多人原型**：用简单 WebSocket 服务器维护 typed state，客户端只提交动作并按相机请求渲染占位视图。成功判据是断线重连、重放和两客户端观察的一致性均可自动验证。
3. **UA-NWM 最小复现**：在 AirGoal-10k 测试集比较 HEP 与 deterministic cosine 的 32 候选排序。成功判据是复现主要排名趋势，并测得单次评分延迟。
4. **物理发布门原型**：从公开视频或现有仿真生成弹跳、滑动、摆动三类轨迹，提取加速度、恢复系数和周期。成功判据是能识别“画面合理但参数不一致”的生成结果，并输出机器可读失败报告。
