# 数字人、世界模型与互动作品研究日报：2026-09-29

检索窗口：2026-09-23 至 2026-09-29。  
信息截点：2026-09-29 09:16（Asia/Shanghai）。已与 9 月 22—24 日及 9 月 28 日日报去重；“相对上期”指相对 2026-09-28 日报。部分论文于 9 月 23—25 日提交、9 月 28 日进入公开检索列表，以下按 arXiv 实际提交日期标注，不把检索日期当作发布日期。

## 1. 本日摘要

本日高质量新增明显集中在机器人世界动作模型：InternW0-Δ、Rolling-WAM、DeltaWAM 分别从大规模预训练、跨控制周期滚动去噪和稀疏视觉增量三个层面降低“预测未来再生成动作”的成本。

世界模型的产品形态也在分化：有的模型不再于推理时生成完整未来视频，而是把未来变化压缩为 Causal Imprint、视觉 delta 或可执行 latent path；这说明工业系统需要的是决策相关状态，不一定需要可观看的 RGB 未来。

实时性进展不再只依赖减少扩散步数。Rolling-WAM 把一次完整去噪摊到多个 replanning cycle，DeltaWAM 缓存稳定场景、只更新变化部分；二者都更接近持续运行时的增量计算范式。

物理一致性方面，OneWorld 指出单条 rollout 看似合理并不够：同一初始世界在不同动作下应共享摩擦、质量等潜在物理机制。这比传统视频质量指标更接近规划可靠性。

数字人方向没有新的通用实时 talking-head 系统。新增主要来自 Timo 和 Motion Style Slider：前者提高文本到全身动作的关节协调，后者提供连续、单调的表演风格强度控制，适合成为角色运行时的动作层，但尚未证明与实时语音、表情和数字人渲染形成完整链路。

Coding × 互动作品的关键机会是把这些连续控制量、delta 状态和候选未来变成 API：代码维护权威状态、动作约束、缓存和调度，模型提供动作、预测或渲染，传统引擎负责碰撞、同步与硬实时执行。

RECAST 进一步展示了这种分工在自动驾驶仿真中的价值：由代码控制车辆轨迹和交互，3D Gaussian 负责生成未被原始日志拍到的视角，再让规划器闭环运行。

安全侧的新增是 ManiVid：检测器从二分类推进到“是否篡改、改在哪里、异常原因是什么”，但代码、模型和数据仍标为未来发布，暂不能视作可部署治理组件。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | 全身动作生成开始提供关节运动学约束和连续风格旋钮 | 动作组件增强，未形成新的实时数字人产品 | 新增 Timo、Motion Style Slider；通用 talking head 暂无高质量新增 |
| 世界模型 | 很高 | 未来预测从完整 RGB 转向 Causal Imprint、视觉 delta、latent path 和滚动去噪 | 从研究结构进一步逼近实时机器人闭环 | 新增 InternW0-Δ、Rolling-WAM、DeltaWAM、OneWorld、DyMD |
| Coding × 互动作品 | 高 | 状态缓存、候选动作评分、连续控制量和 planner-in-the-loop 成为明确软件接口 | “生成一次内容”继续转向“增量维护并持续运行” | 新增多周期调度、delta memory、多 agent 联合动作规划 |
| 实时运行时 | 高 | 计算开始跨帧、跨控制周期复用，而非每次从零推理 | 作者报告 4.5× replanning 加速、36.57% 单步延迟下降 | 相比上期的通用 serving，本期更贴近机器人控制循环 |
| 安全与治理 | 中 | 篡改检测扩展到像素级定位和自然语言解释 | 仍为研究 benchmark | 新增 ManiVid-38K/ManiVidLens，资源尚未开放 |

## 3. 最值得关注的 11 个进展

### 1. InternW0-Δ：把 20K+ 小时开放数据与“未来变化印记”接入动作生成

- **类型 / 日期 / 主线：**模型、论文、数据计划；2026-09-25；世界模型、机器人。
- **官方链接：**[论文](https://arxiv.org/abs/2609.31394)、[项目页](https://internrobotics.github.io/InternW0-Delta/)。
- **相对上期：**新增；上期已报道 InternW0，本项是独立的 Δ 版本，新增 20K+ 小时异构数据、4D 先验蒸馏和 Causal Imprint。
- **核心贡献：**视频 expert 与动作 expert 通过定向注意力协作，冻结 VLM 提供场景语义，4D foundation model 只在训练时蒸馏几何与运动先验；Causal Imprint 用未来监督学习决策相关变化，但推理时无需生成未来视频。
- **Coding 接口：**`图像/本体状态/任务 → predictive representation → action chunk`，可作为机器人策略服务；Sparse Memory Context 只保存 anchor、近期帧和当前帧。
- **场景：**通用机械臂、灵巧手、人类视频到机器人迁移。
- **成熟度 / 证据：**实机研究原型；作者报告 LIBERO-Plus 92.8%、RoboTwin Clean2Random 71.9%。代码、权重及处理后数据仍为 forthcoming。
- **重要性 / 阅读：**高 / 必读。

### 2. Rolling-WAM：把去噪过程变成跨控制周期运行的流水线

- **类型 / 日期 / 主线：**论文、项目页、GitHub 占位仓库；2026-09-24；世界模型、实时运行时。
- **官方链接：**[论文](https://arxiv.org/abs/2609.30247)、[项目页](https://rolling-wam.github.io/)、[GitHub](https://github.com/zyinghua/Rolling-WAM)。
- **相对上期：**新增。
- **核心贡献：**维护不同噪声级别的视频—动作滑动窗口；当前 action chunk 完成去噪并执行，远期 chunk 只部分细化，取得新观测后继续而非重算。
- **Coding 接口：**运行时需维护 session window、chunk noise level、执行游标和观测回写，天然适配异步控制循环。
- **场景：**人形机器人实时操作、连续 NPC 动作规划、低延迟数字孪生。
- **成熟度 / 证据：**Unitree G1 实机研究原型；项目页报告 4.5× 稳态 replanning 加速、215 ms 延迟、实机平均成功率 85%。GitHub 为 Apache-2.0，但代码和 checkpoint 尚未上传。
- **重要性 / 阅读：**高 / 必读。

### 3. DeltaWAM：只预测变化，并把视觉上下文做成流式缓存

- **类型 / 日期 / 主线：**论文、GitHub、权重；2026-09-23；世界模型、推理加速。
- **官方链接：**[论文](https://arxiv.org/abs/2609.28811)、[代码](https://github.com/AIGeeksGroup/DeltaWAM)、[权重](https://huggingface.co/AIGeeksGroup/DeltaWAM)。
- **相对上期：**新增漏项；上期未覆盖其已开放的训练、RoboTwin 评测代码和 checkpoint。
- **核心贡献：**以 dense anchor 保存稳定场景，以 sparse delta 表示变化；Streaming Delta Memory 用观测增量更新缓存，避免视频 expert 每步重读完整画面。
- **Coding 接口：**公开 Hydra 配置、单/多 GPU 训练脚本、RoboTwin manager、日志及评测输出；适合接入机器人 CI。
- **关键结果：**作者报告 RoboTwin clean/randomized 成功率 85.4%/83.9%，单步延迟从 121.02 ms 降至 76.76 ms。
- **成熟度 / 证据：**已开源、MIT；论文实测。依赖 CUDA 12.8、DINOv3 授权组件和 RoboTwin 资产。
- **重要性 / 阅读：**高 / 必读。

### 4. DyMD：少步蒸馏不能只保画质，还必须保交互动力学

- **类型 / 日期 / 主线：**论文、模型蒸馏；2026-09-25；世界模型、支撑基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.31349)。
- **相对上期：**新增；相较此前 DIDO、ViRDM，本项专门处理蒸馏后的机器人—物体运动衰减。
- **核心贡献：**根据 rollout 的时间运动质量自适应选择 re-noise timestep，并提高 critic 对强运动、难拟合样本的权重；将 14B 教师蒸馏为四步 1.3B 学生。
- **Coding 接口：**可替换世界模型 serving 的 sampler/backbone，不增加推理期辅助模块。
- **场景：**机器人候选未来、交互物理预演、低成本 action planning。
- **成熟度 / 证据：**研究原型；作者报告 WorldArena 两任务平均成功率从基础 DMD 的 16% 提升到 34%。未核验到代码或权重。
- **重要性 / 阅读：**高 / 必读。

### 5. OneWorld：不同动作分支必须遵守同一个物理世界

- **类型 / 日期 / 主线：**论文、评测协议；2026-09-25；世界模型、物理一致性。
- **官方链接：**[论文](https://arxiv.org/abs/2609.30946)。
- **相对上期：**新增；WROP 评测单条 rollout 中的对象永久性，本项评测多个反事实分支能否共享同一物理机制。
- **核心贡献：**从多个 action-outcome branch 推断潜在物理机制分布，再聚合为 shared-world evidence，用于约束 flow training 和采样。
- **Coding 接口：**测试框架可固定初始状态、批量注入不同动作，并检查摩擦、质量等隐变量是否跨分支一致。
- **场景：**机器人 MPC、游戏物理回归、自动驾驶反事实仿真。
- **成熟度 / 证据：**受控环境研究原型；仅有论文实测，未证明复杂开放场景泛化。
- **重要性 / 阅读：**高 / 必读。

### 6. Action Forcing：从无标注视频恢复可组合的相机动作轴

- **类型 / 日期 / 主线：**论文、训练方法、评测；2026-09-24；世界模型、数据基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.30595)。
- **相对上期：**新增。
- **核心贡献：**从光流/像素轨迹中用 PCA 恢复 throttle—yaw 控制基，借助冻结的 decoder—tracker—PCA teacher 与 latent critic 训练视频 DiT；无需同步动作标签。
- **Coding 接口：**控制信号为带符号、可缩放和可组合的低维向量，容易映射键盘、摇杆和相机 API。
- **场景：**可导航视频世界、无标注驾驶视频训练、互动电影镜头控制。
- **成熟度 / 证据：**研究原型；能处理训练数据不足 1% 的倒退动作，但仅能恢复数据中实际存在的运动轴。
- **重要性 / 阅读：**高 / 必读。

### 7. MA-WAM：用世界模型在执行前比较多 agent 联合动作

- **类型 / 日期 / 主线：**论文、规划框架；2026-09-25；世界模型、Multi-agent。
- **官方链接：**[论文](https://arxiv.org/abs/2609.31281)、[项目页](https://ma-wam.github.io/)。
- **相对上期：**新增；从单机器人 WAM 扩展到同时建模多个 agent 的联合动作依赖。
- **核心贡献：**冻结既有 multi-agent flow policy，生成候选 joint action，再由 MA-WAM 预测团队后果并评分。
- **Coding 接口：**`policy.sample(k) → world_model.rollout(joint_action) → scorer → execute`；可作为测试时 planner middleware。
- **场景：**协作机器人、多人 NPC 战术、仓储车队和多角色互动表演。
- **成熟度 / 证据：**仿真研究原型；作者报告 30 个设置中相对直接执行平均提升 22%，额外评分开销 12.1 ms。尚无真实多机器人证据。
- **重要性 / 阅读：**高 / 必读。

### 8. RECAST：由单帧车辆生成视角完整 actor，支持闭环驾驶仿真

- **类型 / 日期 / 主线：**论文、dataset 方法、3DGS 仿真；2026-09-25；世界模型、工业仿真。
- **官方链接：**[论文](https://arxiv.org/abs/2609.31374)、[项目页](https://zijunkr.github.io/RECAST/)。
- **相对上期：**新增；相较 HelloWorld 的生成式多传感器视频，本项保持显式 3D actor，并支持 planner-in-the-loop。
- **核心贡献：**从日志中的单个分割车辆观察构建 view-complete Gaussian actor，再注册到重建道路；RECAR 数据含约 2 万辆真实车辆、60 万张透明背景 RGBA 图。
- **Coding 接口：**仿真代码控制 ego/actor pose，Gaussian renderer 输出新视角，规划器消费画面并反馈动作。
- **关键结果：**作者报告超出日志轨迹时，无碰撞率从 Street Gaussians 的 22.2% 升至 63.0%。
- **成熟度 / 证据：**闭环研究 demo；项目页仍带模板残留内容，代码入口真实性和完整性需继续核验。
- **重要性 / 阅读：**中高 / 必读。

### 9. Timo：把关节运动学约束加入文本到全身动作生成

- **类型 / 日期 / 主线：**论文、demo、benchmark；2026-09-25；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2609.30761)、[项目页](https://kyfafyd.wang/projects/timo/)。
- **相对上期：**新增。
- **核心贡献：**共享注意力让文本和 motion token 双向更新，旋转运动学和几何损失约束关节协同；支持把 8 秒训练窗口拼接成更长动作。
- **Coding 接口：**文本序列输入、显式人体参数输出，可接 SMPL/SMPL-X、游戏骨骼和动作状态机。
- **场景：**数字人表演、生成式 NPC、虚拟制作预演、机器人动作参考。
- **成熟度 / 证据：**可运行 demo、论文实测；作者报告六轴平均分相对 Kimodo 提升 40.8%，但“可行性”只是几何穿插代理，不等同于动力学稳定。未见代码。
- **重要性 / 阅读：**中高 / 必读。

### 10. Motion Style Slider：把表演风格变成连续可编程参数

- **类型 / 日期 / 主线：**论文、动作控制方法；2026-09-25；数字人、Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2609.30795)。
- **相对上期：**新增。
- **核心贡献：**只需普通动作与目标风格两个端点，在动作风格 embedding 中构建方向；标量 intensity 控制风格强度，无需每个中间强度的真值动作。
- **Coding 接口：**可暴露为 `style_id + intensity`，由时间线、状态机、观众事件或导演工具实时调节。
- **场景：**NPC 情绪表演、虚拟主播手势强度、互动戏剧、游戏过场。
- **成熟度 / 证据：**研究原型；论文验证插值、外推和动作保持，尚无实时引擎插件或数字人端到端 demo。
- **重要性 / 阅读：**中高 / 可读。

### 11. ManiVid：视频篡改检测从标签推进到定位与解释

- **类型 / 日期 / 主线：**论文、dataset、benchmark、安全模型；2026-09-25；身份安全。
- **官方链接：**[论文](https://arxiv.org/abs/2609.30934)。
- **相对上期：**新增；上期身份治理没有同等级 benchmark。
- **核心贡献：**ManiVid-38K 含约 1.9 万对人工核验的真—假视频，提供真实性标签、像素 mask 和异常解释；ManiVidLens 联合低层取证特征、MLLM 与 SAM2。
- **Coding 接口：**适合形成 `媒体上传 → authenticity score → manipulation mask → explanation → 人工复核` 审核 API。
- **成熟度 / 证据：**研究原型；作者报告检测 Acc 0.914、F1 0.913，但代码、模型和数据均写明“将发布”。
- **重要性 / 阅读：**中高 / 必读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期新增主要是全身动作而非脸部生成。Timo 改善文本语义与关节协同；Motion Style Slider 提供连续的风格强度控制。
- **3D/4D 表示：**没有超过上期 PHOSA、ARS-Avatar 的新人物表示。Timo 输出的显式人体参数可连接 mesh/Gaussian avatar，但这属于组件组合，不是论文已经证明的完整系统。
- **实时交互：**没有新增的首帧时间、音视频流式延迟、打断恢复或并发指标。两项动作工作都不能直接视作实时对话数字人。
- **音视频与情感：**Motion Style Slider 可表达风格强弱，但没有联合建模语音韵律、嘴形、视线和面部情绪。Timo 同样是文本—动作模型。
- **工程部署：**最可行的产品路径是离线或预取动作：LLM 选择动作意图，动作模型生成或检索 clip，Unity/Unreal 状态机进行混合与 IK，现有数字人 RTC 管线继续负责脸部与语音。
- **安全治理：**ManiVid 提供更可解释的篡改证据，但尚未开放；它也不解决肖像授权、训练数据同意、水印或身份撤回。

结论：数字人本日没有出现新的通用生成突破，真正的增量是把“动作表现”变成更精确、更容易由代码控制的中间层。

## 5. 世界模型进展

- **架构与训练：**InternW0-Δ 将视频、语义、4D 几何与动作专家合并；Action Forcing 从普通视频恢复控制轴；DyMD 解决少步蒸馏的运动坍缩。
- **可控 / 可玩：**Action Forcing 的 throttle—yaw 基、MA-WAM 的 joint action、RECAST 的 ego/actor pose 都是明确的结构化控制接口。
- **长时与物理一致性：**Rolling-WAM 和 DeltaWAM 复用历史计算；OneWorld要求多个反事实分支共享物理机制。它们分别处理计算连续性、视觉状态连续性与物理解释连续性。
- **空间表示：**RECAST 使用显式 3D Gaussian actor；InternW0-Δ 通过训练期 4D 蒸馏吸收空间与运动先验，但部署时主要消费压缩后的表示。
- **机器人 / 游戏 / 仿真：**机器人增量最强；游戏暂无超过上期 GameDirector、CoDeR 的新系统。RECAST 为驾驶闭环仿真提供了新的显式 actor 路线。
- **实时部署：**主流方向正在从“更少扩散步”扩展为“跨周期流水线、增量缓存、只计算决策相关变化”。
- **评测：**OneWorld 的多干预一致性、DyMD 的 action-planning success 和 MA-WAM 的闭环收益，都比 FVD/VBench 更接近实际价值。

## 6. Coding × 新型互动作品

### 链路一：滚动生成的机器人或实体角色

**任务脚本 → Rolling-WAM 滑动窗口 → 当前动作执行 → 新观测回写 → 远期预测继续去噪**

代码维护 chunk、噪声级别、执行 deadline 和急停；模型生成视觉未来与动作；传统控制器负责关节约束。已有 Unitree G1 论文证据。映射到游戏角色或数字人属于**本报告推断**。

### 链路二：只更新变化部分的持续世界

**anchor 场景 → 用户/机器人动作 → DeltaWAM visual delta → Streaming Delta Memory → 下一步动作**

稳定背景不再反复编码，代码保存权威 anchor 和缓存版本，模型只预测变化。适用于机器人、数字孪生和低带宽远程操作；DeltaWAM 已提供代码、权重和评测脚本。

### 链路三：多角色联合行动导演

**多个 agent 提议动作 → MA-WAM 预测联合后果 → 团队评分器选择 → 引擎同步执行**

代码处理候选预算、冲突和同步；模型估计多角色相互影响；规则引擎保证禁止状态不被执行。已有离线多 agent 仿真结果，用于多人 NPC 或数字展演仍属**本报告推断**。

### 链路四：可调表演强度的数字角色

**剧情/观众事件 → LLM 选择动作与风格 → `style_intensity` → Motion Style Slider/Timo → 骨骼动画 → Unity/Unreal**

动作模型负责自然动作，代码负责情绪状态、强度曲线、clip blending 和 IK，渲染引擎负责角色资产与碰撞。论文证明动作生成和强度控制；实时数字人链路尚未落地。

### 链路五：反事实一致的互动物理

**权威初始状态 → 多个候选动作 → OneWorld 生成分支 → 共享物理机制检查 → planner 选动作**

可用于玩家改规则的生成式游戏、机器人试动作和互动电影特效预演。OneWorld 只在受控环境验证，开放世界应用属于**本报告推断**。

### 链路六：超越日志回放的驾驶数字孪生

**驾驶日志 → RECAST view-complete actor → 代码修改车辆轨迹 → 3DGS 新视角渲染 → planner 回传动作**

代码负责车辆状态和场景编排，Gaussian 负责补全未观测视角，规划器负责闭环决策。已有 planner-in-the-loop 证据，但项目代码尚需核验。

### 链路七：可解释的数字人内容审核

**生成/编辑视频 → ManiVidLens 检测 → 像素级 mask → 异常解释 → 人工复核或发布阻断**

代码负责准入阈值、审计记录和人工升级；模型负责取证线索。方法已有论文实验，资源未开放，不能视作可直接部署方案。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与技术栈 | 互动机制 | 当前成熟度 | 成本/延迟指标 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | MA-WAM、OneWorld、规则引擎 | 多 NPC 候选动作与物理分支 | 方法研究原型 | MA-WAM 额外 12.1 ms；其他未披露 | 未在游戏引擎验证、视觉与状态错位 | [MA-WAM](https://arxiv.org/abs/2609.31281) |
| 影视/虚拟制作 | Timo、Style Slider、DCC/骨骼系统 | 文本动作与连续风格调节 | 可展示 demo | 未披露 | 面部/语音未联动、资产重定向 | [Timo](https://kyfafyd.wang/projects/timo/) |
| 直播与电商 | Style Slider + 现有数字人 RTC | 观众事件调节动作强度 | 本报告推断 | 未披露 | 动作生成延迟、审核、肖像权 | [论文](https://arxiv.org/abs/2609.30795) |
| 品牌互动 | Action Forcing + 可导航视频世界 | 键盘/摇杆控制镜头 | 研究原型 | 未披露 | 控制轴受训练数据限制、长时漂移 | [论文](https://arxiv.org/abs/2609.30595) |
| 教育培训 | Timo + 课程脚本 + avatar renderer | 教学内容触发动作演示 | 组件原型 | 未披露 | 手势语义正确性、实时性 | [论文](https://arxiv.org/abs/2609.30761) |
| 企业数字员工 | Timo/Style Slider + WebRTC + LLM | 业务事件驱动非语言行为 | 本报告推断 | 未披露 | 全双工协同、动作安全、并发 | 同上 |
| 陪伴与社交 | 动作状态机、长期记忆、Style Slider | 情绪状态连续影响表演 | 本报告推断 | 未披露 | 情绪安全、长期一致性 | 同上 |
| 空间计算 | Action Forcing、3D runtime | 低维相机动作控制生成观察 | 研究原型 | 未披露 | 视角延迟、几何漂移 | [Action Forcing](https://arxiv.org/abs/2609.30595) |
| 机器人/仿真 | InternW0-Δ、Rolling-WAM、DeltaWAM | 观测—预测—动作闭环 | 实机研究原型 | 215 ms；76.76 ms；单位成本未披露 | 安全认证、OOD、硬件成本 | [Rolling-WAM](https://rolling-wam.github.io/) |
| 自动驾驶 | RECAST、3DGS、planner | 修改 ego/actor 轨迹后闭环评测 | 研究 demo | 未披露 | 传感器完整性、代码未核验 | [RECAST](https://arxiv.org/abs/2609.31374) |

## 8. 可复现资源与开发者入口

| 资源 | 开放与许可证 | 硬件/成本 | 最小验证路径 | 判断 |
|---|---|---|---|---|
| [DeltaWAM GitHub](https://github.com/AIGeeksGroup/DeltaWAM) | MIT；训练、RoboTwin 评测代码已开放 | Python 3.10、CUDA 12.8；训练示例支持 1/8 GPU | 下载五任务 checkpoint，在 RoboTwin 固定任务跑 3 个 seed，记录成功率、单步延迟与显存 | **本日最值得复现** |
| [DeltaWAM 权重](https://huggingface.co/AIGeeksGroup/DeltaWAM) | 已有文件，模型卡缺失；第三方依赖另受其许可证约束 | 需接受 DINOv3 gated license | 先验证 checkpoint、统计文件与配置一一匹配 | 可用，但许可证链需审计 |
| [Rolling-WAM GitHub](https://github.com/zyinghua/Rolling-WAM) | Apache-2.0；当前仅 README/素材 | 预计需高端 GPU，未披露 | 暂只能复核项目指标与运行时设计 | 仅跟踪，不能声称已开源实现 |
| [InternW0-Δ 项目](https://internrobotics.github.io/InternW0-Delta/) | 代码、权重、数据均 forthcoming | 未披露 | 等待开放后先跑 LIBERO-Plus，再核验 Causal Imprint 消融 | 高优先级跟踪 |
| [Timo demo](https://timo.kyfafyd.wang/) | demo 可访问；代码未见 | 未披露 | 固定动作语义，更换速度、方向和长序列 prompt，检查脚滑、穿插和关节抖动 | 值得做黑盒验证 |
| Motion Style Slider | 论文；未核验代码 | 未披露 | 暂只能复核强度单调性与越界外推设计 | 等代码后复现 |
| OneWorld / DyMD / MA-WAM | 论文或项目展示 | 未披露 | 可先实现简化多分支物理测试或候选动作评分协议 | 方法值得，端到端复现风险高 |
| ManiVid | 代码、模型、数据承诺未来发布 | 训练成本未披露 | 发布后先取 100 对局部篡改视频，评估检测、mask 和解释三项是否一致 | 安全团队应跟踪 |

## 9. 系统架构与技术趋势判断

1. **增量计算明显升温。** Rolling-WAM 跨周期保留半成品，DeltaWAM 保留视觉 anchor，InternW0-Δ 保留稀疏上下文；每轮从零生成正在退出实时系统设计。
2. **决策相关表示开始替代完整 RGB。** Causal Imprint、visual delta、latent path 都在问“动作需要知道什么”，而不是“怎样把未来每个像素都画出来”。
3. **实时世界模型正形成分层架构：**  
   `事件/传感器 → 权威状态与缓存 → 世界模型预测或候选生成 → verifier/scorer → 状态机/控制器 → 执行 → 增量观测回写`。
4. **多分支一致性成为新评测重点。** OneWorld 检查同一世界的反事实物理，MA-WAM 比较联合动作后果；单视频画质无法回答这类问题。
5. **数字人动作接口趋向参数化。** 文本定义内容，连续标量定义风格强度，骨骼参数承接渲染；这比直接让视频模型生成整个人更容易被代码、时间线和引擎控制。
6. **研究证据仍强于产品证据。** 除 DeltaWAM 外，多数项目没有完整代码、权重、p95 延迟或并发数据；实机成功率也主要来自作者评测。
7. **关键未解问题：**滚动缓存错误如何恢复、半成品预测如何被新观测安全覆盖、多 agent 状态怎样同步、物理机制如何扩展到开放世界、数字人动作怎样与语音和面部同步。
8. **需降级看待：**InternW0-Δ 的“开放数据”目前仍是发布承诺；Rolling-WAM 的 GitHub 不是可运行代码；RECAST 项目页存在明显模板残留；ManiVid 尚无可下载资源。

## 10. 论文精读候选

1. **[InternW0-Δ](https://arxiv.org/abs/2609.31394)**  
   值得读：代表 WAM 从“生成未来视频”转向“训练时用未来、部署时只取未来相关表示”。重点看 Mixture-of-Transformers、定向注意力、Causal Imprint、4D distillation 和异构数据对齐。风险是资源尚未开放。

2. **[Rolling-WAM](https://arxiv.org/abs/2609.30247)**  
   值得读：把扩散采样改造成持续控制调度问题。重点看 rolling noise schedule、新观测注入、窗口保留策略和延迟口径。风险是预测误差可能随保留窗口累积，代码尚未发布。

3. **[DeltaWAM](https://arxiv.org/abs/2609.28811)**  
   值得读：研究增量和工程可复现性最均衡。重点看三种 anchor/delta/action 架构、Streaming Delta Memory、随机视觉测试及实机设置。风险是 RoboTwin 子集和外部预训练组件较多。

4. **[OneWorld](https://arxiv.org/abs/2609.30946)**  
   值得读：提出比单 rollout plausibility 更严格的物理一致性目标。重点看 mechanism interpreter、shared evidence 聚合和 multi-intervention protocol。风险是受控域结论未必迁移到真实视频。

5. **[DyMD](https://arxiv.org/abs/2609.31349)**  
   值得读：解释为何少步蒸馏保住画质却丢失交互动作。重点看 temporal affinity re-noise、fake-score tracking 和 WorldArena 规划实验。风险是缺少公开代码与独立复现。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. InternW0-Δ 是否按承诺开放训练代码、权重、数据处理管线及可再分发的数据分片。
2. Rolling-WAM 是否发布真正可运行的代码和 checkpoint，以及新观测与旧预测冲突时的重置策略。
3. DeltaWAM 权重文件、模型卡、训练数据许可和完整十任务复现实验是否补齐。
4. OneWorld 的多干预物理一致性是否能迁移到复杂视频、机器人或游戏引擎场景。
5. DyMD 是否在更长 rollout 中仍保持接触、速度和物体状态，而非只改善短任务成功率。
6. Timo 与 Motion Style Slider 是否开放代码、实时推理速度、SMPL-X/FBX 导出或 Unity/Unreal 插件。
7. ManiVid-38K、benchmark 和模型是否真正发布，并验证对未见生成器和视频压缩的鲁棒性。
8. RECAST 是否清理项目页、公开代码与 RECAR 许可证，并报告闭环仿真速度。

### 本周可做的小实验

1. **DeltaWAM 最小复现**
   - 目标：验证 sparse delta 是否同时改善延迟和随机视觉鲁棒性。
   - 组件：公开 checkpoint、RoboTwin、单张高端 GPU、固定五任务。
   - 难点：DINOv3 授权、资产版本和配置匹配。
   - 成功判据：完成至少三任务、每任务三 seed；同时报告成功率、平均/p95 单步延迟、显存及缓存失效次数。

2. **滚动去噪调度器模拟**
   - 目标：在不训练 Rolling-WAM 的情况下验证跨周期流水线的时延收益和错误恢复。
   - 组件：简化 diffusion scheduler、固定 action chunks、模拟观测中断和突发变更。
   - 难点：旧的远期预测可能与新观测冲突。
   - 成功判据：deadline miss 比每周期全量去噪降低 50% 以上；场景突变后一至两个周期内完成安全重置。

3. **动作风格旋钮原型**
   - 目标：验证 `动作语义 + 风格类型 + 强度` 是否适合互动角色接口。
   - 组件：任一开放 text-to-motion 模型、SMPL/游戏骨骼、Unity 或 Three.js、简单状态机。
   - 难点：强度单调性、脚滑和动作切换。
   - 成功判据：三种动作、三档强度均获得多数盲评者认可；切换时无明显穿插或速度跳变。

4. **跨分支物理一致性测试**
   - 目标：将 OneWorld 的问题转化为现有世界模型 CI。
   - 组件：固定初始帧、三组相反动作、物体跟踪器、简单质量/摩擦代理指标。
   - 难点：从像素结果反推物理量不稳定。
   - 成功判据：至少 30 组场景，分别报告单分支合理率和跨分支机制一致率；禁止仅以 FVD 或视觉偏好作为结论。
