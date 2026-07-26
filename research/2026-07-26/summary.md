# 数字人、世界模型与互动作品研究日报：2026-07-26

检索窗口：2026-07-20 至 2026-07-26。  
检索截止：2026-07-26 09:01（UTC+8）。

## 1. 本日摘要

本周期新增最强的信号来自互动运行时，而不是新的数字人或世界模型论文：Unity正式发布可供coding agent操作编辑器和运行中Player的CLI与实验性Pipeline接口，使agent首次获得较完整的“观察项目—修改—运行—测试—验收”闭环。Unity 7进一步把公共API、CLI和免费MCP列为平台级能力，但目前仍是路线图，Beta计划于2026年12月开始。与此同时，Unity通过PolySpatial演示了Unity世界在Unreal Engine中原生、实时运行，角色、物理、光照和输入跨引擎同步；这对生成角色、世界模型和传统引擎的解耦比单纯画面互通更重要。Epic则于7月23日开放发布包含Sidekick NPC的Fortnite岛屿：创作者可用Verse、Scene Graph、NPC Spawner和Sidekick API编排外观、状态、动画和行为，并把玩家已有的陪伴角色带入UGC作品。这是本周期少见的正式平台能力，而非论文demo。

数字人核心生成方向本日没有新的高质量talking-head、原生音视频、实时3D/4D头像或turn-taking发布；Sidekick属于可脚本化虚拟角色资产，不应等同于神经数字人生成突破。世界模型方向也没有相对7月24—25日日报的实质性新增，已有重点仍是消费级部署、流式生成、显式状态和物理引擎分层。整体趋势更加明确：代码和传统引擎保存权威状态并执行规则，生成模型负责外观、动作候选或未来预测，coding agent负责创建、修改和自动验收这条流水线。产业证据方面，Unity CLI和Sidekick发布能力最强；Unity跨引擎运行属于技术demo；Unity 7其余能力多为前瞻路线图。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 低 | Sidekick成为可发布、可编程、可继承玩家身份的陪伴角色 | 从角色素材推进为平台原生互动NPC | 没有新的数字人生成模型；新增集中于角色运行时 |
| 世界模型 | 低 | 本周期暂无未报道的高质量新增 | 无变化 | AlayaWorld、ABot、GS-Agent等未发现新权重、接口或第三方评测 |
| Coding × 互动作品 | 很高 | Unity CLI/Pipeline开放agent闭环；Unity与Unreal同步运行；UEFN Sidekick正式可发布 | 出现“实际可用开发接口＋正式内容分发入口” | 从论文中的结构化控制，推进到商业引擎和UGC平台原生接口 |

## 3. 最值得关注的 4 个进展

本日仅确认4个未被最近日报覆盖且具有实质增量的条目。为避免重复，不用已有项目凑满8—12条。

### 1. Unity CLI与Pipeline：coding agent获得完整引擎执行面

- **类型：** CLI、公共API、实验性引擎包、coding agent运行时。
- **官方链接：** [Unity CLI发布说明](https://unity.com/blog/meet-the-unity-cli)。
- **发布或更新时间：** 2026-07-20。
- **所属主线：** Coding × 互动作品。
- **相对上期的新变化：** 全新条目。
- **核心贡献与关键技术：** CLI已可用于管理Editor、模块、项目和认证，提供JSON/TSV输出、明确退出码、非交互安装和CI服务账号认证；实验性`com.unity.pipeline`可通过本地API驱动正在运行的Editor或开发版Player。`unity command eval`能够在不触发项目级重新编译或domain reload的情况下，于运行时执行C#并返回结果。
- **与coding的接口：** 团队可用`[CliCommand]`把项目专属操作注册为agent工具；外部agent、脚本或Unity MCP可以发现并调用同一组命令。agent因而能够修改场景、运行测试、进入Play Mode、读取结果并继续修正。
- **互动作品或工业场景：** 自动搭建NPC状态机、生成培训关卡、批量验证数字人动画、调节物理参数、构建可重复回归的互动原型。
- **成熟度：** CLI正式可用；Pipeline为实验性功能。
- **证据级别：** 官方产品发布与接口说明。
- **重要性 / 阅读优先级：** 高 / 必读。

### 2. Unity 7：把CLI、公共API和MCP提升为平台级协作架构

- **类型：** 引擎路线图、API/MCP规划。
- **官方链接：** [Unity 7路线图公告](https://unity.com/cn/news/unity-7-roadmap-revealed-at-unite-seoul)、[Unity 7产品页](https://unity.com/releases/unity-7)。
- **发布时间：** 页面标记2026-07-20，新闻稿正文日期为7月21日；此处保留该日期差异。
- **所属主线：** Coding × 互动作品、实时运行时。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** Unity计划提供不依赖完整Editor的公共API和CLI，用于资产验证、构建和协作，并提供免费MCP连接coding agent；同时规划CoreCLR、近即时Play Mode、增量domain reload和最高90%的shader构建加速。
- **与coding的接口：** CLI、公共API、MCP、现有C#和Unity 6项目兼容路径。
- **互动作品或工业场景：** 多agent分别负责场景、代码、动画、质量检查和构建发布；数字人或生成世界可在持续运行的Editor/Player中被自动调试。
- **成熟度：** 路线图；早期Beta计划于2026年12月，完整版本计划于2027年第一季度。
- **证据级别：** 官方前瞻声明，不是已交付能力。
- **重要性 / 阅读优先级：** 高 / 必读。

### 3. PolySpatial跨引擎运行：Unity世界在Unreal中实时执行

- **类型：** 运行时协议、跨引擎技术demo。
- **官方链接：** [Unite Seoul官方活动回顾](https://discussions.unity.com/t/join-us-live-on-youtube-for-the-unite-seoul-keynote-on-on-jul-20-2026/1731253)。
- **演示时间：** 2026-07-20；原活动帖建立于7月16日，本次纳入原因是窗口内完成公开技术演示。
- **所属主线：** Coding × 互动作品、世界运行时。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** `Fantasy Kingdom`由Unity模拟、在Unreal Engine内原生实时渲染；角色、物理、光照和输入通过PolySpatial客户端—服务器协议同步。演示中的角色来自Unreal，而环境状态由Unity运行。
- **与coding的接口：** 当前公开材料确认协议式同步，但未披露完整schema、网络预算、SDK许可或开发者可用时间表。
- **互动作品或工业场景：** Unity训练仿真＋Unreal高保真表现、跨引擎数字人、把Unity UGC带入Fortnite、生成式渲染器替换而不改写权威状态层。
- **成熟度：** 可运行官方demo；尚未在Fortnite正式开放，早期接入目标为2027年。
- **证据级别：** 官方演示；缺少公开实现。
- **重要性 / 阅读优先级：** 高 / 必读。

### 4. Fortnite Sidekick NPC：可脚本化陪伴角色正式允许发布

- **类型：** 产品、虚拟角色运行时、Verse API、UGC产业案例。
- **官方链接：** [官方发布说明](https://www.fortnite.com/news/build-worlds-build-bonds-fortnite-sidekicks-now-available-as-npcs-for-your-islands)、[Sidekick开发文档](https://dev.epicgames.com/documentation/fortnite/using-sidekick-npcs-in-fortnite)、[Epic支持确认](https://www.epicgames.com/help/c-202300000001640/c-202300000001741/sidekicks-are-now-available-as-npcs-in-fortnite-unreal-editor-uefn-a202300000085990)。
- **发布时间：** 功能最初于2026-06-05公布；2026-07-23正式允许发布包含Sidekick的岛屿，因此在本窗口纳入。
- **所属主线：** 数字角色、Coding × 互动作品。
- **相对上期的新变化：** 全新条目；变化是从“可开发但不可发布”进入真实分发。
- **核心贡献：** Sidekick可通过NPC Spawner创建，通过Verse、Scene Graph及`npc_sidekick_component`实现跟随、取物、逃跑、攻击和反应；支持外观、尺寸、生命、护盾及动画状态配置。Cosmetic Sync可令NPC变为玩家已装备的Sidekick，形成跨作品连续角色身份。
- **与coding的接口：** Verse NPC Behavior、Sidekick API、Scene Graph、NPC Spawner、healthful/shieldable接口和UEFN修饰器。
- **互动作品或工业场景：** 怪物对战、宠物养成、农场模拟、角色收集RPG、陪伴任务、品牌IP活动和可持续运营的UGC角色经济。
- **成熟度：** 正式产品能力，可发布到Fortnite生态。
- **证据级别：** 官方文档与正式发布状态。
- **重要性 / 阅读优先级：** 高 / 必读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** 本周期暂无新的高质量speech-driven avatar、口型、视线、手势或全身统一生成模型。Sidekick的增量是预制角色资产与脚本化行为，不是从图像、语音或视频生成新身份。

**3D/4D表示。** 今日没有新的mesh、NeRF或4D Gaussian数字人发布。Sidekick使用平台角色rig、预设外观通道和Control Rig工作流，工程可编辑性强于神经视频，但视觉自由度受平台资产约束。

**实时交互。** Sidekick由传统游戏运行时执行，Verse只提交行为和状态变化，因此响应速度主要受游戏帧循环影响，而不是视频生成模型的采样延迟。这是其当前可产品化程度高于多数生成式数字人的原因。

**音视频与情感表达。** Sidekick提供happy、worried、dance、eat、sleep、attack等离散反应状态，但没有新增语音对话、情感TTS或主动倾听能力。

**工程部署。** 可进入产品的是Sidekick的NPC Spawner、Verse行为和Fortnite分发；Unity CLI也可用于批量生成、测试和验收传统rig角色。神经数字人仍需另行组合ASR、LLM、TTS、lip-sync、RTC和安全模块。

**安全与身份治理。** 本日没有新的人脸授权、水印或检测系统。Sidekick的授权边界较清晰：官方角色由平台提供，部分授权IP不能直接作为普通NPC使用；但跨作品Cosmetic Sync仍要求创作者正确处理persona行为、IP范围和用户资产权限。

## 5. 世界模型进展

本日相对7月24—25日日报暂无新的高质量世界模型条目，亦未发现已报道项目的新权重、完整代码或独立评测。

窗口内已有趋势仍成立：视频世界模型负责生成观察，传统引擎负责物理和权威状态，coding agent负责创建及验证运行环境。例如，[ABot-World](https://github.com/amap-cvlab/ABot-World)已公开单张RTX 5090上的推理代码与模型，但其最近一次公开新闻仍停留在7月13，500小时训练集和双向教师模型尚未发布；“无限rollout”也不等于游戏状态永久正确。

Unity—Unreal的PolySpatial演示虽不是世界模型，但提供了重要工程参照：视觉呈现、角色资产、物理模拟和输入系统可以跨进程、跨引擎组合。未来的视频世界模型应争取成为同类协议中的可替换渲染或预测服务，而非垄断整个世界状态。

## 6. Coding × 新型互动作品

1. **自然语言需求 → coding agent → Unity CLI/Pipeline → Play Mode自动验收 → 可运行互动原型**  
   agent负责生成C#、场景和测试；CLI负责执行与返回结构化结果；Unity运行时负责物理、动画和输入。该闭环已有正式CLI和实验性Pipeline证据。

2. **Unity权威世界 → PolySpatial同步协议 → Unreal角色与渲染 → 跨引擎游戏或虚拟制作**  
   Unity保存GameObject、物理及游戏逻辑；协议同步状态；Unreal承担角色和显示。官方demo已经运行，但开发者SDK、带宽和故障恢复尚未公开。

3. **玩家已有Sidekick → Cosmetic Sync → Verse行为 → 持续陪伴式UGC体验**  
   平台负责角色所有权和外观身份；Verse负责任务、战斗及反应；传统导航、伤害和网络系统负责确定性执行。7月23日起已有正式发布证据。

4. **Sidekick状态 → Verse/Scene Graph事件 → 动画与属性修改 → 怪物对战或宠物养成**  
   代码维护经验、生命、护盾、任务和经济；角色系统播放离散表演；生成模型不是必要组件。相比全生成角色，该路线牺牲外观开放性，换取低延迟、可审核和可规模化运行。

5. **世界模型服务 → Unity自定义`[CliCommand]` → 生成画面或动作候选 → 引擎回归测试**  
   团队可以把世界模型推理、视频质量评测和状态一致性检查封装成项目专属命令，让agent自动更换条件并比较结果。接口基础已存在，完整世界模型集成属于本报告推断。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏开发 | Unity CLI＋Pipeline＋C# | agent修改项目后自动运行验收 | CLI正式可用；Pipeline实验性 | 未披露 | `eval`权限、沙箱和错误回滚 | [Unity CLI](https://unity.com/blog/meet-the-unity-cli) |
| UGC游戏 | Sidekick＋Verse＋UEFN | 跟随、取物、战斗、表情和任务 | 正式可发布 | 未披露 | 平台规则、角色和武器限制 | [Sidekick文档](https://dev.epicgames.com/documentation/fortnite/using-sidekick-npcs-in-fortnite) |
| 陪伴与社交 | Cosmetic Sync＋Sidekick API | 玩家角色资产跨岛持续出现 | 正式产品组件 | 未披露 | 长期记忆、隐私、persona一致性 | [官方说明](https://www.fortnite.com/news/build-worlds-build-bonds-fortnite-sidekicks-now-available-as-npcs-for-your-islands) |
| 品牌互动 | UEFN＋IP角色＋Verse | 品牌角色参与任务和活动 | 平台内可落地 | 未披露 | IP授权范围和审核 | [Epic NPC文档](https://dev.epicgames.com/documentation/fortnite/ai-and-npcs-in-unreal-editor-for-fortnite) |
| 教育培训 | Unity CLI＋状态机＋测试脚本 | agent生成情境并重复验收 | 可搭建原型 | 未披露 | 教学正确性和生成代码安全 | [Unity CLI](https://unity.com/blog/meet-the-unity-cli) |
| 影视/虚拟制作 | Unity＋PolySpatial＋Unreal | 跨引擎同步角色、灯光与物理 | 官方技术demo | 未披露 | 协议未开放、资产和时间线转换 | [官方活动回顾](https://discussions.unity.com/t/join-us-live-on-youtube-for-the-unite-seoul-keynote-on-on-jul-20-2026/1731253) |
| 世界模型/仿真 | 世界模型服务＋Unity命令接口 | agent触发rollout并读取评测 | 机会判断 | 未披露 | 状态schema、GPU成本、确定性 | [Unity 7路线图](https://unity.com/cn/news/unity-7-roadmap-revealed-at-unite-seoul) |
| 企业数字员工 | Unity角色前端＋agent工具 | 对话驱动引擎动作和业务工具 | 组件级机会 | 未披露 | ASR/TTS、权限、并发和SLA | [Unity MCP说明](https://unity.com/blog/mcp-servers-game-development) |
| 空间计算 | Unity运行时＋跨引擎协议 | 共享空间状态和角色呈现 | demo/机会判断 | 未披露 | 端侧预算、遮挡和空间锚稳定性 | [PolySpatial演示](https://discussions.unity.com/t/join-us-live-on-youtube-for-the-unite-seoul-keynote-on-on-jul-20-2026/1731253) |

## 8. 可复现资源与开发者入口

- [Unity CLI](https://unity.com/blog/meet-the-unity-cli)：已可用。最小验证路径是安装CLI，创建测试项目，以JSON输出查询项目状态，再用`com.unity.pipeline`注册一个只读`[CliCommand]`，让agent进入Play Mode并读取测试结果。许可证及Pipeline生产支持等级需在实际安装条款中确认。
- [Unity Pipeline](https://unity.com/blog/meet-the-unity-cli)：实验性包。建议先只暴露资产查询、测试运行和截图等低风险命令，不直接开放任意`eval`给不受信任模型。
- [Sidekick NPC文档](https://dev.epicgames.com/documentation/fortnite/using-sidekick-npcs-in-fortnite)：无需下载模型权重。最小路径是NPC Spawner＋Custom Character Definition＋Verse follow行为，再加入happy/worried事件。
- [Sidekick伤害接口](https://dev.epicgames.com/documentation/fortnite/damaging-your-sidekick-in-fortnite)：可验证`healthful`、`shieldable`及限定武器伤害。成功路径应覆盖生成、受击、死亡/恢复和多人同步。
- PolySpatial跨引擎方案目前只有官方demo与早期接入信息，没有确认可公开下载的协议实现，不建议把它作为当前项目依赖。
- 状态审计：[ResponseGuard](https://github.com/ndb796/ResponseGuard)的checkpoint、训练代码和流式harness仍标记“coming soon”；[Structured Dynamics](https://github.com/lukasknobel/StructuredDynamics)仍只有README和“Code coming soon”。相对上期无实质变化。

## 9. 系统架构与技术趋势判断

明显升温的是“引擎原生agent接口”：CLI、公共API、MCP、结构化退出码和运行时命令正在取代单纯读写项目文件。coding agent的价值开始从生成脚本转向执行、观察和验收。

正在形成的通用架构是：

`创作意图 → coding/剧情agent → CLI、MCP或Verse工具 → 权威状态与物理引擎 → 数字角色/生成渲染 → 自动测试、审核与发布`

代码负责状态、规则、权限、网络同步、回滚和可观测性；生成模型负责语言理解、资产或画面生成、动作候选和异常场景扩增；传统引擎负责确定性物理、导航、动画混合与最终交互。

本周期的Sidekick进一步证明，短期商业价值未必来自端到端生成角色。预制高质量资产＋可编程状态＋玩家身份同步＋成熟分发平台，可以更快形成可运营的陪伴、收集和品牌玩法。

仍未解决的问题包括：任意运行时C#执行的安全边界；agent造成场景或资产破坏后的事务回滚；跨引擎状态schema和网络预算；世界模型生成结果如何可靠反写对象状态；实时数字人的脸、手、身体、语音和打断统一；人物与IP授权的机器可执行策略。

需要降级看待的是Unity 7尚未交付的性能与MCP承诺，以及PolySpatial从官方demo到开放SDK之间的工程距离。Sidekick虽然已正式发布，但不具备开放式角色生成、自然语言大脑或长期记忆，不应宣传成完整生成式NPC。

## 10. 论文精读候选

今日没有新的未报道高质量论文。以下为窗口内已在前两期出现、仍值得精读的候选，不重复计入今日进展：

1. [GS-Agent](https://arxiv.org/abs/2607.21522)：重点读多agent生成Genesis代码、执行反馈和视觉验收；与Unity CLI形成“生成仿真代码—运行—验证”的直接对照。复现风险是完整agent系统尚未开放。
2. [Ms. Forcing](https://arxiv.org/abs/2607.20940)：重点读多尺度patch调度、静态计算图和H-DMD；价值在于判断生成渲染是否能进入实时引擎循环。风险是结果依赖H200且首帧延迟未充分披露。
3. [FA-LAM](https://arxiv.org/abs/2607.20922)：重点读静态3D与流式4D Gaussian的联合训练及大视角头部补全。风险是无公开代码、标准引擎导出和实时驱动证据。
4. [GraphVid](https://arxiv.org/abs/2607.21580)：重点读有向交互图schema和GINEConv条件注入。与Sidekick API的差异是生成视频而非确定性角色运行时；风险是推理约200秒且代码入口不完整。

## 11. 下周跟踪与可行动建议

继续追踪：

1. Unity CLI的安装渠道、许可证、CI服务账号和版本锁定文档是否补全。
2. `com.unity.pipeline`何时脱离实验状态，是否提供命令权限白名单和审计日志。
3. Unity MCP能否通过CLI控制运行中Player，以及免费范围的具体限制。
4. PolySpatial跨Unity—Unreal协议是否公开字段schema、带宽、帧延迟和故障恢复机制。
5. Sidekick岛屿的实际稳定性、并发行为和Creator Portal分析指标。
6. 7月30日Fortnite LLM Conversations正式可发布后的延迟、容量限制、persona锁定和语音授权机制。
7. ABot的500小时动作视频数据与双向教师模型是否发布。
8. ResponseGuard、GraphVid、Structured Dynamics和GLAM-SLAM是否从占位仓库变为可复现代码。

本周适合动手验证：

1. **Unity agent闭环实验**  
   目标：让coding agent修改一个NPC巡逻状态机并自动进入Play Mode验收。组件：Unity CLI、Pipeline、C#测试。难点：安全暴露命令和结果结构化。成功判据：三次修改均可自动编译、运行、检测NPC到达目标并输出机器可读结果。

2. **Sidekick陪伴任务实验**  
   目标：实现跟随、取物、受伤担忧、任务完成庆祝的完整状态机。组件：UEFN、NPC Spawner、Verse、Sidekick API。难点：动画中断和多人状态同步。成功判据：两名玩家同时进入时各自角色身份正确，四种状态可重复触发且无串线。

3. **跨引擎状态协议最小模拟**  
   目标：不等待PolySpatial SDK，先定义角色、物体、灯光、物理和输入的JSON状态协议，在两个Unity进程间同步。组件：Unity、WebSocket或gRPC、JSON Schema。难点：权威状态、插值和丢包。成功判据：30分钟运行中状态不分叉，断线重连后可恢复一致。

4. **世界模型作为可替换渲染服务**  
   目标：用录制视频或轻量生成后端模拟世界模型，通过Unity CLI提交相机与动作状态并返回观察帧。组件：Unity、推理服务、状态日志和回放器。难点：动作到首帧延迟及状态—画面对齐。成功判据：每个生成帧均能追溯到确定的状态版本，失败时可立即回退到传统渲染。
