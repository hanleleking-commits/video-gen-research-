# 数字人、世界模型与互动作品研究日报：2026-08-25

检索窗口：2026-08-19 至 2026-08-25。

## 1. 本日摘要

本周期最值得关注的数字人进展，是生成结果正在从不可编辑的视频像素转向可进入传统图形管线的结构化资产：DiGS-Avatar 报告从单图在 0.71 秒内重建可动画 3D Gaussian 人体，AvatarDynamizer 则为静态骨骼角色补充随姿态变化的服装褶皱等表面动力学。与此同时，AESR 证明在无法修改闭源视频模型权重时，可以用 agent、VLM、可复用 playbook 和局部视频编辑组成身份一致性修复流水线。

世界模型侧的主信号不是出现新的通用可玩基座，而是“状态表示必须服务动作决策”：WA-JEPA 将未来 latent 与车辆轨迹联合生成，DECOWAM 显式分离移动底盘、机械臂和相机自运动。两项工作都表明，画面预测只有与动作接口、控制变量和闭环任务指标绑定，才可能形成工业价值。

评测进一步从视觉相似度转向因果与任务正确性。BeyondMasks 检查移除物体后影子、反射、涟漪是否同步消失；VGI-BENCH 显示最强被测视频模型在过程型视觉推理任务上也只有论文报告的 51.0%；MotionPhys 则利用完整动作轨迹的物理不一致检测生成视频。Coding × 互动作品方面，本周期最可靠的模式是：代码维护角色、对象、动作和权限的权威状态，生成模型负责外观、表演或候选未来，VLM/物理评测器负责自动验收和局部返工。窗口内未发现新的、具有一手证据的规模化数字人或生成式世界商业部署。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | 单图 3DGS Avatar、动态表面生成、agentic 身份修复形成三类互补管线 | DiGS、AESR 有代码；AvatarDynamizer 仍为论文/demo | 相比上期的 4D 视频重建与 RTC 运行时，新增“快速资产化”和“自动返工” |
| 世界模型 | 中高 | latent 未来开始与动作、规划和不同 embodiment 控制量共同建模 | WA-JEPA、DECOWAM 仍以论文验证为主 | 从通用视频一致性推进到规划相关状态和移动机器人全身动作分解 |
| Coding × 互动作品 | 高 | agent playbook、typed issue、条件接口和因果评测可组成持续运行的生成流水线 | AESR、BeyondMasks 可运行；完整实时作品仍缺失 | 新增“生成—诊断—局部修复—回归验收”闭环 |
| 工业部署 | 低 | ResiliFlow 展示可审计工具编排，但缺少公开部署和 SLA | 研究平台/论文原型 | 本周期无规模化部署证据 |

## 3. 最值得关注的 10 个进展

### 1. DiGS-Avatar：单图快速生成可动画 3D Gaussian 人体

- **类型 / 日期 / 主线：** 论文、GitHub、3D Avatar；2026-08-21；数字人、Coding。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20759)、[GitHub](https://github.com/KLMAV-CUC/DiGS-Avatar)。
- **相对上期：** 首次收录；不同于上期 4DAnyone 的“视频→多视角视频→4DGS”，本项直接在 UV latent 中补全单图不可见区域。
- **核心贡献：** 多视角 teacher 生成几何对齐的 UV pseudo-ground-truth，单视角 diffusion student 完成 UV latent，再以语义特征补充纹理并解码为 canonical 3D Gaussians。作者报告单个可动画 Avatar 重建耗时 0.71 秒。
- **Coding 接口：** 仓库提供训练、UV 特征导出和推理脚本；输出可作为 3DGS 渲染资产，由代码控制姿态、相机和时间轴。
- **互动与工业场景：** 快速 NPC 创建、虚拟试衣预览、展会分身、互动电影群众角色。
- **成熟度 / 证据：** MIT 代码已开放，但需自行准备 SMPL-X、DINOv3、SANA、IDOL 及 DiGS checkpoint；预训练 checkpoint 下载入口尚不完整。论文实测。
- **评级：** **高 / 必读**。

### 2. AESR：用 agent 自动修复闭源视频模型的人物身份漂移

- **类型 / 日期 / 主线：** 论文、GitHub、agentic 视频流水线；2026-08-21；数字人、Coding。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20749)、[GitHub](https://github.com/oceanflowlab/AESR)。
- **相对上期：** 首次收录；把身份一致性从模型内部条件控制扩展为可替换模型供应商的外部工程闭环。
- **核心贡献：** agent 从官方文档和历史结果维护 prompt playbook；VLM 定位身份或语义错误片段，将问题规范化为 typed issue，修复关键帧，再调用视频编辑模型局部重生成。论文系统获得 ACM MM 2026 IPVG Challenge Track 1 第一名。
- **Coding 接口：** Python 3.10、JSON playbook、provider adapter、FFmpeg、单元测试和完整 repair shell 模板；最终视频编辑调用保持显式。
- **互动与工业场景：** 互动短剧分支生成、品牌人物广告、多轮用户定制视频、虚拟演员资产返工。
- **成熟度 / 证据：** 编排代码和小样例可运行；商业模型 API、私有数据和响应记录未提供，仓库尚未选择软件许可证，只能按 source-available 处理。论文实测。
- **评级：** **高 / 必读**。

### 3. WA-JEPA：未来世界 latent 与车辆动作联合生成

- **类型 / 日期 / 主线：** 论文、模型；2026-08-21；世界模型、自动驾驶。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20974)、[GitHub 占位仓库](https://github.com/AFARI-Research/WA-JEPA)。
- **相对上期：** 首次收录；相较仅预测视觉未来的 JEPA，新增 future latent 与 ego trajectory 联合 flow matching。
- **核心贡献：** 用 hybrid future masking 取代随机 mask completion，并让动作监督直接塑造规划相关的未来表示。作者报告 NAVSIM-v2 EPDMS 91.7，在相同协议下领先所比基线；HUGSIM closed-loop HD-Score 为 0.4462。
- **Coding 接口：** 预期输入历史视频和候选/监督动作，输出未来 scene token 与车辆轨迹，可作为 planner 的预测模块；当前仓库只有“code will be released soon”。
- **互动与工业场景：** 自动驾驶候选轨迹评分、危险场景预演、数据回放和规划回归测试。
- **成熟度 / 证据：** 研究原型；指标为论文实测，代码未发布。
- **评级：** **高 / 必读**。

### 4. AvatarDynamizer：给静态 Avatar 增加可控表面动力学

- **类型 / 日期 / 主线：** 论文、项目 demo；2026-08-20；数字人、4D 表示。
- **官方链接：** [论文](https://arxiv.org/abs/2608.19900)、[项目页](https://vcai.mpi-inf.mpg.de/projects/AvatarDynamizer/)。
- **相对上期：** 首次收录；补足普通骨骼蒙皮无法表达服装褶皱和软组织动态的问题。
- **核心贡献：** 将 pose-dependent surface dynamics 编码为动态 texture map，利用视频 diffusion 生成纹理空间动态，再解码成多视角一致的 3D Gaussians。
- **Coding 接口：** 输入静态 3D Avatar 与骨骼姿态序列；理论输出是可由动画时间轴驱动的动态 Gaussian 表面。未发现公开代码。
- **互动与工业场景：** 虚拟时装、舞蹈角色、数字替身、近景游戏人物和虚拟制作。
- **成熟度 / 证据：** 研究 demo；论文实测，无可复现代码、运行成本或引擎插件。
- **评级：** **高 / 必读**。

### 5. DECOWAM：分离底盘、机械臂与相机自运动的世界—动作模型

- **类型 / 日期 / 主线：** 论文、dataset、真实机器人实验；2026-08-20；世界模型、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20114)。
- **相对上期：** 首次收录；从固定基座世界动作模型扩展到足式移动操作。
- **核心贡献：** 冻结 FastWAM 主干，以约 25.95M 可训练参数的 residual adapter 分离 base/arm latent，并加入 base velocity condition 和 action-equivalent future bottleneck。ARMDOG 数据同步视频、全身状态、动作和语言。
- **Coding 接口：** 底盘速度、机械臂动作和相机观察形成独立条件通道，便于接 ROS、运动控制器和安全状态机。
- **互动与工业场景：** 移动巡检、仓储取放、家庭服务机器人、远程操作预演。
- **成熟度 / 证据：** 作者完成每方法 79 次闭环真实机器人试验，报告 action MSE 降低 21.7%；任务完成率仅与最强基线相当。未见代码和 ARMDOG 下载入口。
- **评级：** **高 / 必读**。

### 6. BeyondMasks：把视频编辑评测改写为因果干预测试

- **类型 / 日期 / 主线：** benchmark、dataset、GitHub；2026-08-20；世界模型评测、视频编辑。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20107)、[GitHub](https://github.com/yigitekin/BeyondMasks)、[数据集](https://huggingface.co/datasets/yigitekin/BeyondMasks)。
- **相对上期：** 首次收录。
- **核心贡献：** 180 组对齐视频覆盖影子、反射、半透明、蒸汽、照明和动态痕迹；CORE 分开报告 ObjectScore 与 AfterEffectScore。项目页报告 Gemini judge 与人类判断相关系数分别为 0.62 和 0.73。
- **Coding 接口：** CC BY 4.0 数据约 2.81GB；官方 `eval.py` 接收输入、干净参考和模型输出，调用 Gemini 后产生可恢复执行的 JSON 结果。
- **互动与工业场景：** 互动场景编辑 CI、影视擦除验收、虚拟制作资产清理、世界模型因果测试。
- **成熟度 / 证据：** 数据和评测代码已开放；论文实测。
- **评级：** **高 / 必读**。

### 7. VGI-BENCH：视频生成模型的过程型视觉推理测试

- **类型 / 日期 / 主线：** benchmark、dataset；2026-08-20；世界模型、基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2608.19583)。
- **相对上期：** 首次收录；补充了只看最终画面或视觉质量的评测盲点。
- **核心贡献：** 27 项任务、810 个实例覆盖视觉组织、物理操作、结构谜题和时空动态，要求生成过程有效，而非仅有合理终态。作者报告最强被测模型 Seedance 2.0 得分 51.0%，后期去噪通常只能细化早期假设，难以纠正推理错误。
- **Coding 接口：** 可转化为生成式游戏或教程视频的过程断言；代码和数据目前仍为“将发布”。
- **成熟度 / 证据：** 论文 benchmark；尚不可完整复现。
- **评级：** **中高 / 必读**。

### 8. MotionPhys：用完整运动轨迹检测生成视频

- **类型 / 日期 / 主线：** 论文、安全检测；2026-08-21；安全、世界模型评测。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20770)。
- **相对上期：** 首次收录。
- **核心贡献：** 从 optical flow 中提取稀疏轨迹，在多个时间尺度分析轨迹几何演化，利用惯性、连续作用力等长期运动规律识别生成视频，而非依赖压缩或模型指纹。
- **Coding 接口：** 适合作为视频上传、数字人发布和合成数据入口的独立检测服务；未发现代码或推理成本披露。
- **互动与工业场景：** UGC 审核、数字人身份保护、训练数据清洗、生成世界物理异常报警。
- **成熟度 / 证据：** 论文原型；论文实测、无第三方验证。
- **评级：** **中高 / 可读**。

### 9. Orthogonal JEPA：将单一 latent 拆成互补预测因子

- **类型 / 日期 / 主线：** 论文、世界模型架构；2026-08-20。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20065)。
- **相对上期：** 首次收录。
- **核心贡献：** 用多个正交 basis 和独立 prediction branch 分解未来状态，减少主要视觉信号占满表示容量的问题；实验覆盖视觉、控制、分子动力学和长期预测。
- **Coding 接口：** 合成后的 latent 可接 decoder、planner 或 autoregressive rollout；各 factor 还可映射为几何、运动、角色状态等独立诊断通道。
- **互动与工业场景：** 可调试世界状态、机器人规划、生成式游戏状态压缩。
- **成熟度 / 证据：** 研究原型；未见公开代码。
- **评级：** **中 / 可读**。

### 10. ResiliFlow：以有界工具调用组织交通韧性“世界模型”

- **类型 / 日期 / 主线：** 论文、工业研究平台；2026-08-21；Coding、工业应用。
- **官方链接：** [论文](https://arxiv.org/abs/2608.20709)。
- **相对上期：** 首次收录。
- **核心贡献：** 把道路感知、网络分析、灾害场景模拟、恢复优先级、验证和记忆组织成八阶段循环；本地 Assistant 或多供应商 LLM 只能调用有边界的可执行工具。
- **Coding 接口：** 地图函数、视觉检测、路由和 scenario simulation 由传统程序执行，LLM 只负责把问题映射到受限工具。
- **互动与工业场景：** 城市灾害指挥桌面、交通数字孪生、基础设施巡检和方案复盘。
- **成熟度 / 证据：** 论文称已实现平台，但未找到公开仓库、客户部署、并发或真实灾害运行证据，应按研究原型处理。
- **评级：** **中 / 仅跟踪**。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：** DiGS-Avatar 将单图身份建模压缩成 UV latent completion；AESR 则不重新训练生成模型，而是在外部管理 prompt、错误诊断和局部修复。
- **3D/4D 表示：** 3D Gaussian 正成为数字人的中间资产格式。DiGS 解决快速静态身份资产化，AvatarDynamizer 解决骨骼动画之上的动态褶皱，两者具备明显的上下游互补性。
- **实时交互：** DiGS 的 0.71 秒是资产重建时间，不等于完整互动系统的端到端延迟；AvatarDynamizer未披露实时 FPS。窗口内没有新的可验证实时 talking-head 基座。
- **音视频与情感：** 本周期暂无高质量新增。AESR可修复视频语义，但不提供语音、情感、监听状态或 turn-taking。
- **工程部署：** 最有产品价值的路径是“单图→结构化 Avatar→传统引擎驱动”，以及“视频模型 API→VLM 验收→局部返工”。直接像素生成仍难以提供精确碰撞、网络同步和确定性回放。
- **安全与身份治理：** AESR提升身份保持但不解决肖像授权；MotionPhys可作为合成视频检测层，但水印、来源凭证、授权撤回和训练身份删除本周期无高质量新增。

## 5. 世界模型进展

- **架构与训练：** WA-JEPA 把确定性 latent 回归改为 conditional flow matching；Orthogonal JEPA 用正交因子减少表示冲突。
- **可控与可玩：** 本周期没有新的高质量键鼠可玩世界模型。控制增量主要体现在轨迹、底盘速度和机械臂动作等工业动作空间。
- **长时与物理一致性：** BeyondMasks、MotionPhys 和 VGI-BENCH分别从因果后效、完整运动轨迹和过程正确性暴露“短时画面合理、长期机制错误”的问题。
- **空间表示：** 3DGS继续承担 Avatar 资产与动态表面的接口；机器人侧则更倾向在语义 latent 中保留规划相关信息。
- **机器人、游戏与仿真：** DECOWAM提供真实移动操作试验；WA-JEPA覆盖驾驶闭环 benchmark。游戏方向本周期没有新增部署证据。
- **实时部署与评测：** 新论文普遍缺少显存、P95 延迟、并发、故障恢复和单位 rollout 成本，不能由参数量或单项 benchmark 推断可部署性。

## 6. Coding × 新型互动作品

### 链路一：照片即建角色的动态 NPC 管线

**单张照片 → DiGS-Avatar → canonical 3DGS → 骨骼姿态/AvatarDynamizer → Unity、Unreal 或 WebGPU → 个性化 NPC。**

模型负责不可见表面、纹理和动态褶皱；代码负责身份 ID、骨骼、碰撞代理、LOD、相机与网络同步；传统引擎维护权威位置和游戏规则。DiGS代码已开放，AvatarDynamizer和完整引擎导入链仍属本报告推断。

### 链路二：可自动返工的互动电影生成器

**剧情图与角色状态 → 视频模型 API → AESR playbook → VLM typed issue → 关键帧修复与局部视频编辑 → BeyondMasks/身份指标验收。**

代码保存服装、伤势、道具和情绪等角色状态，并记录每个分支的生成版本；模型负责镜头表现；评测器决定接受、重试或转人工。AESR和BeyondMasks均有可运行代码，但端到端商业制作案例尚无证据。

### 链路三：面向规划而非观感的驾驶数字孪生

**历史多摄像头视频 → WA-JEPA latent future＋ego trajectory → planner 候选评分 → 规则与安全约束 → 仿真回放。**

生成模型表达多种可能未来；规划器选择轨迹；传统规则系统负责交通规则、车辆动力学边界和紧急制动。论文给出闭环 benchmark，代码仍未发布。

### 链路四：移动机器人全身动作世界模型

**自然语言任务 → 高层 agent → 底盘/机械臂分解 → DECOWAM rollout → ROS 控制器与安全状态机 → 真实移动操作。**

世界模型预测不同动作如何共同改变视觉未来，代码负责任务分解、速度限制、碰撞保护和失败恢复。已有真实机器人论文证据，但数据与实现未开放。

### 链路五：具备因果回归测试的生成世界编辑器

**用户要求删除或替换对象 → 视频/世界编辑模型 → CORE 检查对象及其影子、反射、照明、涟漪 → MotionPhys 检查轨迹 → 自动返工。**

这使互动世界的“编辑”从局部像素修改升级为状态级干预。BeyondMasks可运行；MotionPhys尚无代码，完整双评测链为本报告推断。

### 链路六：可审计的交通韧性互动沙盘

**自然语言问题 → 有界工具路由 → 道路网络模型/视觉检测/灾害模拟 → 人工审核 → 方案记忆与复盘。**

LLM不直接修改基础设施决策，而是编排确定性工具并保留假设和验证记录。ResiliFlow提供架构证据，但公开复现和真实部署待核验。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | DiGS、AvatarDynamizer、引擎角色系统 | 照片创建角色、骨骼驱动、动态表面 | 开源组件＋研究 demo | DiGS 作者报告 0.71 秒重建；运行 FPS 未披露 | 3DGS 碰撞、LOD、多人同步、许可证 | [DiGS](https://github.com/KLMAV-CUC/DiGS-Avatar) |
| 影视/虚拟制作 | AESR、视频编辑 API、BeyondMasks | 分支生成、局部返工、因果验收 | 可运行研究管线 | API 成本未披露 | 镜头连续性、授权、人工终审 | [AESR](https://github.com/oceanflowlab/AESR) |
| 直播与电商 | DiGS、既有 RTC/TTS 运行时 | 快速创建品牌角色 | 资产原型 | 未披露 | 实时表情、发丝/服装、并发 | [DiGS 论文](https://arxiv.org/abs/2608.20759) |
| 品牌互动 | AESR、角色状态库、Web 前端 | 用户定制身份一致视频 | 研究管线 | 未披露 | 商业 API 依赖、肖像授权 | [AESR 论文](https://arxiv.org/abs/2608.20749) |
| 教育培训 | VGI-BENCH、过程断言、视频生成 | 生成操作演示并检查步骤 | benchmark 原型 | 未披露 | 过程错误、事实审核 | [VGI-BENCH](https://arxiv.org/abs/2608.19583) |
| 企业数字员工 | DiGS＋既有 agent/RTC | 个性化 3D 形象与工具调用 | 组件可验证 | 未披露 | SLA、隐私、人工接管 | [DiGS GitHub](https://github.com/KLMAV-CUC/DiGS-Avatar) |
| 陪伴与社交 | DiGS、AvatarDynamizer、记忆服务 | 持续角色外观和动作 | 研究原型 | 未披露 | 情感安全、长期记忆与身份治理 | [AvatarDynamizer](https://arxiv.org/abs/2608.19900) |
| 空间计算 | 3DGS Avatar、姿态输入、空间渲染 | 自由视角观察与全身交互 | 资产原型 | 未披露 | 移动端渲染、mesh/GS 混合 | [DiGS](https://arxiv.org/abs/2608.20759) |
| 机器人/仿真 | DECOWAM、WA-JEPA、ROS/MPC | 候选未来预测与闭环动作 | 真实试验＋benchmark | 未披露 | 安全认证、数据和代码缺失 | [DECOWAM](https://arxiv.org/abs/2608.20114) |
| 城市基础设施 | ResiliFlow、GIS、CV、工具调用 | 问答式灾害推演与人工审核 | 研究平台 | 未披露 | 数据更新、责任边界、公开复现 | [ResiliFlow](https://arxiv.org/abs/2608.20709) |

## 8. 可复现资源与开发者入口

| 资源 | 开放状态 / 许可证 | 要求 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|---|
| [DiGS-Avatar](https://github.com/KLMAV-CUC/DiGS-Avatar) | 代码 MIT；第三方模型和数据各自授权 | SMPL-X、DINOv3、SANA、IDOL、GPU；预训练 DiGS checkpoint 入口不完整 | 值得检查管线，暂不建议承诺产品 | 先运行仓库 sample，确认能否获得 checkpoint；导出 canonical GS 并测试姿态变化 |
| [AESR](https://github.com/oceanflowlab/AESR) | source-available，尚无软件许可证 | Python 3.10、FFmpeg、VLM/视频 API 凭证 | 强烈建议做编排验证 | 先运行 prompt-only；再对一个授权视频执行 critique、typed issue 和 edit prompt 生成 |
| [BeyondMasks](https://github.com/yigitekin/BeyondMasks) | 数据 CC BY 4.0；2.81GB | Python 3.10、Gemini API | 强烈建议 | 下载三元组数据，用现有视频编辑模型生成结果，运行 `eval.py` 比较 Object/Aftereffect Score |
| [BeyondMasks 数据](https://huggingface.co/datasets/yigitekin/BeyondMasks) | 数据与 mask 已开放 | 约 2.81GB | 值得作为视频编辑 CI 小集 | 先选影子、反射、涟漪各 5 例建立回归集 |
| [WA-JEPA](https://github.com/AFARI-Research/WA-JEPA) | 占位仓库，代码未发布 | 未披露 | 等代码 | 先复刻 future-mask 与 trajectory joint-head 的小型 ablation |
| AvatarDynamizer | 论文和 demo，无代码 | 未披露 | 等代码 | 用现有骨骼 Avatar 测试 texture-space pose conditioning 的简化版 |
| DECOWAM / ARMDOG | 仅论文，未见数据和代码 | 未披露 | 论文精读 | 在仿真中分别扰动底盘速度和机械臂动作，验证分离条件是否减少视觉预测误差 |
| VGI-BENCH | 论文承诺发布代码和数据 | 视频模型 API 成本未披露 | 等发布 | 自建 10 个齿轮、迷宫、物体操作过程任务，区分过程与终态正确率 |
| MotionPhys | 仅论文 | optical flow 与轨迹提取 GPU 成本未披露 | 可先做简化实现 | 比较真实/生成抛物、滚动和碰撞视频的轨迹曲率、速度连续性 |

## 9. 系统架构与技术趋势判断

明显升温的方向：

1. **3DGS 成为数字人资产中间层。** 它连接单图生成、动态表面和自由视角渲染，但仍需 mesh 或简化代理处理碰撞与动画工具兼容。
2. **模型外部的 agentic 修复。** 对闭源视频模型，竞争优势开始来自 playbook、诊断、路由、版本管理和局部返工，而非只来自 prompt。
3. **世界状态必须与动作共同学习。** WA-JEPA和DECOWAM都让动作监督进入未来表示，弱化单纯“生成漂亮未来”的目标。
4. **评测从画面相似度转向因果和过程。** 物体删除后是否消除物理后效、操作步骤是否有效、完整轨迹是否符合动力学，比单一 FVD/SSIM 更接近工业验收。
5. **传统运行时继续承担权威状态。** 当前生成模型仍不适合负责库存、碰撞、权限、计费、多人同步和安全规则。

正在形成的可复用架构是：

`用户输入 → agent/剧情或任务规划 → 结构化角色与世界状态 → 生成模型/世界模型候选输出 → VLM＋物理评测 → 局部修复或重生成 → 引擎/控制器执行 → 指标、审核和人工接管`

仍未解决的问题包括：3DGS 与生产 mesh/材质管线互转、动态服装与碰撞同步、分钟级角色状态保持、多用户实时并发、真实 P95 延迟、视频 API 成本、VLM judge 稳定性、肖像与声纹授权、生成资产撤回和跨供应商可重复性。

需要降级看待的说法包括：把 0.71 秒资产重建等同于实时互动；把 benchmark 第一等同于商业稳定性；把真实机器人有限试验等同于规模化部署；把公开占位仓库等同于开源；把视频中视觉合理的动作等同于可用于安全规划。

## 10. 论文精读候选

1. **[DiGS-Avatar](https://arxiv.org/abs/2608.20759)**  
   重点读 UV latent、teacher-student supervision、GASA 和 animation 表示。它与多视角视频再重建路线的差异最清楚。复现风险是 checkpoint、SMPL-X 和第三方资产许可证。

2. **[AESR](https://arxiv.org/abs/2608.20749)**  
   重点读 playbook 更新、错误定位、关键帧修复和 MoE selection。价值在于展示闭源生成 API 的可编程后训练替代方案。风险是结果依赖商业模型且仓库无许可证。

3. **[WA-JEPA](https://arxiv.org/abs/2608.20974)**  
   重点读 hybrid future masking、conditional flow matching 和 joint future-action predictor。它将 JEPA 从通用表示学习推向规划相关状态。风险是代码未发布、benchmark 协议复杂。

4. **[DECOWAM](https://arxiv.org/abs/2608.20114)**  
   重点读 action-equivalent future bottleneck、base/arm adversarial separation 和真实机器人协议。差异在于显式处理移动相机和全身协调。风险是 ARMDOG 和代码未开放。

5. **[BeyondMasks](https://arxiv.org/abs/2608.20107)**  
   重点读 paired data construction、after-effect taxonomy 和 CORE 人类相关性实验。它把视频编辑重写为因果干预问题。复现风险主要是 VLM judge 成本和模型版本漂移。

## 11. 下周跟踪与可行动建议

持续追踪：

1. DiGS-Avatar 是否补充可下载的 teacher/student checkpoint、显存和端到端资产大小。
2. DiGS输出能否稳定转换为 Unity、Unreal、Nerfstudio 或 WebGPU 可用格式。
3. AvatarDynamizer 是否发布代码、数据，以及动态 texture 的实时 FPS。
4. AESR何时增加明确许可证，并开放不同视频模型供应商的可比较结果。
5. WA-JEPA代码发布后，规划增益是否来自世界建模本身而非更强动作监督。
6. DECOWAM与ARMDOG是否开放，以及真实任务成功率、失败类型和安全恢复细节。
7. VGI-BENCH代码与数据发布后，过程评分与人类判断的一致性。
8. MotionPhys对新生成器、真实慢动作、摄影机运动和后期特效的误报率。

本周可动手验证：

1. **单图 Avatar 最小资产测试**  
   目标：判断 DiGS 是否真能进入实时引擎资产管线。  
   组件：DiGS、官方 sample、3DGS viewer、简单骨骼姿态序列。  
   难点：checkpoint 和第三方模型准备。  
   成功判据：同一身份在三个姿态、三个视角下无明显几何破裂，并记录生成时间、显存和资产大小。

2. **身份一致视频自动返工实验**  
   目标：验证 agentic repair 是否比整段重生成更省成本。  
   组件：AESR、一个视频生成 API、VLM、FFmpeg。  
   难点：错误片段定位和 API 随机性。  
   成功判据：20 个样本中身份指标提升，平均只重编辑少于 30% 时长，且总费用低于完整重生成。

3. **因果视频编辑 CI**  
   目标：让对象删除同时消除影子、反射和动态痕迹。  
   组件：BeyondMasks、现有视频编辑模型、CORE。  
   难点：Gemini judge 版本漂移和评测费用。  
   成功判据：固定 15 例回归集可重复运行，ObjectScore 与 AftereffectScore 分别记录，人工抽检排序基本一致。

4. **动作分解世界模型小实验**  
   目标：验证把相机、底盘和机械臂动作分开是否提高未来预测可诊断性。  
   组件：机器人仿真器、小型视频预测模型、三路 action condition。  
   难点：同步数据和独立扰动设计。  
   成功判据：单独扰动任一动作通道时，预测变化集中于对应运动因素，并优于拼接全部动作的基线。
