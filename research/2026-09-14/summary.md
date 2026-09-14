# 数字人、世界模型与互动作品研究日报：2026-09-14

检索窗口：2026-09-08 至 2026-09-14。  
信息截点：2026-09-14 09:50（Asia/Shanghai）。发布日期以 arXiv 提交记录、官方项目页及代码/模型仓库为准；已与 9 月 8—11 日日报去重，“相对上期”指相对 2026-09-11 日报。

## 1. 本日摘要

过去三天最重要的新增是 Vidu S2：实时数字人从“语音驱动头像”推进到 720p、25—42 FPS、运行中动态换参考图、全身舞蹈指令、实时换装/换人/换背景，并开始输出双目空间视频。它是本周期数字人、世界生成与互动运行时结合最紧密的项目，但 S2 API 和完整开发文档仍标为 coming soon，当前可验证形态主要是论文和在线 demo。

世界模型侧出现两条互补路线。World in World 不重新训练模型，而是把源视频、深度、相机轨迹、历史记忆统一成冻结视频模型可读取的视觉证据；首批代码已开放，但 360°、长时记忆和流式生成仍在 roadmap。MaP-WAM 则把长历史压缩成语言—视觉计划，让机器人执行器保持固定上下文，说明“历史供规划使用、短上下文负责实时执行”正在成为长时 agent 的共同设计。

OpenWAM 补齐了训练、推理、部署、八个仿真 benchmark 和多种真实机器人配置，是本周期最完整的世界—动作模型开源栈。数字人资产侧，AvaImg 把任意服装人体扫描注册为统一 SMPL-X+D、UV 纹理和位移图，为用成熟 2D 图像模型生成或编辑 3D avatar 建立了可计算中间格式；MOCO 则尝试同时接受语音、文本和轨迹，解决角色“边走、边说、边做手势”的动作冲突。

基础设施层，ZipCodec 将流式语音压到 6.25 Hz、0.80 kbps，并在消费级 CPU 上实时运行；这对语音 agent、数字人跨端传输和长会话状态压缩有直接价值。总体趋势已经不是让单一模型包办状态、表演和交互，而是形成“代码维护状态与权限—agent 规划—生成模型渲染/预测—传统运行时同步与校验”的分层架构。除 Vidu 在线服务和真实机器人实验外，本周期仍缺少生产 SLA、规模化客户部署、多人并发成本及第三方长期稳定性验证。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | Vidu S2 将实时生成、动态参考、流式编辑和空间视频合并 | 从 talking head demo 推进到可玩的全身互动视频；API 尚未开放 | 新增 Vidu S2、AvaImg、MOCO |
| 世界模型 | 高 | 视觉证据接口与“Memory-as-Plans”分别解决相机控制和长历史成本 | World in World 已部分开源；MaP-WAM 仍待代码 | 新增 World in World、MaP-WAM |
| Coding × 互动作品 | 高 | JSON 相机脚本、模块化 WAM、VLM 反馈循环成为明确接口 | 局部组件可运行，端到端生产合同仍不完整 | OpenWAM 与 World in World 提供新开发入口 |
| 实时运行时 | 高 | 720p 25—42 FPS生成、流式编辑、低帧率语音 codec | 视频实时性显著提高；端到端 p95 延迟未披露 | 新增 Vidu S2、ZipCodec |
| 工业应用 | 中高 | 虚拟试衣、直播角色、空间视频和机器人记忆任务均出现运行证据 | 在线 demo / 真实机器人实验，未到规模化部署 | 比上期更接近创作与生产流程 |
| 安全与治理 | 低 | 本周期无新的高质量身份授权、水印或检测进展 | 无实质提升 | 动态换脸、换装反而扩大授权和审核压力 |

## 3. 最值得关注的 10 个进展

### 1. Vidu S2：实时数字人、实时视频编辑与空间视频合流

- **类型 / 日期 / 主线：**模型、论文、在线 demo；2026-09-10；数字人、世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11638)、[Vidu S 仓库](https://github.com/shengshu-ai/Vidu-S)、[在线服务与 API 平台](https://platform.vidu.com/)。
- **相对上期：**新增。
- **核心贡献：**S2-Avatar 用 Self-Replay Forcing 缓解流式自回归误差，并以单步 Refiner 将实时输出从 540p 提升至 720p，官方报告保持 25—42 FPS；运行中可新增人物、服装、物体或背景参考图。S2-Editing 以 frame-aligned attention 实时执行风格化、换装、换人和换背景；双目转换管线进一步输出空间视频。[论文方法与运行时说明](https://arxiv.org/html/2609.11638)
- **Coding 接口：**内部由 VLM agent 把语音/文本、当前帧和参考图编译为身份、视线、姿态、动作及持有物状态 prompt，再观察生成帧并修正后续 prompt；现有仓库显示 S2 API、指南和 quickstart 尚未发布。
- **场景：**可打断虚拟主播、实时虚拟试衣、互动演出、视频通话角色替换、VR 陪伴与直播特效。
- **成熟度 / 证据：**可玩在线 demo；论文实测和官方声称。尚无 S2 权重、SDK、并发、价格或 SLA。
- **重要性 / 阅读：**高 / 必读。

### 2. World in World：把普通视频变成可脚本化重拍的“世界”

- **类型 / 日期 / 主线：**论文、GitHub、demo；2026-09-10；世界模型、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11548)、[项目页](https://chenxi-song.github.io/worldinworld/)、[代码](https://github.com/Westlake-AGI-Lab/WorldinWorld)。
- **相对上期：**新增。
- **核心贡献：**将源帧、深度重投影、几何代理和缓存外历史转换为带相机与时间标签的视觉证据，通过 correspondence-guided attention 和分证据 CFG 接入冻结的因果视频模型，无需重新训练。
- **Coding 接口：**开发者用 `config.json`、相机 pose 或 `arc/rot/ring/truck` 等轨迹 DSL 控制重拍；已开放自由相机、bullet time、首帧编辑传播和命令行工具。
- **工程边界：**当前 14B 路径需要约 86 GB 权重、单张 80 GB A100，峰值约 72 GB；示例 15×16 帧生成约 5 分钟。360°人体代理、frustum memory、流式交互和动作迁移尚未开放。[仓库复现说明](https://github.com/Westlake-AGI-Lab/WorldinWorld)
- **场景：**互动电影摄影机、历史视频再布景、镜头预演、虚拟制作和可探索档案。
- **成熟度 / 证据：**部分代码已开源，CC BY-NC-SA 4.0；论文实测。
- **重要性 / 阅读：**高 / 必读。

### 3. OpenWAM：世界—动作模型进入可组合的开源工程栈

- **类型 / 日期 / 主线：**论文、GitHub、模型权重、训练与评测框架；代码/权重 2026-09-06，论文 09-07；世界模型、机器人、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.07398)、[官方仓库](https://github.com/OpenWAM-Official/OpenWAM)、[模型集合](https://huggingface.co/OpenWAM)。
- **相对上期：**本轮补录；此前日报未覆盖，纳入原因是完整代码、46 个关联模型条目与部署配置已可核验。
- **核心贡献：**把视频 backbone、latent 表示、world/action 双流、attention mask、联合去噪、训练和评测拆成可替换模块；OpenWAM-α 使用约 6,400 小时、5.185 亿帧人类第一视角和机器人数据。
- **Coding 接口：**Hydra YAML 组合训练和部署任务，统一八个仿真 benchmark，并支持单臂、双臂和灵巧手配置。
- **场景：**机器人策略预训练、sim-to-real 对照实验、动作表征消融与合成 rollout。
- **成熟度 / 证据：**Apache-2.0、代码与预训练/后训练权重已开放；性能为论文实测，规模化工业部署未证明。
- **重要性 / 阅读：**高 / 必读。

### 4. MaP-WAM：长时记忆不再反复进入实时执行器

- **类型 / 日期 / 主线：**论文、项目 demo、真实机器人实验；2026-09-10；世界模型、机器人运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11561)、[项目页](https://sizhezhao.github.io/projects/MaP-WAM/)、[仓库](https://github.com/aipixel/MaP-WAM)。
- **相对上期：**新增。
- **核心贡献：**把完成的任务片段保存为指令加稀疏视觉证据；VLM 和因果世界模型将历史转成下一段语言—视觉计划，WAP 执行器联合预测动作块与进度，并用计划—观察对齐校正漂移。
- **关键结果：**作者报告 RMBench 平均成功率 83.3%，强基线 LingBot-VA 为 77.1%；Franka 真实机器人两项记忆任务平均 78.0%，执行器上下文及单块推理成本近似恒定。[逐任务结果](https://sizhezhao.github.io/projects/MaP-WAM/)
- **Coding 接口：**适合实现为 `episodic store → planner → cached plan → action executor → progress event → replan` 状态机。
- **成熟度 / 证据：**研究原型、论文实测；仓库当前只有 README，训练代码和 checkpoint 未发布。
- **重要性 / 阅读：**高 / 必读。

### 5. AvaImg：把服装人体变成图像模型可处理的标准化 3D 资产

- **类型 / 日期 / 主线：**论文、资产管线；2026-09-10；3D avatar、支撑基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11722)、[作者项目入口](https://yuxuan-xue.com/avaimg)。
- **相对上期：**新增。
- **核心贡献：**将任意服装人体扫描注册为 SMPL(-X)+D，并输出统一 UV 纹理与位移图；signed winding number 约束裸体 mesh 位于服装内部，三级加速使 winding 计算约快 10 倍、存储减少约 95%。
- **关键结果：**六个数据集上平均表面 Chamfer 为 2.62 mm，纹理重渲染 PSNR 34.48 dB；通过冻结 FLUX VAE 往返后只新增 0.76 mm Chamfer 误差。[论文结果表](https://arxiv.org/abs/2609.11722)
- **Coding 接口：**UV texture/displacement 是标准二维张量，可直接接图像 VAE、diffusion、inpainting 和版本化资产流水线。
- **场景：**游戏角色换装、数字服装库、虚拟制作扫描资产清洗、可生成 3D 人体数据集。
- **成熟度 / 证据：**论文实测；代码、数据和 Singularity 容器声明将开放，当前应按待发布处理。
- **重要性 / 阅读：**高 / 必读。

### 6. MOCO：让文本、语音和轨迹同时控制 3D 角色

- **类型 / 日期 / 主线：**论文、3D motion 模型；2026-09-10；数字人、互动角色。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11439)。
- **相对上期：**新增。
- **核心贡献：**不同模态无需逐帧对齐：每个扩散步骤分别生成语音驱动上身动作、文本驱动全身/下身动作及轨迹约束，再按身体区域组装、重新加噪并迭代协调。
- **Coding 接口：**开发者可把角色行为拆成语音、动作语义和导航轨迹三个并行控制通道，再通过骨骼区域 mask 或规则合成。
- **场景：**边走边讲解的 NPC、虚拟主持、教育讲师、舞台数字演员。
- **成熟度 / 证据：**研究原型、作者 benchmark；截至截点未找到可核验的官方代码仓库。
- **重要性 / 阅读：**中高 / 必读。

### 7. DIAL：利用 DiT 内部注意力提高多角色身份保持

- **类型 / 日期 / 主线：**论文、后训练方法；2026-09-10；数字人、视频基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11507)、[OpenS2V-Eval](https://huggingface.co/spaces/BestWishYsh/OpenS2V-Eval)。
- **相对上期：**新增。
- **核心贡献：**发现部分 DiT attention block 自发形成 Intrinsic Spatial Grounding Map；低噪阶段用它调节参考身份强度，高噪阶段用它自动构造偏好对进行 RL，减少多主体语义串位。
- **Coding 接口：**可暴露为 per-character fidelity slider，并在资产生成 CI 中按人物区域计算身份一致性。
- **场景：**多人互动短片、品牌角色同框、连续剧角色资产生成。
- **成熟度 / 证据：**论文实测；暂无生产 SDK 或第三方验证。
- **重要性 / 阅读：**中 / 可读。

### 8. ZipCodec：低帧率流式语音为实时角色降低带宽与 token 压力

- **类型 / 日期 / 主线：**论文、GitHub、权重；2026-09-10；实时音频基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11642)、[Apache-2.0 权重](https://huggingface.co/lucadellalib/zipcodec)、[代码入口](https://github.com/lucadellalib/zipcodec)。
- **相对上期：**新增。
- **核心贡献：**WavLM 蒸馏、球面标量量化器和延迟感知 decoder，将流式语音表示压至 6.25 Hz、0.80 kbps，理论算法延迟 160 ms。
- **Coding 接口：**可作为 WebRTC 前后的语义 codec，或作为语音 agent / avatar 模型的低频离散输入。
- **场景：**移动数字人、低带宽直播、长会话语音记忆、边缘机器人。
- **成熟度 / 证据：**842M 权重和代码已开放；作者报告消费级 CPU 单流实时，尚无大并发测量。
- **重要性 / 阅读：**中高 / 可读。

### 9. Caption-once, Frames-on-Demand：持续视觉 agent 不必每次重看全部视频

- **类型 / 日期 / 主线：**论文、agent 运行时设计；2026-09-10；支撑基础设施、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.11899)。
- **相对上期：**新增。
- **核心贡献：**边缘端一次生成事件骨架与 clip micro-log；云端 agent 默认在文本记忆上推理，只有外观、文字或属性问题才按需请求原始关键帧。论文示例将一小时视频的视觉 working memory 限制到约 16 帧。
- **Coding 接口：**Visual-Need Router 可直接实现为有预算的工具调用：`retrieve_frames(time_range, max_frames)`。
- **场景：**数字人持续观察直播、智能眼镜、陪伴 agent、视频客服与安防复核。
- **成熟度 / 证据：**论文实测；暂无公开生产运行时。
- **重要性 / 阅读：**中 / 可读。

### 10. Show-Harness：模型与数据继续更新，工程完整度高于多数新论文

- **类型 / 日期 / 主线：**GitHub、模型、dataset、机器人运行时；论文 2026-09-09，Hub 资源截至 9 月 14 日仍有更新；Coding、具身互动。
- **官方链接：**[论文页与最新资源状态](https://huggingface.co/papers/2609.10522)、[Apache-2.0 仓库](https://github.com/showlab/Show-Harness)。
- **相对上期：**已报道；本次仅收录实质增量：五个真实数据 LoRA、一个双仿真策略及 42.6k 规模数据集在截点前约十小时继续更新。
- **Coding 接口：**OpenAI-compatible VLM endpoint、语义动作 DSL、机器人 interpreter、浏览器 GUMI 数据采集和可开关插件。
- **场景：**同一 agent 控制 GUI、仿真角色、机械臂和互动装置。
- **成熟度 / 证据：**代码、模型、数据和训练链路已开放；真实硬件仍需要校准、安全限位和人工接管。
- **重要性 / 阅读：**中高 / 必读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**Vidu S2 是本周期唯一同时推进清晰度、实时性、长时生成、动态参考和大幅全身动作的高质量新增。它不再局限于口型，而是把身份、表情、视线、姿态、舞蹈、持物及场景变化置于连续流中。
- **3D/4D 表示：**AvaImg 的价值不是直接生成动态 avatar，而是把扫描资产变成统一的 SMPL-X+D 与 UV 图像表示，使 2D 生成模型、版本控制和传统 mesh 工具能共享资产格式。
- **全身动作：**MOCO 证明语音、文本和轨迹可分别约束不同身体区域，再在扩散循环中联合收敛。它比简单动作叠加更合理，但尚未证明实时性、足底接触或复杂人—物碰撞。
- **实时交互：**Vidu S2 的 720p、25—42 FPS 是强信号；但论文没有给出完整 speech-end-to-first-frame、p95 中断恢复或每会话 GPU 成本。产品团队仍需自行测试感知延迟。
- **音视频与情感：**ZipCodec 降低了音频传输与 token 频率；本周期没有新的端到端情感语音或多语种数字人实测。
- **工程部署：**Vidu S2 demo 可玩，但 S2 API 未开放；AvaImg 和 MOCO 代码不完整。真正可自行部署的新增主要是 ZipCodec，以及上期已开放的 Show-Harness 组件。
- **安全与身份治理：**本周期暂无高质量新增。动态换人、换装和参考图即时注入使授权范围、参考图留存、肖像撤回、输出水印和直播审核更加紧迫，不能用“在线 demo 可用”替代合规审查。

## 5. 世界模型进展

- **架构与训练：**OpenWAM 将 WAM 设计空间模块化；World in World 走冻结 backbone 加外部视觉证据路线；MaP-WAM 则将长历史从执行器中抽离。三者共同否定“持续扩大上下文就是世界记忆”的单一路径。
- **可控 / 可玩生成：**World in World 已支持代码指定镜头轨迹、bullet time 和编辑传播，但当前更接近可编程重摄影工具，而非带权威物理状态的完整游戏世界。
- **长时与一致性：**MaP-WAM 用分段计划和真实执行观察更新记忆；Vidu S2 用 Self-Replay Forcing 训练模型适应自身历史。前者维护任务语义，后者维护像素流稳定性，两者可以组合。
- **空间表示：**World in World 使用深度投影、相机 pose 和点对应；Vidu S2 的空间视频目前主要由单目深度、左右视图 warp 与轻量补洞形成，并非可自由导航的完整 3D 场。
- **机器人 / 仿真：**OpenWAM 已提供最强的可复现入口；MaP-WAM 给出真实 Franka 证据，但尚无代码。Show-Harness 的语义动作解释器仍是连接 agent 和硬件的更成熟方案。
- **实时部署与评测：**实时数字人指标正在进步，生成世界仍缺少统一的 action-to-photon latency、长时漂移、状态正确性、多人同步和单位 GPU 小时成本报告。

## 6. Coding × 新型互动作品

### 链路一：事件脚本 → VLM prompt 编译器 → Vidu S2 → 实时数字演员

代码维护剧情状态、当前持有物、允许变更的服装/背景和用户权限；VLM 把状态差分编译为身份、表情、视线与动作 prompt，并根据生成帧判断动作完成度；Vidu S2 负责连续视觉表演。可形成可打断虚拟主播、互动偶像和实时品牌角色。在线 demo 已有运行证据，S2 API 接入仍是概念阶段。

### 链路二：镜头 DSL → 深度/点轨迹 → 冻结世界模型 → 互动摄影机

创作者或 coding agent 生成 `config.json`、相机 pose 和时间窗口；传统几何模块重投影源视频，视频模型补全新视角。可构建互动电影导演台、bullet-time 网页和虚拟制作预演。World in World 已有可运行 CLI，但当前生成约分钟级，不能称为实时游戏运行时。

### 链路三：角色行为图 → 语音/文本/轨迹通道 → MOCO → 骨骼动画

代码将任务拆成“说什么、向哪里走、上身做什么”，规则层定义身体区域与接触约束，MOCO 合成自然连续动作，Unity/Unreal/Godot 负责骨骼重定向、碰撞和导航。可用于会边走边讲解的 NPC；本周期只有论文证据。

### 链路四：扫描资产 → AvaImg → UV diffusion/editing → 游戏 mesh

资产管线把扫描注册为 SMPL-X+D 和标准 UV 图；图像模型负责服装、材质和局部细节变化；传统 DCC/引擎负责蒙皮、LOD、布料与渲染。它有望把 3D 角色生成转化为成熟 2D 图像模型可处理的问题，但代码尚未发布。

### 链路五：事件记忆 → 语言—视觉计划 → 固定上下文执行器 → 机器人/NPC

长历史只在分段规划时被读取，执行器复用缓存计划并产生动作块；进度事件触发重规划。代码负责 episodic store、任务图、超时和恢复，世界模型负责预测视觉计划，控制器负责安全执行。MaP-WAM 已有真实机器人证据，也可推断用于长期剧情 NPC。

### 链路六：持续视频 → 文本索引 → 按需取帧工具 → 数字人视觉反馈

边缘端一次构建事件和 clip 索引；对话 agent 默认查文本，只有颜色、文字、身份等问题才调用取帧工具；ZipCodec 同时压缩语音流。可形成低成本“持续观看”的直播助理、陪伴角色和智能眼镜。CFD 与 ZipCodec 分别有论文/代码证据，二者端到端组合属于本报告推断。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 所需技术栈与互动机制 | 当前成熟度 | 关键成本/延迟指标 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Vidu S2、MOCO、MaP-WAM | 状态机、行为图、骨骼/视频角色、记忆规划 | 可玩 demo / 研究原型 | Vidu 720p、25—42 FPS；端到端延迟未披露 | 权威状态、碰撞、多人同步 | [Vidu S2](https://arxiv.org/html/2609.11638) |
| 影视/虚拟制作 | World in World、AvaImg | 镜头 DSL、深度投影、UV/mesh 资产管线 | 部分开源 | WiW 单 A100 示例约 5 分钟 | 大角度补全、商业许可证 | [WiW 代码](https://github.com/Westlake-AGI-Lab/WorldinWorld) |
| 直播与电商 | Vidu S2 | VLM agent、动态参考、实时换装/背景、流媒体 | 在线 demo | 720p、25—42 FPS；价格未披露 | 审核、身份授权、并发 | [论文](https://arxiv.org/abs/2609.11638) |
| 品牌互动 | Vidu S2、DIAL | 角色状态、身份参考、多人一致性检测 | demo / 论文 | 未披露 | 品牌一致性和错误传播 | [DIAL](https://arxiv.org/abs/2609.11507) |
| 教育培训 | MOCO、Vidu S2 | 课程 agent、边走边讲动作、实时角色 | 研究原型 / demo | 未披露 | 内容正确性、动作安全 | [MOCO](https://arxiv.org/abs/2609.11439) |
| 企业数字员工 | Vidu S2、CFD | 工具调用、视频记忆、角色前端 | 可验证组件 | CFD 视觉预算可限至约 16 帧/小时视频 | SLA、审计、隐私 | [CFD](https://arxiv.org/abs/2609.11899) |
| 陪伴与社交 | Vidu S2、ZipCodec | 全双工语音、长期记忆、动态数字人 | demo / 开源组件 | codec 160 ms 理论延迟 | 情感安全、依赖、肖像权 | [ZipCodec](https://arxiv.org/abs/2609.11642) |
| 空间计算 | Vidu S2 Spatial | 深度、双目 warp、头显流媒体 | 技术 demo | 头显端延迟未披露 | 眩晕、分辨率、遮挡与全景 | [空间视频章节](https://arxiv.org/html/2609.11638) |
| 机器人/仿真 | OpenWAM、MaP-WAM、Show-Harness | WAM、动作 DSL、interpreter、verifier | 已开源研究栈 / 真实机器人实验 | MaP-WAM 执行上下文 O(1)；绝对延迟未披露 | 安全认证、长尾失败 | [OpenWAM](https://github.com/OpenWAM-Official/OpenWAM) |

本周期未发现可核验的规模化客户数量、长期在线可用率或生产并发报告。

## 8. 可复现资源与开发者入口

| 资源 | 许可与硬件 | 最小验证路径 | 复现建议 |
|---|---|---|---|
| [World in World](https://github.com/Westlake-AGI-Lab/WorldinWorld) | CC BY-NC-SA 4.0；80 GB A100，约 86 GB 权重 | 跑 tennis bullet-time 示例，再修改 `arc:amp` | 高优先；验证可编程镜头接口 |
| [OpenWAM](https://github.com/OpenWAM-Official/OpenWAM) | Apache-2.0；Python 3.10、PyTorch 2.7.1/CUDA 12.8 建议 | 先跑单个仿真 benchmark 与公开 checkpoint | 世界—动作研究最高优先 |
| [ZipCodec](https://huggingface.co/lucadellalib/zipcodec) | Apache-2.0；消费级 CPU 可单流实时 | 编解码内部语音集，测 RTF、延迟及 ASR/TTS 退化 | 低成本，值得立即验证 |
| [Show-Harness](https://github.com/showlab/Show-Harness) | Apache-2.0；模拟路径可先不接机器人 | 启动 GUMI `--sim`，再用 2B LoRA 输出动作 token | 适合接口与安全层实验 |
| [Vidu S2](https://github.com/shengshu-ai/Vidu-S) | 权重/许可未开放 | 在线 demo 测中断、换装、长时身份与动态参考 | 只做黑盒产品评测 |
| [AvaImg](https://arxiv.org/abs/2609.11722) | 代码/数据/容器待发布 | 暂只能审查论文与准备扫描测试集 | 等代码后再复现 |
| [MaP-WAM](https://github.com/aipixel/MaP-WAM) | 仅 README；代码和权重待发布 | 用现有 agent 手工复刻 segment memory schema | 跟踪，不宜宣称复现 |
| [MOCO](https://arxiv.org/abs/2609.11439) | 代码未核验 | 用现有 motion diffusion 做上/下身 mask 组合基线 | 研究团队可做小规模验证 |
| [DIAL](https://arxiv.org/abs/2609.11507) | 官方代码未核验 | 抽取 subject-aware attention，测多人身份串位率 | 适合视频模型团队 |
| [CFD](https://arxiv.org/abs/2609.11899) | 论文；公开实现未核验 | 一小时视频建两级 caption 索引，限制每问 16 帧 | 适合持续视觉 agent |

## 9. 系统架构与技术趋势判断

明显升温的路线包括：实时流式视频生成、运行中动态条件、视觉反馈 agent、显式语言—视觉计划、外部几何证据、固定上下文执行器，以及 world/action 模块化训练。单纯 talking-head、固定参考图和无限扩大视觉上下文的路线相对降温。

正在形成的可复用架构是：

`用户输入/传感器 → coding agent 或 VLM 编译器 → 权威状态与任务图 → 语言/视觉计划 → 视频/动作生成器 → 传统几何、物理或控制器 → verifier → 事件日志与记忆更新`

代码应负责权限、身份授权、任务生命周期、世界事实、持有物、库存、时间线、成本预算、重试和回滚；生成模型负责表情、动作、外观、补全和未来候选；Unity/Unreal、数据库、机器人控制器或 WebRTC 负责确定性物理、同步和交付。

Vidu S2 的 VLM 反馈循环很有价值，但依靠 prompt 保存“仍拿着杯子”并不等价于权威状态数据库。World in World 的几何证据提高可控性，也没有解决动力学。MaP-WAM 的计划压缩降低执行成本，却依赖规划器不遗漏历史关键信息。未来更可靠的产品会把这三种机制叠加，而不是选择其中之一。

需要降级看待的内容包括：没有 API/权重却宣称“实时可部署”的模型；只展示精选长视频却不报告漂移率的项目；将单目 warp 双目直接称为完整 3D 世界；以及只有自建 VLM 指标、没有任务成功率或人工盲评的营销比较。

## 10. 论文精读候选

1. [Vidu S2](https://arxiv.org/html/2609.11638)：重点读 §2.2 Self-Replay Forcing、§2.3 推理基础设施、§2.4 Agentic System、§4 Spatial Video、§5 benchmark。差异在于同时处理实时生成与实时编辑；风险是闭源、内部评测占比较高。
2. [World in World](https://arxiv.org/abs/2609.11548)：重点读视觉证据接口、correspondence router、EWA 及 memory ablation。差异是冻结 backbone、训练免费；风险是完整长时与流式模块尚未开放。
3. [OpenWAM](https://arxiv.org/abs/2609.07398)：重点读模块化消融、world-to-action 信息流、同步联合去噪和数据配比。差异是全栈可比较；风险是大规模训练成本高。
4. [MaP-WAM](https://arxiv.org/html/2609.11561)：重点读 §3.2—3.4、延迟测量附录和 RMBench 分任务结果。差异是 memory 只在 planning 时展开；风险是代码未开放、真实任务数量少。
5. [AvaImg](https://arxiv.org/abs/2609.11722)：重点读 signed winding fitting、UV representation、FLUX VAE roundtrip 和六数据集评测。差异是把 3D avatar 转为统一图像空间；风险是优化耗时及扫描输入门槛。

## 11. 下周跟踪与可行动建议

继续追踪：

1. Vidu S2 的 API、SDK、价格、并发、地区、首帧延迟和中断恢复协议何时开放。
2. Vidu S2 在 5—10 分钟连续流中的身份漂移、动作完成率和动态参考切换失败率。
3. World in World 何时开放 frustum memory、360°人体代理、流式生成和动作迁移。
4. MaP-WAM 训练代码、checkpoint 与真实机器人数据是否按计划发布。
5. AvaImg 代码、数据、容器和商业许可是否落地。
6. OpenWAM 不同 checkpoint 的显存、控制频率、真实机器人安全层和第三方复现结果。
7. MOCO 是否开放代码，以及足底滑动、身体自碰撞和人—物接触指标。
8. 实时换人、换装和动态参考产品是否提供同意记录、来源元数据、水印与撤回机制。

本周可做的小实验：

- **动态数字人黑盒回归。**目标：评估 Vidu S2 是否适合互动直播；组件：在线 demo、20 条动作脚本、三类角色参考；难点：统一计时与录屏；成功判据：720p 连续 5 分钟，无严重身份漂移，动态换装/持物成功率不低于 80%，中断后 2 秒内恢复新指令。
- **视频重摄影原型。**目标：验证代码生成镜头轨迹；组件：World in World、A100 80 GB、5 秒人物视频、LLM 生成 `config.json`；难点：深度错误和新视角空洞；成功判据：三条镜头轨迹可自动运行，主体身份保持且没有额外人物。
- **Memory-as-Plans 轻量复刻。**目标：测试长历史是否可从执行器剥离；组件：现有 VLM agent、SQLite episodic store、分段计划 JSON、模拟环境；难点：计划遗漏视觉细节；成功判据：任务长度增长 4 倍时，执行器 prompt 长度和单步延迟增长不超过 10%，成功率下降不超过 5 个百分点。
- **低带宽持续陪伴栈。**目标：验证 ZipCodec 与按需视觉检索的组合；组件：ZipCodec、WebRTC、小时级视频索引、`retrieve_frames` 工具；难点：codec 对 ASR/情感线索的影响；成功判据：语音带宽低于 1 kbps、CPU 实时，视觉请求不超过每问 16 帧，问答准确率相对全视频基线下降不超过 5%。
