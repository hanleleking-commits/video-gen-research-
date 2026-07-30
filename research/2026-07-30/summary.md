# 数字人、世界模型与互动作品研究日报：2026-07-30

检索窗口：2026-07-24 至 2026-07-30。  
日期说明：已核对 arXiv 实际提交时间与官方发布页，并与 7 月 24—26 日日报去重。FlashRT、AlayaRenderer、ABot、WorldWeaver、GS-Agent、FA-LAM、Unity CLI、Fortnite Sidekick 等不重复计入；Fortnite LLM Conversations 按官方说明将在“美国时间 7 月 30 日”退出实验状态，截至本报告生成时尚未确认 v41.30 已实际上线。

## 1. 本日摘要

本周期最重要的世界模型变化，是“显式状态”开始从外部引擎辅助信号进入模型的联合预测目标：StatePlay同时生成画面、生命值、技能槽和计时器，直接针对“画面像游戏但规则不成立”的问题。Wonder则把6-DoF相机控制、稀疏长期记忆和实时蒸馏协同设计，在作者演示中达到16 FPS、约0.5秒固定延迟和分钟级回访一致性，但代码、权重仍未开放。数字人方向终于出现较强新增：AptAvatar将14B音频驱动头像蒸馏到两步生成，支持720p长视频及帧区间动作提示；不过公开仓库目前缺少关键checkpoint，“production-ready”仍主要是作者定位。Ripple进一步表明原生音视频生成可以通过跨模态循环记忆进入480p、约28 FPS的流式区间，但尚无代码和硬件预算。Coding方向的核心信号来自结构化中间层：CinemaTraj把LLM输出限制为可优化的摄影原语，RoFacto把机器人动作转换为URDF渲染，AgentHOI把人物—物体文本意图转换为逐秒动作计划。Genie Sim PanoWorld则把视频生成重新接回可导航3D Gaussian资产，说明视频不一定是最终输出，也可以成为仿真资产生成的中间过程。产业成熟度仍呈分化：Fortnite的LLM NPC拥有平台、语音、审核和分发链路，但正式发布状态仍待美东时间核验；其余新增主要是论文、作者demo或不完整开源。总体趋势已从“代码调用一个生成模型”转向“代码维护状态、几何与规则，agent产生结构化计划，生成模型负责难以手工制作的感知与表现层”。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | AptAvatar将720p长时音频驱动头像压到2 NFE；Ripple推进原生音视频流式化 | 有推理脚本和demo，但关键权重、RTC接口、并发成本未完整开放 | 上期以Sidekick传统角色运行时为主；本期重新出现神经视频头像增量 |
| 世界模型 | 很高 | StatePlay联合预测规则状态；Wonder实现6-DoF、长期记忆与实时浏览 | 多数仍是项目demo；代码和权重普遍“coming soon” | 从上期“引擎保存权威状态”进一步推进到“状态成为模型显式监督与输出” |
| Coding × 互动作品 | 很高 | LLM摄影原语、URDF渲染动作、HOI逐秒计划形成可编程中间协议 | AgentHOI已有代码和权重；CinemaTraj、RoFacto尚未完整开放 | 从Unity CLI的通用执行面，推进到面向相机、机器人和角色动作的领域工具协议 |

## 3. 最值得关注的 11 个进展

### 1. StatePlay：联合生成画面与游戏规则状态

- **类型 / 时间 / 主线：** 论文、项目demo；2026-07-29；世界模型、Coding × 游戏。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26754)、[项目页](https://jimntu.github.io/stateplay_page/)。
- **相对上期的新变化：** 全新；比上期WorldWeaver的共享状态寄存器更直接地把HP、技能槽和计时器纳入预测目标。
- **核心贡献：** 视觉专家和0.75B状态专家通过Mixture-of-Transformers双向交互；视频用flow matching，紧凑状态用Smooth L1监督。作者报告状态归一化L1误差低于0.06，相对最强无状态基线的机制一致性提高18.6个百分点。
- **Coding接口 / 场景：** 游戏服务器可提供动作和状态schema，模型返回视频与预测状态；适合生成式格斗、训练关卡和规则一致的游戏rollout。生产系统仍应由服务器保存最终权威状态。
- **成熟度 / 证据 / 优先级：** 研究demo；代码、数据、模型均标记coming soon；论文实测；高、必读。

### 2. Wonder：带稀疏长期记忆的实时视频世界模型

- **类型 / 时间 / 主线：** 世界模型、项目demo；2026-07-28。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26037)、[项目页](https://wonder-world-model.github.io/)。
- **核心贡献：** 以密集坐标场表达6-DoF相机运动；保存完整历史KV并用稀疏注意力检索相关位置；通过Mixture-of-Students和控制正则改善蒸馏后的相机漂移。
- **关键结果：** 官方报告16 FPS、约0.5秒固定延迟、最长一分钟的交互和回访，并支持对已有视频重新选择视角。
- **Coding接口 / 场景：** 运行时提交相机轨迹并保存会话memory；可用于生成式漫游、互动电影镜头探索和动态场景重拍。当前没有对象、库存或任务API。
- **成熟度 / 证据 / 优先级：** 作者可交互demo；代码与权重coming soon；官方声称；高、必读。

### 3. Ripple：480p、约28 FPS的流式原生音视频生成

- **类型 / 时间 / 主线：** 论文、运行时研究；2026-07-29；数字人、音视频基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26818)。
- **核心贡献：** 固定滑动窗口控制计算量，音频和视频分别维护循环memory，再通过跨模态memory交互保持同步；训练依次采用因果化教师、端到端蒸馏和在线强化后训练。
- **Coding接口 / 场景：** 理论上可把文本、角色条件和会话状态连续写入chunk队列，输出同步音视频流；适合虚拟主播、互动音乐角色和即时叙事。
- **成熟度 / 证据 / 优先级：** 作者报告480p约28 FPS；未披露首帧延迟、硬件、API和代码；研究原型、论文实测；高、必读。

### 4. AptAvatar：两步生成的720p长时音频驱动头像

- **类型 / 时间 / 主线：** 论文、GitHub、demo；v1为2026-07-27，v2为7月29日；数字人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.24013)、[GitHub](https://github.com/TaoLiveAIGC/AptAvatar)。
- **核心贡献：** Endpoint-Anchored Distribution Distillation用四步桥接生成器为两步学生提供可达的终点分布；Self-Generated History Replay使用历史checkpoint输出模拟推理期污染上下文。
- **关键结果：** 14B模型、720p、2 NFE，作者声称相对多步方案加速60倍；仓库还支持以明确帧区间描述手势。
- **Coding接口 / 场景：** 图片、音频、文本或结构化动作时间线输入，提供Python脚本、Gradio和单/多GPU启动脚本；适合批量虚拟主播、课程讲师和带手势的营销视频。
- **成熟度 / 证据 / 优先级：** Apache-2.0推理代码已出现，但README明确checkpoint尚待发布，因此当前不能完整复现；高、必读。

### 5. Fortnite LLM Conversations：生成式NPC进入正式分发节点

- **类型 / 时间 / 主线：** 平台产品、Persona接口、语音NPC；官方计划美国时间2026-07-30退出Experimental。
- **官方链接：** [发布说明](https://www.fortnite.com/news/publish-islands-with-llm-conversations-starting-july-30)、[开发文档](https://dev.epicgames.com/documentation/fortnite/llm-conversations-in-unreal-editor-for-fortnite)、[Prompt Editor](https://dev.epicgames.com/documentation/fortnite/using-the-prompt-editor-tool-in-unreal-editor-for-fortnite)。
- **相对上期的新变化：** 上期已预告发布日；本期官方文档进一步确认`persona_component`、Persona Modifier、Prompt Editor及底层Gemini 3.1 Flash-Lite＋ElevenLabs Eleven v3组合，但正式上线仍待美国时间核验。
- **Coding接口 / 场景：** UEFN Scene Graph、persona组件、Verse和传统Conversation Device可把自由对话连接到任务、奖励和世界事件。
- **治理：** 36个角色具有平台定义的声音和persona；部分IP角色自v41.30起不可编辑。Epic声明不保存普通语音和转录，举报时会提交文本日志，并提供家长控制。
- **成熟度 / 证据 / 优先级：** 平台正式发布在即；容量、限流、响应延迟和Creator Portal分析尚无实测；官方产品声明；高、必读。

### 6. CinemaTraj：LLM agent用摄影原语编排3D场景

- **类型 / 时间 / 主线：** 论文、agent框架、demo；2026-07-29；Coding × 虚拟制作。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26910)、[项目页](https://cinematraj.github.io/)。
- **核心贡献：** LLM读取3D scene graph，把自然语言拆成dolly、orbit、crane、pan、tilt、zoom和arc；随后以参数化轨迹和SDF优化处理碰撞与遮挡，并同步旁白和字幕。
- **Coding接口 / 场景：** 摄影动作是可扩展工具集，新轨迹类型可作为函数加入；适合房地产导览、数字展馆、虚拟制作预演及agent自动摄影。
- **成熟度 / 证据 / 优先级：** ScanNet++上的作者demo；代码仍coming soon；中高、必读。

### 7. Genie Sim PanoWorld：视频生成回接可导航3D资产

- **类型 / 时间 / 主线：** 论文、3D生成管线；2026-07-29；世界模型基础设施、仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26646)。
- **核心贡献：** 从单张360°全景图出发，以NavMesh规划SE(3)轨迹，通过几何warp条件生成全景视频，再由前馈重建器生成可实时自由浏览的3D Gaussian场景。
- **Coding接口 / 场景：** 代码负责NavMesh、轨迹与仿真规则；视频模型补全遮挡区域；3DGS成为机器人仿真、空间导览或数字孪生的可渲染资产。
- **成熟度 / 证据 / 优先级：** 作者实验；无公开代码、资产导出插件或硬件预算；研究原型；高、必读。

### 8. RoFacto：把机器人动作转换为URDF渲染协议

- **类型 / 时间 / 主线：** 论文、项目demo；2026-07-24；世界模型、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.22535)、[项目页](https://bjkim95.github.io/rofacto/)、[GitHub占位仓库](https://github.com/bjkim95/rofacto)。
- **核心贡献：** 控制器先把命令展开成不包含接触结果的名义轨迹，再通过URDF渲染机器人RGB和末端深度；模型只学习环境对可见机器人运动的响应。
- **Coding接口 / 场景：** 控制器、运动学、URDF渲染器和相机参数构成标准预处理链；作者展示未见机械臂—手组合及双臂的零样本迁移。
- **成熟度 / 证据 / 优先级：** DROID、RoboCasa-GR1作者实验；GitHub目前只有README；中高、必读。

### 9. ViTacWorld：视觉—触觉联合rollout改善接触任务

- **类型 / 时间 / 主线：** 论文、机器人demo；2026-07-24；世界模型、synthetic data。
- **官方链接：** [论文](https://arxiv.org/abs/2607.22530)、[项目页](https://vitacworld.github.io/)。
- **核心贡献：** 同时预测主视角、腕部视角和触觉反馈；把真实触觉数据、任务对齐仿真和真实策略rollout用于分阶段训练。
- **关键结果：** 项目页报告同一触觉策略在四项任务上的平均成功率由42.5%经两轮生成rollout增强至80.0%；实机评估67.5%，模型预测57.5%，表现为偏保守估计。
- **Coding接口 / 场景：** 规划器提交候选动作序列，世界模型返回视觉—触觉rollout，评分器用于策略筛选或数据扩增。
- **成熟度 / 证据 / 优先级：** 实机作者实验；GitHub coming soon；中高、可读。

### 10. AgentHOI：多agent把人物—物体意图转换为逐秒计划

- **类型 / 时间 / 主线：** 论文、GitHub、权重；2026-07-24；Coding × 人物视频。
- **官方链接：** [论文](https://arxiv.org/abs/2607.22241)、[GitHub](https://github.com/bone-11/agenthoi)。
- **核心贡献：** 四个VLM agent先把人物、物体和文本转换为逐秒交互时间线，再由基于Wan2.1-I2V-14B的模型生成视频；训练时用文本到动作教师蒸馏，推理时不再需要显式动作输入。
- **Coding接口 / 场景：** JSON输入、可保存的中间timeline、Shell/Python推理和训练脚本；适合服装试穿、人物骑乘和商品互动素材。
- **成熟度 / 证据 / 优先级：** 代码及模型checkpoint已开放，默认端到端管线含InternVL3.5-38B和Wan 14B；单张80GB GPU加CPU offload可运行。训练数据未发布、许可证未明确；中高、可读。

### 11. FreqForcing：以频域锚点抑制长视频漂移

- **类型 / 时间 / 主线：** 论文、推理方法；2026-07-29；世界模型支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.27110)。
- **核心贡献：** 将长时崩溃解释为低频能量漂移；低频使用anchor attention保持颜色和布局，高频使用局部attention保留动态，无需重新训练。
- **关键结果：** 作者将只在5秒片段上训练的Self-Forcing模型外推到2分钟，即24倍时长。
- **成熟度 / 证据 / 优先级：** 论文实测；论文中的GitHub链接截至核验时返回404，暂不可复现；中高、可读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** AptAvatar是本周期最明确的音频驱动人物新增：图片、语音和结构化动作提示共同控制长视频，重点不是新的唇形表示，而是把14B视频头像压至2 NFE。AgentHOI则补足人物与物体交互，但属于离线视频创作，不支持实时谈话。

**3D/4D表示。** 本周期没有新的高质量3D/4D数字人资产发布。Genie Sim PanoWorld输出3DGS场景而非人物；AptAvatar和Ripple仍输出像素视频，不能直接导入Unity或Unreal继续编辑rig、服装和碰撞体。

**实时交互。** Ripple已达到作者报告的28 FPS，但缺少首帧延迟和硬件信息；不能仅凭吞吐量认定可打断对话。AptAvatar强调快速批量生成，也没有证明音频chunk流式输入、RTC会话或turn-taking。

**音视频与情感表达。** Ripple的跨模态循环memory比ASR→TTS→视频串联更接近原生音视频角色，但尚未展示persona、情绪控制和用户插话。Fortnite则采用可控性更高的级联方案：语音输入、LLM文本、许可语音模型和传统3D角色表演。

**工程部署。** 当前最接近产品的是Fortnite平台链路；最接近可自行实验的是AgentHOI；AptAvatar必须等待checkpoint。神经头像用于直播或客服仍需补齐RTC、并发调度、失败回退、审核和SLA。

**安全与身份治理。** 本周期没有新的头像水印或溯源机制。AptAvatar仓库只有使用规范，不是技术防护；Fortnite对角色声音使用获得同意的专业演员表演，并对部分IP persona实施平台锁定，治理证据明显强于论文系统。

## 5. 世界模型进展

**架构与训练。** StatePlay以视觉—状态双专家建模规则；Wonder以稀疏检索保存完整历史；FreqForcing从频域修正自回归漂移。三者分别处理“规则不知道”“历史找不到”和“长时能量漂移”。

**可控与可玩生成。** Wonder提供6-DoF相机控制，StatePlay提供动作与规则状态，RoFacto提供可编辑机器人轨迹。控制信号正从键盘枚举值变为具几何或业务语义的结构化协议。

**长时与物理一致性。** Wonder的一分钟回访、FreqForcing的两分钟生成属于视觉持续性；StatePlay首次直接测量游戏机制，但仅覆盖五秒片段和少量状态。尚无证据证明任务、库存、复杂经济或多人世界在数小时内保持一致。

**空间表示。** 密集相机坐标场、3D scene graph、NavMesh、URDF、深度和3DGS正在成为视频模型与传统软件之间的公共中间层。

**机器人、游戏与仿真。** RoFacto解决跨机器人动作空间不统一；ViTacWorld加入相机看不到的接触信号；PanoWorld把生成视频转成仿真资产。世界模型的工业价值正在从“替代渲染器”扩展到策略评估和数据生成。

**实时部署与评测。** Wonder和Ripple进入16—28 FPS区间，但几乎都未公开硬件、并发、显存和成本。StatePlay的机制评测是积极信号，但代码与测试集未开放，暂不能独立验证。

## 6. Coding × 新型互动作品

1. **游戏状态机 → StatePlay视觉/状态双分支 → 规则一致的生成战斗 → 游戏与训练仿真**  
   代码保存HP、技能、计时器和最终胜负；模型预测视觉结果并辅助发现状态异常；传统引擎处理碰撞和网络同步。已有论文demo，尚无实时引擎插件。

2. **相机输入 → Wonder稀疏memory → 可回访生成世界 → 互动电影与生成式漫游**  
   客户端提交6-DoF轨迹，会话服务保存KV历史，模型生成新观察；任务和对象状态由外部服务器维护。实时作者demo已存在，公共SDK未开放。

3. **Persona Prompt → Gemini/ElevenLabs → UEFN角色 → 可运营语音NPC**  
   Prompt Editor定义人物性格和知识；persona组件连接Scene Graph；Verse把对话结果绑定到任务或事件；Fortnite负责语音、审核、家长控制和分发。正式开放状态仍需在美国时间7月30日后复核。

4. **3D scene graph → LLM摄影工具调用 → SDF轨迹优化 → 自动虚拟摄影**  
   CinemaTraj中LLM只选择目标和摄影原语，确定性优化器负责碰撞、遮挡和轨迹连续性，渲染器负责最终画面。可形成数字展馆、房地产导览和虚拟制片预演；当前为作者demo。

5. **控制器动作 → URDF/深度渲染 → RoFacto世界模型 → 跨本体策略评估**  
   代码处理运动学和名义轨迹，模型预测接触后的环境变化，安全控制器保留最终执行权。未见本体可以通过相同渲染协议接入，代码尚未发布。

6. **人物/商品/脚本 → 多agent逐秒规划 → AgentHOI → 广告与角色互动素材**  
   agent负责把“穿上、拿起、骑乘”等意图转成可审计timeline；视频模型负责外观和运动合成；人工审核负责商品真实性和品牌规范。已有代码、权重和最小运行路径，但不具实时性。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键指标 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | StatePlay＋状态服务器＋视频模型 | 动作改变HP、技能和画面 | 研究demo | 状态误差<0.06；五秒评测 | 长时状态、实时性、代码缺失 | [StatePlay](https://jimntu.github.io/stateplay_page/) |
| 互动世界 | Wonder＋6-DoF控制＋会话memory | 移动、转向、离开后回访 | 作者demo | 16 FPS、约0.5秒、最长一分钟 | 无对象API、权重未开 | [Wonder](https://wonder-world-model.github.io/) |
| 影视/虚拟制作 | CinemaTraj＋scene graph＋3DGS | 自然语言自动选择镜头 | 作者demo | 50场景评测；运行成本未披露 | 代码、DCC和引擎接口缺失 | [CinemaTraj](https://cinematraj.github.io/) |
| 直播与电商 | AptAvatar＋音频＋动作时间线 | 语音驱动人物与手势 | 不完整开源 | 720p、2 NFE、作者称60× | checkpoint、RTC、并发 | [AptAvatar](https://github.com/TaoLiveAIGC/AptAvatar) |
| 品牌互动 | AgentHOI＋人物/商品图＋VLM agents | 文本生成逐秒商品互动 | 可运行研究原型 | 单80GB GPU可运行；时延未披露 | 成本、授权、商品真实性 | [GitHub](https://github.com/bone-11/agenthoi) |
| 教育培训 | Fortnite Persona＋Verse＋任务状态 | 语音问答触发教学事件 | 平台发布在即 | 延迟、容量未披露 | 仅英语、事实可靠性、限流 | [文档](https://dev.epicgames.com/documentation/fortnite/llm-conversations-in-unreal-editor-for-fortnite) |
| 企业数字员工 | Ripple/AptAvatar＋RTC＋业务工具 | 对话连续驱动音视频表演 | 组件级原型 | 28 FPS或2 NFE；首帧未披露 | SLA、审核、工具权限 | [Ripple](https://arxiv.org/abs/2607.26818) |
| 陪伴与社交 | Fortnite LLM NPC＋Sidekick/Persona | 语音对话和角色反应 | 平台能力，正式状态待核验 | 未披露 | 隐私、persona越界、成本 | [官方说明](https://www.fortnite.com/news/publish-islands-with-llm-conversations-starting-july-30) |
| 空间计算 | PanoWorld＋360图＋3DGS | 自由视角室内漫游 | 研究原型 | 四步生成；硬件未披露 | 几何误差、资产导出 | [论文](https://arxiv.org/abs/2607.26646) |
| 机器人/仿真 | RoFacto/ViTacWorld＋URDF＋触觉 | 候选动作rollout与策略筛选 | 实机作者实验 | ViTac实机67.5%、预测57.5% | sim-to-real、安全、代码缺失 | [ViTacWorld](https://vitacworld.github.io/) |

## 8. 可复现资源与开发者入口

- [AgentHOI](https://github.com/bone-11/agenthoi)：本周期最完整入口。提供推理、训练脚本和Hugging Face checkpoint；默认需要Wan2.1-I2V-14B、InternVL3.5-38B及约80GB GPU。许可证未明确，商业使用前必须核验。最小路径是运行内置JSON样例并检查输出的逐秒agent timeline。
- [AptAvatar](https://github.com/TaoLiveAIGC/AptAvatar)：Apache-2.0推理代码、Gradio和单/多GPU脚本已存在，但模型checkpoint仍未发布，当前不值得投入完整复现环境。
- [FreqForcing论文](https://arxiv.org/abs/2607.27110)：论文声称代码可用，但所链接GitHub截至核验时返回404，应标记待核验。
- [Wonder](https://wonder-world-model.github.io/)：仅项目demo；代码和Hugging Face均coming soon。
- [StatePlay](https://jimntu.github.io/stateplay_page/)：代码、数据、模型均coming soon；可先复现其“HP/技能/计时器＋视频”的状态schema和评测逻辑。
- [CinemaTraj](https://cinematraj.github.io/)：项目页包含算法和评测，但代码coming soon。最小替代实验是用现有scene graph和LLM function calling实现orbit、dolly、pan三个工具。
- [RoFacto](https://github.com/bjkim95/rofacto)：GitHub只有README及“Code coming soon”，暂不可复现。
- [ViTacWorld](https://vitacworld.github.io/)：项目页有完整结果和demo，GitHub尚未发布。
- Ripple和Genie Sim PanoWorld目前未确认公共代码、权重、API或引擎插件。

## 9. 系统架构与技术趋势判断

明显升温的是三类结构化中间层：

`业务/游戏状态`、`几何与相机协议`、`agent生成的动作计划`

正在形成的通用架构是：

`用户意图 → coding/规划agent → 结构化状态或轨迹 → 传统引擎/控制器校验 → 世界模型或数字人生成 → 实时会话、审核与观测`

代码负责权威状态、权限、碰撞、网络同步、回滚和日志；agent负责把自然语言转成有限工具调用或时间线；生成模型负责外观、运动、音视频和难以显式编程的环境响应；传统引擎负责确定性执行。

世界模型路线正在从纯像素条件转向状态、深度、URDF、scene graph和NavMesh。视频生成同时出现两个方向：Wonder/Ripple追求持续在线生成；PanoWorld把视频当作生成3D资产的中间数据。数字人则仍受制于“吞吐量不等于对话延迟”：AptAvatar和Ripple尚未证明打断、turn-taking及规模化并发。

需要降级看待的包括：AptAvatar的“production-ready”表述、Wonder的实时性、Ripple的28 FPS，以及所有未披露硬件的性能数字。StatePlay、CinemaTraj和ViTacWorld的结果均来自作者实验；缺少公开代码时不能当作独立验证。Fortnite的产品证据最强，但当前仍需确认美东时间正式发布以及上线后的容量和响应质量。

## 10. 论文精读候选

1. [StatePlay](https://arxiv.org/abs/2607.26754)：重点读MoT状态—视觉交互、状态损失和mechanics fidelity评测。与WorldWeaver的差异是直接联合预测具体规则状态。复现风险是数据、模型均未开放。
2. [Wonder](https://arxiv.org/abs/2607.26037)：重点读密集相机条件、稀疏KV检索和distillation correction。与AlayaWorld相比更强调相关历史检索和视频重拍。风险是硬件、代码、权重缺失。
3. [Ripple](https://arxiv.org/abs/2607.26818)：重点读跨模态循环memory和在线强化后训练。价值在于判断原生音视频是否能替代ASR/TTS/头像级联。风险是未披露首帧和部署预算。
4. [AptAvatar](https://arxiv.org/abs/2607.24013)：重点读Endpoint-Anchored Distillation和Self-Generated History Replay。与LiveAvatar类四步模型相比进一步压到两步。风险是checkpoint未发布，内部benchmark占主导。
5. [RoFacto](https://arxiv.org/abs/2607.22535)：重点读名义轨迹与真实未来状态的区分、深度条件和未见本体实验。它提供了比原始动作token更可编程的世界模型接口。风险是代码coming soon。

## 11. 下周跟踪与可行动建议

继续跟踪：

1. Fortnite v41.30是否已在美国时间7月30日正式允许发布LLM NPC岛屿，以及首批作品的延迟、容量和故障情况。
2. AptAvatar checkpoint是否真实开放，单卡显存、生成时长和长视频身份漂移是否可复现。
3. Wonder是否发布代码、权重、会话状态格式及16 FPS所用硬件。
4. Ripple是否披露首帧延迟、硬件、文本/角色控制接口和音视频安全机制。
5. StatePlay能否开放状态标注、游戏数据和机制一致性评测代码。
6. FreqForcing的404仓库是否恢复，能否直接接入Self-Forcing或ABot。
7. CinemaTraj的摄影原语能否导出为Unity Cinemachine或Unreal Sequencer轨迹。
8. RoFacto与ViTacWorld能否发布URDF渲染、触觉编码及真实机器人复现脚本。

本周适合动手验证：

1. **状态一致的生成战斗回放**  
   目标：复现StatePlay的核心分层思想。组件：Unity/Godot、HP/技能状态JSON、录制视频和规则检查器。难点：状态与帧时间对齐。成功判据：所有胜负、技能释放和数值变化均可由状态日志重放，生成画面错误不会反写权威状态。

2. **LLM摄影工具最小实现**  
   目标：实现CinemaTraj式orbit、dolly、pan工具。组件：Three.js或Unity、scene graph、function calling和碰撞检测。成功判据：20条自然语言请求中至少18条生成无碰撞、目标始终可见的可回放轨迹。

3. **AgentHOI最小复现**  
   目标：验证agent逐秒计划是否真正改善人物—商品互动。组件：公开仓库、80GB GPU或云实例、三组人物/商品图。难点：显存、模型下载和许可证。成功判据：保存中间timeline，并在盲测中稳定优于跳过agent的直接提示版本。

4. **数字人流式运行时模拟**  
   目标：在等待Ripple/AptAvatar权重期间验证可打断架构。组件：WebRTC、流式TTS、占位头像视频服务、会话状态机。成功判据：用户插话后200毫秒内停止旧音视频队列，新响应不会继续播放旧角色动作，所有chunk可追溯到同一会话版本。
