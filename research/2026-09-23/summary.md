# 数字人、世界模型与互动作品研究日报：2026-09-23

检索窗口：2026-09-17 至 2026-09-23。  
信息截点：2026-09-23 09:40（Asia/Shanghai）。已与 9 月 17、18、21、22 日日报去重；“相对上期”指相对 2026-09-22 日报。9 月 23 日欧美发布尚未完整进入公开索引。

## 1. 本日摘要

本日最强增量不是单项画质提升，而是三条可组合的运行时链路同时变得具体：AVTR-1 把头像模型、双路音频、打断、时钟、WebRTC 和 TensorRT 组成可运行的实时会话栈；WorldCrafter 把相机轨迹、固定容量 3D-aware memory 和视频生成组成可脚本化的持续世界；VideoGen-Agent 则把知识检索、身份保持、生成和验证包装成多轮工具调用。  
数字人方向，AVTR-1 是本周期最接近开发者可直接验证的新增：单张肖像加“自己说话/对方说话”双路音频，可同时生成口型和倾听反应，并开放权重、推理代码和互动 demo。  
3D 头像方面，SAMIRA 将脸部不同语义区域的运动残差、漫反射和高光响应分开建模，改善跨表情驱动和重打光，但仍停留在论文原型。  
世界模型方向，WorldCrafter、GAE 和 CausalWM 分别从记忆、latent 表示和显式因果中间状态解决几何漂移：共同趋势是 RGB 不再是唯一世界状态，深度、相机、点图、光流和视角可查询记忆正成为软件接口。  
机器人侧，DexTacWAM 证明触觉必须作为“待预测的未来状态”而不只是输入条件；另一项蒸馏工作则表明，世界模型可只在训练期提供表示监督，部署策略仍可保持 32 ms 的轻量 VLA 形态。  
Coding × 互动作品方面，新增接口已经覆盖事件总线、相机矩阵、动作字符串、命令行工具、模型缓存、agent tool calls 和验证器；但除 AVTR-1、WorldCrafter、GAE、CausalWM、SVEET 外，多数项目尚未交付可复现代码。  
产业成熟度仍需保守判断：本日没有新的规模化客户部署证据；最可靠的进展是“开发栈可运行”和“研究原型有实机结果”，不能等同于生产 SLA。  
总体上，代码正在从“调用一次生成 API”升级为维护时间、状态、动作、记忆、验证、回退和许可证边界的长期运行层。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | AVTR-1 开放双路音频、主动倾听、打断和流式调度栈 | 从模型 demo 推进到可自托管技术验证 | 昨日 GestureFAR 只有动作延迟；今日补齐端到端运行时 |
| 3D/4D 头像 | 中 | SAMIRA 按脸部语义区建模运动与光照响应 | 仍是论文原型 | 新增可重打光 3D Gaussian 头像路线 |
| 世界模型 | 很高 | WorldCrafter 的可查询记忆、GAE 的几何原生 latent、CausalWM 的光流→点图→RGB 推理链 | 多个项目同时开放代码或权重 | 从“实时生成”进一步转向“状态如何表示和读取” |
| Coding × 互动作品 | 很高 | 事件总线、相机矩阵、动作 DSL、agent 工具调用均成为显式接口 | 出现多个可运行 CLI/demo | 比上期更接近软件组件，而非单体模型 |
| 机器人 / 仿真 | 高 | 触觉世界状态和世界模型表示蒸馏进入动作策略 | 有实机研究验证，暂无产品化 | 新增接触预测和 32 ms 部署证据 |
| 安全与治理 | 低 | AVTR-1 许可证明确禁止未授权肖像使用 | 有使用限制但无新检测/水印突破 | 本周期暂无新的高质量治理技术 |

## 3. 最值得关注的 11 个进展

### 1. AVTR-1：把“会动的头像”补成可打断的实时会话系统

- **类型 / 日期 / 主线：**模型、GitHub、权重、运行时；论文提交 2026-09-19，9 月 22 日进入最近提交列表；数字人、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.22913)、[GitHub](https://github.com/avaturn-live/avtr-1)、[权重](https://huggingface.co/avaturn-live/avtr-1)。
- **相对上期：**新增漏项；上期未收录。
- **核心贡献：**153M 参数自回归 flow-matching 动作生成器同时接收角色和对话者音频；流式音频 encoder、语音调度器、共享时钟和事件总线负责连续播放、主动倾听和 barge-in。
- **Coding 接口：**Python 服务端、TensorRT engine、WebRTC/STUN/TURN、可替换 conversation engine 和 transport worklet。
- **场景：**全双工客服、远程教师、陪伴角色、直播主持。
- **成熟度 / 证据：**已开源、可运行 demo；作者报告 25 FPS，五帧 chunk 在 RTX 4060 Ti 上约 166 ms、RTX 3070 上约 181 ms。播放器到用户的完整 p95 延迟仍未披露。
- **限制：**renderer 和 streamer 为非商业许可证；InsightFace 依赖也限制商业使用，不能直接视为生产级开放方案。
- **重要性 / 阅读：**高 / 必读。

### 2. WorldCrafter：用视角可查询记忆维持分钟级世界一致性

- **类型 / 日期 / 主线：**模型、GitHub、权重、互动 demo；2026-09-21；世界模型、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24984)、[项目页](https://drexubery.github.io/WorldCrafter/)、[代码](https://github.com/TencentARC/WorldCrafter)。
- **相对上期：**首次收录。
- **核心贡献：**memory encoder 将历史多视角观察压缩，pose-conditioned readout 再按目标相机提取固定数量的记忆 token；无需显式深度对应即可支持回访和动态场景。
- **Coding 接口：**接受 `[T,3,4]`/`[T,4,4]` 相机矩阵，也支持 `forward1x2 yaw_left30x3` 形式的动作字符串；每段 33 帧。
- **场景：**生成式展馆、互动电影摄影、游戏原型、空间预演。
- **成熟度 / 证据：**Base/Fast 权重和推理代码已开放；单 GPU Web demo 仍标注“debugging”，分辨率为 384×640，显存与端到端 FPS 未披露。
- **重要性 / 阅读：**高 / 必读。

### 3. GAE：让生成 latent 原生携带 RGB、深度、相机和点图

- **类型 / 日期 / 主线：**论文、GitHub、表示学习；2026-09-21；世界模型基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24981)、[代码](https://github.com/TencentARC/GAE-GeometricAutoEncoder)。
- **相对上期：**首次收录。
- **核心贡献：**将冻结几何基础模型的多层特征压缩为 64/128 通道 latent，同一状态可解码 RGB、深度、相机射线和 point map。受控实验中，作者报告 RealEstate10K、DL3DV 的 FVD 分别下降 12.7% 和 23.1%，前者相机轨迹误差减半。
- **Coding 接口：**提供 codec、训练/评测 CLI、配置、测试和示例；可作为生成器、SLAM 与引擎之间的共享状态格式。
- **场景：**3D 一致视频、自由视角渲染、数字孪生、空间资产生成。
- **成熟度 / 证据：**代码已开放、论文实测；生产吞吐和资源需求未披露。
- **重要性 / 阅读：**高 / 必读。

### 4. CausalWM：显式生成“光流→点图→未来 RGB”

- **类型 / 日期 / 主线：**16B 模型、权重、GitHub；v2 更新于 2026-09-22；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2609.23184)、[代码与文档](https://github.com/AetherLabsAI/CausalWM)。
- **相对上期：**新增；9 月 22 日发布 v2，并补齐推理代码和权重。
- **核心贡献：**同一 Transformer 依次生成光流、相机坐标 XYZ point map 和未来视频；训练使用约 31K 小时具身数据，并结合视频预训练、因果中间训练和多目标 RL。
- **Coding 接口：**`首帧 + 指令 → flow.mp4 + pointmap + rgb.mp4 + provenance.json`，允许控制系统读取中间物理状态，而非只看最终视频。
- **场景：**机器人候选动作解释、合成轨迹审计、物理失败分析。
- **成熟度 / 证据：**权重与 TI2V 推理已开放；默认 121 帧、640×480、16 FPS，但测试硬件为单张 H200，不是实时闭环指标。
- **重要性 / 阅读：**高 / 必读。

### 5. VideoGen-Agent：视频生成变成工具调用与验证循环

- **类型 / 日期 / 主线：**论文、agent、benchmark；2026-09-21；Coding × 内容生成。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24997)、[VABench](https://huggingface.co/datasets/andyli123princeton/VABench)、[项目仓库](https://github.com/AndyCA111/VideoGen_Agent)。
- **相对上期：**首次收录。
- **核心贡献：**agent 多轮编排知识增强、参考生成、视频模型和验证工具；VABench 含 600 个身份、物理、构图和多镜头任务。作者报告基础生成器由 56.5 升至 75.6，更换更强工具后无需重训 agent 即达 86.1。
- **Coding 接口：**核心抽象是 `plan → tool call → inspect → retry/verify`，适合接入工作流引擎和资产管理系统。
- **场景：**多角色短片、产品演示、自动分镜、知识型教育视频。
- **成熟度 / 证据：**论文实测；当前 GitHub 主要是项目页和样例，未见完整 agent 训练及执行代码，必须降级为研究原型。
- **重要性 / 阅读：**高 / 必读。

### 6. DexTacWAM：触觉成为需要预测的未来世界状态

- **类型 / 日期 / 主线：**论文、机器人实验；2026-09-21；世界模型、工业机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24976)、[项目页](https://dextacwam.github.io/)。
- **相对上期：**首次收录；区别于昨日 ME-Dex 的异构统一，重点是触觉世界预测的因果价值。
- **核心贡献：**逐指编码触觉，再以手指与姿态感知 compressor 注入视频 diffusion world model。六项 22-DoF 双手任务平均得分 70.6，对照最佳基线 38.0；去掉触觉未来预测后四任务均值由 74.7 降至 26.6。
- **Coding 接口：**`视觉 + 指尖传感 + proprioception → 未来视觉/触觉 + 动作`，可封装为 ROS 策略节点。
- **场景：**精密装配、插拔、易碎件和遮挡接触操作。
- **成熟度 / 证据：**作者实验；代码未确认，不是可部署 SDK。
- **重要性 / 阅读：**高 / 必读。

### 7. 世界模型表示蒸馏：部署时不再运行世界模型

- **类型 / 日期 / 主线：**论文、机器人策略；v2 更新于 2026-09-22；世界模型部署。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24682)。
- **相对上期：**首次收录。
- **核心贡献：**离线缓存冻结世界模型的中间特征，用一个 alignment loss 训练 0.8B VLA；部署时丢弃 teacher 和 projector。作者报告 RTX 5090 上 32 ms、1.86GB，LIBERO 97.9%，并在单臂和双臂实机上验证。
- **Coding 接口：**训练管线增加一次 feature-cache job；在线服务仍保持普通 `observation → action` API。
- **场景：**边缘机器人、低延迟机械臂、世界模型知识下沉。
- **成熟度 / 证据：**研究原型、作者实测；尚无代码。
- **重要性 / 阅读：**高 / 必读。

### 8. SVEET：双向模型训练控制，因果模型负责流式编辑

- **类型 / 日期 / 主线：**论文、GitHub、LoRA；2026-09-21；实时视频基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24788)、[代码](https://github.com/YujiaHu1109/SVEET)。
- **相对上期：**首次收录。
- **核心贡献：**将源视频独立编码后注入双向 backbone，再通过正交化训练方向把控制能力迁移到因果生成器。作者报告单 H100 15 FPS。
- **Coding 接口：**提供 Wan2.1-VACE、Causal Forcing、LoRA 合并、单段推理和自定义训练脚本。
- **场景：**直播风格化、虚拟制作监看、实时背景与角色外观转换。
- **成熟度 / 证据：**Apache-2.0 代码及 LoRA 已开放；第三方权重许可证需分别审计。
- **重要性 / 阅读：**中高 / 可读。

### 9. SAMIRA：语义分区的可重打光 3D Gaussian 头像

- **类型 / 日期 / 主线：**论文；2026-09-21；数字人、3D/4D。
- **官方链接：**[论文](https://arxiv.org/abs/2609.24158)。
- **相对上期：**首次收录。
- **核心贡献：**在 UV 空间按面部语义区域路由运动残差，并分别学习漫反射和高光响应，再以 deferred PBR shading 重打光。
- **Coding 接口：**理论上可暴露表情参数、环境光和材质区域控制，但当前没有公开运行时。
- **场景：**虚拟制作、数字替身、灯光预演、品牌虚拟人。
- **成熟度 / 证据：**论文原型；代码、FPS、显存均未披露。
- **重要性 / 阅读：**中高 / 可读。

### 10. MoSAT：空间声音与文本共同驱动全身动作

- **类型 / 日期 / 主线：**论文、dataset；2026-09-20；数字人、多模态动作。
- **官方链接：**[论文](https://arxiv.org/abs/2609.23797)。
- **相对上期：**首次收录。
- **核心贡献：**STAM 数据将动作、方向性空间音频和文本意图配对；MoSAT 通过分层 cross-attention 生成对声源方向和行为描述都敏感的全身动作。
- **Coding 接口：**可将空间音频方位、剧情文本和角色骨骼作为事件驱动接口。
- **场景：**XR 角色避让、声源反应 NPC、互动舞台和安全训练。
- **成熟度 / 证据：**论文原型；未确认代码与实时指标。
- **重要性 / 阅读：**中 / 可读。

### 11. GameHorizon：开始评测游戏 agent 的多时间尺度能力

- **类型 / 日期 / 主线：**dataset、benchmark、GitHub；2026-09-21，项目页 9 月 22 日上线；世界模型评测、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.25001)、[仓库](https://github.com/TencentARC/GameHorizon)。
- **相对上期：**首次收录。
- **核心贡献：**5,000 小时、21 款游戏、100 位玩家，将动作、视频和短期操作—中期目标—长期策略对齐；包含 5,000 个离线问题及 20 个在线任务、62 个子任务。
- **Coding 接口：**annotation pipeline 和 stepwise online evaluator 可用于定位 agent 在感知、分解还是执行阶段失败。
- **成熟度 / 证据：**仓库目前主要是说明和榜单；代码、数据计划自 2026-10-25 起逐步开放。
- **重要性 / 阅读：**中高 / 仅跟踪。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**AVTR-1 的增量在于双路音频和主动倾听，而非单纯口型；MoSAT 则把环境声源方位引入全身反应。
- **3D/4D 表示：**SAMIRA 表明 Gaussian 头像开始区分皮肤、嘴唇等区域的运动与材质响应，但没有实时性能证据。
- **实时交互：**AVTR-1 已给出 renderer TTFF、chunk 时长、语音开始/停止延迟模型和 WebRTC 部署路径，是本周期最完整的数字人运行时证据。
- **音视频与情感：**双人对话中的倾听动作得到 R-DGG 指标支持；不过情绪、视线、手势、口型和语言覆盖仍未进入统一评测。
- **工程部署：**AVTR-1 可在部分消费 GPU 达到 25 FPS，但许可证、InsightFace 依赖、多路并发、审核与容灾仍阻碍商业化。Unreal Engine 5.8.3 本周期仅修复了 MetaHuman RigLogic ML mask blending，属于维护性更新，不构成能力跃迁。[官方 hotfix](https://forums.unrealengine.com/t/5-8-3-hotfix-released/2833315)
- **安全与身份治理：**AVTR-1 明确禁止未授权肖像和冒充，但这只是使用条款，不是水印、授权验证或深伪检测能力。本周期暂无高质量新增治理技术。

## 5. 世界模型进展

- **架构与训练：**WorldCrafter 用视角查询记忆，GAE 改造 latent，CausalWM 显式生成物理中间量；三者分别解决“记什么”“状态如何编码”“怎样解释变化”。
- **可控/可玩生成：**WorldCrafter 的相机矩阵和动作字符串已经是程序接口，但目前主要控制观察视角，尚不能稳定控制任意对象和规则。
- **长时与物理一致性：**分钟级视觉回访一致性正在改善；物体守恒、碰撞和规则正确性仍不能由 VBench 或外观 demo 证明。
- **空间表示：**几何原生 latent、point map 和 camera-queryable memory 正取代“纯 RGB 历史帧堆叠”。
- **机器人/游戏/仿真：**DexTacWAM 扩展到多指接触；GameHorizon 为长短期游戏规划提供分层评测；世界模型表示蒸馏则提供低延迟部署路线。
- **实时部署：**“生成未来”与“消费世界知识”正在分离。视觉体验需要 WorldCrafter、SVEET 一类流式生成；硬实时控制更适合蒸馏后的轻量策略。

## 6. Coding × 新型互动作品

### 链路一：全双工数字员工

**麦克风/WebRTC → voice agent → 双路音频事件 → AVTR-1 streamer/TensorRT → 25 FPS 头像**

代码负责 VAD、打断、业务工具、权限和共享时钟；模型负责口型、微表情与倾听动作；浏览器/RTC 负责传输。已有可运行 demo，但商业许可证和多并发 SLA 未解决。

### 链路二：可持续探索的生成世界

**键盘/手柄 → 动作字符串或相机矩阵 → WorldCrafter memory readout → 流式视频 → 历史观察回写**

代码维护玩家位置、碰撞、任务和权威对象状态；模型维护外观和回访一致性；传统引擎决定不可违反的规则。代码与权重已开放，互动 demo 仍在调试。

### 链路三：带物理解释的机器人候选预测

**当前图像 + 文本任务 → CausalWM 光流 → point map → 未来 RGB → verifier → 控制器**

模型提供运动和几何中间状态；代码检查碰撞、动作范围和不确定性；控制器执行。可形成可审计的机器人预测界面，当前只开放文本条件视频推理，闭环控制属于**本报告推断**。

### 链路四：视频创作 agent

**创作需求 → VideoGen-Agent 规划 → 知识/参考/生成工具 → 自动验证 → 重试或合成**

agent 决定何时检索、生成、核验和更换工具；生成模型负责镜头；传统编辑器负责时间线、字幕和最终资产。论文证明工具编排有效，但完整代码尚未开放，因此不能视作现成制作平台。

### 链路五：流式可变外观直播

**摄像头视频 → SVEET causal pipeline → 逐段风格化视频 → WebRTC/OBS**

代码负责帧队列、背压、内容审核和降级；控制 LoRA 负责风格；因果模型负责连续输出。单 H100 15 FPS 已有作者证据，端到端直播延迟和音画同步仍未验证。

### 链路六：低延迟工业机器人

**离线轨迹 → 世界模型 feature cache → VLA 蒸馏训练 → 32 ms 策略 → 传统安全控制器**

世界模型只在训练期提供物理表征；部署代码保持轻量 action API；PLC/控制器负责急停和关节约束。这比在线生成视频更符合工业周期，但目前仍是研究实机验证。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | WorldCrafter + 权威状态机 + GameHorizon | 相机/动作持续输入、回访场景 | 开源研究栈 | GPU/显存未披露 | 对象规则、多玩家同步、审核 | [代码](https://github.com/TencentARC/WorldCrafter) |
| 影视/虚拟制作 | SAMIRA + SVEET + 灯光/时间线 | 实时重打光、流式风格化 | 论文原型至可运行 demo | SVEET 单 H100 15 FPS | 色彩稳定、商业许可、长镜头漂移 | [SVEET](https://github.com/YujiaHu1109/SVEET) |
| 直播与电商 | AVTR-1 + voice agent + WebRTC | 可打断对话、主动倾听 | 可运行技术验证 | 4060 Ti 约166 ms/5帧 | 并发、许可证、审核 | [AVTR-1](https://github.com/avaturn-live/avtr-1) |
| 品牌互动 | WorldCrafter + Web/Unity 事件层 | 用户探索、相机与剧情触发 | 技术验证 | 未披露 | 品牌资产一致性、内容安全 | [项目页](https://drexubery.github.io/WorldCrafter/) |
| 教育培训 | AVTR-1 + MoSAT + 业务知识库 | 语音问答、环境声反应 | 组合原型 | 未披露 | 事实可靠性、语言覆盖 | [MoSAT](https://arxiv.org/abs/2609.23797) |
| 企业数字员工 | AVTR-1 + LLM/function calling | 全双工语音和工具执行 | 研究级可运行栈 | 完整 p95 未披露 | 数据合规、身份授权、容灾 | [论文](https://arxiv.org/abs/2609.22913) |
| 陪伴与社交 | AVTR-1 双路音频 | 说话、倾听、打断 | 可运行 demo | 25 FPS，RTC 成本未披露 | 情绪安全、长期记忆 | [权重](https://huggingface.co/avaturn-live/avtr-1) |
| 空间计算 | GAE + WorldCrafter + XR pose | 视角移动和空间回访 | 开源研究原型 | 未披露 | 几何尺度、动态物体、设备算力 | [GAE](https://github.com/TencentARC/GAE-GeometricAutoEncoder) |
| 机器人/仿真 | CausalWM、DexTacWAM、VLA 蒸馏 | 视觉/触觉预测、动作执行 | 实机研究验证 | 蒸馏策略 32 ms；CausalWM 需 H200 | 安全认证、真实接触误差 | [蒸馏论文](https://arxiv.org/abs/2609.24682) |

## 8. 可复现资源与开发者入口

| 资源 | 开放与许可 | 硬件/成本 | 最小验证路径 | 建议 |
|---|---|---|---|---|
| [AVTR-1](https://github.com/avaturn-live/avtr-1) | 权重可取；renderer/streamer 非商业，模型为社区许可证 | Ampere+、CUDA 12、TensorRT 10 | 跑 offline 双路音频，再启互动 WebRTC demo，测 barge-in | **优先复现** |
| [WorldCrafter](https://github.com/TencentARC/WorldCrafter) | Base/Fast 权重和代码已开放；具体许可证需读 `LICENSE.txt` | 单 NVIDIA GPU，显存未披露 | 用动作字符串做前进—转向—回访，比较场景一致性 | **优先复现** |
| [GAE](https://github.com/TencentARC/GAE-GeometricAutoEncoder) | 完整代码、配置和测试；自定义许可需核验 | 未披露 | 编码同一场景多视图，检查 RGB/深度/点图共同重建 | 值得复现 |
| [CausalWM](https://github.com/AetherLabsAI/CausalWM) | 代码与权重；LTX-2 Community License | 官方测试为单 H200 | 跑一个机械臂案例并导出 flow、point map、provenance | 适合实验室 |
| [SVEET](https://github.com/YujiaHu1109/SVEET) | 主仓 Apache-2.0；依赖权重另行审计 | 论文速度使用 H100 | 先用发布 LoRA 完成 21 帧风格编辑，再测长流漂移 | 值得复现 |
| [VABench](https://huggingface.co/datasets/andyli123princeton/VABench) | prompt 为 CC-BY-4.0；参考图片保留上游许可 | 生成成本依模型 | 先跑身份保持和物理一致性各 10 条 | 适合作为 CI 子集 |
| [GameHorizon](https://github.com/TencentARC/GameHorizon) | 仓库 Apache-2.0；正式资源计划 10 月 25 日起发布 | 未披露 | 当前只能审阅任务定义和榜单 | 仅跟踪 |
| SAMIRA / MoSAT / DexTacWAM / VLA 蒸馏 | 论文或项目页，未确认代码 | 未披露 | 等待代码和权重 | 暂不安排工程复现 |

## 9. 系统架构与技术趋势判断

1. **明显升温：状态接口。** 相机矩阵、光流、点图、深度、触觉、双路音频和动作字符串正在替代不可解析的纯视频输出。
2. **主流架构趋向分层：**`权威状态/事件 → 生成或预测模型 → verifier → 引擎/控制器 → 观测回写`。
3. **记忆由“更多历史帧”转向“可查询压缩状态”。** WorldCrafter 的目标视角读取和 GAE 的几何 latent 都强调固定容量、结构可读。
4. **教师—学生分工扩大：**高成本双向或世界模型用于训练，因果生成器、LoRA 或轻量 VLA 用于运行时。
5. **数字人真正的工程门槛已转向媒体系统。** 时钟、chunk、背压、打断、RTC、TURN、并发和降级与模型质量同等重要。
6. **传统引擎仍是权威层。** 世界模型适合外观、动作先验和候选未来，不能替代碰撞、库存、胜负、支付或设备安全状态。
7. **未解决问题：**长时身份与物体绑定、多人同步、p95 延迟、每并发成本、物理校准、肖像授权、训练数据来源和商业许可证。
8. **需降级看待：**VideoGen-Agent 的仓库尚非完整代码；GameHorizon 数据尚未开放；SAMIRA 和 MoSAT 没有运行时；CausalWM 的“16 FPS”是输出视频规格，不是实时生成速度。

## 10. 论文精读候选

1. **[AVTR-1](https://arxiv.org/abs/2609.22913)**  
   原因：少见地同时讨论模型、renderer、事件总线、流时钟、TTFF、回复和停止延迟。重点读第 4—5 节。复现风险主要是多许可证和 TensorRT 环境。

2. **[WorldCrafter](https://arxiv.org/abs/2609.24984)**  
   原因：目标视角决定如何从记忆中读取历史，适合研究生成世界的状态 API。重点读 memory encoder、pose-conditioned readout、回访评测和蒸馏。风险是 demo 仍在调试且硬件口径缺失。

3. **[GAE](https://arxiv.org/abs/2609.24981)**  
   原因：直接质疑“外观 latent 加几何辅助头”的惯例。重点读 codec、冻结几何 decoder、latent loss 和受控比较。风险是迁移到动态人物和通用视频尚未证明。

4. **[CausalWM](https://arxiv.org/abs/2609.23184)**  
   原因：将物理中间变量变成可输出的 reasoning trace。重点读 stage-causal attention、三阶段训练和多视角 benchmark。风险是中间状态可能视觉合理但数值不可靠。

5. **[Think Like a World Model, Act Like a VLA](https://arxiv.org/abs/2609.24682)**  
   原因：给出世界模型进入工业控制的低成本路径。重点读 feature caching、alignment loss、消融和实机部分。风险是增益幅度依任务而变，且尚无代码。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. AVTR-1 是否开放完整 production backend，以及多会话 GPU 并发和 p95 stop latency。
2. WorldCrafter 互动 demo 修复后，真实输入到显示延迟、显存和一分钟漂移情况。
3. GAE latent 能否迁移到动态人物、可编辑对象和在线 SLAM。
4. CausalWM 的 action-conditioned checkpoint、低显存版本及数值几何误差。
5. VideoGen-Agent 是否真正开放 agent policy、tool schema、训练轨迹和 verifier。
6. GameHorizon 10 月开放时的数据版权、游戏许可和在线环境可复现性。
7. DexTacWAM、MoSAT、SAMIRA 的代码与权重发布。
8. Unreal、Unity 或 WebGPU 是否出现对上述模型的正式插件，而非研究者自制 demo。

### 本周可做的小实验

1. **AVTR-1 打断延迟测试**  
   目标：验证数字人是否真正适合全双工对话。组件：AVTR-1、任一流式 voice agent、浏览器 WebRTC。难点：时钟同步和 TURN。成功判据：连续 30 次插话中，95% 在 500 ms 内停止旧回复且无明显音画错位。

2. **WorldCrafter 回访一致性压力测试**  
   目标：验证记忆而非短期上下文。组件：Fast 权重、动作字符串、图像相似度和几何特征。难点：区分合理视角变化与身份漂移。成功判据：前进—转向—回到起点至少运行一分钟，关键对象数量和相对位置保持稳定。

3. **CausalWM 物理中间量审计**  
   目标：检查光流和 point map 是否真的能帮助动作拒绝。组件：公开 checkpoint、三类机械臂初始帧、简单碰撞规则。难点：point map 为相对尺度。成功判据：verifier 使用中间量后，对明显穿透或错误运动的拒绝率显著高于只看 RGB。

4. **视频创作工具路由原型**  
   目标：复现 VideoGen-Agent 的最小价值，而非训练完整 agent。组件：LLM function calling、一个参考图生成器、一个视频模型、VABench 20 条子集。难点：稳定的视觉验证。成功判据：允许“生成—检查—重试”后，身份/动作 checklist 通过率较单次生成提高至少 15 个百分点。
