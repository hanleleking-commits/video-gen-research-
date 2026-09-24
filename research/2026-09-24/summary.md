# 数字人、世界模型与互动作品研究日报：2026-09-24

检索窗口：2026-09-18 至 2026-09-24。

## 1. 本日摘要

过去七天最明确的数字人增量来自 Meta Muse Realtime Avatar：重点不再是单段口型质量，而是把语音、表情、全身动作、持续生成、量化部署和水印合并为一套实时服务。Meta 披露 448×768、25 FPS、约 870 ms 轮次响应延迟，以及单张 GB200 支持 12 路视频生成会话；这是强工程证据，但模型、SDK 和 Avatar API 尚未开放，产品可用范围也小于技术演示范围。世界模型侧没有出现新的通用可玩视频模型跃迁，主要增量转向机器人状态结构、跨视角一致性和“是否真正听懂指令”的评测。MachEmbodied-U0 将子任务、affordance、RGB、深度、法线、光流与动作联合建模；TriWorldBench 和 RoboFollow 则分别揭示多相机状态冲突和低场景熵造成的虚假成功率。Coding 主线更具落地性：Qwen-Live-Harness 把实时音视频对话、后台 coding agent、长期记忆和工具调用放进同一运行时；CraftBench-UE 则证明“代码或 Blueprint 能保存、能编译”不等于游戏机制真的能运行。OpenDM 的 9 月 23 日更新进一步给出数据转换、记忆 SFT、HTTP 推理和仿真评测的完整入口。总体趋势是：生成模型负责表演、预测和候选状态，代码运行时负责记忆、权限、结构化动作、验证与失败恢复，传统引擎或控制器继续持有权威状态。除 Meta 内部服务和开放开发工具外，本周期大多数进展仍是研究原型，尚无新的规模化客户部署证据。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | Muse 将全身实时表演、语音同步、4-bit serving 与视频水印合并 | 从单机 demo 推进到高并发内部产品服务；未开放 API | 新增 Muse；相较 AVTR-1，服务数据更完整但开放性更弱 |
| 世界模型 | 高 | 从“生成一段合理视频”转向联合状态预测、跨视角一致性和指令有效性 | 新增实机验证与可运行 benchmark，通用可玩世界无明显跃迁 | 新增 ME-U0、TriWorldBench、RoboFollow |
| Coding × 互动作品 | 高 | 实时多模态前台、后台 coding agent、引擎内执行验证开始形成标准闭环 | Qwen 运行时和 Unreal 基准均已开源 | 新增 Qwen-Live-Harness、CraftBench-UE、RoboDawn |
| 支撑基础设施 | 中高 | HTTP 动作服务、视频转 Skill、3D 场景显式坐标接口增强 | OpenDM 可部署性提高；Mira-Scene 仍缺训练代码 | 新增 OpenDM 9/23 更新、Mira-Scene |

## 3. 最值得关注的 9 个进展

### 1. Muse Realtime Avatar：面向持续对话的实时全身数字人

- **类型 / 日期 / 主线：**模型、产品能力、官方 demo；2026-09-23；数字人 / 实时运行时。
- **官方链接：**[Meta AI Research](https://research.meta.ai/blog/bringing-your-muse-to-life)。
- **相对上期：**首次收录。此前日报的 AVTR-1 强调开放权重和 WebRTC；Muse 新增的是 Meta 级 serving、并发和水印证据。
- **核心贡献：**共享 Muse Realtime Voice 的语音 VQ，使声音、口型和表情同步；因果 DiT 以短 chunk 连续生成，并把最新 latent 回写为下一段上下文。教师需每段 120 次模型评估，学生被蒸馏为两次无 CFG 评估，计算量降 60 倍。
- **Coding 接口：**内部运行时已有固定 KV cache、动态批处理和语音—视频编排，但未见公开 Avatar API、SDK 或自定义事件接口。
- **体验 / 场景：**可打断数字员工、陪伴角色、虚拟主持、直播导购；当前主要属于 Meta 产品体验，而非第三方可编程平台。
- **成熟度 / 证据：**正式产品能力与官方 demo；官方声称。448×768、25 FPS、约 870 ms turn-end-to-first-byte；单 GB200 12 路视频生成会话。
- **重要性 / 阅读：**高 / 必读。

### 2. Qwen3.8-Omni、Live Harness 与 MM Plugins：实时多模态 agent 运行时成形

- **类型 / 日期 / 主线：**模型、GitHub、插件、API 运行时；模型论文 2026-09-22，Harness v1.0 于 9 月 21 日发布；Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2609.25611)、[Qwen-Live-Harness](https://github.com/QwenLM/Qwen-Live-Harness)、[Qwen-MM-Plugins](https://github.com/QwenLM/Qwen-MM-Plugins)。
- **相对上期：**首次收录；9 月 22 日论文将此前分散的实时 API、插件和 Harness 统一为完整系统。
- **核心贡献：**前台维持实时音视频对话，后台委派 Qwen Code、Codex、Claude Code 等 coding agent，任务结果异步回到会话；同时提供长期记忆、主动视觉/声音监测和多后端适配。
- **Coding 接口：**Node/Electron、本地 WebSocket、ACP/agent adapter、MCP/Skill 插件；MM Plugins 本周期还新增物理硬件控制、视频空间推理，以及把演示视频转成可复用 Skill 的工具。
- **体验 / 场景：**“边讲边做”的桌面助手、语音导演、可观察直播现场并调度后台工具的制作 agent。
- **成熟度 / 证据：**Apache-2.0 开源可运行；macOS 桌面可用，模型推理依赖 DashScope，Windows/Linux 尚在推进。
- **重要性 / 阅读：**高 / 必读。

### 3. MachEmbodied-U0：理解、世界预测和动作生成进入同一模型

- **类型 / 日期 / 主线：**论文、世界—动作模型；2026-09-22；世界模型 / 机器人。
- **官方链接：**[arXiv 2609.25627](https://arxiv.org/abs/2609.25627)。
- **相对上期：**首次收录。
- **核心贡献：**以 Mixture-of-Transformers 连接理解和生成专家；子任务预测与 affordance grounding 约束 RGB、深度、表面法线、光流和动作的联合 flow matching。预训练使用约 4,200 小时机器人及第一视角演示。
- **Coding 接口：**结构上可向运行时输出子任务、交互位置、未来多模态状态和动作，但尚未发现公开推理代码或 checkpoint。
- **场景：**机械臂装配、视觉失败诊断、带可解释中间状态的机器人 UI。
- **成熟度 / 证据：**研究原型、含实机验证；论文实测。作者报告 LIBERO 99.0%、LIBERO-Plus 82.5%。
- **重要性 / 阅读：**高 / 必读。

### 4. TriWorldBench：多相机视频必须描述同一个世界

- **类型 / 日期 / 主线：**论文、benchmark、dataset、GitHub；论文 2026-09-22；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2609.26314)、[代码与数据](https://github.com/TriWorldBench/TriWorldBench)。
- **相对上期：**首次收录。仓库和挑战早于窗口发布，本次纳入原因是论文在窗口内正式发布并明确了三视角一致性方法。
- **核心贡献：**500 个双臂操作 episode、50 项任务、头部和左右腕部三路同步视频，用 19 个指标评估跨视角状态、任务对齐、物理/3D、运动和视觉质量。
- **Coding 接口：**标准化 `head.mp4 / left.mp4 / right.mp4` 输入、预处理脚本、批量评测和 leaderboard。
- **场景：**多相机机器人生成数据验收、合成轨迹 CI、数字孪生回放质量门禁。
- **成熟度 / 证据：**已开源 benchmark；论文与代码实测。
- **重要性 / 阅读：**高 / 必读。

### 5. RoboFollow：揭示机器人“指令遵循成功率”的假象

- **类型 / 日期 / 主线：**论文、benchmark、dataset、GitHub；2026-09-22；世界模型评测 / Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.25636)、[MIT 许可仓库](https://github.com/AutoLab-SAI-SJTU/RoboFollow)。
- **相对上期：**首次收录。
- **核心贡献：**指出许多场景只有一个可执行任务，策略即使忽略语言也可能高分。基准让相同场景承载多个动作分支，并用 L0—L3、Intent/Execution 分级判断模型是没听懂，还是执行失败。
- **Coding 接口：**统一 policy adapter、本地/TCP 推理、episode 级原因和可选视频输出，适合作为 agent 回归测试。
- **场景：**自然语言机器人、生成式 NPC 指令系统、培训模拟中的规则遵循测试。
- **成熟度 / 证据：**代码与数据已开放；论文评测九种 VLA/WAM，常见强化方法仍未消除高阶退化。
- **重要性 / 阅读：**高 / 必读。

### 6. CraftBench-UE：coding agent 必须通过真实游戏运行测试

- **类型 / 日期 / 主线：**论文、GitHub、benchmark；2026-09-19；Coding × 游戏。
- **官方链接：**[论文](https://arxiv.org/abs/2609.23142)、[MIT 许可仓库](https://github.com/ramenvr/craftbench-ue)。
- **相对上期：**首次收录；论文发布后仓库已提供 117 项公开任务，构成实质性开源增量。
- **核心贡献：**在全新 Unreal 5.8 工程中重建 agent 提交物，并以编译、资产结构和固定步长 PIE 断言确定性验收。相同玩法任务中，C++ 完成率比 Blueprint 高 30.0—42.9 个百分点；大量通过资产检查的 Blueprint 仍在运行时失败。
- **Coding 接口：**Unreal MCP、C++/Blueprint/Python、无 LLM judge 的 runtime verifier。
- **场景：**自动生成玩法、NPC 逻辑、关卡脚本和互动展项的 CI。
- **成熟度 / 证据：**已开源 benchmark；论文实测。需自行安装 UE 5.8，Epic 资产仍受 EULA 约束。
- **重要性 / 阅读：**高 / 必读。

### 7. RoboDawn：把机器人变成可由 agent 调用的语义工具

- **类型 / 日期 / 主线：**论文、交互 demo；2026-09-19；Coding × 机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2609.22966)、[项目页与轨迹回放](https://robodawn.top/)。
- **相对上期：**首次收录。
- **核心贡献：**冻结 VLM 通过 `move / rotate / point / gripper / home / wait` 等离散命令闭环控制机器人；单个上下文示例使 RoboTwin 2.0 C2R 从 53.2% 提升至 73.6%。
- **Coding 接口：**多视角观察、结构化命令、运动规划器、执行反馈和 scratchpad memory，形式上接近机器人 function calling。
- **场景：**低频实验机器人、语音示教、可解释遥操作；不适合高频精密控制。
- **成熟度 / 证据：**研究原型、含 Franka 实机结果；代码仍标注 coming soon。一次推理约 9.74 秒，碰撞与精细接触仍是明显风险。
- **重要性 / 阅读：**中高 / 可读。

### 8. OpenDM 9/23 更新：记忆型机器人策略开放完整工程路径

- **类型 / 日期 / 主线：**GitHub、模型权重、训练/推理框架；2026-09-23；Coding × 工业机器人。
- **官方链接：**[OpenDM 仓库](https://github.com/dexmal/opendm)。
- **相对上期：**DM0.5 本身早于窗口；本次因新增 XPolicyLab policy、RoboDojo 数据转换、DM05-MEM SFT 与仿真评测而纳入。
- **核心贡献：**补齐数据注册、归一化、LoRA/全量 SFT、checkpoint、TensorRT 快速后端和 RoboDojo 评测。
- **Coding 接口：**`/v1/infer` HTTP 接口接受头部/双腕图像和 14 维状态，输出 action chunk；便于接入 ROS、仿真器和安全控制器。
- **场景：**多臂操作、长任务记忆、企业自有机器人数据微调。
- **成熟度 / 证据：**Apache-2.0、权重与代码可运行；官方 benchmark。推理需一张 NVIDIA GPU，训练建议八卡。
- **重要性 / 阅读：**高 / 必读。

### 9. Mira-Scene：单图变为对象级可编辑 3D 场景

- **类型 / 日期 / 主线：**论文、GitHub、3D 表示；2026-09-20；世界模型支撑基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.23796)、[推理仓库](https://github.com/VAST-AI-Research/Mira-Scene)。
- **相对上期：**首次收录。
- **核心贡献：**Canonical Coordinate Map 将可见像素映射到对象规范空间，再与场景点云建立稠密对应，恢复对象尺度与位姿。作者报告相对 SAM3D，3D-IoU 提升 39.8%、2D-IoU 提升 16.5%。
- **Coding 接口：**分阶段、可恢复、多后端、多 GPU 的场景装配管线；对象级输出比纯视频更适合引擎编辑。
- **场景：**照片转关卡草稿、数字孪生、虚拟制作布景、机器人仿真资产。
- **成熟度 / 证据：**推理与评测代码已开放，训练代码和示例训练数据仍在 ToDo；仓库未明确展示许可证，商用前需核验。
- **重要性 / 阅读：**中高 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期唯一达到高重要性的新增是 Muse。它使用同一语音 token 同时驱动声音和视觉，减少独立 TTS、口型和动作模块之间的同步误差。
- **3D/4D 表示：**暂无新的高质量 3D/4D avatar 模型。Mira-Scene 面向一般对象和场景，不应包装为人物重建能力。
- **实时交互：**Muse 已覆盖脸、手势和全身运动，并能在固定容量上下文中持续生成。与上一期开放的 [AVTR-1](https://github.com/avaturn-live/avtr-1) 相比，Muse 的并发与服务数据更可信，但第三方可编程性更弱。
- **音视频与情感：**语音 VQ 同时编码“说什么”和“怎么说”，比文本或声学幅度驱动更接近韵律—表情联合控制；仍未见显式情绪、视线或手势 API。
- **工程部署：**4-bit 量化感知训练、CUDA Graph、融合 kernel、动态批处理和 persistent KV cache 表明瓶颈已经转向 serving，而不只是模型 FPS。
- **安全与身份治理：**Muse 使用 Meta Video Seal 在实时视频中嵌入不可见水印，且官方称不增加延迟；但肖像授权验证、可撤销身份凭证和第三方审计仍未披露。
- **产品判断：**Muse 可视为正式产品能力，不等于开放数字人平台；其博客明确提示演示中的角色并非全部可在 Muse 应用中使用。

## 5. 世界模型进展

- **架构与训练：**ME-U0 的关键变化是用子任务和 affordance 约束世界预测，使 RGB、几何、运动和动作共享训练目标。
- **可控 / 可玩生成：**本周期暂无新的高质量通用可玩视频模型。相比上期 WorldCrafter，增量集中在机器人而非游戏世界。
- **长时与物理一致性：**OpenDM 用显式 memory SFT 服务长任务；RoboDawn 依靠交互历史恢复失败。两者都仍缺严格的小时级稳定性测试。
- **空间表示：**深度、法线、光流、CCM、点云和三相机状态正在成为比 RGB 单流更可靠的程序接口。
- **机器人 / 仿真：**ME-U0、RoboDawn、OpenDM 分别代表端到端联合模型、VLM 工具调用和可部署策略服务三种路线。
- **实时部署：**ME-U0 未披露控制周期；RoboDawn 明显慢于实时控制；OpenDM 已有 TensorRT backend，但未披露统一硬件下的端到端 p95。
- **评测：**TriWorldBench 负责判断多相机是否描述同一状态，RoboFollow 判断语言是否真的改变动作选择；二者比普通视频美学指标更接近工业风险。

## 6. Coding × 新型互动作品

### 链路一：持续会话数字角色

**用户语音 → 对话模型产生语音 VQ → Muse Avatar 生成同步表演 → RTC/产品前端 → 数字员工或陪伴角色**

模型负责声音和视觉表演，代码负责会话状态、权限、工具调用、打断、审核和失败降级。目前 Meta 已有可运行产品证据，但第三方无法直接调用 Avatar API；外部团队只能把该架构作为设计参考。

### 链路二：会观察、会委派工作的互动主持人

**摄像头/屏幕/麦克风 → Qwen-Live-Harness → 实时对话 → 后台 coding agent/MCP → 结果回传与长期记忆**

前台模型维持自然交流，后台 agent 执行耗时任务；Harness 负责调度、审批、取消和记忆。若再接入独立 avatar renderer，可形成会观察直播现场、调用业务系统并口头汇报的虚拟主持人。Harness 已可运行，avatar 组合属于**本报告推断**。

### 链路三：可验证的生成式游戏开发

**自然语言玩法 → coding agent → Unreal MCP/C++/Blueprint → PIE 固定步长测试 → 失败定位与迭代**

agent 负责生成代码和资产，Unreal 负责物理与权威状态，CraftBench 式 verifier 判断玩法是否真的成立。适合生成 NPC 能力、互动装置和小型关卡；论文同时表明 Blueprint 自动化仍比 C++ 脆弱。

### 链路四：显式中间状态的机器人世界—动作系统

**多相机观察 → ME-U0 预测子任务、affordance 与未来几何 → 安全检查 → 控制器执行**

模型负责语义理解和候选未来；代码检查工作空间、碰撞、置信度和任务阶段；控制器负责高频执行。ME-U0 有作者实验，但无公开运行时，完整工业闭环仍属研究原型。

### 链路五：function calling 式机器人控制

**VLM 观察 → RoboDawn 离散命令 → 运动规划器 → 执行反馈 → memory 更新**

其价值在于动作可读、可记录、可拒绝，也可由人类插入审批。适合低频示教和实验室机器人；9.74 秒级推理、碰撞和接触精度使其暂不适合生产节拍。

### 链路六：照片生成可编排的互动场景

**单图 → 分割/深度/CCM → 对象 mesh 与位姿 → Unity/Unreal/仿真器 → 规则、碰撞和交互脚本**

Mira-Scene 负责把像素变成对象级资产，代码负责实体 ID、碰撞体、状态机和行为。可用于品牌展馆、影视预演和机器人数字孪生；当前需要多模型拼接和人工验收。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | CraftBench-UE + Unreal MCP + PIE | 自然语言生成玩法并在引擎中验证 | 开源 benchmark | 单任务预算 40 分钟 | Blueprint 行为错误、资产 EULA | [论文](https://arxiv.org/abs/2609.23142) |
| 影视/虚拟制作 | Muse + Mira-Scene | 实时角色表演、照片转可编辑布景 | 产品 demo + 研究管线 | Muse 约 870 ms；3D 成本未披露 | API、许可、长镜头一致性 | [Muse](https://research.meta.ai/blog/bringing-your-muse-to-life) |
| 直播与电商 | Muse / Qwen Harness / RTC | 语音互动、工具调用、后台任务 | 组件可用，组合未验证 | 25 FPS；业务并发未披露 | Avatar API、审核、品牌身份授权 | [Qwen Harness](https://github.com/QwenLM/Qwen-Live-Harness) |
| 品牌互动 | Mira-Scene + 游戏引擎 | 用户探索对象级生成场景 | 研究原型 | 未披露 | 商业许可、资产清理 | [仓库](https://github.com/VAST-AI-Research/Mira-Scene) |
| 教育培训 | Qwen Harness + 数字人 renderer | 实时讲解、看屏幕、委派工具 | Harness 可运行，数字人组合推断 | API 费用未披露 | 事实可靠性、课堂隐私 | [论文](https://arxiv.org/abs/2609.25611) |
| 企业数字员工 | Muse / Qwen-Live-Harness | 可打断对话、长期记忆、业务工具 | 产品能力至开源运行时 | Muse 约 870 ms | SLA、数据隔离、形象授权 | [Meta](https://research.meta.ai/blog/bringing-your-muse-to-life) |
| 陪伴与社交 | Muse | 持续语音、表情和全身反应 | 官方产品体验 | 单 GB200 12 路视频会话 | 情绪安全、成本、长期记忆 | [官方博客](https://research.meta.ai/blog/bringing-your-muse-to-life) |
| 空间计算 | Mira-Scene + WebXR/引擎 | 单图重建、对象级编辑 | 可运行研究管线 | 未披露 | 尺度、动态人物、设备算力 | [论文](https://arxiv.org/abs/2609.23796) |
| 机器人/仿真 | ME-U0、OpenDM、TriWorldBench | 多视角预测、动作服务、自动验收 | 实机研究验证至开放部署栈 | OpenDM 一卡推理；p95 未披露 | 安全、精细接触、跨本体标定 | [OpenDM](https://github.com/dexmal/opendm) |

## 8. 可复现资源与开发者入口

| 资源 | 开放与许可 | 硬件/成本 | 最小验证路径 | 建议 |
|---|---|---|---|---|
| [Qwen-Live-Harness](https://github.com/QwenLM/Qwen-Live-Harness) | Apache-2.0；依赖 DashScope API | macOS、Node 22.13+；无需本地 GPU | 连接一个后台 coding agent，测试语音打断、委派、取消和结果回传 | **优先复现** |
| [Qwen-MM-Plugins](https://github.com/QwenLM/Qwen-MM-Plugins) | Apache-2.0；Skill + MCP | 部分能力需云 API、本地 Blender/FreeCAD | 用演示视频生成 Skill，再让 agent 重放一次工作流 | **优先复现** |
| [CraftBench-UE](https://github.com/ramenvr/craftbench-ue) | 自有代码 MIT；Epic 内容受 UE EULA | UE 5.8、Python 3.11+ | 先运行 smoke task，再比较 C++ 与 Blueprint 同玩法任务 | **优先复现** |
| [OpenDM](https://github.com/dexmal/opendm) | Apache-2.0；权重另查模型卡 | 推理一张 4090/A100/H100/H20；训练建议八卡 | 启动 `/v1/infer`，用三路图像跑一个 RoboDojo episode | **优先复现** |
| [RoboFollow](https://github.com/AutoLab-SAI-SJTU/RoboFollow) | MIT；第三方资产按上游许可 | RoboTwin 仿真环境、GPU | 在同一场景给出两条不同指令，比较 Intent/Execution | 值得复现 |
| [TriWorldBench](https://github.com/TriWorldBench/TriWorldBench) | 代码和数据可取；未见主许可证声明 | 官方 val100：8×A100 40GB 约两小时 | 先限制 10 个 episode，检查三视角抓取状态是否一致 | 成本较高，适合评测团队 |
| [Mira-Scene](https://github.com/VAST-AI-Research/Mira-Scene) | 推理/评测开放；许可证待核验 | 多阶段模型、多 GPU 可分片 | 跑单张室内图，导出对象 mesh、位姿并导入引擎 | 研究验证后再商用 |
| ME-U0 / RoboDawn / Muse Avatar | 未开放完整代码或 API | 未披露或依赖服务方基础设施 | 仅能审阅论文、轨迹和 demo | 暂不安排核心复现 |

## 9. 系统架构与技术趋势判断

1. **实时数字人开始成为媒体系统问题。** 模型质量之外，KV cache、动态批处理、量化、共享语音 token、RTC、审核和水印共同决定能否上线。
2. **世界模型输出正在结构化。** 子任务、affordance、深度、法线、光流、CCM 和多视角状态比“未来 RGB 看起来合理”更容易被代码消费和验证。
3. **主流系统趋向五层：**`感知与事件 → agent/规划 → 生成或预测模型 → verifier/安全门 → 引擎或控制器`。
4. **coding agent 正从写文件转向控制真实运行时。** Qwen Harness 管理长任务，Unreal MCP 操作引擎，CraftBench 以真实游戏行为验收。
5. **传统引擎仍持有权威状态。** 生命值、库存、碰撞、支付、机器人关节限制和急停不应交给生成视频隐式维护。
6. **评测重心从画质转向条件是否有效。** TriWorldBench 检查不同视角是否同世界；RoboFollow 检查语言是否真正改变行为；CraftBench 检查生成资产是否真的实现玩法。
7. **未解决问题：**开放 Avatar API、多人同步、长时身份和物体绑定、端到端 p95、每并发成本、机器人安全认证、数据及肖像许可。
8. **需降级看待：**Muse 的技术 demo 大于当前用户可用范围；ME-U0 暂无公开实现；RoboDawn 的高成功率以慢速推理换取；Mira-Scene 缺训练代码和明确许可证。

## 10. 论文精读候选

1. **[MachEmbodied-U0](https://arxiv.org/abs/2609.25627)**  
   值得读：理解、世界状态和动作如何在同一 MoT 中交互。重点看多模态动态监督、MRPE、affordance 消融和实机部分。风险是无代码、控制周期未披露。

2. **[TriWorldBench](https://arxiv.org/abs/2609.26314)**  
   值得读：单视角高分为何不能证明世界状态正确。重点看 19 项指标、跨视角语义判断和 TWB-Score 聚合。风险是评测成本高，部分指标依赖视觉模型。

3. **[RoboFollow](https://arxiv.org/abs/2609.25636)**  
   值得读：解释 benchmark 低场景熵如何夸大指令遵循。重点看 L0—L3 构造、Intent/Execution 分离和九种策略对比。复现依赖 RoboTwin 资产。

4. **[CraftBench-UE](https://arxiv.org/abs/2609.23142)**  
   值得读：把 coding agent 验收从“编译成功”推进到运行时语义。重点看第 3 节 harness、第 5 节 Blueprint 失败和 runtime assertion。风险是 UE 环境重、部分 MCP 为专有实现。

5. **[RoboDawn](https://arxiv.org/abs/2609.22966)**  
   值得读：结构化工具接口能否替代任务特定 action head。重点看 command grammar、ICL、test-time scaling 与失败分析。风险是推理极慢且代码未开放。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. Muse Realtime Avatar 是否开放 API、SDK、自定义角色上传及计费。
2. Muse 的 870 ms 指标在网络波动、打断和多人会话下能否维持。
3. ME-U0 是否发布权重、推理代码和真实控制频率。
4. WorldCrafter、CausalWM 等上一期项目是否补齐对象级状态或运行时 demo。
5. TriWorldBench 能否成为 WAM 训练后固定 CI，而不仅是一次性榜单。
6. RoboFollow 上的性能退化究竟来自语言理解、视觉 grounding 还是动作 head。
7. CraftBench-UE 是否扩展到网络同步、NPC 行为树、动画与 Niagara。
8. OpenDM 快速后端的端到端 p50/p95、动作安全和跨本体迁移成本。

### 本周可动手实验

1. **实时 agent—数字人分层原型**  
   目标：验证前台语音不中断、后台 coding 任务可持续。组件：Qwen-Live-Harness、任一 RTC/avatar 服务、简单业务工具。难点：共享会话时钟与打断。成功判据：后台任务运行时前台仍能对话，任务完成后由角色主动播报。

2. **Unreal 生成玩法的确定性 CI**  
   目标：比较 agent 生成 C++ 与 Blueprint 的可靠性。组件：CraftBench-UE、UE 5.8、同一玩法的两种交付形式。难点：环境和 Epic 资产恢复。成功判据：连续两次重建和 PIE 测试结果一致，失败能定位到具体状态断言。

3. **三视角合成数据验收**  
   目标：检查视频世界模型是否在三路相机中保持同一抓取状态。组件：任一动作条件视频模型、TriWorldBench 10 个 episode 子集。难点：相机与帧同步。成功判据：至少输出每个 episode 的跨视角错误类型，而非只报告平均画质。

4. **语义动作接口安全壳**  
   目标：复现 RoboDawn 式结构化调用，但不直接运行真实机械臂。组件：VLM、RoboTwin/OpenDM、命令 schema、碰撞和范围检查器。难点：命令粒度及失败反馈。成功判据：所有越界或碰撞命令在执行前被拒绝，且 agent 能基于拒绝原因重新规划。
