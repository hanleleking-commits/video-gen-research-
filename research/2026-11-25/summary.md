# 数字人、世界模型与互动作品研究日报：2026-11-25

检索窗口：2026-11-19 至 2026-11-25。

## 1. 本日摘要

本周期最明确的变化不是单一生成模型画质升级，而是“生成能力如何持续运行”开始成为研究中心。数字人方向没有新的高质量 talking-head 或实时音视频模型发布；主要增量来自 Motion Mamba 2.0 的长时人体运动控制，以及 3AGameFactory-Motion 将任意形态资产的绑定和动画转换成可执行参数程序。世界模型方向同时向三类可用性推进：Plan4D、WorldLatent 将 RGB、几何和动态放进统一或共享的 4D 表示；ViDecide、Native LAM、Fast-JEPA 则把视频预测转化为可验证决策、紧凑动作或低延迟规划。Coding × 互动作品是本日最强主线：AutoPILOT 让模型编写可持续执行的闭环控制器，GameAnything 将编码、实机试玩、视觉检查和修复组织成持续迭代系统，GameDojo 则提供实时而非“暂停等待模型”的评测环境。工程上的共同趋势是让代码和传统运行时掌握规则、状态、碰撞、计时与验证，把生成模型用于外观、动作细节、未来预测和创意扩展。已有可运行证据主要集中在游戏开发、游戏代理、机器人规划和人体动画；本周期没有发现达到客户试点或规模化部署级别的新案例。身份授权、水印、肖像权和深伪检测也没有高质量新增，这意味着数字人的生成及驱动进展仍快于治理接口。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---|---|---|---|
| 数字人 / 虚拟形象 | 中 | 长时运动生成与任意资产绑定开始转成可执行程序 | 从论文演示推进到代码、权重和引擎资产管线 | 新增 Motion Mamba 2.0、3AGameFactory-Motion；无新的高质量 talking-head |
| 世界模型 | 高 | 视频模型正在同时承担几何预测、动作抽取、决策和低延迟规划 | 多个项目开放代码、权重或 benchmark，但仍以研究原型为主 | 新增 Plan4D、ViDecide、Native LAM、ViewDreamer、Fast-JEPA、WorldLatent |
| Coding × 互动作品 | 很高 | 模型一次思考，生成代码在实时环境中持续执行；开发代理开始自动试玩和回归 | GameAnything 已可安装运行；AutoPILOT 仅开放演示，代码待发布 | 从“代码生成场景”进一步转向“代码维持运行、模型按需介入” |

## 3. 最值得关注的 12 个进展

### 1. AutoPILOT：让视觉模型编写持续运行的游戏控制器

- **类型 / 日期 / 主线：** 论文、benchmark、交互 demo；2026-11-20；Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2611.28202)、[项目页与回放](https://autopilot-harness.github.io/)。
- **相对上期的新变化：** 首次收录；代码仍标注“coming soon”。
- **核心贡献：** 每次模型调用可选择原子动作、开环宏或闭环 Python 控制器。控制器直接读取屏幕并连续行动，直至主动交还控制。AFK-Bench 覆盖120个浏览器游戏；论文报告 GPT-6.1 Sol 的实时成功率由 VISTA 的38.3%提高到68.6%，在 GameWorld 上保持80.0%实时成功率。项目页还披露模型调用中位延迟6.1秒，而控制循环中位步长约0.41秒。
- **Coding 接口：** 模型输出受限 Python，运行时提供 `observe`、`act`、`request_handoff` 等 API；这是“LLM写控制器、控制器跑高频循环”的直接实现。
- **场景 / 成熟度 / 证据：** 实时 NPC、游戏测试机器人、机器人视觉控制；可运行录制 demo，代码未开放；论文实测；**重要性高，必读**。

### 2. GameAnything：Coding agent 自动构建、试玩和修复游戏模组

- **类型 / 日期 / 主线：** 开源平台、论文、benchmark；2026-11-22；Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2611.29238)、[GitHub](https://github.com/AetherLabsAI/GameAnything)。
- **核心贡献：** 将照片、视频、小说或影片转化成现有游戏的可玩扩展；编码代理、独立试玩 critic、持久开发状态和经验记忆构成迭代闭环。公开示例覆盖 Minecraft、Cities: Skylines、Overcooked! 2、Slay the Spire 等。
- **Coding 接口：** 提供 CLI、Web UI、Codex/Claude Code/Gemini CLI/OpenCode 插件或 skill；代理能够构建模组、启动游戏、录制试玩、按固定镜头比较结果并回归修复。代码为 Apache-2.0，但游戏资产和改编作品仍受原权利限制。
- **场景 / 成熟度 / 证据：** 个性化游戏、品牌模组、互动叙事、创作者原型；已开源且有完整运行示例，尚非正式商业产品；官方演示与论文实测；**重要性高，必读**。

### 3. 3AGameFactory-Motion：把任意3D资产动画变成可执行程序

- **类型 / 日期 / 主线：** 论文、GitHub、benchmark；2026-11-23；数字人、Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2611.30013)、[GitHub](https://github.com/OpenDCAI/GameFactory-3A)。
- **核心贡献：** DivDynBench 覆盖六类形态和多阶段复杂动作；其 Agentic Motion Framework 不直接逐帧生成，而由 agent 编写紧凑的骨架与运动参数程序，再由确定性执行器完成绑定、运动学求解和几何修正。
- **Coding 接口：** 3AGameFactory 以 agent skill 和流水线暴露能力，支持 UE5、Unity、Godot 4、Blender、Three.js；Apache-2.0。该结构特别适合 coding agent，因为运动控制可检查、可修改、可回放。
- **场景 / 成熟度 / 证据：** 游戏角色、非人形 NPC、影视预演、批量资产生产；已开源并有引擎 demo；论文实测与官方演示；**重要性高，必读**。

### 4. Motion Mamba 2.0：长时、可续接的人形运动控制

- **类型 / 日期 / 主线：** 模型、代码、权重、数据；2026-11-23；数字人。
- **官方链接：** [论文](https://arxiv.org/abs/2611.29897)、[GitHub](https://github.com/AIGeeksGroup/MotionMamba2.0)。
- **核心贡献：** 结合局部注意力、可续接 Mamba 状态和 Persistent Content Memory，支持文本、历史运动及空间目标条件下的流式运动生成。论文报告最长39.52秒终点控制；RTX 4090上完整窗口延迟由1685.35毫秒降至276.14毫秒，并在 Unitree G1 上验证键鼠控制和连续文本动作组合。
- **Coding 接口：** Python训练/评测脚本、Viser交互界面、SONIC物理策略栈和Web桥；代码、权重、训练数据开放，Apache-2.0。
- **场景 / 成熟度 / 证据：** 3D数字演员、游戏角色和人形机器人共用动作层；已开源研究原型；论文实测；**重要性高，必读**。

### 5. ViDecide：视频扩散模型按“决策步骤”生成并自行停止

- **类型 / 日期 / 主线：** 论文、模型、代码、dataset；2026-11-19；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2611.26389)、[GitHub](https://github.com/Kevin-thu/ViDecide)。
- **核心贡献：** 把固定长度视频改造成因果状态转换序列，并用视觉 EOS 决定何时结束；Corrective Fine-tuning 从失败分叉点寻找成功延续。论文报告严格平均成功率83.8%，高于普通微调的23.3%及最强对照的67.8%。
- **Coding 接口：** 开放训练、推理、规则验证器和 benchmark 构造脚本；单次视频推理约需30GB显存。代码限非商业使用，数据与权重为 CC BY-NC 4.0。
- **场景 / 成熟度 / 证据：** 可验证谜题、机器人多步规划、交互剧情状态转换；已开源研究原型；论文实测；**重要性高，必读**。

### 6. Plan4D：一次生成同步输出 RGB 与可规划4D几何

- **类型 / 日期 / 主线：** 论文、GitHub、权重；2026-11-19；世界模型、支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2611.26516)、[GitHub](https://github.com/AMAP-ML/plan4d)。
- **核心贡献：** 将 LongCat-Video 与 Depth Anything 3 缝合，在同一潜变量轨迹上生成视频、深度、相机、点云和 scene flow，避免生成视频后再做独立重建。
- **Coding 接口：** 提供三个训练阶段及完整推理脚本，开放 stitched decoder 与 LoRA 权重；核心代码 Apache-2.0，但基础模型和数据许可证需单独检查。具体显存需求未披露。
- **场景 / 成熟度 / 证据：** 机器人预演、自动驾驶仿真、可编辑4D镜头和空间交互；已开源研究原型；论文实测；**重要性高，必读**。

### 7. Native LAM / PAWlicy：从世界模型中抽取跨机器人动作

- **类型 / 日期 / 主线：** 论文、项目页；2026-11-23；世界模型、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2611.30898)、[项目页](https://pawteam-eai.github.io/native-lam/)。
- **核心贡献：** 在预训练视频 DiT 中加入 dynamics registers，把视频动态压缩成紧凑 latent action；PAWlicy 用这些动作预训练 VLA，而无需在策略训练时生成视频。论文报告3.8B模型在 RoboTwin 2.0达到93.6%平均成功率，并展示人手到机器人、不同机器人之间的动作迁移。
- **Coding 接口：** 目前是训练框架概念和项目演示，代码标注“coming soon”；未来可作为策略网络的动作 tokenizer 或技能接口。
- **场景 / 成熟度 / 证据：** 跨本体机器人策略、动作检索、低成本视频预训练；研究原型；论文实测、尚不可复现；**重要性高，必读**。

### 8. ViewDreamer：适配可变摄像头配置的机器人世界模型

- **类型 / 日期 / 主线：** 论文、benchmark；2026-11-20；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2611.26981)。
- **核心贡献：** 每个视角沿用单视角生成路径，仅通过共享 slot token 交换信息；支持一至四个视角及不同相机位置和顺序。论文报告跨视角一致性提升，并将该表示用于 WAM 与 action-conditioned world model，在多个机器人 benchmark 和真实 Franka 上验证。
- **Coding 接口：** 适合作为ROS/机器人运行时的可变相机适配层，但当前未见公开代码。
- **场景 / 成熟度 / 证据：** 仓储机器人、实验室多相机迁移、仿真数据生成；研究原型；论文实测；**重要性中高，可读**。

### 9. Fast-JEPA：绕开递归 rollout 的低延迟规划

- **类型 / 日期 / 主线：** 论文、GitHub、benchmark；2026-11-19；世界模型。
- **官方链接：** [论文](https://arxiv.org/abs/2611.25927)、[GitHub](https://github.com/DeepCybo-PhysAI/Fast-JEPA)。
- **核心贡献：** 先生成目标导向候选计划，再直接预测完整动作序列的终态，避免在搜索期间逐状态递归 rollout。论文报告平均成功率96.50%，规划速度较其本地 INTACT 配置快2.41倍；DynaGoal进一步测试目标变化、截止时间和0—800毫秒响应延迟。
- **Coding 接口：** MIT代码、Docker、训练和评测脚本；参考时延测于 RTX PRO 6000 Blackwell。
- **场景 / 成熟度 / 证据：** 机器人在线规划、交互系统的预测服务、延迟预算调度；已开源研究原型；论文实测；**重要性中高，可读**。

### 10. GameDojo：把实时约束纳入游戏 agent benchmark

- **类型 / 日期 / 主线：** benchmark、模型、dataset、GitHub；2026-11-23；Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2611.30771)、[GitHub](https://github.com/THUSI-Lab/GameDojo)。
- **核心贡献：** 120场 boss 战、20个环境、600小时轨迹，同时支持实时与暂停时钟、像素与语义动作。环境由 coding agent 重建并编写脚本；GameDojo-4B在公开模型中表现出较强实时能力。
- **Coding 接口：** Gym式接口、OpenAI兼容模型端点、RGB/语义动作和PPO示例。代码与游戏环境仅限非商业研究；截至检查时，仓库仍把完整benchmark、600小时数据和训练接口列为待发布。
- **场景 / 成熟度 / 证据：** 游戏agent、QA机器人、实时交互回归测试；部分代码可用，资源未完整；论文实测；**重要性中高，可读**。

### 11. Future-OPSD：用无标注视频训练“预见下一步”

- **类型 / 日期 / 主线：** 论文、代码、模型权重；2026-11-20；支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2611.26954)、[GitHub](https://github.com/Jingchensun/future-opsd)。
- **核心贡献：** 学生只看过去视频，冻结教师分别看到真实未来和随机未来，由二者差异构造决策相关的自监督信号；论文报告开放预测最高提升14.2点、闭合规划最高提升15.9点。
- **Coding 接口：** Apache-2.0训练和评测代码，开放四组Qwen系权重；完整训练约需8×B200、2—4小时，适合研究复现而非轻量部署。
- **场景 / 成熟度 / 证据：** NPC意图预测、视频操作指导、机器人动作候选排序；已开源研究原型；论文实测；**重要性中，可读**。

### 12. WorldLatent：统一4D感知、生成与编辑的潜空间

- **类型 / 日期 / 主线：** 论文、项目页；2026-11-23；世界模型、支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2611.31209)、[项目页](https://worldlatent.github.io/)。
- **核心贡献：** 用多模态VAE把外观、几何和动态压入统一结构化潜空间，感知与生成成为同一空间中的互补去噪过程。
- **Coding 接口：** 若开放，可作为统一场景协议供编辑器、规划器和生成器读写；目前未见代码、权重或运行时接口。
- **场景 / 成熟度 / 证据：** 4D编辑、空间计算、机器人世界状态；研究原型；论文实测但复现材料不足；**重要性中，仅跟踪**。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：** 本周期暂无高质量新增的 talking-head、lip-sync 或单图音频驱动模型。新增集中在全身运动：Motion Mamba 2.0解决长时续接、远期空间目标与推理延迟；3AGameFactory-Motion解决任意形态资产的绑定与复杂动作控制。
- **3D/4D表示：** 没有新的专用数字人重建模型；Plan4D和WorldLatent的4D表示可间接支持数字演员与动态场景统一，但尚未验证人物身份、服装和毛发的长期一致性。
- **实时交互：** Motion Mamba的276.14毫秒窗口生成已接近交互动画层，而不是逐帧渲染层；其输出仍需物理策略和传统渲染器执行。
- **音视频与情感：** 本周期暂无高质量新增。语音、表情、视线、倾听行为和可打断对话没有新的一手发布。
- **工程部署：** 最成熟的新信号是“动作程序化”：代码或状态机决定目标和时序，模型补全自然动作，确定性执行器负责骨架、约束、碰撞及导出。
- **安全与身份治理：** 本周期暂无高质量新增。上述项目主要处理运动与资产，未提供肖像授权、生成水印、身份撤销或深伪检测闭环，进入面向消费者的数字人产品前仍需额外治理层。

## 5. 世界模型进展

- **架构与训练：** Plan4D采用模型缝合连接视频生成与几何解码；WorldLatent尝试统一4D潜空间；ViewDreamer用共享slot处理可变视角；Native LAM从视频DiT中抽取动作。
- **可控与可玩：** ViDecide把每段视频定义成可验证状态转换，并用视觉EOS停止；这比“生成固定时长、事后看结果”更接近可执行世界模型。
- **长时与物理一致性：** 本周期未出现解决任意长视频漂移的新模型。进步主要是显式几何、验证器和终态预测，属于把不可靠生成包进可检查结构，而非证明生成器已经掌握稳定物理。
- **空间表示：** Plan4D的RGB、深度、相机、点云、scene flow同步输出，以及WorldLatent的统一4D潜空间，是最值得关注的两条路线。
- **机器人、游戏与仿真：** Native LAM、ViewDreamer和Fast-JEPA分别覆盖动作抽取、多相机适配及低延迟规划；GameDojo和AutoPILOT则补齐实时游戏环境与agent harness。
- **部署与评测：** 延迟正在从单一FPS指标转向完整闭环：模型推理、控制器频率、环境时钟、截止时间和API成本。AutoPILOT、Fast-JEPA和GameDojo都直接测量了这一问题。

## 6. Coding × 新型互动作品

### 链路一：模型写一次控制器，代码持续行动

**视觉模型 → AutoPILOT受限Python API → 闭环控制器读取帧并高频执行 → 实时游戏角色/测试机器人。**

模型负责识别目标和编写策略；代码负责毫秒级循环、像素检测、动作与交还控制；传统游戏引擎负责规则、计时和物理。已有录制回放和量化评测，但框架代码尚未发布。

### 链路二：多模态素材进入自动游戏开发闭环

**照片/小说/视频 → GameAnything coding agent → 构建、启动游戏、录制、视觉比较、修复 → 个性化模组与互动叙事。**

模型负责理解素材、设计内容与生成代码；模组SDK和游戏运行时负责确定性行为；试玩critic负责验收。已有可安装CLI、插件、真实项目记录和多轮版本，属于本周期最接近创作者工具的方案。

### 链路三：任意角色从资产到动作程序

**3D资产/文本动作 → 3AGameFactory-Motion agent → 骨架与参数程序 → 确定性绑定、IK、几何修正 → Unity/UE/Godot/Three.js角色。**

生成模型负责结构推断和紧凑参数；代码执行器确保重复、角度、轨迹和时序；引擎负责碰撞、状态机和渲染。已有开源代码与多引擎示例。

### 链路四：可规划4D场景服务

**当前观测＋文本动作 → Plan4D → RGB、深度、相机、点云、scene flow → 规划器/规则引擎 → 机器人预演或可编辑镜头。**

模型负责合成可能未来；代码把几何结果送入碰撞检查、可达性验证与UI。当前有代码和权重，但还没有证明能满足实时互动延迟，工业落地仍属推断。

### 链路五：视频模型输出动作接口而非视频

**人类/机器人视频 → Native LAM → latent action → VLA策略或技能库 → 跨本体机器人动作。**

世界模型承担动态教师；代码负责动作映射、策略训练、安全约束与设备驱动；机器人控制栈负责真实执行。论文有真实机器人结果，但代码未开放。

### 链路六：带验证器的生成式剧情与任务系统

**规则或任务状态 → ViDecide逐步生成 → 视觉EOS＋规则验证器 → 失败分叉修正 → 谜题、训练模拟、互动剧情。**

模型负责提出下一状态；代码验证合法性、终止条件和得分；传统状态机保留权威状态。已有开放代码和benchmark，但示例仍集中在抽象游戏和机器人任务。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 技术栈与互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | AutoPILOT、GameAnything、GameDojo | coding agent、受限Python、模组SDK、试玩回放 | 可运行demo/开源原型 | AutoPILOT模型中位延迟6.1秒；成本曲线已披露 | 代码安全、IP授权、环境适配、回归稳定性 | [AutoPILOT](https://autopilot-harness.github.io/)、[GameAnything](https://github.com/AetherLabsAI/GameAnything) |
| 影视/虚拟制作 | 3AGameFactory-Motion、Plan4D | 参数动作程序、Blender/UE、4D几何 | 开源研究原型 | 未披露端到端制作成本 | 高质量资产修整、镜头连续性、导演可控性 | [3AGameFactory](https://github.com/OpenDCAI/GameFactory-3A)、[Plan4D](https://github.com/AMAP-ML/plan4d) |
| 直播与电商 | Motion Mamba 2.0 | 文本/空间目标到全身动作，再由实时渲染器输出 | 研究原型 | RTX 4090窗口中位276.14毫秒 | 缺少语音、表情、视线和安全授权闭环 | [Motion Mamba 2.0](https://github.com/AIGeeksGroup/MotionMamba2.0) |
| 品牌互动 | GameAnything | 品牌素材、coding agent、现有游戏或Web运行时 | 可运行demo | 取决于模型API与目标游戏，未统一披露 | 商标、资产版权、审核和部署适配 | [GitHub](https://github.com/AetherLabsAI/GameAnything) |
| 教育培训 | ViDecide、Plan4D | 状态转换、验证器、4D情景模拟 | 研究原型 | ViDecide单视频约30GB显存 | 事实可靠性、失败状态覆盖、内容审核 | [ViDecide](https://github.com/Kevin-thu/ViDecide) |
| 企业数字员工 | Motion Mamba 2.0 | 角色大脑、动作生成、传统3D渲染 | 组件级原型 | 未披露并发与会话成本 | 缺少全双工语音、表情和身份治理 | [论文](https://arxiv.org/abs/2611.29897) |
| 陪伴与社交 | 无直接新增 | 可复用全身动作层，但缺少实时音视频主体 | 暂无新增部署证据 | 未披露 | 长期记忆、情感安全、肖像权 | — |
| 空间计算 | WorldLatent、Plan4D | 统一4D潜空间、点云、相机与动态编辑 | 研究原型 | 未披露 | 端侧算力、真实尺度、漂移和交互延迟 | [WorldLatent](https://arxiv.org/abs/2611.31209) |
| 机器人/仿真 | Native LAM、ViewDreamer、Fast-JEPA | latent action、多视角slot、低延迟终态预测 | 论文实机验证/开源原型 | Fast-JEPA参考时延测于RTX PRO 6000 | sim-to-real、安全约束、设备接口和复现成本 | [Native LAM](https://arxiv.org/abs/2611.30898)、[Fast-JEPA](https://github.com/DeepCybo-PhysAI/Fast-JEPA) |

## 8. 可复现资源与开发者入口

| 资源 | 许可证/要求 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|
| [GameAnything](https://github.com/AetherLabsAI/GameAnything) | Apache-2.0；需自有游戏、模组加载器及coding agent | 高 | 安装CLI和agent插件，用一个小型Minecraft或Stardew Valley改动验证“构建—试玩—修复” |
| [3AGameFactory](https://github.com/OpenDCAI/GameFactory-3A) | Apache-2.0；不同生成模型有独立条款 | 高 | 选择一个非人形GLB，生成骨架与单个复杂动作并导入Godot/Three.js |
| [Motion Mamba 2.0](https://github.com/AIGeeksGroup/MotionMamba2.0) | Apache-2.0；Linux、CUDA GPU，完整仿真还需SONIC/TensorRT | 中高 | 下载权重，先跑缓存文本特征的离线评测，再接Viser界面 |
| [ViDecide](https://github.com/Kevin-thu/ViDecide) | 代码非商业；模型/数据CC BY-NC 4.0；约30GB推理显存 | 中 | 在FlowFree或Sokoban跑一个短序列，检查视觉EOS和规则验证器 |
| [Plan4D](https://github.com/AMAP-ML/plan4d) | Apache-2.0；基础模型另行授权；显存未披露 | 中 | 下载decoder与LoRA，对一段短输入同时导出RGB、深度和scene flow |
| [Future-OPSD](https://github.com/Jingchensun/future-opsd) | Apache-2.0；完整训练约8×B200 | 中 | 不训练，直接评估公开8B/9B权重在单一视频未来问答集上的增益 |
| [Fast-JEPA](https://github.com/DeepCybo-PhysAI/Fast-JEPA) | MIT；CUDA GPU；参考硬件RTX PRO 6000 | 高 | Docker运行PushT，比较递归rollout与终态预测的时延和成功率 |
| [GameDojo](https://github.com/THUSI-Lab/GameDojo) | 非商业研究；完整数据尚待发布 | 中低 | 运行一个公开boss环境，以暂停和实时两种模式比较同一agent |
| [AutoPILOT demo](https://autopilot-harness.github.io/) | 代码未发布 | 仅观察 | 下载公开控制器源文件，审查其API边界与闭环控制模式 |
| [Native LAM](https://pawteam-eai.github.io/native-lam/) | 代码待发布 | 暂不建议复现 | 等待动作tokenizer、模型权重及机器人映射代码 |

## 9. 系统架构与技术趋势判断

1. **明显升温：模型生成代码，代码接管实时循环。** AutoPILOT、3AGameFactory-Motion和GameAnything都避免让大模型逐帧或逐动作在线决策，而是让其生成控制器、参数程序或构建脚本。
2. **世界状态正在外置。** 规则、碰撞、计时、资产版本和验证结果由引擎或数据库保存；生成模型只处理难以手工制作的视觉、动作和候选未来。这比要求单一视频模型同时记住世界、执行规则和渲染像素更现实。
3. **视频世界模型开始分化为三种服务：** 视觉未来生成器、latent action提取器、终态/价值预测器。Native LAM和Fast-JEPA说明工业系统未必需要实际生成视频。
4. **4D表示成为感知与生成之间的协议候选。** Plan4D强调同步输出，WorldLatent强调统一潜空间；但目前仍缺少稳定API、通用资产格式和引擎插件。
5. **实时评测从FPS转向完整闭环。** 模型思考时间、控制器步频、环境时钟、调用预算和恢复能力必须一起测量。
6. **尚未解决：** 长时因果一致性、复杂多主体互动、跨镜头身份、低成本并发、沙箱安全、可观测性、模型版本回归，以及生成资产和改编内容的版权。
7. **需要降级看待：** WorldLatent和Native LAM方向重要，但尚无代码；AutoPILOT有强演示与数据，但框架未开放；任何把这些直接描述为“可部署产品”的说法都超出当前证据。

## 10. 论文精读候选

1. **[AutoPILOT](https://arxiv.org/abs/2611.28202)**  
   值得读：清楚展示harness比更换模型更能改变实时表现。重点读执行模式、AFK-Bench协议、成本与解码速度分析。与单步视觉agent相比，它把策略编译为持续运行的控制器。复现风险是代码尚未发布。

2. **[GameAnything](https://arxiv.org/abs/2611.29238)**  
   值得读：把coding agent、试玩critic、开发状态和经验记忆串成完整创作闭环。重点读系统架构、九种agent配置、玩法质量评测。复现风险来自不同商业游戏的模组接口和版权边界。

3. **[ViDecide](https://arxiv.org/abs/2611.26389)**  
   值得读：将视频生成重新定义为可验证决策过程。重点读视觉EOS、Corrective Fine-tuning及严格成功率定义。区别于普通视频RL，它从失败决策的噪声映射到模型自身成功延续。复现风险是高显存及非商业许可证。

4. **[Plan4D](https://arxiv.org/abs/2611.26516)**  
   值得读：提供从视频潜变量直接读出4D几何的明确系统路线。重点读三阶段训练、latent-to-token stitching和几何critic。复现风险是基础模型组合复杂、显存需求未明确。

5. **[World Models as Native Latent Action Models](https://arxiv.org/abs/2611.30898)**  
   值得读：说明世界模型的工业价值可能是动作表示，而不是视频输出。重点读dynamics registers、counterfactual temporal pairs和PAWlicy。复现风险最高：代码尚未开放，机器人对比需要复杂环境。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. AutoPILOT何时开放harness、沙箱和AFK-Bench环境，生成控制器是否有权限与资源限制。
2. Native LAM能否开放动作tokenizer、权重及跨机器人动作映射。
3. GameDojo的600小时轨迹、完整环境和GameDojo-4B训练代码是否按计划发布。
4. Plan4D在单卡上的实际显存、吞吐和几何漂移，能否支持交互式规划。
5. 3AGameFactory-Motion在非人形资产、复杂接触和引擎重定向中的失败率。
6. GameAnything跨版本回归是否能量化降低人工试玩时间与API成本。
7. Motion Mamba 2.0能否与语音、表情、视线及上半身手势模型低延迟组合。
8. 本周期缺失的身份授权、水印和可撤销数字人资产机制是否出现新的API级实现。

### 本周可动手实验

**实验一：模型控制器与逐动作调用对比**

- 目标：验证“LLM写一次闭环控制器”是否真正降低实时失败和API成本。
- 组件：浏览器小游戏、视觉模型、受限Python沙箱、`observe/act/handoff`接口。
- 难点：阻止任意文件/网络访问，并控制死循环。
- 成功判据：同一任务成功率不下降，模型调用量减少至少50%，控制循环保持5Hz以上。

**实验二：任意3D角色的程序化动作管线**

- 目标：验证3AGameFactory-Motion是否能处理非标准骨架。
- 组件：一个非人形GLB、3AGameFactory、Godot或Three.js。
- 难点：骨架拓扑、足部接触、动作重定向和自穿透。
- 成功判据：从文本要求到可播放资产全程无需手工改骨架，轨迹和重复次数满足指令。

**实验三：4D世界模型作为碰撞预检器**

- 目标：测试Plan4D输出是否比纯RGB视频更适合规划。
- 组件：Plan4D、短动作序列、点云/scene-flow碰撞检查脚本。
- 难点：尺度、相机坐标和动态点云噪声。
- 成功判据：在十组动作候选中，几何预检能稳定剔除明显碰撞，并与真实或引擎结果达到80%以上一致。

**实验四：自动游戏开发的回归闭环**

- 目标：评估GameAnything式“构建—试玩—视觉比较—修复”对制作效率的实际增益。
- 组件：支持模组的轻量游戏、coding agent、固定摄像机、截图差异与需求状态表。
- 难点：把审美反馈转成可验证条件，避免修复一处破坏另一处。
- 成功判据：三轮内完成五项明确需求，旧需求零回归，并保留可追踪的版本、证据截图和失败原因。
