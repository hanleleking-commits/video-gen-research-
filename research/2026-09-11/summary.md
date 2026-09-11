# 数字人、世界模型与互动作品研究日报：2026-09-11

检索窗口：2026-09-05 至 2026-09-11。  
发布日期以 arXiv 提交历史、官方项目页、GitHub 和产品文档为准；已与 9 月 7—10 日日报去重，“相对上期”指相对 2026-09-10 日报。

## 1. 本日摘要

本日最强信号是世界模型开始明确放弃“让视频模型隐式记住一切”的路线，转而把权威状态、规则执行和视觉渲染拆开。Programmable World Model 让 coding agent 生成可执行世界程序，由轻量引擎维护离屏实体、生命值和事件，再将状态编译为视频模型的时空条件；这比单纯增加上下文或视觉记忆更接近可编程游戏运行时。

Show-Harness 在具身侧给出相似答案：VLM 不直接输出连续关节坐标，而是调用跨机器人共享的离散语义动作，再由确定性 interpreter 翻译为具体硬件动作。它同时开放运行时、插件、浏览器数据采集、训练管线、数据和 LoRA，是本日工程完整度最高的新增。

数字人形象生成本身仍无新的高质量 talking-head、3D/4D avatar 或长时身份保持模型。新增集中于两端：阿里云更新了可直接调用的 Wan2.2-S2V 数字人 API、价格和并发约束；UCF-Net 则补强生成面孔的检测，但其零样本跨生成器 AUC 很低，说明安全能力仍追不上生成器变化。

SynthGait-19K 把真实 MoCap、SMPL、可控相机和视频扩散组合成合成全身运动数据管线，显示“代码定义物理运动，生成模型负责外观域随机化”可用于训练工业视觉系统。

机器人世界模型方面，多模态并不自动带来控制可靠性：最新视触觉实验中，触觉预测误差下降并未直接转化为满足力约束的成功率，甚至简单反馈控制仍明显更强。

运行时评测正在成为独立工程层。No Free Checker 表明，人类、规则、学习型奖励和模型内置信号之间存在不可消除的成本—可信度权衡，生成世界不能只依赖单一 VLM/reward model 验收。

总体上，研究重心正在从“模型能生成什么”转为“状态由谁维护、动作如何类型化、结果怎样验证”。不过除现有云 API 外，本周期新增仍主要是论文、开源研究栈和项目 demo，没有新增规模化客户、生产 SLA 或长期在线运行证据。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中低 | Wan2.2-S2V API 文档补齐价格、并发和异步任务合同 | 既有能力更易工程接入；生成质量无跃迁 | 新增产品接口信息、UCF-Net 与 SynthGait |
| 世界模型 | 高 | Programmable World Model 将显式状态、规则引擎和生成渲染解耦 | 从“视觉记忆”推进到可查询、可验证的世界状态 | 新增 PWM；视触觉控制给出重要负结果 |
| Coding × 互动作品 | 很高 | coding agent 写世界程序；语义动作经 interpreter 驱动不同机器人 | 可执行接口、插件、数据采集和回归测试证据明显增强 | 新增 PWM、Show-Harness |
| 实时运行时 | 中 | OpenAI-compatible VLM endpoint、动作插件、异步视频任务成为接口形态 | 局部组件可运行；端到端实时生成仍不足 | Show-Harness 可复现，PWM 仍未开代码 |
| 工业应用 | 中 | 数字人云 API 已正式产品化；机器人与生成世界仍属研究验证 | 产品与研究的成熟度分化加剧 | 新增明确价格与限流证据 |
| 安全与治理 | 中 | UCF-Net 开放检测代码、模型与数据，但零样本泛化弱 | 工具可复现，尚不能作为独立生产防线 | 相对上期新增安全条目 |

## 3. 最值得关注的 8 个进展

### 1. Programmable World Model：把世界状态从像素历史中“解耦出来”

- **类型 / 日期 / 主线：**论文、项目 demo、benchmark；2026-09-09；世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2609.10540)、[项目页](https://alaya-lab.github.io/pwm/)、[GitHub](https://github.com/AlayaLab/pwm)。
- **相对上期：**新增。
- **核心贡献：**VLM coding agent 将参考图与自然语言规则转换为世界程序；state executor 维护实体、关系、生命值和离屏状态；确定性 compiler 把带状态的 3D OBB 与相机轨迹投影为像素对齐条件；视频模型仅承担生成式渲染。
- **关键结果：**作者报告 CombatStateBench 的 Count Accuracy 为 94%、State Accuracy 为 98%，并展示 897 帧、约 30 秒的多实体 rollout。均属论文实测，尚无独立验证。
- **Coding 接口：**世界规则、事件触发和状态转移是可执行程序；代码能直接查询、修改和验证 canonical state。
- **场景：**可玩生成视频、互动电影、品牌世界、预演与训练模拟器。
- **成熟度 / 证据：**项目 demo、论文实测；GitHub 当前只有 README 与素材，推理代码和权重仍列在 roadmap，不能视为已开源。
- **重要性 / 阅读：**高 / 必读。

### 2. Show-Harness：用语义动作和 interpreter 连接 VLM、GUI 与真实机器人

- **类型 / 日期 / 主线：**论文、GitHub、数据、模型、插件式运行时；2026-09-09；Coding、机器人、具身交互。
- **官方链接：**[论文](https://arxiv.org/abs/2609.10522)、[项目页](https://showlab.github.io/Show-Harness/)、[Apache-2.0 代码](https://github.com/showlab/Show-Harness)。
- **相对上期：**新增。
- **核心贡献：**将控制空间约束为 `MV_FWD/BACK/LEFT/RIGHT/UP/DOWN`、`GRASP`、`RELEASE` 等离散语义动作；Franka、AgileX Piper、ManiSkill 和 Isaac Lab 通过各自 interpreter 映射到本地控制量。
- **关键结果：**作者报告跨任务成功率为零样本 89%、微调模型 86%，最好基线为 57%；跨具身分别为 93%/87%，最好基线为 52%。这是作者实验，不是第三方评测。
- **Coding 接口：**支持 OpenAI-compatible VLM endpoint、YAML 配置、可开关插件、浏览器 `/api/step`、实时人工接管、日志与训练脚本。
- **场景：**自然语言机器人操作、可编程展览装置、数字孪生遥操作、GUI agent 采集具身数据。
- **成熟度 / 证据：**代码、六个 LoRA、数据和训练链路已开放；真实机器人仍需校准、视觉与硬件安全层。
- **重要性 / 阅读：**高 / 必读。

### 3. Compact Visuotactile World Models：预测更准不等于控制更安全

- **类型 / 日期 / 主线：**论文、模拟控制实验；v1 为 2026-09-09，v2 更新于 09-10；世界模型、机器人评测。
- **官方链接：**[论文及修订记录](https://arxiv.org/abs/2609.09597)。
- **相对上期：**新增。
- **核心贡献：**用小型随机初始化视触觉世界模型、轨迹级不确定性校准和 imagination actor-critic 研究抓取过程中的接触与力约束。
- **关键结果：**加入触觉后，端点力误差由 1.058 N 降至 0.228 N；但简单 tactile persistence 更低至 0.095 N。修改奖励后，10 cm 抬升成功率由 20.0% 升至 93.3%，满足每指 8 N 限制的成功率却只有 33.3%，低于力反馈控制的 70.0%。
- **Coding 接口：**适合接入“rollout—不确定性边界—力约束 verifier—安全控制器”的闭环；作者未提供公共 SDK。
- **场景：**易碎物体抓取、装配、触觉数字孪生与策略安全筛选。
- **成熟度 / 证据：**论文实测；控制实验限于 MuJoCo，GelSight 真实数据与模拟控制之间没有完成迁移验证。
- **重要性 / 阅读：**高 / 必读。

### 4. Wan2.2-S2V 数字人 API：接口、价格与限流合同补齐

- **类型 / 日期 / 主线：**正式产品、API；文档更新于 2026-09-11；数字人、Coding。
- **官方链接：**[API 文档](https://www.alibabacloud.com/help/en/model-studio/wan-s2v-api)、[模型总览](https://www.alibabacloud.com/help/en/model-studio/use-video-generation)。
- **相对上期：**新增的是文档与部署信息，不是新模型。
- **核心能力：**单图加音频生成说话、唱歌或表演视频，支持真人与卡通、头像到全身，输出 480p/720p、最长 20 秒。
- **Coding 接口：**HTTP 异步任务：提交后返回 `task_id`，需在 24 小时内查询结果；仅支持北京地域。
- **价格与限制：**官方列价为 480p 约 0.071677 美元/秒、720p 约 0.129018 美元/秒；提交上限 5 RPS、并发任务数 1。
- **场景：**批量虚拟讲解、商品口播、企业培训、非实时虚拟主播。
- **成熟度 / 证据：**正式云产品、官方文档；异步且并发低，不适合直接承担实时可打断数字人。
- **重要性 / 阅读：**中高 / 必读。

### 5. UCF-Net：检测开放，但零样本跨生成器仍失败

- **类型 / 日期 / 主线：**论文、GitHub、模型、dataset、demo；2026-09-07；数字人安全。
- **官方链接：**[论文](https://arxiv.org/abs/2609.07670)、[代码](https://github.com/XavierJiezou/UCF-Net)。
- **相对上期：**新增。
- **核心贡献：**融合 CLIP 的语义先验与 DINO 的视觉结构特征，通过分层专家聚合和基于熵的不确定性加权检测合成面孔。
- **关键结果：**论文汇总约 400 万张图，并增加八类新生成器、超过 8,000 张脸的测试集；作者报告统一 benchmark 的跨域平均 AUC 为 92.15%，但新生成器零样本 AUC 仅 40.90%，50-shot 后才升至 98.24%。
- **Coding 接口：**命令行、JSON 输出、CPU/GPU 推理、训练评测脚本和 Gradio demo，可部署为上传、直播抽帧或资产入库检查器。
- **场景：**数字人资产审核、内容取证、生成平台回归测试。
- **成熟度 / 证据：**已开源，CC BY-NC 4.0；论文实测，商业部署受许可证限制。
- **重要性 / 阅读：**中高 / 必读。

### 6. SynthGait-19K：物理动作与生成外观分工的合成数据范式

- **类型 / 日期 / 主线：**论文、dataset、GitHub、模型、demo；2026-09-08；数字人全身运动、仿真数据。
- **官方链接：**[论文](https://arxiv.org/abs/2609.08108)、[项目页](https://soroushmehraban.github.io/SynthGait-19k/)、[代码](https://github.com/TaatiTeam/SynthGait-19k)。
- **相对上期：**新增。
- **核心贡献：**Gait2Vid 将五个 MoCap 来源统一到 SMPL，以可控相机和深度条件视频扩散改变人物及场景外观，同时保留步态运动和六类参数标签。
- **规模与结果：**19,272 个视频、6,427 条动作、437 名受试者、671 分钟；GaitXFormer 在真实 GPJATK 上平均 Pearson 相关系数为 0.84，单个 5 秒片段在 RTX 3090 上约 0.27 秒。
- **Coding 接口：**Hugging Face WebDataset、训练/验证脚本、checkpoint、CLI JSON 输出和 Gradio。
- **场景：**康复评估、老人照护、运动分析；也可迁移为全身 avatar 动作 QA 数据管线。
- **成熟度 / 证据：**数据、模型与代码已开放；临床用途仍需前瞻性验证。
- **重要性 / 阅读：**中 / 可读。

### 7. No Free Checker：为世界模型和机器人 verifier 建立验收框架

- **类型 / 日期 / 主线：**survey、评测框架；2026-09-08；世界模型评测、可靠性。
- **官方链接：**[论文](https://arxiv.org/abs/2609.09250)。
- **相对上期：**新增。
- **核心贡献：**梳理约 150 项机器人 verifier，将其分为人类、规则/形式化、学习/预训练和模型内置四类，并用 availability 与 credibility 描述不可消除的权衡。
- **Coding 接口：**明确 temporal logic、终态 predicate、物理约束、模型编写规则和 rollout reward 各自适合放在哪个 CI 或在线控制节点。
- **场景：**机器人策略门禁、生成世界回归测试、自动驾驶仿真、数字人行为审核。
- **成熟度 / 证据：**综述，不是可直接部署的软件；对设计验收架构价值高。
- **重要性 / 阅读：**中高 / 可读。

### 8. VideoLLM 推理效率综述：持续感知的成本瓶颈被拆成四层

- **类型 / 日期 / 主线：**survey、GitHub 索引；2026-09-09；实时交互基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.10355)、[MIT 许可资源库](https://github.com/momentslab/awesome-efficient-videollm)。
- **相对上期：**新增。
- **核心贡献：**分析 125 篇工作，将成本优化拆为输入帧选择、视觉编码、connector/token 压缩、LLM prefill/KV-cache 四层，并明确区分可比实验与跨论文异构数字。
- **Coding 接口：**提供按机制组织的持续更新索引，可用于为数字人“持续观看”或机器人视频 agent 选择抽帧、token pruning、稀疏注意力与缓存方案。
- **场景：**低成本视频客服、直播理解、数字人视觉感知、边缘机器人。
- **成熟度 / 证据：**研究综述；指出音视频联合效率和标准化延迟评测仍明显缺失。
- **重要性 / 阅读：**中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期暂无新的高质量 talking-head、单图身份建模、角色一致性或长时数字人模型。Wan2.2-S2V 的增量是 API 合同和价格透明化，不是生成能力升级。
- **3D/4D 表示：**暂无数字人专用 NeRF、3DGS、mesh 或 4D avatar 高质量新增。PWM 的 3D OBB 是世界状态控制表示，不能等同于可驱动高保真人体。
- **全身动作：**SynthGait 展示了更实用的混合管线：SMPL/MoCap 保存运动真实性，视频扩散只负责身份、服装、环境和相机域随机化。它适合训练感知器，不等同于生成可编辑 3D avatar。
- **实时交互：**新增 API 仍是异步任务。可打断、turn-taking、视觉观察和工具调用仍需沿用上期 Gander 类交互控制层，再接独立 avatar 渲染器。
- **音视频与情感：**Wan 支持说话、唱歌和表演，但没有公开情绪控制、首包延迟或中途打断指标。
- **工程部署：**非实时内容生产已经能进入产品；实时虚拟主播仍需 `VAD/VAP → 对话 agent → 流式 TTS → 实时 avatar → WebRTC` 的独立栈。
- **安全与身份治理：**UCF-Net 可作为辅助检测器，但 40.90% 的零样本跨生成器 AUC 说明其不能单独承担生产审核。真人数字人仍需授权记录、声音许可、水印、来源元数据和人工复核。

结论：数字人本日新增是“可调用性和检测工具增强”，不是“形象生成跃迁”。

## 5. 世界模型进展

- **架构与训练：**PWM 将状态演进与观察生成解耦，形成 `program → canonical state → 3D OBB → video renderer`。这与只增加视觉历史、检索帧或 latent memory 的路线有本质差异。
- **可控 / 可玩生成：**规则能精确触发实体进入、停止、倒地或保持死亡状态，玩家动作改变权威状态，视频只表现结果。这比 prompt-only 控制更适合游戏机制。
- **长时与物理一致性：**PWM 解决的是离屏状态和事件不可逆性，但项目页的 30 秒展示尚不能证明小时级视觉稳定。物理碰撞、动力学和多人同步仍未被解决。
- **空间表示：**带状态 3D OBB 成为代码与生成模型之间的中间表示；它比纯文本可渲染，比隐式 latent 更可检查，但不足以表达软体、接触和复杂拓扑。
- **机器人 / 仿真：**Show-Harness 强调动作接口，视触觉论文强调世界预测与安全执行之间的差距。二者共同表明，可靠系统需要生成/预测模型与确定性控制器协作。
- **实时部署与评测：**本周期仍无新增生成世界端到端 p50/p95 动作延迟、每小时 GPU 成本、连续在线失败率或多用户并发数据。

## 6. Coding × 新型互动作品

### 链路一：自然语言规则 → 世界程序 → 显式状态 → 可玩生成视频

coding agent 根据参考图和创作者描述生成实体 schema、状态变量、事件触发器和转移规则；state executor 处理玩家输入；compiler 把状态变成 3D OBB 和时空控制图；视频模型渲染画面。

代码负责规则、库存、生命值、离屏实体和不可逆事件；生成模型负责外观与动态表现；传统碰撞或物理系统仍应负责确定性约束。PWM 已有 demo 和 benchmark，但公共代码未开放。

### 链路二：VLM → 语义动作 DSL → embodiment interpreter → 机器人或装置

VLM 每步选择 `MV_LEFT`、`GRASP` 等动作 token；interpreter 根据机器人、仿真器或舞台装置配置转换为控制量；安全 floor 和人工接管在执行前拦截危险动作。

开发者可用同一“角色大脑”控制机械臂、虚拟角色或互动展项，只替换解释器。Show-Harness 已提供代码、数据和真实机器人流程，是本周期证据最完整的链路。

### 链路三：业务脚本 → 异步数字人 API → 视频资产 → 可交互页面

业务系统生成口播文本与 TTS 音频，提交 Wan2.2-S2V 任务，轮询 `task_id`，将结果存入资产库；React/Unity 页面根据状态机选择预生成回复片段。

代码负责脚本、审批、任务重试、缓存和分支；模型负责音驱表演。适合培训、营销和低频数字员工，不适合真正的实时对话。

### 链路四：MoCap/SMPL → 深度控制 → 生成域随机化 → 感知模型

运动数据提供物理正确的骨架与参数；代码改变相机、人物、服装和背景配置；视频扩散生成多样 RGB；训练流水线再用成对标签训练 gait 或动作感知器。

这是“生成模型生产训练世界”的工业形态，可用于康复、运动与虚拟角色动作 QA。SynthGait 已开放数据和最小 demo。

### 链路五：候选 rollout → 多层 verifier → 安全执行

世界模型产生未来轨迹；规则 verifier 检查力、边界与时序约束；学习型 verifier 判断任务完成；低比例人工样本持续校准；真正执行前由传统控制器再次限制动作。

视触觉论文已证明“预测误差更低”不能代替控制验收；No Free Checker 则解释了为什么必须组合多种验证器。这条链路具有研究证据，尚无新增生产部署。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 技术栈与互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | PWM | 世界程序、状态引擎、OBB compiler、视频 renderer | 项目 demo | 未披露 | 代码未开、物理、多玩家、长时视觉漂移 | [项目页](https://alaya-lab.github.io/pwm/) |
| 影视/虚拟制作 | PWM、Wan S2V | 剧情状态图、数字演员、异步资产任务 | 组件可用 | S2V 约 $0.0717–0.1290/秒 | 精细修改、镜头一致性、版权 | [API](https://www.alibabacloud.com/help/en/model-studio/wan-s2v-api) |
| 直播与电商 | Wan S2V | 脚本/TTS、任务队列、预生成分支 | 正式产品但非实时 | 并发 1、5 RPS | 无流式首包、不可打断 | [文档](https://www.alibabacloud.com/help/en/model-studio/use-video-generation) |
| 品牌互动 | PWM | 事件规则、观众输入、生成渲染 | 概念验证 | 未披露 | 品牌一致性、实时审核 | [论文](https://arxiv.org/abs/2609.10540) |
| 教育培训 | Wan S2V、Show-Harness | 虚拟讲解、任务状态、具身演示 | 视频产品 / 机器人研究原型 | 未披露 | 事实可靠性、硬件安全 | [Show-Harness](https://github.com/showlab/Show-Harness) |
| 企业数字员工 | Wan S2V | 业务 agent、异步口播、前端状态机 | 可做非实时内容 | 最高约 $0.1290/输出秒 | 实时性、并发、审计 | [API](https://www.alibabacloud.com/help/en/model-studio/wan-s2v-api) |
| 陪伴与社交 | UCF-Net | 数字角色加生成内容检测 | 研究组件 | 未披露 | 零样本检测弱、身份与情感安全 | [代码](https://github.com/XavierJiezou/UCF-Net) |
| 空间计算 | PWM、Show-Harness | 3D OBB、场景状态、语义动作 | 研究原型 | 未披露 | 空间锚定、端侧算力 | [PWM](https://arxiv.org/abs/2609.10540) |
| 机器人/仿真 | Show-Harness、视触觉 WM | VLM、动作 DSL、interpreter、多层 verifier | 已开源研究栈 / 模拟实测 | 小模型微调为数个 H200 GPU 小时；在线延迟未披露 | 安全认证、长尾失败、sim-to-real | [Show-Harness](https://showlab.github.io/Show-Harness/) |

本周期没有新增能证明规模化部署的客户数量、并发、可用性或生产 SLA。

## 8. 可复现资源与开发者入口

| 资源 | 状态与许可 | 最小验证路径 | 复现判断 |
|---|---|---|---|
| [Show-Harness](https://github.com/showlab/Show-Harness) | Apache-2.0；runtime、插件、数据、LoRA、训练代码 | 用 `--sim` 启动 GUMI 浏览器，采集动作，再接 OpenAI-compatible VLM | 本日最高优先 |
| [SynthGait-19K](https://github.com/TaatiTeam/SynthGait-19k) | 数据、GaitXFormer、checkpoint、CLI/Gradio；仓库未清晰显示许可证，商业使用前核验 | 下载 checkpoint，对 5 秒单人全身步行视频输出六项 JSON 指标 | 可立即验证 |
| [UCF-Net](https://github.com/XavierJiezou/UCF-Net) | CC BY-NC 4.0；代码、模型、数据、CPU/GPU 推理 | 用内部真实脸和近期生成器各 100 张测零样本 AUC | 适合安全团队 |
| [VideoLLM 效率索引](https://github.com/momentslab/awesome-efficient-videollm) | MIT；方法与代码入口索引 | 按四层成本模型盘点现有持续视频 agent | 低成本、高实用 |
| [Wan2.2-S2V API](https://www.alibabacloud.com/help/en/model-studio/wan-s2v-api) | 北京地域正式服务；按输出秒计费 | 用同一人物图分别生成说话、唱歌、表演，记录失败率与实际费用 | 产品团队可验证 |
| [PWM](https://github.com/AlayaLab/pwm) | 当前仅 README 与素材；代码、权重待发布 | 只能审查 demo、论文和接口设计 | 暂不可复现 |
| [Compact Visuotactile WM](https://arxiv.org/abs/2609.09597) | 论文；未确认代码/权重 | 复做 MuJoCo Lift 的预测误差与力约束成功率分离实验 | 仅研究跟踪 |
| [No Free Checker](https://arxiv.org/abs/2609.09250) | 综述，无统一软件包 | 用其九项指标审计一个现有 reward/verifier | 适合制定验收标准 |

## 9. 系统架构与技术趋势判断

明显升温的路线是：显式世界状态、代码生成规则、类型化动作 DSL、具身 interpreter、生成式渲染器以及多层 verifier。单纯扩大视频上下文、缓存更多历史帧或依赖 VLM “记住状态”的路线正在被证明不够可靠。

当前更可能成为主流的架构是：

`自然语言需求 → coding agent → 可执行规则/任务图 → 权威状态 → 类型化动作 → 物理或业务校验 → 生成式视觉/数字人渲染 → 结果 verifier → 日志与回滚`

其中，代码负责离散事实、权限、状态机、任务生命周期、成本控制和测试；生成模型负责难以手工制作的外观、语音、动作变化和未来候选；传统引擎、数据库或控制器负责确定性物理和安全。

PWM 与 Show-Harness 分别在生成世界和真实机器人上证明，中间接口往往比扩大模型更重要：前者选择“状态增强 OBB”，后者选择“离散语义动作”。这些接口可以被测试、版本化、人工接管和替换后端，是 coding 与生成模型结合的核心资产。

尚未解决的问题包括：小时级视觉与身份一致性、毫秒级动作响应、复杂接触物理、多用户确定性同步、端侧运行、实时审核、商业许可证、肖像授权和生产可观测性。

需要降级看待的说法包括：将项目页 demo 称为开源系统；将预测误差下降等同于控制成功；将异步数字人 API 称为实时 avatar；将一次 30 秒 rollout 称为长期世界；将检测器在旧 benchmark 上的高 AUC 等同于对新生成器的可靠防护。

## 10. 论文精读候选

1. [Programmable World Model](https://arxiv.org/abs/2609.10540)  
   **原因：**最直接回答 coding 如何成为世界模型的“规则与状态层”。重点读 world-program schema、state executor、OBB compiler、CombatStateBench。与隐式视频记忆路线的关键差异是状态可查询、可修改、可验证。风险是代码未开、benchmark 集中于战斗事件。

2. [Show-Harness](https://arxiv.org/abs/2609.10522)  
   **原因：**提供跨具身动作接口、数据采集和部署合同。重点读 semantic action space、interpreter、GUMI、零样本与微调对比。风险是成功率主要来自作者环境，需验证动作离散化对精细操作的上限。

3. [Compact Visuotactile World Models](https://arxiv.org/abs/2609.09597)  
   **原因：**罕见地公开“感知改善但安全控制未改善”的负结果。重点读 reward revision、trajectory calibration、8 N 约束和反馈控制对比。风险是数据量小且未完成真实控制迁移。

4. [UCF-Net](https://arxiv.org/abs/2609.07670)  
   **原因：**高跨域总榜与低新生成器零样本结果形成重要反差。重点读数据拆分、uncertainty fusion 和 few-shot adaptation。风险是图像级检测不能直接覆盖视频时序伪造。

5. [SynthGait-19K](https://arxiv.org/abs/2609.08108)  
   **原因：**展示了“物理真值来自 MoCap，视觉多样性来自生成模型”的可迁移数据 recipe。重点读 SMPL 统一、深度条件、force-platform 验证和 synthetic-to-real。风险是临床群体覆盖和外观域差异。

## 11. 下周跟踪与可行动建议

### 继续追踪

1. PWM 是否正式发布 world-program schema、state executor、compiler、推理代码和权重。
2. PWM 的状态正确率能否扩展到库存、关系、软体、碰撞和分钟级重访。
3. Show-Harness 在消费级 GPU 上的实际 action latency、失败恢复和 token/调用成本。
4. 语义动作 DSL 能否同时驱动机器人、Unity/Unreal 角色与虚拟制作设备。
5. Wan2.2-S2V 的平均排队时间、任务失败率和并发扩容机制。
6. UCF-Net 对连续视频、压缩、二次录屏和最新 avatar 模型的独立测试。
7. SynthGait 的许可证、数据磁盘需求及对非正常步态和遮挡场景的覆盖。
8. 世界模型 verifier 是否开始报告 reward hacking、人工一致率和真实任务收益。

### 本周可做的小实验

1. **最小可编程生成世界。**  
   目标：验证显式状态是否比 prompt 历史更可靠。组件：JSON 世界状态、Python 状态机、简单 3D OBB 投影、任一可控视频 API。难点：状态到视觉条件的编译。成功判据：角色离屏后返回仍保持生命值、位置和道具状态，且所有事件有可重放日志。

2. **同一动作 DSL 驱动仿真与虚拟角色。**  
   目标：验证 Show-Harness 式接口能否跨物理与数字角色复用。组件：Show-Harness `--sim`、一个 Godot/Unity 动作解释器、OpenAI-compatible VLM。难点：动作粒度和时钟同步。成功判据：同一 token 序列完成仿真抓取和虚拟角色取物剧情，无需修改 agent prompt。

3. **数字人 API 生产基线。**  
   目标：测清 Wan2.2-S2V 的真实成本与吞吐。组件：20 张不同构图人物图、三类音频、异步任务队列和质量评分。难点：并发 1、失败重试、肖像合规。成功判据：记录成功率、排队/生成时间、每分钟成片成本及口型/身份失败类型。

4. **生成内容安全双层门禁。**  
   目标：确认单一 deepfake detector 是否足够。组件：UCF-Net、内容来源元数据、100 张内部真人图、多个近期生成器。难点：新生成器分布漂移。成功判据：分别报告零样本 AUC、误杀率和漏检率；若低于业务阈值，自动转人工或要求来源证明。
