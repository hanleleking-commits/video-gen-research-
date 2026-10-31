# 数字人、世界模型与互动作品研究日报：2026-10-31

检索窗口：2026-10-25 至 2026-10-31。  
信息截点：2026-10-31 09:00（Asia/Shanghai）。已与 10 月 25—30 日日报去重；日期按论文原始提交或官方仓库更新记录核验。

## 1. 本日摘要

过去一天最值得关注的变化，是世界模型进一步从“生成连续画面”转向“由显式代码状态约束的持续运行系统”。Fight World Model 将格斗规则放在可执行引擎中，把动作、命中、受击与血量编译成时空代理，再由视频模型负责渲染；这是本周期最完整的“代码维护真相、模型生成外观”案例。

L3G-World 则把潜在特征直接存入自适应 3D Gaussian 记忆，使记忆随探索范围而不是视频时长增长，作者报告达到 9 FPS。不过其代码、权重和快速模型仍处于发布计划状态，现阶段是论文和演示，不是可部署运行时。

FutureWorld 与 Context Compaction 分别从模型层和 agent harness 层处理长时漂移：前者在生成下一段视频前预测未来状态，后者由外部运行时选择应重新送入模型的历史片段。这表明世界模型的“上下文工程”正在形成与 coding agent 相似的独立系统层。

WorldProbe 首次用多模态 agent 在线选择相机动作，对十个模型开展最长 180 秒的主动探索。结果显示局部画质保持时间有所延长，但远距离重访、对象状态和世界演化仍明显不可靠；“能连续播放”仍不能等同于“拥有持久世界”。

GenSight 与 WorldFlow 都把视频之外的显式物理输出纳入生成：前者同时输出深度、具身分割和二维流，后者联合预测视频与可查询的三维位移。这些接口比纯 RGB rollout 更容易接规划器、控制器和验证代码，但尚未形成稳定 SDK。

数字人侧只有 HairDeck 达到高质量新增门槛：它从单图生成带 spring-bone 的 stylized hair cards，并能导出 GLB/VRM，直接解决角色资产进入游戏运行时前的人工建模与绑定瓶颈。本周期没有新的实时 talking-head、lip-sync、全双工对话数字人或客户部署证据。

工业成熟度仍以论文原型和开源研究组件为主。新增项目均未披露生产级并发、P95 延迟、SLA、单位会话成本或规模客户案例。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 10 月 30 日 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 低—中 | HairDeck 将单图发型直接变成可动画 hair-card＋spring-bone 资产 | 从研究几何推进到可导出 GLB/VRM 的开源资产组件 | 新增角色制作管线；实时对话数字人无变化 |
| 世界模型 | 高 | 显式规则、潜在 3DGS 记忆、未来状态预测与 agent 主动评测同时出现 | 多个可运行 demo；生产服务仍未验证 | 从 Hy-Nomas 的流式生成栈推进到状态、记忆和验收层 |
| Coding × 互动作品 | 高 | Fight World Model 用规则引擎维护权威状态；Gym-Anything 让 coding agent 生成仿真环境 | 已有可执行引擎/benchmark，尚无商业部署 | “代码编排视频”进一步变成“代码维护持续世界” |
| 支撑基础设施 | 高 | WorldProbe、WorldFlowBench、上下文压缩和显式 3D 运动输出 | 评测与接口明显增强 | 从片段画质转向长时漂移、重访和执行价值 |
| 工业部署 | 低 | HairDeck 接近资产生产工具；其余多为研究原型 | 无客户试点或规模部署新增 | 成本与可靠性证据仍缺失 |

## 3. 最值得关注的 11 个进展

### 1. Fight World Model：规则引擎与生成渲染正式分层

- **类型 / 日期 / 主线：**论文、模型、demo、dataset；2026-10-29；世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2610.40607)、[项目页](https://fight-world-model.github.io/)。
- **相对上期：**全新条目。
- **核心贡献：**可执行引擎维护角色、技能、命中和血量；确定性编译器把执行结果转成按角色与时间对齐的 interaction proxy；视频模型只负责画面。无需重训渲染器即可修改伤害倍率、免疫、吸血与多阶段技能。
- **Coding 接口：**规则配置、状态更新函数、事件流、身份绑定与代理编译器；天然适合接游戏服务器或剧情状态机。
- **互动/工业场景：**可换角色和规则的生成式格斗、品牌 IP 对战、互动直播。
- **成熟度 / 证据：**研究 demo；作者在 52 种技能、187 种角色外观和 15 个场景上实测，未见公开代码或实时服务指标。
- **重要性 / 阅读：**高 / 必读。

### 2. L3G-World：潜在 3D Gaussian 记忆支持 9 FPS 世界生成

- **类型 / 日期 / 主线：**论文、项目仓库、demo；2026-10-29；世界模型、运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2610.40614)、[仓库](https://github.com/cvlab-kaist/L3G-World)。
- **相对上期：**全新条目。
- **核心贡献：**新区域生成 Gaussian、已见区域持续细化；每个 Gaussian 保存 VAE latent，可直接渲染到 DiT latent，避免 RGB 渲染后再编码。作者报告相对既有 3D-memory 基线最多减少 38.6× primitive、加速记忆渲染 359×，整体达到 9 FPS。
- **Coding 接口：**图像、文本和相机轨迹输入；潜在记忆可作为会话状态。
- **场景：**持续漫游、虚拟摄影、生成式关卡视觉层。
- **成熟度 / 证据：**论文 demo、官方声称；仓库的代码和权重清单截至截点仍未勾选发布。
- **重要性 / 阅读：**高 / 必读。

### 3. HairDeck：单图生成可动画游戏发型

- **类型 / 日期 / 主线：**论文、GitHub、权重、资产生成；2026-10-29；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.40599)、[代码](https://github.com/hyz317/HairDeck)、[模型](https://huggingface.co/hyz317/HairDeck)。
- **相对上期：**全新条目。
- **核心贡献：**将 hairstyle 建模为无序 hair-card 集合，每张 card 一个 token，并同时生成 spring-bone chain。作者报告相对 CHARM 降低 44% Chamfer distance、方向误差减少一半以上、速度提高 33×。
- **Coding 接口：**Apache-2.0 代码和权重；输出带 UV 的 GLB，或绑定到角色后的 VRM 0.x。
- **场景：**游戏角色、VTuber、社交 avatar、UGC 换装。
- **成熟度 / 证据：**已开源；作者实测。训练分布以 VRoid/动漫风格为主，不能外推到真实毛发。
- **重要性 / 阅读：**高 / 必读。

### 4. WorldProbe：让 agent 主动把世界模型“玩坏”

- **类型 / 日期 / 主线：**benchmark、agent harness；2026-10-29；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2610.39721)、[声明的代码入口](https://github.com/Untitled-W/WorldProbe)。
- **相对上期：**全新条目。
- **核心贡献：**goal-conditioned 多模态 agent 在线决定相机动作，而非预先固定轨迹；164 个测试案例、十个模型、1,600 余条 rollout、约 800 分钟视频，最长主要评测时长 180 秒。
- **Coding 接口：**runner 连接任务 agent、感知模块、世界模型接口和指标服务，可用于模型升级 CI。
- **结论：**局部画质改善快于长期记忆和世界演化；远距离重访仍是普遍弱点。
- **成熟度 / 证据：**论文实测；摘要称已开放资源，但 GitHub 链接在截点返回 404，故标为待核验。
- **重要性 / 阅读：**高 / 必读。

### 5. FutureWorld：在下一段生成前先预测状态

- **类型 / 日期 / 主线：**论文、插件式模块、demo；2026-10-29；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2610.39619)、[项目页](https://GuangYuanKK.github.io/futureworld)。
- **相对上期：**全新条目。
- **核心贡献：**Causal Next-Chunk Predictor 根据当前 latent 和下一动作预测未来状态，再调制下一段生成，减少“错误历史被反复写回”的递归漂移。
- **Coding 接口：**作为基础视频生成器前的可插拔预测器；作者接入四个生成器、三个 benchmark。
- **场景：**长时探索、互动电影连续分支、世界模型服务的状态校正。
- **成熟度 / 证据：**论文 demo与作者实测；未确认公开代码和生产延迟。
- **重要性 / 阅读：**高 / 必读。

### 6. Context Compaction for World Models：把上下文工程搬到视频世界

- **类型 / 日期 / 主线：**论文、benchmark、agent harness；2026-10-28；Coding × 世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2610.38980)。
- **相对上期：**全新条目。
- **核心贡献：**外部 harness 决定为当前视角重新提供哪些历史房间，而不改模型参数。约 14M 参数的研究模型中，检索范围最多扩展到原上下文窗的 4×。
- **Coding 接口：**世界状态索引、历史片段选择、上下文打包与模型调用循环。
- **场景：**长时 NPC 空间记忆、探索游戏、持续仿真。
- **成熟度 / 证据：**小规模论文原型；结果不能直接外推到 5B—20B 视频模型。
- **重要性 / 阅读：**高 / 必读。

### 7. GenSight：一次 rollout 同时生成 RGB、深度、分割与流

- **类型 / 日期 / 主线：**论文、项目 demo；2026-10-29；世界模型、4D 感知。
- **官方链接：**[论文](https://arxiv.org/abs/2610.39232)、[项目页](https://gensight-rbs.github.io/)。
- **相对上期：**全新条目。
- **核心贡献：**共享视频 DiT、VAE 与 velocity prediction space；生成模式同步读出 RGB、深度、具身 mask 和二维流，感知模式则从真实视频单步预测这些属性。
- **Coding 接口：**显式深度、mask 和 flow 比 RGB 视频更容易交给规划器、碰撞检查和轨迹跟踪器。
- **成熟度 / 证据：**研究 demo；playground 标注“Coming Soon”，无公开 SDK。部分训练标签与评测参考来自相同估计器，需独立复测。
- **重要性 / 阅读：**高 / 必读。

### 8. WorldFlow：联合生成视频和三维运动轨迹

- **类型 / 日期 / 主线：**论文、benchmark、机器人实验；2026-10-29；世界模型、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2610.39278)、[项目页](https://worldflow-rbs.github.io/)。
- **相对上期：**全新条目。
- **核心贡献：**用与视频网格对齐的连续三维位移 token，在同一 MoT 中联合去噪视频与运动；训练数据约 144 万个对象样本，并提出含 1,678 个样本的 WorldFlowBench。
- **Coding 接口：**输入图像、语言与三维 query point，输出可供控制器消费的度量轨迹。
- **成熟度 / 证据：**论文原型；联合视频训练优于同配方 flow-only 的消融较可信，但外部基线比较存在训练源重合，真实集上简单静止预测也可能更强。
- **重要性 / 阅读：**中高 / 必读。

### 9. Gym-Anything for Robotics：coding agent 生成机器人仿真

- **类型 / 日期 / 主线：**论文、代码、benchmark、dataset；2026-10-29；Coding × 仿真。
- **官方链接：**[论文](https://arxiv.org/abs/2610.40565)、[项目页](https://gym-anything.github.io/robotics)。
- **相对上期：**全新条目。
- **核心贡献：**coding agent 创建仿真，独立 audit agent 检查任务和环境；RoboWorld 包含 25 个仿真、25 个测试及 175 个训练 RL 环境，按经济活动选择任务覆盖。
- **Coding 接口：**环境代码、状态读取、测试、对抗审计、代码策略和训练轨迹生成。
- **成熟度 / 证据：**官方声明开放管线和数据；最强模型在测试任务平均 reward 仅 0.06，说明自动生成高质量仿真仍很难。
- **重要性 / 阅读：**高 / 必读。

### 10. Continuous-Action World Models：用连续时间函数替代固定动作块

- **类型 / 日期 / 主线：**论文、机器人策略；2026-10-29；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2610.40401)。
- **相对上期：**全新条目。
- **核心贡献：**先从带时间戳轨迹学习 latent action function，再让世界模型由视觉、语言和机器人状态预测该函数；decoder 可在任意查询时间输出控制命令。
- **Coding 接口：**控制频率成为运行时查询参数，而非模型固定 chunk size。
- **成熟度 / 证据：**研究原型；当前实验主要是单一双臂平台，论文标题中的跨具身能力尚未被充分验证。
- **重要性 / 阅读：**中 / 可读。

### 11. Video Generation Meets Agent Harness：系统化定义视频世界运行时

- **类型 / 日期 / 主线：**survey / position report；2026-10-29；Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2610.40191)。
- **相对上期：**全新条目。
- **核心贡献：**把 harness 定义为围绕视频模型构建上下文、暴露工具、维护状态、路由反馈和管理预算的可执行运行时；明确区分输出修补、状态更新、模型更新与评价器/改进流程更新。
- **Coding 接口：**工具调用、共享项目状态、镜头规划、资产管理、执行反馈、成本与来源记录。
- **成熟度 / 证据：**综述与架构判断，不是新模型或产品，不能作为性能证据。
- **重要性 / 阅读：**中高 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期没有新的高质量 talking-head、speech-driven avatar 或全身语音动作模型。
- **3D/4D 表示：**HairDeck 把最难自动化的 stylized hair 从融合 mesh 转成真实游戏管线使用的 hair-card primitive，并直接生成动画骨链。
- **实时交互：**HairDeck 的生成速度已接近交互式角色定制，但它生成的是离线资产，不是运行时人物表演。
- **音视频与情感：**本周期暂无高质量新增；口型、视线、倾听动作、情绪和可打断对话没有成熟度变化。
- **工程部署：**HairDeck 的 GLB/VRM 输出可进入 Unity、VRM 或 DCC 管线；纹理、碰撞和跨引擎 spring-bone 参数仍需人工验收。
- **安全与身份治理：**暂无新的授权、水印、肖像撤回或深伪治理进展。HairDeck 面向 stylized asset，身份风险低于照片级头像，但训练素材和角色 IP 许可证仍需审核。

## 5. 世界模型进展

- **架构与训练：**显式 3D 记忆、未来状态预测、视频＋几何联合生成和连续时间动作函数同步升温。
- **可控/可玩生成：**Fight World Model 证明复杂规则不应由视频模型隐式猜测，而应先执行再渲染；这是比自然语言动作条件更强的控制接口。
- **长时与物理一致性：**L3G-World、FutureWorld 和 Context Compaction 分别处理空间记忆、错误递归和上下文检索，但尚无一个系统同时解决三者。
- **空间表示：**3D Gaussian latent、深度、mask、二维流和三维位移正在形成视频模型与传统引擎之间的中间表示。
- **机器人/仿真：**WorldFlow、GenSight、CoAWM 和 Gym-Anything 把重点从生成视觉未来扩展到可执行动作、轨迹和训练环境。
- **实时部署与评测：**L3G-World 的 9 FPS 是最强新增吞吐声明；WorldProbe 则说明局部实时不代表 180 秒后的状态仍正确。

## 6. Coding × 新型互动作品

### 链路一：规则可编辑的生成式格斗

**玩家输入 → 规则引擎执行 → 状态/事件编译为 interaction proxy → 视频模型渲染 → 客户端播放**

代码负责命中判定、伤害、血量、技能窗口和胜负；模型负责角色外观、特效和画面。已有作者 demo，尚无公开实时网络服务。

### 链路二：带持久空间记忆的生成世界

**相机输入 → L3G 记忆查询 → 视频 chunk 生成 → 新区域 Gaussian 写入/旧区域更新 → 下一轮**

代码负责相机、会话、记忆生命周期和存档；模型负责补全外观。9 FPS 有作者证据，但发布包仍未完成。

### 链路三：可恢复的互动电影

**剧情状态机 → 当前动作 → FutureWorld 状态预测 → 视频生成 → 视觉/规则验收 → 提交或回滚**

模型先预测下一段应该出现的状态，再生成画面；代码保留分支、资产引用和回滚点。完整系统属于本报告推断，C-NCP 模块已有论文证据。

### 链路四：单图生成可直接绑定的 avatar 发型

**角色图 → HairDeck → hair cards＋spring bones → GLB/VRM → Unity/VRM 运行时**

模型负责生成结构化资产，传统引擎负责物理摆动和渲染，代码负责批处理、许可证与质量门禁。该链路已有开源资源，最接近短期产品化。

### 链路五：coding agent 自动生产机器人训练世界

**任务规范 → 创建 agent 编写环境 → audit agent 对抗检查 → RL 环境 → 策略训练/合成示范**

代码本身就是世界；物理引擎提供权威动力学，生成模型负责创建和修复程序。Gym-Anything 已开放 benchmark，但测试结果显示自动生成质量仍远未稳定。

### 链路六：可观测的机器人世界模型

**相机＋指令 → GenSight/WorldFlow → RGB＋深度/mask/3D轨迹 → 规划器 → 安全控制器**

世界模型负责预测可见未来和运动，代码负责碰撞、约束、急停和执行验证。论文已有机器人相关实验，生产接口和安全认证均缺失。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Fight WM＋规则引擎＋视频 renderer | 攻击、命中、血量、规则编辑 | 研究 demo | 未披露 | 实时延迟、多人同步、内容审核 | [FWM](https://arxiv.org/abs/2610.40607) |
| 影视/虚拟制作 | HairDeck＋DCC＋角色 rig | 单图生成可动画发型 | 已开源组件 | 作者报告约 33× 加速 | 写实风格、纹理、人工修整 | [HairDeck](https://github.com/hyz317/HairDeck) |
| 直播与电商 | 本周期无直接新增 | RTC＋数字人＋审核 | 无变化 | 未披露 | 全双工、口型、并发、肖像权 | — |
| 品牌互动 | FWM / L3G-World＋Web客户端 | 可换角色、场景、规则、漫游 | demo级 | L3G 声称 9 FPS | GPU成本、状态漂移、品牌一致性 | [L3G](https://arxiv.org/abs/2610.40614) |
| 教育培训 | FutureWorld＋状态机 | 分支演练、错误恢复 | 论文原型 | 未披露 | 可重复性、事实与评分一致性 | [FutureWorld](https://arxiv.org/abs/2610.39619) |
| 企业数字员工 | 本周期无直接新增 | LLM工具调用＋avatar | 无变化 | 未披露 | SLA、权限、审计、身份授权 | — |
| 陪伴与社交 | HairDeck＋VRM；长时 avatar 仍缺 | 角色定制与持续会话 | 资产组件可用；会话未成熟 | 未披露 | 表演、记忆、安全 | [HairDeck](https://huggingface.co/hyz317/HairDeck) |
| 空间计算 | L3G-World＋3DGS＋XR | 持续探索与重访 | 研究 demo | 9 FPS，硬件未充分披露 | 几何可编辑性、碰撞、设备性能 | [论文](https://arxiv.org/abs/2610.40614) |
| 机器人/仿真 | Gym-Anything、GenSight、WorldFlow | 自动建环境、预测几何和轨迹 | 研究原型 | 未披露；RoboWorld 最佳测试 reward 0.06 | 仿真真实性、真机安全、跨具身泛化 | [Gym-Anything](https://arxiv.org/abs/2610.40565) |

## 8. 可复现资源与开发者入口

| 资源 | 开放状态 / 许可证 | 硬件与成本 | 最小验证路径 |
|---|---|---|---|
| [HairDeck](https://github.com/hyz317/HairDeck) | 代码、权重；Apache-2.0，依赖项另审 | 单 NVIDIA GPU；最小显存未正式披露 | 单图生成 GLB，再导出 VRM，检查 card 穿插和 spring-bone 稳定性 |
| [L3G-World](https://github.com/cvlab-kaist/L3G-World) | MIT 仓库；核心代码和权重仍在 release plan | 论文未给出消费级最小配置 | 暂只能审阅 demo；等待 inference 与 Fast 权重 |
| [WorldProbe](https://github.com/Untitled-W/WorldProbe) | 论文称开放，截点链接 404 | 十模型完整评测成本很高 | 等仓库恢复后先跑单模型、单重访任务 |
| [Gym-Anything Robotics](https://gym-anything.github.io/robotics) | 作者声明开放 pipeline、环境和数据 | 取决于仿真器和 agent API | 先运行一个训练环境，再让 audit agent 注入三类物理错误 |
| [GenSight](https://gensight-rbs.github.io/) | 项目页和 demo；playground 尚未开放 | 未披露 | 暂复核 RGB—深度—flow 对齐，不安排部署 |
| [WorldFlowBench](https://worldflow-rbs.github.io/) | benchmark/代码开放状态需逐项确认 | 未披露 | 优先比较 joint 与 flow-only，不直接采用外部 SOTA 排名 |
| [FutureWorld](https://GuangYuanKK.github.io/futureworld) | 项目 demo；代码入口未确认 | 未披露 | 等代码后在固定动作序列上比较是否减轻重复漂移 |

## 9. 系统架构与技术趋势判断

明显升温的路线是“权威状态层＋代理编译层＋生成渲染层＋运行时记忆＋主动验收”。Fight World Model 是最清晰实例：规则由代码执行，interaction proxy 是稳定接口，视频模型不能自行修改血量或命中结果。

另一条主流路线是把可验证信息从 RGB 中抽出来。深度、mask、3D flow、Gaussian memory、动作函数和事件状态均比视频像素更适合作为规划、测试与回滚接口。

记忆也开始分成两层：模型内部使用 KV、recurrent state 或 3DGS；外部 harness 决定检索、压缩、提交与丢弃哪些历史。未来生产系统很可能同时需要两层，而不是只扩大上下文窗口。

数字人研究仍存在结构性断层：资产生成快速改善，HairDeck 已能输出引擎友好资产；但实时交谈、身体表演、情绪、RTC、审核和身份治理没有同步推进。

应降级看待的表述包括：L3G-World 的 9 FPS 尚无公开代码复测；CoAWM 的跨具身标题缺少真正跨机器人验证；WorldFlow 的外部排名受训练源重合影响；WorldProbe 的代码链接尚不可访问；agent-harness 综述是架构框架而非运行性能证明。

## 10. 论文精读候选

1. [Fight World Model](https://arxiv.org/abs/2610.40607)：重点读状态转移、interaction proxy vocabulary、identity binding 与规则干预实验。与普通 action-conditioned 模型的差异是“执行结果先于渲染”。风险是固定视角、双角色、封闭格斗域。
2. [L3G-World](https://arxiv.org/abs/2610.40614)：重点读 Gaussian spawn/refine、latent rendering、记忆复杂度和 9 FPS 计时边界。风险是代码未发布、硬件与端到端延迟需核验。
3. [WorldProbe](https://arxiv.org/abs/2610.39721)：重点读 agent action policy、四类 probing、服务数据流和长时斜率指标。风险是部分指标依赖模型评判，仓库暂不可访问。
4. [Context Compaction for World Models](https://arxiv.org/abs/2610.38980)：重点读 context retrieval 与 reconstruction/inference 的区分、harness 策略及 dreaming 条件。风险是模型规模和任务过小。
5. [WorldFlow](https://arxiv.org/abs/2610.39278)：重点读视频—flow token 对齐、联合 rectified flow 和消融。复现时应忽略受数据重合影响的外部排名，优先验证 joint-vs-flow-only。

## 11. 下周跟踪与可行动建议

继续跟踪：

1. L3G-World 是否真正发布 inference、L3G-Mem、Fast 权重与完整 9 FPS 复现配置。
2. Fight World Model 是否开放规则 schema、interaction proxy 编译器和实时 demo。
3. WorldProbe 仓库是否恢复，是否支持 Hy-Nomas、L3G-World 等最新模型。
4. FutureWorld 在不同基础模型上的额外计算量、首帧延迟和长时收益。
5. HairDeck 导出 VRM 后在 Unity、Godot 与 Web avatar runtime 中的兼容性。
6. GenSight 与 WorldFlow 是否开放权重，以及伪标签之外的独立几何评测。
7. Gym-Anything 生成环境能否通过人工物理审计并迁移到真机。
8. 数字人方向是否出现新的全双工语音驱动、情绪或客户部署证据。

本周可动手实验：

- **实验一：引擎权威状态＋生成画面。**用简化格斗状态机维护血量、命中与冷却，将事件编译为 JSON 和彩色代理视频。成功判据是修改伤害规则后无需改渲染提示，所有后续帧仍保持正确血量。
- **实验二：HairDeck 资产落地。**从三张不同风格角色图生成 GLB/VRM，导入 Unity 或 VRM viewer。难点是穿插、头部坐标和 spring-bone 参数；成功判据是自动导入、转头无明显穿模、60 秒运行无骨链发散。
- **实验三：世界模型长时 CI。**复刻 WorldProbe 的“离开—探索—返回”任务，记录每次重访的对象身份、布局和视觉相似度。成功判据是形成可自动比较模型版本的曲线，而非只保留精选视频。
- **实验四：双层记忆原型。**用外部 SQLite/向量索引保存关键帧、相机位姿和状态摘要，按当前相机检索后注入现有世界模型。成功判据是在不增加模型上下文窗的情况下，将可正确重访距离提高至少 2×，且延迟增量可测。
