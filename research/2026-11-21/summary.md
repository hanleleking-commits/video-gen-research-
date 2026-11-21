# 数字人、世界模型与互动作品研究日报：2026-11-21

检索窗口：2026-11-15 至 2026-11-21。  
说明：11 月 21 日为周六，arXiv 最新有效批次主要是 11 月 19 日提交、11 月 20 日公开的论文。本期已对照 11 月 15—20 日日报；以下重点条目此前均未正式收录，MetaPerson 属于已报道项目的实质更新。

## 1. 本日摘要

本周期最强信号不是单个视频模型变得更逼真，而是“生成世界”开始出现三种可以写代码的状态接口：Genie 4 提供实时世界 API，WM^MLLM 用可版本化全景图保存状态，Coda-DaVinci 用 Blender/Three.js 代理场景保存结构与物理事件。  
Genie 4 已从有限研究预览推进到 Gemini API 和 AI Studio，可生成 720p、24fps、原生音频的可导航世界；但对象持久性、复杂物理、交互延迟和价格仍未得到充分披露，因此应视作开发者预览，而非成熟游戏运行时。  
Coda-DaVinci 的价值在于实验证明：不是所有内容都应建成高精度 3D；环境和刚体适合中粒度代理，人类动作适合粗粒度代理或直接生成，代码代理的粒度必须随任务调整。  
WM^MLLM 进一步把“世界状态”从生成模型上下文中剥离出来，用 revision、局部 patch、检查与 commit 形成接近软件工程事务的世界编辑协议。  
机器人方向的重点从“生成的视频是否像真的”转向“动作扰动是否造成正确后果”：AFFECT 将仿真中的策略成功率与真实机器人成功率相关性从论文报告的 0.47 提升到 0.79；Mind2Act World 则显示最强系统在组合推理与执行任务上只有 12% 成功率。  
WAM-Policy 和 VSpark分别通过蒸馏与推测解码降低动作模型推理成本，但 WAM-Policy 的完整闭环仍需 0.66 秒，尚未达到实时控制。  
数字人方向没有新的生成模型突破，主要增量来自工程运行时：MetaPerson 2.1 扩充可编程 3D 角色资产，Avatar Live 增加 VR 头显面部与手指追踪。  
整体趋势是代码负责权威状态、规则、校验和恢复，生成模型负责视觉、声音与候选行为，传统引擎继续负责碰撞、物理和低延迟反馈；这比“让视频模型包办整个世界”更接近可落地系统。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期 |
|---|---|---|---|---|
| 数字人 / 虚拟形象 | 低—中 | MetaPerson 2.1、Avatar Live 2.5 把角色资产、VR 面捕和手指追踪接入现有运行时 | 资产与驱动工具改善；生成质量无明显突破 | 从“新模型”转为“生产工具补齐” |
| 世界模型 | 高 | Genie 4 API、WM^MLLM 显式状态、AFFECT 动作效果训练目标 | 从论文/demo 推进到开发者 API、可运行代码和真实机器人验证 | 首次同时出现 API、状态协议和动力学评测 |
| Coding × 互动作品 | 很高 | Coda-DaVinci、WM^MLLM 把代码代理、revision、检查和 commit 放到生成循环中心 | 已有 API 和开源原型，但生产 SLA 普遍缺失 | 从代码生成资产进一步走向代码维护持续运行的世界 |

## 3. 最值得关注的 10 个进展

### 1. Genie 4：实时世界模型进入 Gemini API

- **类型 / 日期 / 主线：**模型、API、demo；11 月 18 日产品开放，11 月 19 日 API 上线；世界模型、Coding × 互动作品。
- **官方链接：**[产品发布](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/genie-4-now-available-project-genie/)、[Gemini API 发布](https://blog.google/innovation-and-ai/technology/developers-tools/genie-4-in-gemini-api/)、[模型卡](https://deepmind.google/models/model-cards/genie-4/)。
- **相对上期：**此前日报漏报；本期补录的关键原因是已确认开发者 API，而不只是浏览器 demo。
- **核心贡献：**文本、参考图和实时操作驱动 720p/24fps 世界，同时生成环境声、音效和角色对话；Gemini 代理持续观察并规划事件。
- **Coding 接口与场景：**通过 Gemini Interactions API、AI Studio starter app 接入键盘事件和实时文本指令，可用于互动电影、生成式关卡、空间设计预演。
- **成熟度 / 证据：**开发者预览；能力为官方声称，尚缺第三方延迟、成本和长时稳定性评测。
- **重要性 / 阅读：**高 / 必读。

### 2. Coda-DaVinci：代码代理选择如何建模世界

- **类型 / 日期 / 主线：**论文、项目页；11 月 19 日提交；Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2611.26595)、[Coda 项目仓库](https://github.com/FrankYang-17/Coda)。
- **相对上期：**全新条目。
- **核心贡献：**多 agent 在直接生成、粗粒度和中粒度 3D proxy 之间动态选择，并用 Blender 物理、参考素材绑定、视觉检查和独立 reviewer 修复代理或提示词。论文报告其物理分数由直接生成的 47.16 提升到 60.77。
- **Coding 接口与场景：**Blender、Three.js、HTML 和持久化场景代码可成为互动电影、广告和可反复修改镜头的中间表示。
- **成熟度 / 证据：**论文与录制 demo；主框架代码仍标注 coming soon，当前仓库主要开放 CodeCap-Bench，且为非商业学术许可证。
- **重要性 / 阅读：**高 / 必读。

### 3. WM^MLLM：用 revision 和 commit 构造持久世界

- **类型 / 日期 / 主线：**论文、GitHub、可运行原型；论文 11 月 18 日提交，代码 v0.1.1 同日发布；世界模型、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2611.24193)、[Apache-2.0 代码](https://github.com/ztMotaLee/WM_mllm)。
- **相对上期：**全新条目。
- **核心贡献：**世界状态保存为带修订历史的全景图；MLLM 将请求编译为 rotate、revisit、local edit、global edit 或 walk，候选结果通过局部性检查后才 commit。论文报告相机往返完成率 85.3%，而六个基线最高 50.9%。
- **Coding 接口与场景：**提供 session engine、provider 接口和 `VideoBackend`；适合持久化虚拟展厅、互动叙事、可撤销的空间设计。
- **成熟度 / 证据：**已开源最小实现；视频生成器、权重和论文输出未包含，真实质量依赖外部付费 API。
- **重要性 / 阅读：**高 / 必读。

### 4. AFFECT：训练世界模型理解“动作究竟改变了什么”

- **类型 / 日期 / 主线：**论文；11 月 19 日；世界模型、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2611.25980)。
- **相对上期：**全新条目。
- **核心贡献：**向动作序列注入时间相关扰动，要求生成视频对扰动产生与真实数据一致的视觉变化。论文报告 DROID 上世界模型成功率与真实机器人成功率的 Pearson 相关系数由 0.47 升至 0.79，规划成功率最高提升 8.3%。
- **Coding 接口与场景：**可作为世界模型训练 loss，并进入策略候选生成—rollout—评分—执行循环。
- **成熟度 / 证据：**研究原型、论文实测；未发现同步开放的官方代码。
- **重要性 / 阅读：**高 / 必读。

### 5. WAM-Policy：把机械臂世界—动作模型迁移到四足机器人

- **类型 / 日期 / 主线：**论文、代码、权重、benchmark、真实机器人 demo；11 月 18 日；世界模型、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2611.24011)、[代码与复现说明](https://github.com/AIGeeksGroup/WAM-Policy)。
- **相对上期：**全新条目。
- **核心贡献：**用 on-policy self-distillation 将慢速教师压缩为一视频步、两动作步学生，采样加速 14 倍；十项仿真任务成功率最终达到 85%，并部署到 Unitree Go2 Air。
- **Coding 接口与场景：**提供训练、QLM-Bench、权重和部署目录，可构建语言控制的巡检、推箱、开门等四足任务。
- **成熟度 / 证据：**已开源、真实机器人验证；闭环 0.66 秒，仍慢于动作块覆盖的 0.32 秒。
- **重要性 / 阅读：**高 / 必读。

### 6. Mind2Act World：暴露“会想、会动，但不会组合”的缺口

- **类型 / 日期 / 主线：**benchmark、论文；11 月 19 日；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2611.26444)。
- **相对上期：**全新条目。
- **核心贡献：**30 个任务、3 个难度、Clean/Random 两种场景，共 180 个配置，分别测 Mind、Act 和闭环 Mind2Act。最强系统在 Mind、Act 上分别达到 52.7% 和 39.0%，组合后仅 12.0%。
- **Coding 接口与场景：**诊断 harness 可把失败归因到状态记忆、动作执行或反馈组合，适合作为机器人 agent CI。
- **成熟度 / 证据：**论文 benchmark；暂未确认公开运行代码。
- **重要性 / 阅读：**高 / 必读。

### 7. VSpark：VLA 的无损推测解码

- **类型 / 日期 / 主线：**论文、代码、权重；11 月 17 日；实时部署基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2611.22666)、[Apache-2.0 仓库](https://github.com/suho-yoo/VSpark)。
- **相对上期：**全新条目。
- **核心贡献：**小型 draft model 预测动作 token，目标 VLA 一次验证多个 token；通过动作噪声、隐藏特征对齐和 rejected continuation reuse，实现 1.28—2.10 倍无损加速。
- **Coding 接口与场景：**已提供 pi0-FAST、ActionCodec、StarVLA 适配和 LIBERO CLI，适合机器人 serving 侧的透明加速层。
- **成熟度 / 证据：**已开源研究代码与 draft checkpoint；训练 rollout 可能需要数百 GB 存储。
- **重要性 / 阅读：**中高 / 可读。

### 8. Depth Pro 2：视频到稳定度量深度的通用组件

- **类型 / 日期 / 主线：**论文、代码、权重；11 月 19 日；空间与实时基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2611.26603)、[官方仓库](https://github.com/apple-aiml-research/ml-depth-pro-2)。
- **相对上期：**全新条目。
- **核心贡献：**同一网络处理图片、视频和实时摄像头，兼顾边界锐利、米制深度与时间稳定性；提供 24M—872M 四档模型。
- **Coding 接口与场景：**Python API/CLI 输出 depth、focal length、16-bit PNG 或 NPZ，可接 Blender、Nuke、WebGPU 点云和 AR 遮挡。
- **成熟度 / 证据：**代码与权重已开放，但权重仅限非商业研究；S 模型 97MB，H+ 为 3.5GB。
- **重要性 / 阅读：**中高 / 可读。

### 9. MetaPerson 2.1：3D 数字人资产层扩容

- **类型 / 日期 / 主线：**产品、REST/JS API；11 月 19 日；数字人、Coding。
- **官方链接：**[2.1 发布记录](https://docs.metaperson.avatarsdk.com/business-integration/release_notes/desktop/)、[Cloud API](https://api.avatarsdk.com/)。
- **相对上期：**此前已报道 2.0/2.0.1；本期新增 2.1、多图 Generic 模型生成、更多头发服装、mask/prop/teen blendshape 和材质资源。
- **Coding 接口与场景：**REST、iframe JS API、GLB、骨骼、PBR、LOD，可继续进入 Unity、Unreal 和 WebGL。
- **成熟度 / 证据：**正式商业产品；功能由官方文档验证，生成质量和生产性能缺少独立评测。
- **重要性 / 阅读：**中 / 可读。

### 10. Avatar Live 2.5：VR 面捕和手指追踪进入直播运行时

- **类型 / 日期 / 主线：**产品更新；11 月 20 日；数字人、实时互动。
- **官方链接：**[官方更新记录](https://zzubi-works.github.io/avatar-live/en/updates.html)。
- **相对上期：**全新条目。
- **核心贡献：**实验性支持 VRCFaceTracking 和 SteamVR 精细手指追踪，并修复 motion、pose、live face 与 PhysBone 的状态冲突。
- **Coding 接口与场景：**连接 VRCFT、SteamVR、VRM/VRChat 资产和直播软件，适合 VR 游戏主播、虚拟演出和远程角色表演。
- **成熟度 / 证据：**可用产品更新，但新增追踪仍为实验性；与 VRChat 不能同时占用 VRCFT。
- **重要性 / 阅读：**中 / 仅跟踪。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期暂无高质量 talking-head、单图真人或长时音频驱动模型新增。MetaPerson 的增量是可编辑 3D 资产，不是神经数字人生成质量突破。
- **3D/4D 表示：**MetaPerson 2.1 继续采用传统 mesh、骨骼、blendshape、PBR 和 LOD。这条路线创新性有限，但比不可编辑视频头像更容易进入引擎、换装和网络同步管线。
- **实时交互：**Avatar Live 把头显面部和手指输入接入直播角色；它解决输入与动作层冲突，不解决角色大脑、对话延迟或高真实感面部重建。
- **音视频与情感：**没有新的联合情感音视频模型。Genie 4 的角色对话属于世界模型内原生音频，尚不能替代具有身份、授权和可控口型的数字人系统。
- **工程部署：**MetaPerson 已是商业 API，Avatar Live 是可运行工具；两者比本周数字人论文更接近生产，但并发、P95 延迟和客户 ROI 均未披露。
- **安全治理：**本周期没有新的身份授权或肖像治理机制。Genie 4 对输出加入 SynthID，但这不能自动解决角色肖像授权、声音克隆许可和实时冒充风险。

## 5. 世界模型进展

- **架构与训练：**AFFECT 将训练目标从视频相似度推进到动作效果分布；这比单纯降低 FVD 更接近规划用途。
- **可控与可玩：**Genie 4 支持键鼠和实时文本事件；Coda-DaVinci 用代理场景控制结构和物理；WM^MLLM 用显式状态保证返回和局部编辑。
- **长时与物理一致性：**显式 revision 能保证“回到旧视角时像素一致”，但并不等于隐式视频模型理解了 3D 物理。Genie 4 官方也承认复杂交互、对象持久性和长时漂移仍未解决。
- **空间表示：**WM^MLLM 的全景状态和 Depth Pro 2 的米制深度，代表“低成本显式空间缓存”；它们牺牲任意 6DoF 几何完整性，换取可检查、可编辑和可恢复。
- **机器人与仿真：**WAM-Policy证明 WAM 可跨 morphology 迁移；Mind2Act 则提醒，单独提升语言推理或动作精度都不能自动得到可靠闭环。
- **部署与评测：**VSpark、蒸馏和小模型正在降低动作输出成本；评测则从画质、FVD 转向动作敏感性、策略排序、失败归因与真实机器人相关性。

## 6. Coding × 新型互动作品

### 链路一：API 驱动的生成式互动电影

**Genie 4 → Interactions API、键盘事件、实时 prompt → 可边行走边改变天气、角色和剧情的世界 → 互动电影、品牌体验、空间预演**

代码负责会话、用户权限、剧情状态、事件节流和日志；Genie 4 负责连续视听生成；若涉及精确游戏规则，仍应由传统引擎保存权威状态。已有官方 starter app 和 API，属于可运行开发者预览，不是规模化案例。

### 链路二：代码代理导演视频模型

**自然语言剧情 → coding agent 生成 Blender/Three.js proxy → 物理模拟和视觉 reviewer 修复 → 视频模型完成质感与细动作 → 广告、动作镜头、互动叙事**

Coda-DaVinci 表明，代码最适合表达物体、相机、时序和可验证的状态变化；视频模型更适合人物细动作和视觉风格。主框架尚未完整开源，因此目前是论文级生产方法，而非现成 SDK。

### 链路三：带版本控制的持久虚拟世界

**用户编辑请求 → MLLM 编译为状态操作 → panorama revision、局部 patch、检查、commit → 可返回、撤销和继续编辑的世界 → 虚拟展厅、设计评审、长期互动叙事**

这里最重要的变化是软件工程语义进入生成世界：失败候选不写入状态，视频帧不直接充当记忆，revision 可恢复。WM^MLLM 已有可运行 session engine，但完整视觉质量依赖外部模型 API。

### 链路四：可编程 VR 数字角色

**文本或多图 → MetaPerson 2.1 生成骨骼/PBR 角色 → Unity/Unreal/WebGL 加载 → VRCFT/SteamVR 驱动表情与手指 → VR 主播、数字员工、在线演出**

代码负责角色 ID、骨骼映射、动作状态机、RTC 和安全权限；生成服务负责建模；Avatar Live 或游戏引擎负责低延迟渲染。这条链已有正式产品组件，但角色智能、情绪语音和长期记忆需要另行集成。

### 链路五：机器人“先模拟再执行”

**候选动作 → AFFECT 世界模型生成后果 → 策略评分 → WAM-Policy/传统控制器执行 → 巡检、仓储、四足操作**

生成模型负责短期后果与动作提案；代码负责安全约束、超时、碰撞规则和真实执行器；传统控制器负责高频稳定控制。AFFECT 的策略排序相关性和 WAM-Policy 的实机部署提供了研究证据，但延迟尚不满足高频闭环。

### 链路六：互动世界的空间感知层

**普通视频/摄像头 → Depth Pro 2 输出稳定米制深度 → WebGPU/Unity/Blender 构建点云、遮挡和碰撞代理 → 空间网页、AR 导览、虚拟制作**

这属于本报告推断：Depth Pro 2 没有直接发布互动作品，但其轻量 S 模型和统一视频接口适合成为浏览器或引擎的空间预处理层。商业落地受研究用途权重许可证限制。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Genie 4 API＋权威游戏状态 | 键鼠导航、实时文本事件 | 开发者预览 | 720p/24fps；响应延迟与价格未披露 | 漂移、规则不可验证、并发 | [官方发布](https://blog.google/innovation-and-ai/technology/developers-tools/genie-4-in-gemini-api/) |
| 影视/虚拟制作 | Coda＋Blender/Three.js＋视频模型 | 修改代理场景后重新生成镜头 | 论文/demo | 未披露 | 主代码未开放、转场不稳定 | [论文](https://arxiv.org/abs/2611.26595) |
| 直播与电商 | MetaPerson＋Avatar Live＋RTC | 面捕、手指、动作序列、换装 | 正式工具/实验功能 | 未披露 | 身份授权、平台兼容、口型与对话整合 | [Avatar Live](https://zzubi-works.github.io/avatar-live/en/updates.html) |
| 品牌互动 | Genie 4＋Web 前端＋事件服务 | 观众改变天气、角色或事件 | 原型可做 | 未披露 | 内容审核、品牌一致性、成本 | [模型页](https://deepmind.google/models/genie/) |
| 教育培训 | WM^MLLM＋状态机＋讲解 agent | 可撤销编辑、场景返回、步骤记录 | 开源原型 | 外部 API 成本未披露 | 几何真实性和事实审核 | [代码](https://github.com/ztMotaLee/WM_mllm) |
| 企业数字员工 | MetaPerson＋LLM＋工具调用 | 语音/文本驱动角色和业务动作 | 角色资产正式，整栈需集成 | 未披露 | 权限、审计、P95 延迟 | [API](https://api.avatarsdk.com/) |
| 陪伴与社交 | 3D avatar＋VR tracking＋长期记忆 | 表情、手指、语音和共享空间 | 可运行组件 | 未披露 | 情感风险、记忆治理、冒充 | [更新记录](https://zzubi-works.github.io/avatar-live/en/updates.html) |
| 空间计算 | Depth Pro 2＋WebGPU/Unity | 实时深度、遮挡、点云交互 | 研究代码 | S 权重 97MB；实时指标依设备 | 非商业权重、移动端功耗 | [仓库](https://github.com/apple-aiml-research/ml-depth-pro-2) |
| 机器人/仿真 | AFFECT＋WAM-Policy＋VSpark | 预测后果、选择动作、执行反馈 | 仿真成熟、实机研究验证 | WAM 闭环 0.66s；VSpark 1.28—2.10× | 实时性、sim-to-real、安全证明 | [WAM-Policy](https://github.com/AIGeeksGroup/WAM-Policy) |

## 8. 可复现资源与开发者入口

| 资源 | 许可与要求 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|
| [Genie 4 / AI Studio](https://deepmind.google/models/genie/) | 闭源 API；价格与额度需控制台确认 | 高 | 用 starter app 建一个键盘导航世界，连续执行三次天气/角色修改，记录响应时间和漂移 |
| [WM^MLLM](https://github.com/ztMotaLee/WM_mllm) | Apache-2.0；核心离线测试仅需 Python/CPU/ffmpeg，真实生成需自备 API | 高 | 先跑 deterministic offline session，再接一个图像编辑 API，测试 local edit 是否越界 |
| [WAM-Policy](https://github.com/AIGeeksGroup/WAM-Policy) | 主代码/权重 Apache-2.0；部署和 QLM-Bench 部分为 CC BY-NC | 中高 | 先跑 QLM-Bench 已发布 checkpoint，不从头训练；验证成功率和单 chunk 延迟 |
| [VSpark](https://github.com/suho-yoo/VSpark) | Apache-2.0；CUDA 12.8、LIBERO；目标权重沿用各自许可证 | 中高 | 下载一个 pi0-FAST draft，在同一任务比较 AR 与 VSpark 的成功率和 decoding time |
| [Depth Pro 2](https://github.com/apple-aiml-research/ml-depth-pro-2) | 代码开放；权重限非商业研究；CUDA/MPS/CPU | 高 | 用 S 模型处理 10 秒手机视频，检查边缘、帧间抖动和米制尺度 |
| [Coda](https://github.com/FrankYang-17/Coda) | 非商业学术许可；Coda-DaVinci 主代码待发布 | 仅跟踪 | 先复现 CodeCap-Bench 单样例；主框架开放前不宜投入完整复现 |
| [MetaPerson 2.1](https://docs.metaperson.avatarsdk.com/business-integration/release_notes/desktop/) | 商业服务；REST 访问受套餐限制 | 中 | 生成一个多图 Generic 角色，导出 GLB，检查骨骼、LOD、PBR 和 Unity 导入 |
| [Avatar Live 2.5](https://zzubi-works.github.io/avatar-live/en/updates.html) | 产品工具；需 VRCFT/SteamVR 硬件 | 中 | 验证面部和手指输入切换时是否与 motion/pose 状态冲突 |
| [Mind2Act World](https://arxiv.org/abs/2611.26444) / [AFFECT](https://arxiv.org/abs/2611.25980) | 目前主要为论文；官方完整代码待确认 | 先读后复现 | 将其诊断维度和动作扰动测试迁移到现有机器人 CI |

## 9. 系统架构与技术趋势判断

明显升温的方向有三个：

1. **显式世界状态。** panorama revision、3D proxy、规则图和传统引擎状态正在取代“把全部历史塞回视频上下文”的做法。
2. **生成事务。** propose—inspect—validate—commit—rollback 正在成为生成世界的通用控制流，和数据库、版本控制及 CI 的思路高度相似。
3. **动作敏感评测。** 世界模型是否能区分微小动作差异、正确排序策略，比单纯画质和 FVD 更有产业价值。

正在形成的可复用架构是：

**事件/API 网关 → agent 规划器 → 权威状态与版本库 → 物理/规则引擎 → 图像、视频、声音生成器 → reviewer/约束检查 → RTC 或渲染客户端 → 观测与回放系统。**

数字人可作为该架构中的角色表现层，世界模型作为环境表现和短期预测层，代码与传统引擎则承担身份、规则、资产、交易、权限和恢复。

仍未解决的问题包括：复杂接触物理、多人共享一致状态、数分钟以上对象永久性、可预测的 P95 延迟、推理成本、内容审核、用户生成资产版权，以及真实机器人上的安全闭环。

需要降级看待的材料也很明确：只有录制视频而无 API/代码的“可玩世界”、只有单镜头动作而无状态回放的“世界模型”、以及把换装资产扩容包装成生成模型突破的数字人产品，都不代表核心研究或工业成熟度提升。

## 10. 论文精读候选

1. **[Coda-DaVinci](https://arxiv.org/abs/2611.26595)**  
   值得读：直接研究 code-to-video 代理粒度与质量之间的因果关系。重点看 proxy granularity、physics simulation、multi-agent harness、长视频评测和 limitations。相较 WorldDraft，它更强调任务自适应选择。风险是主代码尚未开放，且视频后端依赖闭源模型。

2. **[WM^MLLM](https://arxiv.org/abs/2611.24193)**  
   值得读：将世界模型重构成状态管理系统。重点看 operation routes、locality check、commit semantics、MBench-A。与纯视频 rollout 的差异是显式 revision 保证返回与局部性。风险是全景图并非完整 3D，论文配置还依赖高成本外部模型。

3. **[Learning What Actions Change / AFFECT](https://arxiv.org/abs/2611.25980)**  
   值得读：将动作效果敏感性变成训练目标。重点看扰动生成、action-effect matching loss、真实机器人策略排序和 planning 实验。风险是缺少公开代码，复现需要 DROID/ALOHA 数据与世界模型训练资源。

4. **[WAM-Policy](https://arxiv.org/abs/2611.24011)**  
   值得读：展示跨机器人形态迁移、蒸馏、RL specialist 和多教师合并。重点看 OPSD、M-OPD、QLM-Bench、实机延迟。风险是双摄像头配置、任务特定 reward 和 sim-to-real 差距。

5. **[Mind2Act World](https://arxiv.org/abs/2611.26444)**  
   值得读：把失败拆为思考、动作、组合三个维度。重点看任务构造、诊断 harness、随机场景和系统 profile card。风险是目前指标主要来自作者评测，需等待公开代码和外部复现。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. Genie 4 API 的模型 ID、正式定价、并发配额、单次世界最长持续时间和输入到画面的 P50/P95 延迟。
2. Genie 4 在连续多次事件编辑后，对象、人物身份和空间布局的漂移速度。
3. Coda-DaVinci 主框架何时开放，是否包含 Blender agent、reviewer 和 Seedance 接口。
4. WM^MLLM 接入其他图像/视频后端后，论文中的局部性和恢复指标能否保持。
5. AFFECT 是否开放训练代码，以及能否迁移到驾驶、游戏角色和非机器人 action-conditioned 模型。
6. Mind2Act World benchmark 是否开放任务、模拟器、评测脚本和 leaderboard。
7. WAM-Policy 能否把 0.66 秒闭环压到 0.32 秒以内，并减少对顶视摄像头的依赖。
8. MetaPerson/Avatar Live 能否与实时语音、turn-taking 和工具调用形成端到端参考项目。

### 本周可动手实验

**实验一：持久世界事务测试**

- 目标：比较“生成模型上下文记忆”和“显式 revision 状态”。
- 组件：WM^MLLM、一个图像编辑 API、pytest。
- 难点：定义局部编辑边界和可重复视角。
- 成功判据：连续五次局部编辑后，区域外像素变化小于预设阈值；回到旧 revision 可恢复同一视图。

**实验二：Code2Video 最小代理实验**

- 目标：验证什么内容值得交给代码，什么内容应留给视频模型。
- 组件：Blender 或 Three.js、任一 I2V API、三个任务：刚体碰撞、人物挥手、相机往返。
- 难点：代理粒度和提示词对齐。
- 成功判据：刚体任务的状态正确率提高，人物动作自然度不低于直接生成，相机返回后关键物体仍保持状态。

**实验三：数字人实时链路测量**

- 目标：建立可量化的 VR 数字人延迟基线。
- 组件：MetaPerson/现有 VRM、Avatar Live、VRCFT、SteamVR、OBS。
- 难点：跨进程时钟同步和输入冲突。
- 成功判据：测得面部、手指和语音三条链路的 P50/P95 延迟；动作切换无明显状态残留。

**实验四：机器人模型 CI**

- 目标：在现有仿真任务中区分“想错、动错、组合错”。
- 组件：LIBERO 或 QLM-Bench、VSpark、Mind2Act 风格诊断脚本。
- 难点：统一失败标签和可重复初始状态。
- 成功判据：每次回归测试输出 Mind/Act/Composition 三类失败比例，同时确认 VSpark 加速不改变任务成功率。
