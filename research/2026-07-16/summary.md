# 数字人、世界模型与互动作品研究日报：2026-07-16

检索窗口：2026-07-10 至 2026-07-16。  
截至时间：2026-07-16 09:00 CST。arXiv 最新可核验批次为 7 月 15 日公告；发布日期均按论文提交历史、官方仓库或项目页核对，不采用搜索排序时间。

## 1. 本日摘要

本周期最强增量来自世界模型与实时运行时，而不是传统 talking-head 数字人。阿里通义实验室的 [WanToFight](https://arxiv.org/abs/2607.12592) 将双人键盘输入、角色绑定、局部因果控制、流式自回归生成和单卡 30 FPS 放入同一系统，表明“可玩视频”开始从单人漫游推进到多人对抗，但当前仍只有官方演示，没有代码、权重或可调用 API。机器人侧，[FlowWAM](https://arxiv.org/abs/2607.13017) 用光流统一“预测动作”和“按动作生成未来画面”，并开放训练、推理、WebSocket 服务、权重和 RoboTwin 接口，是本日工程证据最完整的世界模型项目。

与此同时，[The Seriality Gap](https://arxiv.org/abs/2607.13031) 给出一个重要反面结论：增加扩散去噪步数并不会自动增加处理连续因果链所需的串行计算，长链碰撞、接触和状态传播仍需自回归分块、更深网络或显式搜索。它解释了为何视觉上稳定的生成世界仍会在物理与规则层失真，也支持“生成模型负责外观、传统引擎保存权威状态”的混合架构。

Coding 侧出现了比通用 agent wrapper 更具体的接口：[Contract-Grounded Behavior Tree Synthesis](https://arxiv.org/abs/2607.12220) 让 coding agent 通过 MCP 获取机器人技能、合法算子和组合模板，再由运行时验证行为树后执行；这套“能力契约—生成代码—验证门—执行器”同样适用于 NPC、数字员工和互动展演。腾讯 [Hy-Embodied-VLM-1.0](https://github.com/Tencent-Hunyuan/HY-Embodied) 则开放 OpenAI-compatible vLLM 服务和流式图像请求，但约 30B 总参数、BF16 权重约 86GB，离低成本端侧运行仍有距离。[Jetson-PI](https://arxiv.org/abs/2607.12659) 从另一端解决异步推理与执行错位，已开放 Jetson Orin 推理引擎和部分训练评测代码。

数字人正面能力本周期暂无新的高质量 talking-head、lip-sync、语音驱动表情或可打断对话头像基座。新增主要是 [SPIN-4DGS](https://arxiv.org/abs/2607.12362) 对高速人体/体育运动的 4D 重建，以及情绪识别等表演感知组件；它们可进入资产或感知管线，但不能等同于完整数字人产品。产业成熟度整体仍以研究原型、开放组件和官方 demo 为主，未发现过去七天内可核验的客户规模化部署。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 低 | SPIN-4DGS改善高速人体运动重建；无新 talking-head 基座 | 仍停留在4D表示和感知组件 | 相比上期 Motion4Motion、HandFlow，今天没有同等级直接新增 |
| 世界模型 | 很高 | WanToFight实现双人实时对抗；FlowWAM统一动作与视频预测；Seriality Gap揭示因果瓶颈 | 从单人漫游进一步进入多人动作绑定、机器人闭环和结构化评测 | 新增实时多人战斗、光流动作接口和串行因果理论 |
| Coding × 互动作品 | 很高 | MCP技能契约、WebSocket策略服务、OpenAI-compatible VLM接口、端侧异步调度 | 出现可复用的软件边界和运行时验证门 | 相比上期的 UE5 数据引擎和视频记忆，进一步进入“代码生成可执行控制结构” |

## 3. 最值得关注的 10 个进展

### 1. WanToFight：双人实时生成式战斗引擎

- **类型 / 时间 / 主线：** 论文、项目 demo；2026-07-14；世界模型 / Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12592)、[项目页](https://humanaigc.github.io/wantofight/)。
- **相对上期的新变化：** 全新纳入；上期的 MIRA、LingBot-World 2.0 已覆盖多人或多 agent，本项目首次把双人对抗、局部因果按键控制和单卡实时视频生成同时落到完整比赛。
- **核心贡献：** 基于 Wan-1.3B，采用 block-causal attention、rolling KV cache、玩家—角色关联模块和局部因果键盘注入；四步 DMD 蒸馏与裁剪 VAE 解码器支持完整比赛期间连续生成。
- **Coding 接口：** 输入是两套键盘事件流；模型按 chunk 生成下一段画面。当前没有公开 SDK、网络协议、代码或权重。
- **互动/工业场景：** AI 原生格斗游戏、互动直播赛事、角色战斗预演、广告角色即时对抗。
- **成熟度 / 证据：** 官方项目 demo；作者报告 RTX 5090、512×384、30 FPS。项目页明确仅用于研究和效果展示。
- **重要性 / 阅读：** 高 / 必读。

### 2. FlowWAM：用光流统一世界预测与机器人动作

- **类型 / 时间 / 主线：** 论文、GitHub、权重、dataset、推理服务；2026-07-14；世界模型 / 机器人 / Coding。
- **官方链接：** [论文](https://arxiv.org/abs/2607.13017)、[项目页](https://flow-wam.github.io/)、[GitHub](https://github.com/YixiangChen515/FlowWAM)。
- **相对上期的新变化：** 全新纳入；相比 LingBot-VA 2.0 的语义动作 tokenizer，FlowWAM 给出了更直接、可从无动作标签视频提取的视觉动作表示。
- **核心贡献：** 将光流编码为 RGB 格式，与场景视频共用 Wan2.2-TI2V-5B 的 VAE 和 DiT。策略模式生成未来 RGB、光流和动作 chunk；世界模型模式固定目标光流并生成受控未来画面。
- **Coding 接口：** Apache-2.0 仓库提供训练入口、WebSocket 推理服务器、RoboTwin drop-in policy、50 项任务批量评测脚本、权重和数据。
- **场景：** 机器人规划、策略离线验收、运动轨迹可视化、遥操作预演。
- **成熟度 / 证据：** 已开源；作者报告 RoboTwin Clean/Random 成功率 92.94%/92.14%，真实机器人七项任务平均 75.7%，尚无第三方复测。
- **重要性 / 阅读：** 高 / 必读。

### 3. The Seriality Gap：视频扩散模型的串行因果缺口

- **类型 / 时间 / 主线：** 论文、benchmark、GitHub、权重；2026-07-14；世界模型 / 评测。
- **官方链接：** [论文](https://arxiv.org/abs/2607.13031)、[项目页](https://seriality-gap.jdiazchao.com/)、[代码](https://github.com/jdiazchao/seriality-gap)。
- **相对上期的新变化：** 全新纳入；为 Cycle-World 所处理的“漂移”补充了更基础的计算结构解释。
- **核心贡献：** 在多球硬碰撞中控制视频长度和因果链长度，证明双向扩散随依赖事件链增长而退化；增加去噪步数不能提供可扩展的串行深度，自回归分块和更深 backbone 更有效。
- **Coding 接口：** 仓库包含数据合成、Wan VAE 编码、双向/自回归训练和 IoU、位移误差评测脚本；未发现明确许可证。
- **场景：** 世界模型架构选型、物理仿真验收、生成式游戏回归测试。
- **成熟度 / 证据：** 已开放研究代码和模型入口；结论基于受控合成物理实验。
- **重要性 / 阅读：** 高 / 必读。

### 4. MCP 契约驱动的行为树生成

- **类型 / 时间 / 主线：** 论文、机器人系统；2026-07-13；Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12220)。
- **相对上期的新变化：** 全新纳入；首次把 MCP 明确放在“可执行技能发现—行为树合成—运行时验证”链路中。
- **核心贡献：** coding agent 从机器人侧 MCP server 查询技能库、允许的行为树算子和组合模板，生成后必须通过 runtime validation gate。
- **Coding 接口：** MCP、行为树、技能参数 schema、Nav2、PyRoboSim；自然语言操作者不需要知道底层技能签名。
- **场景：** 服务机器人、训练 NPC、数字员工工作流、互动展览设备。
- **成熟度 / 证据：** 研究系统；论文报告 110 个仿真任务及 14 个 Husarion Panther 实机任务，代码开放状态未确认。
- **重要性 / 阅读：** 高 / 必读。

### 5. Hy-Embodied-VLM-1.0：可服务化的具身推理模型

- **类型 / 时间 / 主线：** 模型、权重、GitHub、服务接口；2026-07-14；Coding / 具身 agent。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12894)、[GitHub](https://github.com/Tencent-Hunyuan/HY-Embodied)。
- **相对上期的新变化：** 全新 1.0 版本；相对 0.5，作者报告平均提升 8.4%，并加入完整 vLLM 服务路径。
- **核心贡献：** 约 30B 总参数、每 token 激活约 3B 的 MoE VLM，覆盖状态理解、动作转移推理和长序列适应性推理。
- **Coding 接口：** Apache-2.0；提供 Transformers 推理、vLLM 插件、OpenAI-compatible `/v1/chat/completions`、图像输入、流式响应和 `enable_thinking` 开关。
- **场景：** 导航 agent、机器人任务规划、空间数字员工、视觉操作助手。
- **成熟度 / 证据：** 已开放代码和权重；BF16 权重约 86GB、缓存约 120GB，推荐四张 80GB GPU 服务。
- **重要性 / 阅读：** 高 / 必读。

### 6. Jetson-PI：端侧异步 VLA 运行时

- **类型 / 时间 / 主线：** 论文、GitHub、推理引擎；2026-07-14；实时部署 / Coding。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12659)、[算法代码](https://github.com/PKU-SEC-Lab/Jetson-PI)、[端侧引擎](https://github.com/PKU-SEC-Lab/Jetson-PI-Edge)。
- **相对上期的新变化：** 全新纳入；补足此前世界模型“模型快但控制频率低”的部署缺口。
- **核心贡献：** 用已承诺动作预测未来环境表示，校正异步感知—执行错位；结合置信调度、CUDA Graph、GPU 常驻缓冲和 flow unrolling。
- **Coding 接口：** Apache-2.0 主仓库开放 LIBERO 训练/评测；另有 llama.cpp 派生的 Jetson Orin 推理引擎。
- **场景：** 移动机器人、端侧远程操作、低功耗空间交互装置。
- **成熟度 / 证据：** 已开源部分系统；作者报告相对朴素 PyTorch 控制频率提升 8.66 倍，真实硬件长期稳定性未披露。
- **重要性 / 阅读：** 高 / 必读。

### 7. Hallo4D：以多模态模型检测并修复 4D 幻觉

- **类型 / 时间 / 主线：** 论文、4D 生成质量控制；2026-07-14，7 月 15 日修订；数字人/世界模型基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12752)。
- **相对上期的新变化：** 全新纳入；与上期 3D-DefectBench 的验收思路相似，但增加了候选修复和多模型投票。
- **核心贡献：** 从多视图、多帧渲染中检测结构重复、几何错位、身份闪烁和漂移，再以共识驱动的图像空间优化修正；无需修改原生成模型。
- **Coding 接口：** 可编排为“生成—渲染—LMM检测—候选修复—投票—回写”的资产 CI；代码尚未确认开放。
- **场景：** 数字人 4D 资产、动态场景、虚拟制作质量门。
- **成熟度 / 证据：** 研究原型，论文作者实验。
- **重要性 / 阅读：** 中高 / 必读。

### 8. SPIN-4DGS：高速运动的稳定 4D Gaussian 重建

- **类型 / 时间 / 主线：** ICLR 2026 论文、项目页；2026-07-14；数字人 / 4D 表示。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12362)。
- **相对上期的新变化：** 全新纳入；补足 OmniX、HandFlow 对高速、大帧间位移场景覆盖不足。
- **核心贡献：** 不直接建模时间位移，而从显式时空位置通过轻量前馈网络预测 Gaussian 属性，减少快速运动目标丢失。
- **Coding 接口：** 可作为多机位人体、体育和虚拟制作重建模块；尚未确认代码、标准导出或引擎插件。
- **场景：** 体育数字人、舞蹈重建、自由视角回放、动作资产采集。
- **成熟度 / 证据：** 研究原型；作者报告 CMU Panoptic 篮球场景较 D3DGS 提升 1.83 PSNR。
- **重要性 / 阅读：** 中高 / 可读。

### 9. ACID：训练免费的自适应视频生成缓存

- **类型 / 时间 / 主线：** 论文、推理加速；2026-07-14；支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12358)。
- **相对上期的新变化：** 全新纳入；与上期 TMD 的少步生成互补，ACID 可直接包装现有动态缓存方案。
- **核心贡献：** 识别漂移信号变化迅速的关键去噪步，在关键步保守计算、其他步骤激进缓存；兼容 TeaCache、EasyCache 和 DiCache。
- **Coding 接口：** 设计为 training-free wrapper，覆盖 HunyuanVideo、Wan 2.1、CogVideoX；代码未确认开放。
- **场景：** 批量视频生成、交互预览、世界模型非关键帧加速。
- **成熟度 / 证据：** 论文原型；作者报告 TeaCache＋HunyuanVideo 相对无缓存最高 2.16 倍加速。
- **重要性 / 阅读：** 中高 / 可读。

### 10. X-Lens：异构相机实时度量深度

- **类型 / 时间 / 主线：** 论文、synthetic dataset；2026-07-14，7 月 15 日修订；空间基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.12993)。
- **相对上期的新变化：** 全新纳入；相对 FoundationGeo 的单目输入，X-Lens 面向鱼眼与针孔混合的实时多机位系统。
- **核心贡献：** 0.04B 参数，利用标定 token 和 Jacobian 畸变偏置统一异构投影；同步输出深度与全局度量尺度。
- **Coding 接口：** 可接入 XR、机器人或多机位捕捉前端；API、代码和数据下载入口尚未确认。
- **场景：** 数字人捕捉空间、移动机器人、沉浸式场馆和安全交互区域。
- **成熟度 / 证据：** 研究原型；作者报告最高 41 FPS，OmniScene 含约 26.6 万组六视图帧。
- **重要性 / 阅读：** 中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** 本周期暂无新的高质量 talking-head、speech-driven avatar、lip-sync 或全身音频驱动基座。WanToFight 包含角色动作与身份绑定，但它生成的是完整游戏画面，不输出可复用骨骼、mesh 或 blendshape。

**3D/4D 表示。** SPIN-4DGS 对快速运动和大帧间位移更稳，Hallo4D则处理结构漂移、身份闪烁和视角不一致。两者组合后可形成“重建—检测—修复”管线，但当前缺少标准化资产导出和引擎插件。

**实时交互。** 没有新的完整实时数字人系统。X-Lens 的 41 FPS 只代表度量深度前端，不能推导出包括人脸、手势、渲染和语音在内的端到端帧率。

**音视频与情感表达。** 未发现新的生成式情绪表演模型。窗口内的 [HSEmotion ABAW 系统](https://arxiv.org/abs/2607.12774)以轻量面部特征、HuBERT 音频和文本分类器识别情绪及犹豫，可作为数字人 turn-taking 信号，但不是表情生成器。

**工程部署。** 当前能进入产品的仍是感知、深度、4D 质量门等单组件；完整表演应继续由骨骼、blendshape、状态机和 RTC 引擎承担确定性实时部分。

**安全与身份治理。** 本周期未发现新的肖像授权、水印或数字人身份溯源机制。Hallo4D 的一致性检测不等价于身份安全。

## 5. 世界模型进展

**架构与训练。** WanToFight 采用流式分块、自回归 KV cache 和四步蒸馏；FlowWAM 以光流作为与视频预训练天然兼容的动作模态。共同趋势是把动作从低维编号转换成可与视觉 token 对齐的稠密时空信号。

**可控/可玩生成。** WanToFight 已证明双玩家按键可分别绑定到两个角色，强于单相机漫游；但没有开放试玩、状态 API 或代码，仍不能视作开发者平台。

**长时与物理一致性。** Seriality Gap 表明“视觉连续”和“因果正确”是两件事。碰撞链、连招判定、库存和任务状态不应只存在于视频 latent 中。

**空间表示。** SPIN-4DGS、Hallo4D 和 X-Lens分别覆盖动态表示、质量修复与度量感知，可作为生成世界的空间缓存和观测层。

**机器人/仿真。** FlowWAM 已连接 RoboTwin、WorldArena 和真实 Franka/ARX；Hy-Embodied 负责高层理解与计划，Jetson-PI负责低延迟执行，形成较完整的推理—预测—控制栈。

**实时部署与评测。** 新增明确指标包括 WanToFight 单张 RTX 5090 30 FPS、X-Lens 最高 41 FPS、Jetson-PI 控制频率提升和 ACID 最高 2.16 倍加速。除 FlowWAM 和 Jetson-PI 外，多数尚无第三方复现。

## 6. Coding × 新型互动作品

1. **双人按键 → WanToFight 局部因果控制 → 实时生成比赛画面 → AI 原生对战游戏**  
   代码负责输入采样、房间同步、比分和碰撞规则；生成模型负责连续视觉与角色表现；传统服务器必须保存权威战斗状态。30 FPS 有官方证据，开发 API 和可复现性没有。

2. **目标运动 → 光流轨迹 → FlowWAM WebSocket 服务 → 可预演的机器人动作**  
   业务代码生成或编辑目标光流；FlowWAM渲染未来并解码动作 chunk；机器人控制器执行并用新观测闭环。代码、权重、仿真 policy 与真实机器人实验均已存在。

3. **自然语言任务 → MCP 技能契约 → coding agent 生成行为树 → 运行时验证 → NPC/机器人执行**  
   agent 只组合运行时声明的技能和合法算子；状态机/行为树承担确定性控制；生成模型负责计划结构。实机机器人已有论文证据，迁移至游戏 NPC 属本报告推断。

4. **视觉观测 → Hy-Embodied OpenAI-compatible API → 高层计划 → Jetson-PI 异步执行**  
   Hy-Embodied负责空间理解、步骤规划和目标验证；Jetson-PI负责预测执行时刻的未来表示并调度动作专家；安全控制器处理急停和硬约束。组件分别可运行，端到端组合属于本报告推断。

5. **4D资产 → 多视图/多帧渲染 → Hallo4D检测与投票修复 → 数字人资产 CI**  
   代码负责批量渲染、错误 schema、候选版本和回归阈值；LMM负责诊断；4D生成器负责重优化；人工审核处理低置信结果。论文验证了方法，生产 CI 属本报告推断。

6. **互动世界回放 → Seriality Gap 测试集 → 因果链长度分桶 → 发布质量门**  
   代码从游戏日志抽取碰撞、接触和连锁事件；世界模型生成候选 rollout；评测器比较对象数、轨迹和最终状态。受控 benchmark 已开源，针对商业游戏的适配属于本报告推断。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本或延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | WanToFight＋权威游戏服务器 | 双人实时按键对抗 | 官方 demo | RTX 5090、512×384、30 FPS | 无代码/API；规则状态不可审计 | [项目页](https://humanaigc.github.io/wantofight/) |
| 影视/虚拟制作 | SPIN-4DGS＋Hallo4D＋DCC | 快速人体重建、自动修复 | 研究原型 | 未披露 | 导出、精修、遮挡和版权 | [SPIN-4DGS](https://arxiv.org/abs/2607.12362) |
| 直播与电商 | 情绪识别＋数字人＋RTC | 犹豫检测、切换讲解策略 | 组件原型 | 未披露 | 误判、口型、审核 | [HSEmotion](https://arxiv.org/abs/2607.12774) |
| 品牌互动 | 生成式世界＋事件服务器 | 多用户控制角色和事件 | 机会判断 | 未披露 | 品牌资产一致性和并发成本 | [WanToFight](https://arxiv.org/abs/2607.12592) |
| 教育培训 | MCP行为树＋Hy-Embodied | 自然语言生成受约束步骤 | 研究系统 | Hy模型推荐4×80GB服务 | 错误计划、权限和审计 | [MCP行为树](https://arxiv.org/abs/2607.12220) |
| 企业数字员工 | Hy-Embodied＋业务技能契约 | 看图、规划、调用有限工具 | 已开源组件 | BF16约86GB | 成本、数据隔离、幻觉 | [GitHub](https://github.com/Tencent-Hunyuan/HY-Embodied) |
| 陪伴与社交 | 数字人＋情绪感知＋状态机 | 多模态轮次和情绪响应 | 机会判断 | 未披露 | 情绪安全和长期身份一致性 | [ABAW系统](https://arxiv.org/abs/2607.12774) |
| 空间计算 | X-Lens＋XR引擎 | 多相机度量空间和近场交互 | 研究原型 | 作者报告最高41 FPS | 移动端验证、标定漂移 | [X-Lens](https://arxiv.org/abs/2607.12993) |
| 机器人/仿真 | FlowWAM＋Hy-Embodied＋Jetson-PI | 未来预演、行为规划、端侧执行 | 已开源研究栈 | Jetson频率最高提升8.66倍 | sim-to-real、安全和算力 | [FlowWAM](https://github.com/YixiangChen515/FlowWAM) |

## 8. 可复现资源与开发者入口

- [FlowWAM](https://github.com/YixiangChen515/FlowWAM)：Apache-2.0，包含训练、权重、数据、WebSocket server 和 RoboTwin policy。本周最值得复现。最小路径是下载 Wan2.2-TI2V-5B 与 FlowWAM checkpoint，启动服务后评测一个 `place_dual_shoes` 任务；显存需求未明确披露。
- [FlowWAM WorldArena](https://github.com/YixiangChen515/FlowWAM_WorldArena)：Apache-2.0，用于光流条件世界模型评测；适合先复现 121 帧 rollout 的轨迹指标。
- [Seriality Gap](https://github.com/jdiazchao/seriality-gap)：提供合成数据、双向/自回归训练和评测。许可证未标明，适合研究验证，不宜直接嵌入商用代码。
- [Hy-Embodied-VLM-1.0](https://github.com/Tencent-Hunyuan/HY-Embodied)：Apache-2.0；提供 Transformers 与 vLLM 路径、OpenAI-compatible endpoint。最小验证是单图空间问答；完整 BF16 模型需约 86GB 显存和约 120GB 磁盘缓存。
- [Jetson-PI](https://github.com/PKU-SEC-Lab/Jetson-PI)：Apache-2.0 主代码，当前开放 LIBERO 训练评测；最小路径是在仿真中比较同步与不同推理延迟下的成功率。
- [Jetson-PI-Edge](https://github.com/PKU-SEC-Lab/Jetson-PI-Edge)：面向 Jetson Orin 的 llama.cpp 派生引擎；需注意上游组件各自许可证。
- WanToFight、Hallo4D、SPIN-4DGS、ACID、X-Lens：目前只有论文或项目页，暂不建议将其作为生产依赖。

## 9. 系统架构与技术趋势判断

明显升温的是多人动作绑定、视频原生动作表示、MCP能力契约、端侧异步运行时和因果链评测。传统 talking-head 与语音驱动头像本周期降温，没有新的强基座发布。

正在形成的可复用架构是：

- coding agent 从 MCP、schema 或技能注册表读取允许能力；
- 行为树、状态机或权威服务器保存对象、剧情、比分和安全状态；
- 世界模型生成视觉未来、动作建议或候选计划；
- 光流、深度、4DGS等表示承担模型与运行时之间的空间接口；
- WebSocket、OpenAI-compatible API 或引擎插件提供持续调用；
- 运行时验证门、因果 benchmark 和 LMM 质量门拦截错误；
- 传统渲染/物理引擎在模型延迟或置信度不足时降级接管。

尚未解决的问题包括：生成画面向权威对象状态的可靠反写、多人网络同步、接触物理、低成本并发、可打断数字人表演、统一的脸手身语音驱动、许可证和肖像权。

需要降级看待的说法包括：将单卡 30 FPS 等同于可商用游戏引擎；将视觉稳定等同于碰撞和规则正确；将单组件 41 FPS 等同于完整空间数字人实时运行；仅有项目视频却没有可运行入口的“实时系统”。WanToFight 研究意义很高，但工程透明度目前明显低于 FlowWAM、Hy-Embodied 和 Jetson-PI。

## 10. 论文精读候选

1. [The Seriality Gap](https://arxiv.org/abs/2607.13031)：重点读受控硬球数据、复杂度定义、确定性扩散证明和 AR/depth 消融。它与一般“长视频漂移”工作的差异是解释计算结构而非只给纠错模块。复现风险为训练成本及仓库无明确许可证。
2. [FlowWAM](https://arxiv.org/abs/2607.13017)：重点读光流 RGB 编码、共享 DiT、action expert、无标签视频预训练及 WorldArena 指标。差异是一个表示同时服务世界预测和动作解码。风险是 RAFT 光流误差和真实接触动作表达不足。
3. [WanToFight](https://arxiv.org/abs/2607.12592)：重点读玩家关联、局部因果键盘注入、DMD 蒸馏和完整比赛稳定性。差异是双玩家对抗与实时推理并存。风险是不可复现、分辨率低及缺少权威状态接口。
4. [Contract-Grounded Behavior Tree Synthesis](https://arxiv.org/abs/2607.12220)：重点读 MCP contract schema、验证门、组合模板和实机任务。差异是把技能可执行性从 prompt 约定升级为机器可读契约。风险是代码未确认开放。
5. [Jetson-PI](https://arxiv.org/abs/2607.12659)：重点读未来表示校正、置信调度、CUDA Graph 和 Orin 实验。差异是处理异步推理导致的时间错位，而非单纯量化模型。风险是不同机器人和视觉变化下的泛化。

## 11. 下周跟踪与可行动建议

继续追踪：

1. WanToFight 是否开放试玩、代码、权重、输入协议及端到端输入延迟。
2. FlowWAM 在消费级 GPU 上的显存、单步延迟和真实机器人第三方复现。
3. Seriality Gap 的结论能否在复杂游戏、驾驶和机器人接触数据上复现。
4. MCP 行为树系统是否开放 server、contract schema 和验证器。
5. Hy-Embodied 是否发布量化权重、低显存配置与真实服务吞吐。
6. Jetson-PI 在相机抖动、突发障碍和长时运行中的反应延迟。
7. Hallo4D 是否开放代码，LMM 诊断和多模型投票的实际 API 成本。
8. 数字人方向是否出现统一脸、手、全身、语音和 turn-taking 的实时系统。

本周可执行实验：

1. **FlowWAM 最小闭环。** 目标：验证光流是否比离散动作更容易调试。组件：FlowWAM checkpoint、RoboTwin、WebSocket server。难点：双环境依赖和 Wan 权重。成功判据：单任务连续运行，并能从光流可视化定位至少一种失败。
2. **生成世界因果回归测试。** 目标：区分视觉漂移与连续因果失败。组件：Seriality Gap 代码、双向与 AR 配置。难点：训练资源。成功判据：复现因果链增长时双向扩散退化、AR 相对稳定的趋势。
3. **MCP 技能契约原型。** 目标：让 coding agent 只能生成合法 NPC 行为树。组件：本地 MCP server、十个技能 JSON schema、行为树验证器。难点：失败恢复和循环条件。成功判据：50 条自然语言任务中无不存在的技能或非法参数进入执行器。
4. **4D资产自动质量门。** 目标：验证 Hallo4D 式流程能否减少人工初筛。组件：现有 4DGS、六视图渲染、VLM judge、候选版本存储。难点：错误 schema 与误报。成功判据：对身份闪烁、重复结构和快速运动丢失三类缺陷达到可接受召回率，并保留人工终审。
