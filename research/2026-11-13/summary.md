# 数字人、世界模型与互动作品研究日报：2026-11-13

检索窗口：2026-11-07 至 2026-11-13（截至 10:10 CST）。已对照 11 月 9–12 日日报去重；日期采用官方公告、arXiv 提交/修订记录或仓库发布状态，不采用搜索抓取时间。

## 1. 本日摘要

本周期没有新的高质量 talking-head、语音驱动头像或全双工数字人基础模型发布；数字人方向的有效增量主要来自两类“可编排状态”：AnyMo 把稀疏可穿戴 IMU 转换为完整身体运动 token、文本检索和动作描述，Deformable Texture Maps 则把服装微观形变编译为可缓存的神经纹理。它们更适合作为动作理解和实时角色材质组件，而不是端到端数字人产品。

世界模型侧最重要的变化不是更强的视频画质，而是可验证性和状态接口进一步细化：ForeseeDrive用未来状态 token 在轨迹生成前影响候选分布；Action-Guided Reachability用无 latent 的确定性视觉世界模型计算闭环可达域；GridWM-Judge证明视觉评审器可能对表现形式敏感，却不能正确识别因果反事实。另一项负面证据表明，VLA 的“记忆”可能只是计时器：插入不改变任务状态的暂停，成功数可从47/50降至16/50。

Coding × 互动作品的本期核心信号来自运行时。Unreal Engine 5.9 Preview 将 Blueprint、StateTree、Niagara、音频、性能分析、场景捕获和资产安装暴露给 MCP/Python，并恢复实验性 WebAssembly/WebGPU 发布；GTAGym则提供浏览器可运行的3D任务、引擎状态验收、Codex工具接口和RL任务生成。BEHAVIOR 2026方案进一步展示了“学习策略负责常规动作、coding agent接管困难长尾任务”的真实竞赛架构，但其私有练习会话尚未发布，不能完整复现。

总体成熟度由“论文中的生成能力”向“可执行环境、回归测试、状态恢复和工具权限”前移。新增证据仍以作者实验、开源研究环境和引擎 Preview 为主；本周期未发现新的规模化数字人客户部署或生成式世界商业化案例。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 11 月 12 日 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 低—中 | IMU动作转token；服装微观形变可缓存渲染 | AnyMo代码、0.5B权重和benchmark可用；DTM仍为论文 | 未出现新的实时说话头像；由直接视频表演转向结构化动作和材质组件 |
| 世界模型 | 中高 | 未来token、确定性可达性、因果评审、事件记忆 | GridWM-Judge已开源；其余多为论文或局部代码 | 从“生成与几何”进一步转向闭环验证和失败诊断 |
| Coding × 互动作品 | 高 | Unreal原生MCP；AAA游戏自动验收；agent/VLA路由 | 有可运行引擎Preview、浏览器环境及开源竞赛栈 | 相较昨日的模型接口，今天新增更完整的运行时和CI证据 |
| 产业部署 | 低 | 引擎供应商开始提供正式工具面，但仍标记Experimental/Preview | 可试用，尚不宜直接用于生产项目 | 无新增客户试点或规模化部署 |

## 3. 最值得关注的 9 个进展

### 1. Unreal Engine 5.9：引擎成为 coding agent 的可观察运行时

- **类型 / 日期 / 主线：**引擎 Preview、MCP、Python API；11月9日；Coding × 互动作品。
- **官方链接：**[公告](https://forums.unrealengine.com/t/unreal-engine-5-9-megajam-preview/2852840)、[Release Notes](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-9-release-notes)、[MCP文档](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-mcp-in-unreal-editor?application_version=5.9)。
- **相对上期：**首次收录；此前日报覆盖生成模型和后期agent，但未覆盖本次原生引擎MCP。
- **贡献：**MCP工具覆盖渲染、源码管理、调试、性能分析、玩法测试、几何、音频、UI和关卡；Python扩展到MetaSounds、物理、Niagara、PCG和网络。Canvas可调用自选图像、视频、网格模型，并用场景截图与空间标注保持构图。
- **Coding接口：**本地HTTP MCP默认监听`127.0.0.1:8000/mcp`；可生成Codex等客户端配置，并通过Toolset Registry限制权限。
- **作品 / 场景：**agent持续搭建和测试NPC关卡、数字人舞台、互动电影分支及浏览器品牌体验。
- **成熟度 / 证据：**可下载Preview、官方文档；官方明确警告QA周期短且不适合直接升级生产项目。官方声称＋可运行Preview。
- **重要性 / 阅读：**高 / 必读。

### 2. One Policy for 100 Household Tasks：VLA与coding agent按任务路由

- **类型 / 日期 / 主线：**论文、GitHub、权重、竞赛方案；11月10日；世界模型/Coding × 工业仿真。
- **官方链接：**[论文](https://arxiv.org/abs/2611.14432)、[Apache-2.0代码](https://github.com/Robot-Learning-Collective/behavior-1k-solution-2026)、[权重](https://huggingface.co/IliaLarchenko/BEHAVIOR2026VLA)。
- **相对上期：**首次收录。
- **贡献：**单一VLA checkpoint覆盖100个OmniGibson长时家务任务；学习策略总体Q为0.392。14个弱项任务改由coding agent通过`observe`、`act`、`xyz`工具控制，作者报告这14项平均Q为0.639，而学习策略为0.108；组合提交得分0.446、完整成功率27.3%。
- **Coding接口：**policy server按任务路由，agent从自身模拟器练习会话的冻结分支开始，失败时回退学习策略。
- **场景：**机器人训练、游戏NPC长尾任务、数字员工在仿真业务系统中的工具执行。
- **成熟度 / 证据：**代码和VLA权重已开源，但agent练习会话未公开，组合结果不能完整复现。作者实验。
- **重要性 / 阅读：**高 / 必读。

### 3. GTAGym：把AAA游戏变成agent CI与RL环境

- **类型 / 日期 / 主线：**benchmark、GitHub、dataset、浏览器demo；11月9日；Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2611.13028)、[代码与在线任务](https://github.com/ljbro/GTAGym)。
- **相对上期：**首次收录。
- **贡献：**包含103个可交互3D任务和47个图像射击任务；以引擎状态自动判断成功，而不是让VLM观看录像打分。最佳受测配置总体准确率70.9%，人类基线89.8%。
- **Coding接口：**Babylon.js网页运行时、截图—键鼠JSON接口、并行worker、`mcp__game__game_step`、Codex harness、任务YAML生成及录像/令牌/轨迹留档。
- **作品 / 场景：**生成式NPC回归测试、玩法机器人、自动QA、训练可操作游戏agent。
- **成熟度 / 证据：**仓库含任务、资产、评测器和在线demo；RL使原生GTA任务成功率由15%升至23%。论文实测＋可运行代码。
- **重要性 / 阅读：**高 / 必读。

### 4. GridWM-Judge：视觉评审器必须同时测稳定性与因果敏感性

- **类型 / 日期 / 主线：**论文、benchmark、dataset、GitHub；11月10日；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2611.13596)、[MIT代码](https://github.com/xinyuw922-pixel/Towards-Reliable-VLM-Judge)、[数据](https://huggingface.co/datasets/nahco318/Towards-Reliable-VLM-Judges)。
- **相对上期：**相较昨日PhyRubric的物理rubric，本工作新增“相同状态不同呈现”和“真正改变结果的反事实”配对诊断。
- **贡献：**在MiniGrid、MiniWorld和Habitat中构造完整证据、证据移除、反事实及表现形式变体，发现当前VLM judge存在表现敏感、动作不敏感和反事实区分不足等独立失败模式。
- **Coding接口：**生成—API推理—评分脚本支持resume、分片、hash校验和原始响应留档。
- **场景：**世界模型CI、机器人轨迹验收、互动内容自动审核。
- **成熟度 / 证据：**已开源、可smoke test；Habitat需外部授权HSSD资产。论文实测。
- **重要性 / 阅读：**高 / 必读。

### 5. AnyMo：从稀疏IMU到动作token和语言接口

- **类型 / 日期 / 主线：**模型、GitHub、权重、dataset；论文v3于11月10日更新；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2605.22715)、[MIT代码](https://github.com/Breezelled/AnyMo)、[Apache-2.0权重](https://huggingface.co/CRUISEResearchGroup/AnyMo)。
- **相对上期：**旧论文因窗口内公开完整代码、0.5B权重、Transformers接口和AnyMo-Bench而纳入。
- **贡献：**利用身体表面物理仿真学习与设备位置相对无关的运动表示，将IMU离散为全身motion token，并支持活动识别、IMU—文本检索和动作描述。
- **Coding接口：**`pipeline("anymo")`接收`[T,S,6]`加速度/角速度，公开`classify`、`encode_imu`、`encode_text`和`caption`。
- **场景：**低成本数字人动作理解、健身教练、XR化身意图识别、表演检索。
- **成熟度 / 证据：**代码、权重和154,695窗口benchmark已开放；完整合成数据需本地生成约2.1 TB，数据授权混合。论文实测。
- **重要性 / 阅读：**中高 / 必读。

### 6. 机器人记忆的“幽灵进度”漏洞

- **类型 / 日期 / 主线：**论文、可靠性审计；11月10日；世界模型/长期记忆。
- **官方链接：**[论文](https://arxiv.org/abs/2611.14488)。
- **相对上期：**对昨日SolarWM等“增加记忆”的正向结果补充负面证据。
- **贡献：**在不改变任务状态的情况下插入暂停或手腕小动作，某公开memory-VLA在两个计数任务上的成功数分别从47降至16、从44降至10；原因是时间采样槽位被误当成任务进度。
- **Coding接口：**作者建议由按时间索引改为按事件索引；无需重训即可恢复一半以上损失。
- **场景：**可打断数字人、陪伴角色、机器人和长时NPC的事件记忆。
- **成熟度 / 证据：**论文实测，未见独立仓库。
- **重要性 / 阅读：**高 / 必读。

### 7. ForeseeDrive：将未来场景压缩为可插拔控制token

- **类型 / 日期 / 主线：**论文、补充代码；11月10日；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2611.14286)。
- **相对上期：**首次收录。
- **贡献：**从演员运动、占用变化、路线交互和碰撞时间余量提取future token，在生成轨迹候选前改变其几何分布，而不是事后从固定候选中评分。
- **Coding接口：**零token门控确保模块关闭时近似no-op，适合挂接到现有生成式规划器。
- **场景：**自动驾驶、交通仿真、具有危险预判的车辆NPC。
- **成熟度 / 证据：**代码仅包含于补充材料；89.2 PDMS、0.05%碰撞率等均为作者按不同benchmark协议报告，不宜横向合并。
- **重要性 / 阅读：**中高 / 可读。

### 8. Action-Guided Reachability：用确定性视觉世界模型验证控制器

- **类型 / 日期 / 主线：**重大论文修订；v4于11月10日；世界模型/安全基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2512.08991)。
- **相对上期：**旧工作因窗口内发布v4并补充闭环可达性结果而纳入。
- **贡献：**从物理状态直接生成相机画面，不引入随机latent，使代理闭环可以计算可达管；在CARLA制动和三个Gym环境中，相比cGAN代理得到窄1.9–69倍的鲁棒性区间。
- **Coding接口：**仿真采样、Gaussian Process优先级和按动作敏感维度切分构成固定算力预算下的验证器。
- **场景：**机器人/车辆控制器上线门禁、世界模型回归测试。
- **成熟度 / 证据：**论文实测；未见生产安全认证或公开部署。
- **重要性 / 阅读：**中高 / 可读。

### 9. Deformable Texture Maps：把服装微观形变编译进实时材质

- **类型 / 日期 / 主线：**论文、神经渲染表示；11月10日；数字人基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2611.13677)。
- **相对上期：**首次收录。
- **贡献：**将纹理坐标和局部形变映射为透明度、法线、深度及材质ID，并缓存变化区域；光照与材质响应仍由传统渲染器控制。
- **Coding接口：**输出可应用到粗服装mesh的纹理空间属性，适合接材质系统和动画形变。
- **场景：**数字人服装、游戏角色近景、虚拟试衣和虚拟制作。
- **成熟度 / 证据：**论文原型，未见代码或Unity/Unreal插件。论文实测。
- **重要性 / 阅读：**中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期没有新的高质量语音驱动头像。AnyMo解决的是“从可穿戴信号理解动作”，尚未直接输出骨骼姿态；若用于角色驱动，仍需动作解码或检索层。
- **3D/4D表示：**DTM把动态服装微结构从高面数几何降为可缓存纹理；相较纯视频数字人，它保留了灯光、材质和引擎编辑权。
- **实时交互：**Unreal 5.9的AI动画、StateTree、Control Rig和MCP形成较完整的运行时入口，但没有公布数字人端到端延迟或并发数据。
- **音视频与情感：**本周期暂无高质量新增。没有新的凝视、倾听反馈、情绪控制或可打断turn-taking独立评测。
- **工程部署：**AnyMo适合先进入动作标签、内容检索和传感器理解模块；DTM仍需引擎适配。完整实时数字人仍需TTS、RTC、口型、动作解码和审核层。
- **安全与身份治理：**本周期没有新的肖像授权、水印或内容溯源方案。长期交互系统反而新增了“时间记忆伪进度”风险。

## 5. 世界模型进展

架构上出现三种状态层：ForeseeDrive使用紧凑future token；GEMOR-GS使用可重定位的显式3D Gaussian地图；Action-Guided Reachability直接从物理状态生成确定性相机画面。它们共同指向“世界模型不必只输出视频，也可以输出可规划、可查询或可证明的中间状态”。

可控性方面，future token在候选生成前介入，比事后打分更可能补出原本不存在的安全轨迹。长时性方面，记忆审计说明固定时间采样会把等待误判为进度，事件索引和权威状态表应优先于单纯增加上下文长度。

评测方面，GridWM-Judge把正确性拆成“语义不变时保持稳定、因果变化时必须改变判断”。这对世界模型自动验收尤其重要：一个稳定但从不响应反事实的judge同样不可靠。

机器人和仿真方向已有较强工程证据，但没有新通用可玩视频模型。实时视频生成、多人交互和长时视觉身份在本周期没有新的突破。

## 6. Coding × 新型互动作品

### 链路一：agent原生的互动世界制作

**需求/参考图 → coding agent → Unreal MCP/Python修改Blueprint、StateTree、关卡和音频 → Play Mode测试/场景捕获 → WebAssembly/WebGPU发布**

代码负责权威状态、资产版本、碰撞和测试；生成模型负责概念图、视频、网格及开放式行为草案；引擎负责确定性运行。已有官方Preview，但Web平台和agent工具均存在已知稳定性问题。

### 链路二：可自动验收的生成式NPC

**任务模板 → GTAGym生成3D关卡 → agent通过截图和键鼠工具行动 → 引擎状态判断结果 → CI保存视频和轨迹 → RL或提示词迭代**

过去难点是“NPC看起来在玩，但无法自动判断是否真正完成任务”。GTAGym通过引擎真值解决验收问题，已具备可运行代码和浏览器demo。

### 链路三：策略与coding agent的分层角色大脑

**常规状态 → VLA低成本连续控制；长尾任务 → 路由到coding agent → observation/motion工具 → 失败回退VLA**

模型策略负责高频动作，agent负责分解任务、利用工具和恢复异常，模拟器负责碰撞与目标谓词。BEHAVIOR方案已有竞赛结果，但agent练习记忆不公开，仍不能视为完整生产参考实现。

### 链路四：可穿戴动作驱动的数字角色

**手表/IMU → AnyMo运动token与caption → 代码检索动作库或驱动动作生成器 → Control Rig/IK → 数字教练或XR化身**

AnyMo已经提供原始IMU Python接口；从token到高质量骨骼动作仍属本报告推断。短期更可行的是动作识别、触发动画状态和异常检测，而不是端到端动作重建。

### 链路五：可信世界模型回归测试

**交互脚本 → 世界模型/控制器rollout → GridWM-Judge完整、删证据和反事实三组测试 → DWM可达域检查 → 阈值门禁**

代码管理种子、证据、模型版本和失败回放；VLM只承担语义判断；确定性验证器覆盖安全边界。GridWM-Judge可运行，DWM验证仍是研究实现。

### 链路六：低成本动态服装角色

**角色粗mesh与形变 → DTM神经纹理 → 缓存未变化区域 → 传统PBR光照 → 实时数字人/互动试衣**

生成模型编译微结构，运行时代码决定缓存更新，传统renderer保持灯光和材质可编辑。论文链路成立，但尚无引擎插件。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | GTAGym＋Unreal MCP＋状态机 | 截图/键鼠行动、引擎真值验收 | benchmark可运行；引擎Preview | 未披露 | 资产许可、多人同步、agent成本 | [GTAGym](https://github.com/ljbro/GTAGym) |
| 影视/虚拟制作 | DTM＋Control Rig＋PBR | 服装形变、可编辑灯光 | 论文原型 | 未披露 | 无代码、引擎适配 | [DTM](https://arxiv.org/abs/2611.13677) |
| 直播与电商 | 本期无直接新增 | RTC＋数字人＋业务状态机 | 无变化 | 未披露 | 可打断延迟、事实审核、并发 | — |
| 品牌互动 | Unreal Canvas＋WebGPU | 场景提示、生成资产、浏览器交互 | Experimental Preview | 至少8 GB NVIDIA显存用于Canvas起步配置 | Web构建稳定性、API成本 | [Canvas文档](https://dev.epicgames.com/documentation/en-us/unreal-engine/getting-started-with-unreal-canvas-in-unreal-engine) |
| 教育培训 | AnyMo＋动作库＋数字教练 | 识别动作并触发反馈 | 开源研究组件 | 未披露 | 姿态重建、传感器偏移 | [AnyMo](https://huggingface.co/CRUISEResearchGroup/AnyMo) |
| 企业数字员工 | Agent/VLA路由＋工具权限 | 常规策略、长尾agent接管 | 仿真竞赛验证 | 未披露 | SLA、审计、私有练习数据 | [BEHAVIOR方案](https://github.com/Robot-Learning-Collective/behavior-1k-solution-2026) |
| 陪伴与社交 | 事件记忆＋角色状态机 | 打断、等待、事件进度 | 风险诊断阶段 | 未披露 | 时间捷径、长期安全 | [记忆审计](https://arxiv.org/abs/2611.14488) |
| 空间计算 | AnyMo＋IMU＋Control Rig | 身体活动识别与动画触发 | 开源组件 | 0.5B模型；最低硬件未披露 | 动作解码、移动端功耗 | [代码](https://github.com/Breezelled/AnyMo) |
| 机器人/仿真 | VLA＋coding agent＋GridWM-Judge＋DWM | 工具控制、反事实验收、可达性验证 | 研究栈/竞赛结果 | 六个模拟器共享一GPU可快于实时，为作者口径 | sim-to-real、安全认证 | [论文](https://arxiv.org/abs/2611.14432) |

## 8. 可复现资源与开发者入口

| 资源 | 开放情况 / 许可证 | 硬件或成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---:|---|
| [GTAGym](https://github.com/ljbro/GTAGym) | 运行环境、任务、评测器和dataset；继承资产分别授权 | Node 22、Python、Chromium；GPU评测需图形驱动 | 高 | 浏览器跑一个任务，再用Codex harness执行三次并核对引擎判定 |
| [GridWM-Judge](https://github.com/xinyuw922-pixel/Towards-Reliable-VLM-Judge) | MIT；代码与数据齐全 | MiniGrid可CPU运行；VLM API另计费 | 高 | 跑smoke profile，对同一轨迹比较Full与counterfactual判定 |
| [AnyMo](https://github.com/Breezelled/AnyMo) | 代码MIT、权重Apache-2.0；Nymeria衍生数据含非商业约束 | 0.5B；完整合成数据约2.1 TB | 高 | 取三传感器五秒IMU，测试分类、检索和caption |
| [BEHAVIOR 2026方案](https://github.com/Robot-Learning-Collective/behavior-1k-solution-2026) | Apache-2.0代码与VLA权重 | 完整训练使用B300及8×H200；推理最低配置未披露 | 中高 | 先在单任务回放checkpoint；agent组合因缺练习会话不可完整复现 |
| [Unreal 5.9 Preview](https://forums.unrealengine.com/t/unreal-engine-5-9-megajam-preview/2852840) | Launcher/GitHub/Linux可取；Experimental | Canvas文档建议至少8 GB NVIDIA显存；Web源码构建较重 | 高但限副本项目 | 启用All Toolsets，让agent创建灯光、运行玩法测试并截图 |
| [ForeseeDrive](https://arxiv.org/abs/2611.14286) | 补充材料含代码，未见独立仓库 | 未披露 | 中 | 在同一规划器中比较zero-token、真实token和打乱token |
| [Action-Guided Reachability](https://arxiv.org/abs/2512.08991) | 论文v4；未确认官方代码 | 未披露 | 仅研究团队 | 先在CartPole复刻状态到图像代理与区间宽度实验 |
| [DTM](https://arxiv.org/abs/2611.13677) | 论文；未见代码 | 未披露 | 仅跟踪 | 代码开放后比较完整微几何与缓存神经纹理的帧时、法线和遮挡 |

## 9. 系统架构与技术趋势判断

明显升温的是“生成模型＋权威运行时＋可执行验收”的三层结构。模型不再独占世界状态：引擎保存确定性状态，coding agent通过MCP和工具读写，生成模型补充视觉、动作及开放式规划，judge和形式验证器负责回归。

动作接口正在从连续控制量扩展为future token、IMU token、事件索引和任务谓词。这些结构化协议比直接传递视频更便于缓存、审计、回放和跨模型替换。

实时内容生产也趋向编译式架构：DTM在离线阶段把微观几何编译为运行时纹理；VLA在训练阶段吸收常规动作；在线时只把长尾交给昂贵agent。这比让大模型逐帧控制一切更符合成本和稳定性要求。

关键未解问题包括：数字人动作token到高质量骨骼动画的解码、多人权威状态、弱网和中断恢复、agent行为的确定性复现、judge因果可靠性、引擎Preview稳定性、端到端P95延迟及每会话成本。

需要降级看待的表述包括：把AnyMo称为全身动作生成器；把Unreal 5.9 Preview当作生产稳定版本；把BEHAVIOR组合分数视为完全开源复现；把单一VLM judge得分视作真实任务成功；把“有记忆槽位”解释成真正理解了事件进度。

## 10. 论文精读候选

1. [One Policy for 100 Household Tasks](https://arxiv.org/abs/2611.14432)：重点读agent路由、冻结练习分支、云端评测状态恢复和多样本flow matching。差异是把coding agent直接放在VLA旁边承担长尾控制；复现风险是私有练习会话与巨大训练数据。
2. [GridWM-Judge](https://arxiv.org/abs/2611.13596)：重点读SCI定义、Full/证据移除/反事实配对和presentation probe。价值在于揭示“稳定但因果迟钝”的judge；风险是格子世界结论向开放视频迁移有限。
3. [Is Your Robot’s Memory Just a Clock?](https://arxiv.org/abs/2611.14488)：重点读null event构造、时间戳缩放、event-indexed修复。与一般记忆论文的差异是把不变状态干预作为因果测试。
4. [ForeseeDrive](https://arxiv.org/abs/2611.14286)：重点读future token构造、zero-token gate和candidate-preservation loss。风险是多个benchmark协议不同，不能用单一汇总指标判断泛化。
5. [AnyMo](https://arxiv.org/abs/2605.22715)：重点读身体表面IMU仿真、23节点图编码、PQ-VAE token和motion-language alignment。复现风险是2.1 TB合成数据、旧版Nymeria工具链及混合许可证。

## 11. 下周跟踪与可行动建议

持续跟踪：

1. Unreal 5.9本周计划的QFE是否修复Mac/Linux Web支持缺失、Wasm内存和MCP无工具提示问题。
2. BEHAVIOR团队是否公开agent练习会话、完整14任务轨迹及调用成本。
3. GTAGym是否出现第三方复现、更多本地模型或真实游戏QA基准结果。
4. GridWM-Judge能否扩展到生成视频、可玩世界及连续物理环境。
5. AnyMo motion token能否稳定解码为SMPL、BVH或引擎骨骼动作。
6. 事件索引记忆能否迁移到可打断数字人和生成式NPC。
7. ForeseeDrive的补充代码是否形成独立仓库及统一延迟报告。
8. DTM是否开放代码、训练资产和Unreal/Unity shader示例。

本周可执行实验：

1. **Unreal agent闭环实验。**目标：验证“创建NPC关卡—编辑StateTree—Play Mode测试—截图审查”。组件：UE 5.9副本项目、Unreal MCP、Codex。难点：Preview崩溃和Toolset默认未启用。成功判据：同一脚本连续运行三次，关卡可保存、测试结果一致、失败可回滚。
2. **世界模型judge反事实实验。**目标：确认评审器是否真正响应状态变化。组件：GridWM-Judge、一个VLM API。难点：控制提示格式和图像编码差异。成功判据：语义不变变体一致率高，同时Full—counterfactual配对区分率明显高于随机。
3. **低成本数字人动作触发实验。**目标：用手表/手机IMU触发角色动作状态。组件：AnyMo、三个五秒IMU窗口、Unreal/Unity动作库。难点：设备坐标与60 Hz重采样。成功判据：至少五类动作的离线识别稳定，并能可靠触发对应动画而不要求神经动作生成。
4. **策略—agent路由实验。**目标：验证常规策略与长尾agent分层是否优于单一路径。组件：GTAGym、规则或小模型baseline、Codex harness。难点：定义可解释的路由条件和成本上限。成功判据：在固定调用预算下，总成功率提高，同时保存每次路由、回退和引擎验收证据。
