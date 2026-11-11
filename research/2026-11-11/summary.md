# 数字人、世界模型与互动作品研究日报：2026-11-11

检索窗口：2026-11-05 至 2026-11-11（截至 09:00 CST）。已对照 11 月 4、5、6、9、10 日日报去重；日期采用官方公告或 arXiv 实际提交时间。

## 1. 本日摘要

本周期最显著的变化，是“代码”从外围编排工具进一步进入模型训练和世界状态构造：PhysOPSD 把可执行仿真代码当作物理监督，WorldFoundry 则试图统一模型发现、运行、产物和评测契约。  
世界模型评测也开始从单图起步的漂亮漫游，转向“能否忠实探索一段真实动态视频”：WanderBench 显示，八个受测模型都会随时间丢失源事件进程，且没有模型能稳定保持重访地点的一致性。  
SolarWM-H3 Memory 给出了较直接的工程解法：按相机位姿检索历史 latent，并以独立 Memory LoRA 为现有实时模型增添重访能力，无需重跑完整蒸馏链。  
机器人方向出现两条互补路线：RoboScript 让 agent 在模拟器中排练、把经验写入上下文；SWAM 则通过约 4.7 万小时语言—视频—动作数据，把视频先验继续训练成动作输出。  
数字人侧最重要的新增是 Nuance Labs 的 Nuplex 1.0：它将听、说、面部反馈和工具委派放进全双工会话，并同时开放受限 Beta API。  
这一进展比普通 talking head 更接近“持续运行的角色进程”，但其 89% 人类回复时机重合率来自厂商自建数据和自测，尚无独立评测。  
AI Studios 5.0 则代表更成熟但技术增量较弱的产品路线：脚本、场景、数字人、角色扮演、教练和面试被封装为可视化工作流。  
总体上，代码正承担物理真值、任务记忆、状态接口、评测和安全边界；生成模型承担视觉、语音和开放式表演；传统模拟器及运行时继续掌握可验证执行。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 11 月 10 日 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | Nuplex 把视觉 backchannel、全双工语音、工具委派和安全策略合并 | 从实时渲染推进到持续会话 Beta API | 上期偏重 Runway serving；本期转向自然 turn-taking 与角色治理 |
| 世界模型 | 高 | WanderBench 暴露动态探索失败；SolarWM 增加可检索历史 latent | 记忆、评测和动作适配出现可运行组件 | 从离屏因果推理扩展到重访、真实视频探索和机器人执行 |
| Coding × 互动作品 | 高 | PhysOPSD 让可执行代码直接监督生成模型；WorldFoundry统一运行产物 | 从单项目 harness 走向模型注册表、CI 和证据契约 | 上期是代码生成草模，本期是代码参与训练、评测和持续运行 |
| 工业应用 | 中 | Nuplex Beta、AI Studios 5.0 正式可用 | 数字人产品继续成熟；生成世界仍主要是研究系统 | 新增培训、面试和工具委派入口，但缺并发、SLA、客户 ROI |

## 3. 最值得关注的 11 个进展

### 1. WorldFoundry：为异构世界模型建立统一运行与证据层

- **类型／主线：**论文、GitHub、评测基础设施；世界模型＋Coding。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.10771)、[GitHub](https://github.com/OpenEnvision/WorldFoundry)、[文档与目录](https://openenvision.org/WorldFoundry/)，论文 11 月 7 日提交，项目 11 月 10 日公告。
- **相对上期：**首次收录。
- **贡献：**以声明式 manifest、模型适配器、持久产物和 evidence-aware scorecard 连接视频、3D/4D、VLA 与交互世界。论文记录 270 个模型、74 个 benchmark、62 个 metric；在线目录已增长到 275 个模型。
- **Coding 接口：**`worldfoundry-eval` CLI、Python 包、Docker/runtime recipe、可重评分产物契约。
- **场景：**世界模型回归测试、模型选型、内部 leaderboard、生成式运行时 CI。
- **成熟度／证据：**Apache-2.0、已开源；目录覆盖面强，但“登记可见”不等于每个模型均可运行。
- **重要性／阅读：**高／必读。

### 2. PhysOPSD：用可执行物理代码训练视频模型

- **类型／主线：**论文、研究框架；Coding × 视频生成。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.12411)、[GitHub](https://github.com/aigc3d/PhysOPSD)，11 月 9 日提交。
- **相对上期：**相较 Video-Coder“推理时先搭草模”，本工作把代码推进到训练期监督。
- **贡献：**coding agent 把现实物理情境转成可执行场景程序，渲染物理 draft，并通过 on-policy self-distillation 将物理约束蒸馏进视频模型。作者称仅使用 3K 样本即可优于 SFT/RL 基线。
- **Coding 接口：**PhysOPSD-Engine、仿真程序、参数化物理变化和可执行校验。
- **场景：**物理事件镜头、工业培训动画、产品行为预演。
- **成熟度／证据：**作者实验；仓库目前只有说明材料，训练代码仍“pending approval”，权重未开放。
- **重要性／阅读：**高／必读。

### 3. WanderBench / WanderLand：首次专门评测“从视频探索动态世界”

- **类型／主线：**benchmark、方法；世界模型。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.12557)，11 月 9 日提交。
- **贡献：**745 段 3–25 秒视频配合键盘相机路径，以 24 个经人工验证的子指标检查身份、事件进程、相机控制和重访一致性。八个受测模型都随视频增长而丢失源动态，且无一保持稳定重访。
- **Coding 接口：**相机 action 序列、源视频时间索引、历史静态内容检索；适合接入游戏输入和自动回放测试。
- **场景：**自由视角重拍、互动纪录片、real-to-sim、视频场景漫游。
- **成熟度／证据：**论文实测；WanderLand 对五个模型全维度改善，源动态最高提升 8.3 分，但代码/数据入口尚未在论文页明确开放。
- **重要性／阅读：**高／必读。

### 4. Nuplex 1.0 / Emma：原生全双工视听数字人进入 Beta API

- **类型／主线：**模型、产品、API、demo；数字人。
- **链接／日期：**[官方研究公告](https://home.nuancelabs.ai/research/meet-emma-the-first-nuplex-persona)，11 月 9 日。
- **贡献：**Nuplex 同时接收视听流并生成语音、表情、凝视和 backchannel。厂商在 1,785 个自录问题上报告，回复开始时间与原说话者重合 89%，下一名为 35%。
- **Coding 接口：**Beta API 支持 persona prompt，并可在角色继续交谈时把长任务交给外部工具；当前由平台托管 LLM。
- **场景：**企业接待、角色陪练、品牌主持人、持续在线数字员工。
- **成熟度／证据：**可运行 demo＋受限 Beta；指标为官方自测，未披露 FPS、端到端 P95、并发和价格。
- **安全：**可见水印、禁止自助克隆、真人脸声须授权、开发者 prompt 与会话双重审核；当前仅成人且排除医疗、未成年人教育等领域。
- **重要性／阅读：**高／必读。

### 5. World In-Context Learning / RoboScript：让 agent 在模拟世界中排练

- **类型／主线：**论文、GitHub、机器人系统；世界模型＋agent。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.12885)、[代码](https://github.com/ArcheOne-AI/RoboScript)，11 月 9 日。
- **贡献：**冻结基础模型，从一次示范开始，在模拟器中排练、反思并把经验冻结为 Task Memory，而非更新权重。作者报告 RoboTwin 2.0 的 50 个任务平均成功率 83.4%，较最强训练策略高逾 20 分。
- **Coding 接口：**模块化 agent、模拟器、任务记忆、评测与反思循环；Apache-2.0。
- **场景：**少样本工站换线、机器人教学、数字孪生中的任务验证。
- **成熟度／证据：**已开源研究系统，含双臂真机迁移；结果仍为作者实验。
- **重要性／阅读：**高／必读。

### 6. SWAM：从视频预训练扩展到语言—视频—动作联合模型

- **类型／主线：**论文、模型、数据、训练代码；世界动作模型。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.12870)，11 月 9 日。
- **贡献：**研究添加动作学习时如何避免破坏原有语言—视频对齐，数据池约 47,000 小时。作者报告 RoboTwin Easy/Hard 成功率分别为 88.10%/61.56%，三项真机任务为 40.00%，最强基线为 23.33%。
- **Coding 接口：**语言、视频和双臂动作统一数据接口；训练与动作执行管线。
- **场景：**机器人操作、合成数据生成、可视化动作预测。
- **成熟度／证据：**论文称数据、训练代码和权重已发布，但 arXiv 主页面未给出直接入口，复现入口仍需核验；真机绝对成功率仍低。
- **重要性／阅读：**高／必读。

### 7. SolarWM-H3 Memory：给实时世界模型外挂可检索记忆

- **类型／主线：**模型更新、权重、GitHub；世界模型。
- **链接／日期：**[技术页](https://solarx-team.github.io/SolarWM-H3-Memory/)、[GitHub](https://github.com/Junchao-cs/SolarWM)，11 月 5 日。
- **相对上期：**窗口内补录；此前日报只在 TouchéWorld 依赖项中提到 SolarWM，未覆盖本次 Memory 发布。
- **贡献：**历史帧 latent 与相机位姿写入 bank，生成新 chunk 前检索五个相关 latent 作为 memory token；最近五个 chunk 仍使用 KV cache。独立 Memory LoRA 可直接组合既有 Stage2。
- **指标：**作者在 720 个 Orbia 样例上报告 persistence 从 70.28 提升至 75.50，总分从 76.32 升至 77.46，并展示 180 秒 rollout。
- **Coding 接口：**相机 pose、latent bank、LoRA 组合与实时部署 CLI。
- **成熟度／证据：**代码与权重已开放；Apache-2.0 仅覆盖框架，MiniMax-H3 派生权重另受社区许可证约束。
- **重要性／阅读：**高／必读。

### 8. Library of Hands：不重训世界模型，只修正动作接口

- **类型／主线：**论文、机器人适配方法；世界模型。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.12678)，11 月 9 日。
- **贡献：**把相机或控制器变化视为“动作命令到真实动作”的映射变化，通过两帧观察反推该映射并存入可复用 library。作者报告三项模拟任务成功率从 28.6% 提升至 62.3%。
- **Coding 接口：**可作为 planner 与具体机器人控制器之间的 adapter；运行时读写配置身份和动作映射。
- **场景：**更换相机、控制器或末端执行器后的快速恢复。
- **成熟度／证据：**论文研究原型；无公开代码入口，主要为模拟闭环结果。
- **重要性／阅读：**中高／可读。

### 9. AnimWeb-Bench：从参考视频恢复可执行网页动画

- **类型／主线：**benchmark、dataset、代码；Coding × 互动作品。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.09769)、[项目入口](https://starry1001.github.io/AnimWeb-Bench-website/)，11 月 6 日。
- **贡献：**124 个真实网页、7 个领域、33 种动画；分别评测关键帧静态外观与组件位置、大小、速度和外观的动态一致性。14 个 MLLM 和三个 coding agent 的结果显示，显式逐帧检查和 render feedback 明显有益。
- **Coding 接口：**浏览器渲染、DOM/CSS/JS、视频帧检测、视觉回归测试。
- **场景：**互动品牌页、数字展览、动效原型和视频到 Web 的可编辑迁移。
- **成熟度／证据：**论文称代码和数据已开放；快速连续动画仍明显弱于离散状态切换。
- **重要性／阅读：**中高／可读。

### 10. FewStepRL：为少步扩散模型统一强化学习目标

- **类型／主线：**论文、代码、权重；实时生成基础设施。
- **链接／日期：**[论文](https://arxiv.org/abs/2611.08240)，11 月 5 日。
- **贡献：**把 FlowGRPO、AWM、DiffusionNFT、DiffusionOPSD 和 ReFL 转换到共同的少步预测形式，降低视频模型 RL 中反复 rollout 的成本。
- **Coding 接口：**模块化目标、anchor 和 reward 配置；已出现 SD3.5 与 Wan2.1 实现及权重。
- **场景：**为实时数字人和互动世界模型做物理、动作及偏好后训练。
- **成熟度／证据：**开源研究实现；目前主要验证生成 reward，不等同于已证明实时世界模型的长时稳定性。
- **重要性／阅读：**中／可读。

### 11. AI Studios 5.0：数字人被封装为视频 agent 与培训运行时

- **类型／主线：**正式产品；数字人＋创作工具。
- **链接／日期：**[官方发布页](https://www.aistudios.com/whats-new-ai-studios-5)、[计费说明](https://help.aistudios.com/en/articles/18242035-ai-studios-pricing-update)，11 月 9–10 日。
- **贡献：**Video Agent 从一句话、URL、文档或脚本创建场景、旁白和数字人；另提供 Role Play、Interactive Coaching、AI Interview、双 Avatar 和时间线。
- **Coding 接口：**当前以托管工作流、嵌入与现有 API 为主，未见开放状态机或底层 Avatar runtime SDK。
- **场景：**企业培训、招聘、课程教练和批量营销视频。
- **成熟度／证据：**正式产品；数字人视频为 9 credits/分钟，但实时交互成本、延迟和学习效果未披露。
- **重要性／阅读：**中／仅跟踪。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**Nuplex 的增量不在单图克隆，而在听说并行、凝视和倾听反应；AI Studios 仍属于单图 talking avatar 加工作流。
- **3D/4D 表示：**本周期暂无高质量新增。没有新的可独立验证的全身 4D Avatar、Gaussian 人体或实时 mesh 发布。
- **实时交互：**Nuplex 已可被打断并能在交谈中委派工具，但没有公布帧率、端到端延迟或弱网指标，不能与 Runway 的生产遥测直接比较。
- **音视频与情感：**Emma 采用真人演员授权数据塑造特定角色的小动作和情感，说明角色质量正从“通用情绪标签”转向定向表演与 post-training。
- **工程部署：**当前最容易落地的是封装式 API 和 SaaS；可自托管、可量化、可审计的全双工视听数字人仍稀缺。
- **安全与身份治理：**Nuplex 的可见水印、禁止自助克隆和授权定制比单纯内容审核更具体，但缺少可携带的授权凭证、C2PA 溯源和第三方深伪评测。

## 5. 世界模型进展

本周期形成了四层较清晰的技术栈：

1. **状态与记忆：**SolarWM 用位姿检索 latent；TT-MBRL 则尝试在测试时把单局记忆写入模型参数。前者适合视觉重访，后者目前主要是 Memory Maze 研究。
2. **可控生成：**WanderBench 证明相机可控不等于源事件可控；动态人物、事件进度和重访地点必须分别评测。
3. **动作接口：**Library of Hands 修正动作映射，QCC-WM 用宏观因果节点约束视觉变化，避免让像素生成器独自承担全部控制语义。
4. **策略学习：**RoboScript 把模拟器当作 agent 的排练环境；SWAM 把视频生成与动作预测合并进训练表示。

长时一致性的主流方向因而不是无限扩大 KV cache，而是“近期 cache＋可检索历史＋权威状态／任务记忆”。目前仍缺多人世界、复杂接触物理和数十分钟开放玩法的可靠证据。

## 6. Coding × 新型互动作品

### 链路一：代码监督的物理生成

物理需求 → coding agent 生成仿真程序 → simulator 渲染可校验 draft → PhysOPSD 蒸馏进视频模型 → 生成物理更可靠的互动镜头。

代码负责参数、碰撞和真值；模型负责最终视觉。已有论文证据，但发布代码尚未到位。

### 链路二：可重访的互动视频世界

源视频＋键盘/手柄动作 → 时间同步条件 → latent bank 按相机位姿检索历史 → 世界模型生成新视角 → 引擎校验事件和位置。

WanderBench 给出验收标准，SolarWM 提供一种记忆实现。组合成产品仍是本报告推断。

### 链路三：模拟排练驱动机器人 agent

一次真人示范 → RoboScript 在模拟器中排练 → 反思器修订 Task Memory → 冻结记忆部署到新场景 → 真机执行。

模拟器负责安全试错，agent 负责任务分解，传统控制器负责低层执行。已有开源和真机作者实验。

### 链路四：全双工数字员工

摄像头/麦克风 → Nuplex 持续感知与视听反馈 → LLM/persona 管理对话 → 后台工具执行长任务 → Avatar 在等待期间继续交流。

可构建接待、访谈和协作角色；已有 Beta API，但企业权限、审计和并发仍未披露。

### 链路五：视频到可编辑互动网页

参考动效视频 → coding agent 生成 DOM/CSS/JS → 浏览器逐帧渲染 → AnimWeb 指标比较位置、速度和外观 → agent 修正。

代码成为最终可交付资产，生成模型负责观察与改写；适合品牌互动页和数字展演，已有 benchmark，不等于可无人值守上线。

### 链路六：世界模型工程 CI

模型或权重更新 → WorldFoundry manifest 准备 runtime → 执行标准输入 → 保存视频、几何、动作和 trace → benchmark 重评分 → 发布证据卡。

这条链路不直接创造内容，却是生成世界从 demo 进入生产所必需的可观测与回归层。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展／技术栈 | 互动机制 | 成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | WanderLand＋SolarWM＋状态机 | 相机控制、重访、事件延续 | 研究原型 | 未披露 | 动态事件漂移、多人确定性 | [WanderBench](https://arxiv.org/abs/2611.12557) |
| 影视/虚拟制作 | PhysOPSD＋仿真程序＋视频模型 | 参数化物理镜头 | 论文原型 | 未披露 | 代码未开放、仿真偏差 | [论文](https://arxiv.org/abs/2611.12411) |
| 直播与电商 | Nuplex＋工具 API | 全双工主持、后台调用 | Beta | 未披露 | 并发、弱网、商品事实核验 | [Nuance](https://home.nuancelabs.ai/research/meet-emma-the-first-nuplex-persona) |
| 品牌互动 | AnimWeb＋浏览器运行时 | 视频动效转可点击网页 | benchmark | 浏览器侧，模型成本未披露 | 快速连续动画、可访问性 | [论文](https://arxiv.org/abs/2611.09769) |
| 教育培训 | AI Studios Role Play/Coaching | 角色对练、问答、评分 | 正式产品 | Avatar 导出 9 credits/分钟 | 教学效果与评分可靠性 | [产品页](https://www.aistudios.com/whats-new-ai-studios-5) |
| 企业数字员工 | Nuplex＋业务工具 | 交谈中委派后台任务 | 受限 Beta | 未披露 | 权限、审计、SLA | [API 公告](https://home.nuancelabs.ai/research/meet-emma-the-first-nuplex-persona) |
| 陪伴与社交 | Nuplex persona | 视觉 backchannel、长期对话 | 受限 Beta | 未披露 | 情感依赖、记忆治理 | [安全说明](https://home.nuancelabs.ai/research/meet-emma-the-first-nuplex-persona) |
| 空间计算 | SolarWM Memory | 自由视角与地点重访 | 已开源研究模型 | 33B，高显存；具体未披露 | 几何真值、移动端成本 | [技术页](https://solarx-team.github.io/SolarWM-H3-Memory/) |
| 机器人/仿真 | RoboScript、SWAM、LoH | 模拟排练、动作预测与适配 | 开源研究／真机作者实验 | 未披露 | 40% 真机成功率、安全认证 | [RoboScript](https://github.com/ArcheOne-AI/RoboScript) |

## 8. 可复现资源与开发者入口

| 资源 | 开放与许可证 | 硬件/成本 | 建议 | 最小验证路径 |
|---|---|---|---|---|
| [WorldFoundry](https://github.com/OpenEnvision/WorldFoundry) | Apache-2.0 | 取决于目标模型 | 高 | 先运行一个轻量视频 recipe，检查 request、artifact、scorecard |
| [RoboScript](https://github.com/ArcheOne-AI/RoboScript) | Apache-2.0 | 需模拟器与多模态模型 API/权重 | 高 | 单个 RoboTwin 任务完成示范—排练—Task Memory—评测 |
| [SolarWM-H3 Memory](https://github.com/Junchao-cs/SolarWM) | 代码 Apache；权重另有许可证 | 33B backbone，高显存 | 中高 | 固定 seed 做“离开—返回”，比较同一物体重访一致性 |
| [PhysOPSD](https://github.com/aigc3d/PhysOPSD) | 目前仅说明材料 | 未披露 | 暂缓 | 等待训练代码；现阶段只复核项目页案例和仿真程序设计 |
| [AnimWeb-Bench](https://starry1001.github.io/AnimWeb-Bench-website/) | 论文称代码与数据开放；许可证需核对 | 浏览器＋MLLM/API | 高 | 用一个 CSS transform 案例跑生成、截图和逐帧评分 |
| [JoVox](https://github.com/yuyujunjun/JoVox) | Apache-2.0，含权重 | A100 测试；依赖 TRELLIS.2 | 中 | 用内置图片生成 64³ voxel，再导出 PLY |
| [FewStepRL](https://arxiv.org/abs/2611.08240) | SD3.5/Wan 实现和权重已关联；逐项核许可证 | 视频训练成本高 | 中 | 先在小图像模型比较单一 reward 的两种 objective |
| [Nuplex Beta API](https://platform.nuancelabs.ai/) | 托管 Beta | 价格未披露 | 产品团队试用 | 记录用户停顿、Avatar backchannel、首音频和首视频响应时间 |
| [AI Studios 5.0](https://www.aistudios.com/whats-new-ai-studios-5) | 托管产品 | $29/月起；按 credits | 中 | 从同一培训文档生成视频，再配置 Role Play 并检查评分报告 |

## 9. 系统架构与技术趋势判断

明显升温的方向包括：

- **代码作为模型监督，而非只作为调用胶水。**
- **相机位姿索引的外部视觉记忆。**
- **语言—视频—动作联合表示。**
- **全双工数字人与异步工具执行。**
- **将生成产物、状态 trace 和评测证据纳入 CI。**

正在形成的可复用架构是：

`用户／传感器输入 → agent 与权威状态 → 代码或模拟器推演 → 世界模型预测 → 数字人／视频渲染 → 引擎执行与视觉测试 → 记忆写回`

关键未解问题包括动态事件重访、多人同步、动作接口跨硬件迁移、低于 500 ms 的完整视听响应、世界模型 reward hacking、长时身份稳定、可审计肖像授权及真实并发成本。

需要降级看待的信号包括：仅凭漂亮漫游自称“世界模型”；只报告平均 FPS 而不报告控制到显示延迟；论文声称开源但缺少可访问仓库；以及产品宣称情感智能却没有盲测、失败率或安全审计。

## 10. 论文精读候选

1. [PhysOPSD](https://arxiv.org/abs/2611.12411)：重点读 PhysOPSD-Engine、dual-level privilege 和 controlled variation。与 Video-Coder 的关键差异是代码用于训练期物理蒸馏；风险是代码 agent 自己生成并验证仿真。
2. [WanderBench](https://arxiv.org/abs/2611.12557)：重点读动态时间对齐、重访协议和 24 个子指标。价值在于区分静态场景保持与事件保持；风险是数据、代码入口尚不明确。
3. [WorldFoundry](https://arxiv.org/abs/2611.10771)：重点读 artifact contract、readiness 与 evidence scorecard。它解决的不是模型能力，而是跨模型证据可比性；复现风险主要来自第三方模型依赖。
4. [World In-Context Learning](https://arxiv.org/abs/2611.12885)：重点读 Task Memory、数据隔离和反思循环。差异是把学习结果写入上下文而非参数；风险是基础模型成本和模拟器覆盖不足。
5. [SWAM](https://arxiv.org/abs/2611.12870)：重点读数据配比、语言标注消融和 video/action 双分支。风险是 47K 小时来源与公开范围不完全等价，真机成功率也仍只有 40%。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. PhysOPSD 何时真正开放训练代码、3K 数据和权重。
2. WanderBench 是否开放 745 段视频、action route 和全部评测器。
3. WorldFoundry 的“runner verified”模型比例及跨硬件可复现率。
4. Nuplex 的 P50/P95 turn latency、并发价格和 API 事件协议。
5. SolarWM Memory 在动态人物、门窗状态和物体移动后的重访表现。
6. SWAM 的仓库、权重、47K 小时数据实际可下载范围及许可证。
7. RoboScript 是否能从双臂桌面任务扩展到移动操作与长时任务。
8. AI Studios 的 Role Play 分数是否与人工教练评价相关。

### 本周可动手的小实验

1. **重访记忆 A/B 测试**  
   目标：验证外部 latent memory 是否保持场景事实。组件：SolarWM-H3、固定相机轨迹、同一 seed。难点：严格匹配重访位姿。成功判据：20 个案例中，至少 15 个关键物体在返回后保持颜色、位置和数量。

2. **物理代码监督小实验**  
   目标：验证可执行代码是否比文本更适合作为视频物理真值。组件：Blender/PyBullet、coding agent、任一结构条件视频模型。难点：分离物理提升与视觉参考泄漏。成功判据：10 个碰撞/倾倒案例中，代码 draft 条件版本的事件顺序错误减少 30%。

3. **全双工数字人延迟与打断实验**  
   目标：测量 Nuplex 的真实 turn-taking。组件：Beta API、统一时间戳、20 组正常停顿和长停顿语料。难点：区分网络、语音和视频阶段。成功判据：得到 backchannel、首音频、首视频、打断生效的 P50/P95，并记录误抢话率。

4. **视频到互动网页闭环**  
   目标：复现 AnimWeb-Bench 的 render-feedback 优势。组件：浏览器自动化、coding agent、帧截图和组件跟踪。难点：字体、时间轴和 easing 对齐。成功判据：同一案例经过三轮视觉反馈后，关键帧误差和组件轨迹误差均下降至少 20%。
