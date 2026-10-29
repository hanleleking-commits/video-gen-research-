# 数字人、世界模型与互动作品研究日报：2026-10-29

检索窗口：2026-10-23 至 2026-10-29。

## 1. 本日摘要

过去 24 小时最重要的增量不是单一模型画质提升，而是生成系统逐渐形成“显式状态、可执行代码、流式记忆、自动验收”的软件架构。数字人方向新增较弱，但 ReferStream 首次把“运行中加入新角色”明确建模为流式生成事件；WorldCast 和 TopX 则分别推进大角度镜头下的身份保持、陌生骨架的零样本动作生成。世界模型方向，GameVerse 将训练数据从不足十秒的片段扩展到分钟级连续游戏序列，Predictable Latents 则说明长时漂移和推理成本的一部分根因在于视频 autoencoder 的 latent 并未为动力学预测优化。Coding × 互动作品的信号更强：CodeCap-Bench 把可执行程序作为视频的结构化描述，RoboDuo 用代码编排“尝试—检查—原子恢复”，LabWorld 把实验视频编译成带几何、关节、事件的可编辑 4D 世界。ScIntBench 的结果同时给出警告：静态看起来合理的 3D 场景并不等于可交互，12 个受测系统中有 7 个完全无法支持规定动作，最佳系统的交互成功率也只有 36.92%。整体成熟度仍以研究原型为主；本周期最完整的可复现资源是 Predictable Latents，其代码、训练流程和评测脚本已按 MIT 许可证开放。除论文作者自测外，本批新增尚无独立第三方复现，因此所有性能结论均应视为“论文实测”，不能等同于工业部署。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期 |
|---|---|---|---|---|
| 数字人 / 虚拟形象 | 中 | 流式视频可在生成途中注入新角色；角色镜头控制和陌生骨架动作泛化继续增强 | 论文 demo 增多，未出现新的正式产品或客户部署 | 新增 ReferStream；补录 WorldCast、TopX |
| 世界模型 | 高 | 分钟级训练数据、可预测 latent、实验室 Real2Sim 和交互物理评测同时出现 | 从“生成可看视频”进一步走向“长期训练、闭环选择、可验证交互” | 相比上期偏重控制协议，本期补上数据、表示和验收层 |
| Coding × 互动作品 | 高 | 代码开始同时承担视频结构描述、VLA 编排、任务检查与失败恢复 | 出现可运行研究代码，但大多尚无 SDK、引擎插件或生产 SLA | 从“代码调用模型”推进到“代码维护状态并监督模型” |

## 3. 最值得关注的 10 个进展

### 1. ReferStream：生成途中注入新角色

- **类型 / 主线**：论文、benchmark、录制 demo；数字人、Coding × 互动作品。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.37566)、[项目页](https://xushuolin.github.io/ReferStream/)、[GitHub](https://github.com/Xushuolin/ReferStream)；2026-10-27 提交。
- **相对上期**：新增，上一期尚未覆盖。
- **核心贡献**：Joint Latent 将新参考图放入当前场景 latent 的目标区域；Cache Restoration 只临时覆盖流式缓存，注入后恢复原全局缓存，因此不重写已经输出的帧，同时保留新角色的生成历史。论文还提出带参考到达时间和角色存在标注的 ReferStream Bench。[论文摘要](https://arxiv.org/abs/2610.37566)
- **Coding 接口**：可抽象成 `inject_reference(image, region, instruction, timestamp)` 事件，但当前仓库只有项目网页和 24 个录制样例，没有推理代码、API 或模型权重。[仓库说明](https://github.com/Xushuolin/ReferStream)
- **应用**：互动电影中途登场、直播临时加入商品或嘉宾、生成式 NPC 增援、连续品牌故事。
- **成熟度 / 证据**：可运行结果 demo；论文实测，无独立验证。
- **重要性 / 阅读优先级**：高 / 必读。

### 2. GameVerse：分钟级可交互世界训练数据

- **类型 / 主线**：dataset、论文；世界模型、支撑基础设施。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.37257)；2026-10-27 提交，NeurIPS 2026 接收。
- **相对上期**：新增。
- **核心贡献**：包含 6.97 万段、1,655 小时、3.496 亿帧游戏视频，其中 1,358 小时为连续两分钟序列；提供分钟级稳定相机轨迹，并把静态场景描述与动态事件 caption 分开。作者称它能显著延长交互世界模型的时间跨度。[论文摘要](https://arxiv.org/abs/2610.37257)
- **Coding 接口**：事件边界、相机轨迹和前/第三人称控制数据可以直接进入训练 sampler、动作条件和 rollout 测试脚本；暂未看到公开下载入口。
- **应用**：长时可玩视频、游戏行为模拟、NPC 训练和游戏测试数据生成。
- **成熟度 / 证据**：论文数据集；数据可得性待核验，性能为作者实测。
- **重要性 / 阅读优先级**：高 / 必读。

### 3. Video World Models Need Predictable Latents

- **类型 / 主线**：论文、GitHub；世界模型、推理基础设施。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.34663)、[代码](https://github.com/mattia-scardecchia/predictable-latents)；2026-10-26。
- **相对上期**：首次收录。
- **核心贡献**：在 autoencoder 上加入 temporal latent prediction 或 inverse-dynamics 目标，并使用由过去帧条件化的 decoder。作者报告同训练预算下推理最高便宜 5 倍、训练快 7 倍以上，长时及分布外像素/感知误差降低 15%–24%。[论文摘要](https://arxiv.org/abs/2610.34663)
- **Coding 接口**：MIT 仓库包含数据转换、AE/WM 两阶段训练、长时漂移、OOD rollout、WorldGym 策略评估与 VLM judge 脚本；要求 Python 3.11、CUDA GPU 和受条款约束的 FLUX.1-schnell 权重。[复现说明](https://github.com/mattia-scardecchia/predictable-latents)
- **应用**：低成本机器人 world-model serving、策略预评估和长时预测。
- **成熟度 / 证据**：已开源研究代码；论文实测，无预训练 checkpoint。
- **重要性 / 阅读优先级**：高 / 必读。

### 4. LabWorld：把实验视频编译成可编辑 4D 实验室

- **类型 / 主线**：论文、Real2Sim 原型；世界模型、工业应用。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.36592)；2026-10-27。
- **相对上期**：新增。
- **核心贡献**：从单目 RGB 视频恢复可复用资产、度量场景、时变对象状态和实验步骤事件。五段移液视频中，事件约束让满足的物理约束从 5/35 提升到 35/35。[论文摘要](https://arxiv.org/abs/2610.36592)
- **Coding 接口**：输出几何、关节、运动和事件等显式状态，原则上可被模拟器脚本、实验规划 agent 和回放工具编辑；目前未发布代码或标准文件格式。
- **应用**：实验机器人训练、SOP 回放、教育实验、实验室数字孪生。
- **成熟度 / 证据**：研究原型；小规模真实实验视频上的论文实测。
- **重要性 / 阅读优先级**：高 / 必读。

### 5. ScIntBench：让生成 3D 场景接受物理交互验收

- **类型 / 主线**：benchmark；世界模型、Coding × 工程验收。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.37326)；2026-10-27。
- **相对上期**：新增；相较上期 WMEval 更聚焦生成式 3D 场景的操作可用性。
- **核心贡献**：在物理仿真中执行推、开、表面探测等动作，测量任务成功、过程完整性和动态合理性。12 个系统中，7 个不能完成任何交互；静态合理性接近满分的系统交互成功率仅 11.54%，最佳系统为 36.92%。[论文摘要](https://arxiv.org/abs/2610.37326)
- **Coding 接口**：最适合作为生成资产流水线的 CI gate：导入场景、执行动作脚本、输出失败类型和分数。目前尚无公开代码入口。
- **应用**：生成式游戏关卡、机器人仿真、虚拟培训和自动化资产验收。
- **成熟度 / 证据**：论文 benchmark；作者实测。
- **重要性 / 阅读优先级**：高 / 必读。

### 6. CodeCap-Bench：用可执行代码描述视频

- **类型 / 主线**：benchmark、论文；Coding × 互动作品。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.37341)；2026-10-27。
- **相对上期**：新增。
- **核心贡献**：以图形 primitive、布局规则和时间函数组成的程序补充自然语言 caption。作者在问答和视频重建中观察到更高的推理准确率与重建保真度，但主流 MLLM 对复杂时间动态仍常生成错误程序；benchmark 含 323 个结构化 2D 视频。[论文摘要](https://arxiv.org/abs/2610.37341)
- **Coding 接口**：视频可先转成可执行场景脚本，再被代码 agent 修改、调试、重放或编译到 Canvas/WebGL/动画运行时。
- **应用**：可编辑动态图表、互动教育动画、UI 演示、轻量游戏和视频到网页转换。
- **成熟度 / 证据**：论文 benchmark；数据与生成器代码尚未确认开放。
- **重要性 / 阅读优先级**：高 / 必读。

### 7. MoGAP：把视频模型适配成机器人控制器

- **类型 / 主线**：论文、world action model；世界模型、机器人。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.36624)、[项目页](https://ismty0805.github.io/mogap/)；2026-10-27。
- **相对上期**：新增。
- **核心贡献**：训练时对动态区域施加更强噪声，让模型从较干净的静态上下文恢复手、机械臂和物体运动；不引入外部 teacher，推理时移除辅助分支。作者报告 GR-1 Tabletop 上 Fast-WAM 从 38.8% 提升到 47.5%，Cosmos3 从 52.3% 提升到 62.3%；真实机器人 OOD 任务平均从 40.6% 升至 74.0%。[项目页结果](https://ismty0805.github.io/mogap/)
- **Coding 接口**：作为视频扩散 backbone 的训练 recipe，而非应用 API；项目页明确标记代码和 checkpoint “Coming Soon”。
- **应用**：操作机器人、动态物体追踪和控制策略适配。
- **成熟度 / 证据**：真实机器人研究验证；未开源，论文实测。
- **重要性 / 阅读优先级**：高 / 必读。

### 8. Best-of-N World Action Model：用预测未来选择动作

- **类型 / 主线**：论文、闭环控制方法；世界模型。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.37196)；2026-10-27。
- **相对上期**：新增。
- **核心贡献**：通过可复现分支、重复 continuation 和 common random numbers 剥离环境随机性，测量候选动作真正可选择的提升空间；fail-gated selector 只在 WAM 预测失败时偏离原策略。作者报告已见任务成功率从 76.9% 到 81.6%，未见任务从 62.6% 到 69.8%。[论文摘要](https://arxiv.org/abs/2610.37196)
- **Coding 接口**：运行时循环为 `sample candidates → imagine futures → score failure → selectively override`，适合接入机器人或游戏 agent 的 policy wrapper。
- **应用**：机器人容错、NPC 动作选择、自动驾驶候选轨迹筛选。
- **成熟度 / 证据**：研究原型；超过 10 万次分支 rollout 的论文实测，未见代码。
- **重要性 / 阅读优先级**：高 / 必读。

### 9. WorldCast：大角度镜头中的角色身份保持

- **类型 / 主线**：论文、角色视频模型；数字人。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.33135)；2026-10-23。
- **相对上期**：窗口内补录，近期日报未单列。
- **核心贡献**：用初始帧深度反投影点云提供相机几何引导，再以 Geometry-Aligned Memory Attention 从多视图角色 anchor 检索目标视角特征，解决大旋转、平移及半径变化时的身份和几何漂移。[论文摘要](https://arxiv.org/abs/2610.33135)
- **Coding 接口**：可由摄影机轨迹、相对位姿和角色 anchor 编排，但尚无代码或引擎插件。
- **应用**：虚拟摄影、互动电影、多镜头数字主播和游戏过场。
- **成熟度 / 证据**：研究原型；论文实测。
- **重要性 / 阅读优先级**：中高 / 可读。

### 10. TopX：仅凭 skeleton 文件驱动陌生角色

- **类型 / 主线**：论文、动作模型、dataset；数字人、角色工程。
- **官方链接 / 日期**：[论文](https://arxiv.org/abs/2610.34321)；2026-10-25。
- **相对上期**：首次收录。
- **核心贡献**：73M 参数 flow-matching Transformer 从骨架树、rest pose 和关节名称生成旋转与根轨迹，不需要该角色的动作样本、模板、retargeting 或微调。SkelShift 汇总 143,334 个 captioned motion clips，骨架关节数覆盖 4–142。[论文摘要](https://arxiv.org/abs/2610.34321)
- **Coding 接口**：理想接口是输入 FBX/GLTF skeleton 与文本动作，再输出 animation clip；但当前无公开转换器、代码和 Unity/Unreal 插件。
- **应用**：UGC 游戏角色、异形 NPC、低成本动画预演和自动资产适配。
- **成熟度 / 证据**：研究原型；论文实测。
- **重要性 / 阅读优先级**：中高 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动**：ReferStream 的增量不在口型或清晰度，而在“演员表可以在运行中改变”；TopX 则把动作生成从固定人体模板推广到任意骨架树。二者均尚未形成产品 API。
- **3D/4D 表示**：WorldCast 用点云和多视图 memory 稳定大角度摄影；GaussianSpline 用每个 Gaussian 自适应 Bézier 轨迹表示快速、非均匀运动，适合作为动态角色或场景回放表示，但目前只有论文。[GaussianSpline](https://arxiv.org/abs/2610.35224)
- **实时交互**：ReferStream 支持因果流式注入，却未披露帧率、端到端延迟、显存和并发，不能据此声称已达到直播产品要求。
- **音视频与情感表达**：本周期没有高质量新增能证明语音、情绪、视线和可打断 turn-taking 同时提升。
- **工程部署**：本周期缺少可直接接入 WebRTC、Unity 或 Unreal 的新 SDK。最有工程价值的工作仍停留在可以映射为事件接口或 animation clip 的论文层。
- **安全与身份治理**：没有新的授权、肖像权、水印或身份溯源方案。ReferStream 允许任意时刻注入人物参考图，反而提高了对授权校验、参考图 provenance 和输出水印的需求。

## 5. 世界模型进展

- **架构与训练**：Predictable Latents 表明 video tokenizer/autoencoder 不能只优化重建；latent 的时间可预测性直接影响长时 rollout、训练成本和 serving 成本。
- **可控与可玩生成**：GameVerse 为分钟级相机与事件控制提供数据基础；Best-of-N WAM 则让生成的未来真正进入动作选择闭环。
- **长时与物理一致性**：GameVerse解决训练序列过短，ScIntBench解决“看起来正确但操作即坏”的评测盲点。前者偏数据，后者偏 CI，两者互补。
- **空间表示**：LabWorld 将像素中的设备、动作和步骤恢复为可编辑几何及事件状态；这比只生成下一帧更接近可复用数字孪生。
- **机器人、游戏与仿真**：MoGAP 证明视频预测目标的空间分配会影响控制性能；ScIntBench 则显示现有生成场景离机器人训练资产仍有显著差距。
- **实时部署与评测**：Predictable Latents 给出成本收益，但没有生产吞吐、P95 延迟或多租户数据；Best-of-N 会增加候选 rollout 成本，需要与成功率收益共同评估。

## 6. Coding × 新型互动作品

### 链路一：生成途中加入新角色

**ReferStream 流式参考注入 → 事件系统调用 `inject_reference` → 已生成剧情继续且新角色进入 → 互动电影、直播和品牌体验。**

代码负责参考到达时间、目标区域、角色权限和剧情状态；生成模型负责把新主体融入视觉流；传统运行时负责音频、UI、审核和回退。已有录制 demo，但没有可调用代码或实时延迟证据。

### 链路二：视频编译成可编辑程序

**CodeCap → MLLM 生成图形 primitive 与时间函数 → 浏览器或图形运行时执行并允许 coding agent 修改 → 可交互教育动画、数据故事和网页作品。**

代码负责精确布局、时序和交互逻辑；模型负责从视频恢复程序与语义；Canvas/WebGL/Three.js 负责确定性渲染。当前 benchmark 证明方向可行，但复杂运动仍容易被错误编码。

### 链路三：长时可玩世界的数据与运行时

**GameVerse 分钟级视频、相机轨迹和事件 caption → action-conditioned world model → 游戏服务器维护玩家与规则状态 → 长时可玩视频和生成式关卡。**

代码应是状态与规则的权威来源，模型承担写实观察生成；不能让视频模型单独保存库存、生命值和离屏实体。当前新增是数据层，尚非完整可玩产品。

### 链路四：生成场景的物理 CI

**3D 生成器 → ScIntBench 动作脚本 → 物理引擎执行推、开、探测 → 自动拒绝不可交互资产 → 游戏、机器人和培训仿真。**

代码负责动作测试、碰撞规则、阈值和失败报告；生成模型负责场景与资产；物理引擎提供权威动力学。已有 12 个系统的论文评测证据，但工具尚未公开。

### 链路五：实验视频到机器人数字孪生

**实验录像 → LabWorld 恢复资产、关节和事件 → agent 编辑 SOP 并在模拟器回放 → 实验机器人训练、操作审计和教学。**

代码负责程序步骤、约束和异常处理；模型负责感知、资产恢复和运动估计；模拟器负责可重复执行。已有真实移液与涡旋实验验证，样本规模和泛化能力仍有限。

### 链路六：代码监督长时机器人任务

**RoboDuo coding agent → 调用 full-task VLA 尝试 → 检查完成状态 → 调用 atomic VLA 恢复 → 可靠机器人工作流。**

RoboDuo 的代码不直接产生低层动作，而是决定何时尝试、如何检查以及调用哪个 VLA；论文报告其可包装冻结的 X-VLA 和 π0.5，并在真实机器人上恢复失败 rollout。[RoboDuo](https://arxiv.org/abs/2610.34108) 这是“coding agent 作为运行时监督器”而不是“coding agent 直接控制电机”的更稳健分工。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 技术栈与互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | GameVerse、ScIntBench | 世界模型＋游戏服务器＋物理 CI；玩家动作驱动画面 | 数据集/benchmark | 未披露 | 状态漂移、资产不可交互 | [GameVerse](https://arxiv.org/abs/2610.37257) |
| 影视/虚拟制作 | WorldCast、ReferStream | 相机轨迹、角色 anchor、流式角色事件 | 录制 demo | 未披露 | 镜头连续性、授权、渲染稳定性 | [WorldCast](https://arxiv.org/abs/2610.33135) |
| 直播与电商 | ReferStream | 事件总线＋参考图注入＋实时音视频 | 研究 demo | 未披露 | P95 延迟、审核、商品一致性 | [项目页](https://xushuolin.github.io/ReferStream/) |
| 品牌互动 | CodeCap、ReferStream | 代码生成动画、运行中加入品牌主体 | 概念到 demo | 未披露 | 复杂时序代码不可靠 | [CodeCap](https://arxiv.org/abs/2610.37341) |
| 教育培训 | CodeCap、LabWorld | 可执行动画＋可编辑实验步骤 | 研究原型 | 未披露 | 内容正确性与教学审核 | [LabWorld](https://arxiv.org/abs/2610.36592) |
| 企业数字员工 | ReferStream | 会话 agent＋流式视觉角色＋工具调用 | 本报告推断 | 未披露 | 本周期无语音/turn-taking 新证据 | [ReferStream](https://arxiv.org/abs/2610.37566) |
| 陪伴与社交 | TopX、ReferStream | 多角色事件＋任意骨架动作 | 研究原型 | 未披露 | 长期身份和安全治理 | [TopX](https://arxiv.org/abs/2610.34321) |
| 空间计算 | WorldCast、GaussianSpline | 6DoF 相机＋动态 4D 表示 | 研究原型 | 未披露 | 移动端预算、视角外补全 | [GaussianSpline](https://arxiv.org/abs/2610.35224) |
| 机器人/仿真 | MoGAP、RoboDuo、ScIntBench | WAM/VLA＋代码监督＋物理验收 | 仿真及小规模真机 | 未披露 | 安全认证、算力、场景泛化 | [MoGAP](https://ismty0805.github.io/mogap/) |

## 8. 可复现资源与开发者入口

| 资源 | 开放情况 / 许可证 | 硬件或成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|---|
| [Predictable Latents](https://github.com/mattia-scardecchia/predictable-latents) | 完整训练与评测代码，MIT；无论文预训练权重 | Python 3.11、CUDA GPU、Bridge V2 数据；FLUX 权重需接受条款 | 值得 | 先在 CPU 跑 toy study，再用小数据运行 AE→WM 两阶段训练 |
| [ReferStream 项目仓库](https://github.com/Xushuolin/ReferStream) | 仅网页、论文和 24 个 MP4；未声明模型代码许可证 | 浏览器即可查看 | 仅验证视觉效果 | 检查 InShot/MultiShot 长样例中的旧角色保持和新角色出现时刻 |
| [MoGAP 项目页](https://ismty0805.github.io/mogap/) | 代码、checkpoint 标记为 Coming Soon | 未披露 | 暂不复现 | 等待代码；先复核 GR-1 和真实机器人表格 |
| [GameVerse](https://arxiv.org/abs/2610.37257) | 论文已公开；数据下载与许可证待核验 | 预计存储和训练成本高 | 跟踪 | 先确认数据清单、相机轨迹格式和商业使用许可 |
| [CodeCap-Bench](https://arxiv.org/abs/2610.37341) | 论文使用 CC 许可；benchmark 入口未确认 | 可先用现有 MLLM＋Canvas | 值得做小型替代实验 | 选择 10 个动态图标视频，生成 JS/Canvas 并逐帧比较 |
| [ScIntBench](https://arxiv.org/abs/2610.37326) | 论文公开；代码未见 | 需要物理仿真运行时 | 值得跟踪 | 自建“门可开、抽屉可拉、杯可推”三个最小测试 |
| [Honeycomb-Surfels](https://honeycomb-surfels.github.io/) | 项目页和论文；代码未见 | 未披露 | 暂跟踪 | 等代码后以相同显存预算对比 2DGS；论文称速度最高提升 2.31 倍 |
| [RoboDuo](https://arxiv.org/abs/2610.34108) | 论文；代码未见 | 需要 VLA、LIBERO 或真机 | 可复刻框架思想 | 在 LIBERO 上实现 attempt/check/recovery 三函数 wrapper |

## 9. 系统架构与技术趋势判断

明显升温的方向有四个：分钟级训练数据、动力学友好的 latent、模型外部的显式状态、以及可执行的自动验收。纯粹依靠视频模型隐式记住世界状态的路线正在降温；越来越多方案把规则、事件、几何或任务完成状态交还给代码和运行时。

正在形成的主流架构是：

`用户/agent 输入 → 显式状态与规则层 → 候选动作或事件 → 生成模型渲染/预测 → judge 或物理引擎验收 → 状态更新与失败恢复`

其中代码负责确定性和审计，生成模型负责高维视觉与行为先验，传统引擎负责碰撞、权威状态和低延迟交互。RoboDuo、ScIntBench、CodeCap 和 LabWorld 分别对应监督编排、物理验收、程序化表达和显式数字孪生。

仍未解决的问题包括：流式视频的真实端到端延迟；分钟级 rollout 的身份、几何和离屏状态保持；生成资产的碰撞体、关节和材质完整性；工业并发与成本；肖像和训练数据授权；模型失败时的安全降级。

需要降级看待的项目特征是：只展示精选视频、没有长样例；把“实时”写入标题却不披露硬件和延迟；GitHub 只托管项目页；没有模型权重、输入输出格式或可重复 benchmark。ReferStream 的任务定义重要，但当前“GitHub 已有仓库”不能表述为“代码已开源”；MoGAP 同样应等待实际代码和 checkpoint。

## 10. 论文精读候选

1. [Video World Models Need Predictable Latents](https://arxiv.org/abs/2610.34663)  
   值得读：直接挑战以重建质量主导 tokenizer 设计的惯例。重点看 temporal objectives、past-conditioned decoder、matched-budget 实验及 WorldGym correlation。差异在于同时保留像素重建和 latent 可预测性。风险是完整训练成本较高且无预训练权重。

2. [GameVerse](https://arxiv.org/abs/2610.37257)  
   值得读：分钟级数据可能比继续扩大模型更能改善长时世界生成。重点看数据过滤、跨分钟相机轨迹、事件 caption 和训练消融。风险是数据尚未确认可下载，游戏视频许可也需单独审查。

3. [ScIntBench](https://arxiv.org/abs/2610.37326)  
   值得读：把静态场景质量与真正的 interaction readiness 分开。重点看动作协议、成功判据、失败分类和 12 个系统的差异。风险是仿真器设定可能影响排名。

4. [ReferStream](https://arxiv.org/abs/2610.37566)  
   值得读：首次系统化处理流式生成过程中新参考的到达事件。重点看 Joint Latent、cache 恢复边界、InShot/MultiShot protocol。风险是没有代码和延迟披露，暂难验证“流式”工程价值。

5. [LabWorld](https://arxiv.org/abs/2610.36592)  
   值得读：展示从视频世界模型走向显式、可编辑工业数字孪生的路径。重点看事件约束、资产复用和物理约束统计。风险是实验规模小，设备类别泛化未知。

## 11. 下周跟踪与可行动建议

### 持续跟踪

1. GameVerse 是否开放原始视频、相机轨迹、事件 caption、数据过滤代码及商业许可证。
2. ReferStream 是否发布推理代码、基础模型依赖、峰值显存、首帧和注入延迟。
3. MoGAP 的代码和 checkpoint 能否复现论文中的 Cosmos3、Fast-WAM 与真实机器人增益。
4. ScIntBench 是否开放场景转换器、动作脚本和物理引擎配置。
5. CodeCap-Bench 的 323 个视频、ground-truth 程序及执行沙箱是否公开。
6. Predictable Latents 的收益能否迁移到更大视频 tokenizer 和可玩世界模型。
7. LabWorld 能否输出 USD、URDF、MJCF 或 SDF 等工业可消费格式。
8. 流式角色注入、人物参考和语音克隆是否出现统一授权与 provenance 机制。

### 本周可动手的小实验

1. **视频到可执行 Canvas**
   - 目标：验证代码 caption 是否比自然语言更适合互动动画。
   - 组件：10 个简单动态图标视频、MLLM、JavaScript Canvas、逐帧 SSIM/LPIPS。
   - 难点：时间函数、遮挡和对象持久 ID。
   - 成功判据：程序可执行；关键事件顺序正确率超过 90%；修改速度或颜色无需重新生成视频。

2. **生成场景物理冒烟测试**
   - 目标：建立简化版 ScIntBench。
   - 组件：任一 text-to-3D/scene generator、Blender 或 Unity、三个动作脚本。
   - 难点：自动生成碰撞体、关节识别和尺度归一化。
   - 成功判据：场景导入后能够自动完成“开门、拉抽屉、推杯子”，且无穿透或整体解体。

3. **Attempt–Check–Recovery 角色或机器人 agent**
   - 目标：验证 RoboDuo 式代码监督是否优于单次大模型调用。
   - 组件：游戏 NPC 或仿真机器人、主策略、原子技能库、视觉完成检查器。
   - 难点：可靠判定部分完成状态，避免无限恢复循环。
   - 成功判据：固定 30 个长任务上，成功率相对单策略提高至少 10%，额外延迟低于 30%。

4. **长时 world-model latent 对照**
   - 目标：验证“可预测 latent”是否降低 rollout 漂移。
   - 组件：[Predictable Latents 代码](https://github.com/mattia-scardecchia/predictable-latents)、小规模 Bridge 子集、单张 CUDA GPU。
   - 难点：训练预算匹配和 OOD 数据准备。
   - 成功判据：相同训练步数与采样步数下，50 帧以上 rollout 的感知误差下降，同时推理时间不增加。
