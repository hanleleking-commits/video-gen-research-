# 数字人、世界模型与互动作品研究日报：2026-08-14

检索窗口：2026-08-08 至 2026-08-14。  
检索截止：2026-08-14 09:03（UTC+8）。截至检索时，arXiv 最新正式批次为 8 月 13 日；重点论文实际提交时间多为 8 月 12 日 UTC，未将检索排序或页面抓取时间视作发布日期。

## 1. 本日摘要

本周期最强的数字人信号来自 Avatar-Forever：22B 视频模型通过四步蒸馏、长时恢复适配器和历史特征缓存，在单张 H100 上实现 768×512、端到端 27.2 FPS，并展示超过 11 分钟的连续音频驱动结果；但代码和权重仍未开放，因此属于高质量研究 demo，而不是可集成产品。[论文](https://arxiv.org/abs/2608.12107)、[项目页](https://leeruibin.github.io/avatarforever-project-page/)

世界模型的关键变化并非继续扩大像素生成，而是把“未来”压缩成代码可消费的状态接口。RIFT 用一次前向写入 future-position K/V cache，在 LIBERO 上以接近 current-only policy 的 247.9 ms/action chunk 获得 98.8% 成功率，说明机器人控制可能需要未来表征，但不一定需要部署时真的生成未来视频。[论文](https://arxiv.org/abs/2608.11521)

StateFlow 与 D3D-GEN 从另一方向强化“权威世界状态”：前者以持久、对象中心的 3D 状态支持反复编辑、镜头规划和游戏预演；后者把法规、空间约束、资产语义编译为 `JSON → scene graph → world.yaml → Isaac Sim/Gazebo`，是本批次 Coding × 世界生成最完整的因果链。[StateFlow](https://yuyangyin.github.io/StateFlow/)、[D3D-GEN](https://arxiv.org/abs/2608.11876)

视频创作侧出现了明确的 agentic optimization：多模态模型生成可验证问题、评估生成结果、修改提示词，再用贝叶斯优化搜索 CFG 与 seed。这让 coding agent 从“调用一次视频 API”升级为具有测试、反馈和搜索预算的生成控制器，但实验消耗最多 100 次 Veo 2 调用，生产成本仍是核心障碍。[论文](https://arxiv.org/abs/2608.12290)

世界模型评测也开始区分“合理未来”和“属于这一具体事件的反事实未来”。新的驾驶研究表明，仅输入历史和替代动作会遗漏事实续篇中的事件证据，需要遵循 abduction—action—prediction，并显式搬运可观测证据。[论文](https://arxiv.org/abs/2608.11601)

本批次没有新的正式客户部署、公开 SLA、Unity/Unreal 商用插件或规模化并发案例。证据主要是论文实测、项目网页和模拟器实验；产业成熟度仍显著落后于模型能力。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | Avatar-Forever 报告单 H100、27.2 FPS、11 分钟连续生成；TGRHuman生成显式 3D 人体 | 实时指标显著改善，但无可运行权重/API | 上期 Ex-Omni-2D 仍慢于实时；本期转向真正实时、长时稳定和缓存优化 |
| 世界模型 | 高 | RIFT 用一次 cache prefill 替代视频 rollout；反事实研究要求保留事实事件证据 | 从“生成未来”推进到“未来表示接口”和因果评测 | 上期 Flex-π 按需切换输出流；本期进一步删除迭代 rollout，同时保留显式未来读取 |
| Coding × 互动作品 | 高 | StateFlow 持久 3D 状态、D3D-GEN 可编译世界描述、agentic I2V 自动优化 | 形成状态、约束、评测和生成闭环，但开放代码不足 | 相比上期的行为回放验收，本期增加生成前约束编译与生成后自动搜索 |
| 工业部署证据 | 低 | Isaac Sim/Gazebo、CARLA 和闭环机器人 benchmark 增多 | 仍停留在研究原型和作者 demo | 未出现新客户试点、正式产品或规模化部署 |

## 3. 最值得关注的 12 个进展

### 1. Avatar-Forever：单 H100 实时、长时音频驱动数字人

- **类型 / 时间 / 主线：** 论文、项目页、占位 GitHub；2026-08-12；数字人、实时运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2608.12107)、[项目页](https://leeruibin.github.io/avatarforever-project-page/)、[GitHub](https://github.com/leeruibin/avatarforever)。
- **相对上期的新变化：** 新条目；相较上期四卡且 RTF 1.293 的 Ex-Omni-2D，本项目首次在本周新增中给出单卡实时吞吐和超过 11 分钟的连续结果。
- **核心贡献：** 将四步 DMD 蒸馏与长时 RRT 分支并行训练，部署时合并；ForeverCache 每个 chunk 只在首个去噪步计算完整历史，后续复用历史特征。
- **Coding 接口：** 理论接口为“持续音频 chunk＋首帧身份锚点＋历史缓存 → 视频 chunk”；适合接入 RTC、取消队列和会话状态机，但官方推理接口尚未发布。
- **场景：** 虚拟主播、数字讲师、陪伴角色、长时直播表演。
- **成熟度 / 证据：** 作者 demo、代码仓库仅 README。论文报告 768×512、27.2 FPS、单 H100，30 秒生成由 38.85 秒降至 26.71 秒；未披露首帧延迟、并发和打断延迟。
- **重要性 / 阅读：** 高 / 必读。

### 2. StateFlow：用持久 3D 状态替代一次性视频生成

- **类型 / 时间 / 主线：** 论文、项目 demo；2026-08-12；世界模型、Coding × 影视与游戏。
- **官方链接：** [论文](https://arxiv.org/abs/2608.12314)、[项目页](https://yuyangyin.github.io/StateFlow/)。
- **相对上期的新变化：** 延续 PBD-AG 的持久状态思想，但从机器人记忆扩展到影视预演、镜头规划和可玩原型。
- **核心贡献：** 以对象几何、姿态、属性和相机构成持久 3D working state；用户编辑被翻译为局部状态转移，高保真视频模型只承担表现增强。
- **Coding 接口：** 可映射为版本化 scene graph、对象 CRUD、状态 diff、相机轨迹和渲染反馈循环。
- **场景：** 互动电影预演、可探索概念场景、建筑和品牌空间快速迭代。
- **成熟度 / 证据：** 可视化 demo；项目页标明代码 “soon”，无引擎插件、运行成本或用户研究。
- **重要性 / 阅读：** 高 / 必读。

### 3. RIFT：保留未来表征，删除部署时视频 rollout

- **类型 / 时间 / 主线：** 论文、闭环机器人评测；v1 为 2026-08-12，v2 为 08-13；世界模型、机器人运行时。
- **官方链接：** [论文](https://arxiv.org/abs/2608.11521)。
- **相对上期的新变化：** 上期 World Tokens 删除测试时世界分支，Flex-π允许切换流；RIFT 则保留明确的 future-position read interface，用一次前向替换迭代生成。
- **核心贡献：** anticipation tokens 在一次 backbone pass 中填充未来位置的 K/V cache；干预实验验证 action expert 确实使用未来内容及其时空位置。
- **Coding 接口：** `observation → cache prefill → action flow`；可把 cache 当作模型—控制器 ABI，并附加 shadow uncertainty monitor。
- **场景：** 低延迟机械臂、仓储双臂和高频闭环操作。
- **成熟度 / 证据：** 论文实测；LIBERO 98.8%、247.9 ms/action chunk（单 A800），较 rollout 方法降低 68.2%–89.1% 延迟；未见代码。
- **重要性 / 阅读：** 高 / 必读。

### 4. D3D-GEN：把领域知识编译成 Isaac Sim/Gazebo 世界

- **类型 / 时间 / 主线：** 论文、系统原型、IROS 2026；2026-08-12；Coding × 世界生成、机器人仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2608.11876)。
- **相对上期的新变化：** 相较 PhysX-CoT 的单资产生成，本项目覆盖领域研究、平面图、资产布置、语义区域和模拟器导出。
- **核心贡献：** agent 检索法规和领域规范，生成带来源及置信度的约束库；再编译为 scene graph、几何、碰撞约束、机器人通行区和 `world.yaml`。
- **Coding 接口：** 严格 JSON schema、RAG database、Shapely 几何校验、`WorldDescription`、`world.yaml`、Isaac Sim/Gazebo 和本地 Web 前端。
- **场景：** 医院服务机器人、办公室配送、住宅导航、合成训练环境。
- **成熟度 / 证据：** 系统论文；生成并评估 450 个世界，领域数据库生成约需 184–221 秒；无公开仓库。
- **重要性 / 阅读：** 高 / 必读。

### 5. Agentic Self-Improvement：coding agent 自动调试视频生成

- **类型 / 时间 / 主线：** 论文、agent 框架；2026-08-12；Coding × 创作工具。
- **官方链接：** [论文](https://arxiv.org/abs/2608.12290)。
- **相对上期的新变化：** 上期 VideoVIBE/DashArena 主要验收最终互动作品；本项目把评测直接写入视频生成搜索环。
- **核心贡献：** Gemini 2.5 Pro 把目标拆为 DSG 和常见错误问题，循环修订 prompt；随后由 Vizier 贝叶斯优化共同搜索 seed 与 CFG。
- **Coding 接口：** 黑盒视频 API、VQA evaluator、指标服务、优化器、预算控制和 artifact store。
- **场景：** 广告镜头、角色动作、产品展示和影视素材的自动重试与质量门控。
- **成熟度 / 证据：** 论文原型，基于 Veo 2 API；100-run 设置下部分指标的人类偏好胜率最高 69%。CMQ 判断准确率仅 82%，且调用成本未披露。
- **重要性 / 阅读：** 高 / 必读。

### 6. 驾驶世界模型反事实评测：合理未来不等于正确反事实

- **类型 / 时间 / 主线：** 论文、benchmark 方法；2026-08-12；世界模型、评测。
- **官方链接：** [论文](https://arxiv.org/abs/2608.11601)。
- **相对上期的新变化：** 从 GAUGE 的物理参数误差推进到事件级因果证据是否被保留。
- **核心贡献：** 构建 186 个 CARLA 匹配反事实案例；指出直接 action-conditioned prediction 没有使用事实续篇，无法知道该事件实际发生过。
- **Coding 接口：** 事件日志、事实轨迹、替代动作、证据 transport、生成补全和 matched replay 组成反事实测试流水线。
- **场景：** 自动驾驶事故分析、策略验证、what-if 回放和保险定责辅助。
- **成熟度 / 证据：** 论文 benchmark；短时、脚本化 CARLA 场景，无公开工具链。
- **重要性 / 阅读：** 高 / 必读。

### 7. TGRHuman：文本生成显式 3D 人体、服装几何和纹理

- **类型 / 时间 / 主线：** 论文；2026-08-12；数字人、3D 资产。
- **官方链接：** [论文](https://arxiv.org/abs/2608.12175)。
- **核心贡献：** 将几何和纹理解耦；多视角法线和 geometry carving 生成含宽松服装的形体，再由密集环视 RGB 与 diffusion renderer 合成纹理。
- **Coding 接口：** 可作为文本到 3D 角色资产服务的离线阶段，但未披露 mesh/骨骼导出规范、API 或 DCC 插件。
- **场景：** 游戏群众角色、虚拟试装底模、影视概念角色。
- **成熟度 / 证据：** 论文实测；没有代码、生成时长、拓扑编辑性、rigging 或动画结果。
- **重要性 / 阅读：** 中高 / 必读。

### 8. LoSA：按保留注意力质量阈值做训练外稀疏化

- **类型 / 时间 / 主线：** 论文、推理加速；2026-08-12；支撑基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2608.12032)。
- **核心贡献：** 不固定稀疏率，而是在一个早期 dense step 中寻找保留 99% attention mass 的最小 K/V block 集，并在后续去噪步复用。
- **Coding 接口：** 可作为 DiT attention backend，与 feature cache 组合；适合长时数字人和世界模型 worker。
- **场景：** 视频生成服务降本、更多并发、互动内容低延迟预取。
- **成熟度 / 证据：** 论文实测，无代码；Wan2.1-1.3B 为 1.36×，与缓存组合后 HunyuanVideo 达 3.2×，VBench Overall 下降 0.02。
- **重要性 / 阅读：** 中高 / 必读。

### 9. GeoFlow：以几何对齐先验缩短驾驶视频采样路径

- **类型 / 时间 / 主线：** 论文、ECCV 2026；2026-08-12；世界模型基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2608.12203)。
- **核心贡献：** 将上一帧和多视角几何变换为 Geometry-Aligned Prior，不再从每帧独立高斯噪声开始，减少重复重建静态结构。
- **Coding 接口：** 相机标定、ego trajectory、几何 warp 和空间自适应噪声可组成 simulator-to-generator condition pipeline。
- **场景：** 驾驶视频合成、闭环测试可视化、传感器数据扩充。
- **成熟度 / 证据：** 论文作者实验；摘要未披露绝对 FPS、硬件或代码。
- **重要性 / 阅读：** 中高 / 可读。

### 10. Video2Track：从公开视频生成可执行的对抗测试场景

- **类型 / 时间 / 主线：** 论文、闭环测试系统；2026-08-12；Coding × 仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2608.11592)。
- **核心贡献：** VLM 从交通视频抽取结构化语义，RAG 将其匹配至测试场拓扑；条件扩散生成多主体轨迹，Stackelberg game 调节风险和对抗强度。
- **Coding 接口：** `video → semantic schema → topology anchor → trajectory set → track controller`。
- **场景：** 自动驾驶封闭场复现、风险分级测试和监管验证。
- **成熟度 / 证据：** 作者封闭场实验；无代码、设备接口或执行成本。
- **重要性 / 阅读：** 中高 / 必读。

### 11. Seed2GS：无需原相机和逐场训练的 3DGS 对象抽取

- **类型 / 时间 / 主线：** 论文、3D 编辑组件；2026-08-12；Coding × 空间资产。
- **官方链接：** [论文](https://arxiv.org/abs/2608.11928)。
- **核心贡献：** 单参考视图确定对象身份，虚拟轨道扩展三维覆盖；场景被冻结，只学习临时 foreground logit。
- **Coding 接口：** 可成为 `select(object) → extract splats → transform/delete/replace` 编辑命令的后端。
- **场景：** 3D 场景清理、资产拆分、数字孪生和空间内容编辑。
- **成熟度 / 证据：** 论文实测，无代码；LERF-MASK 92.1% mIoU、compute-only 9.3 秒，不是逐帧实时编辑。
- **重要性 / 阅读：** 中 / 可读。

### 12. InViStream：体积视频在采集端同时清除 RGB 与深度隐私

- **类型 / 时间 / 主线：** 论文、实时系统；2026-08-12；空间计算、安全。
- **官方链接：** [论文](https://arxiv.org/abs/2608.11645)。
- **核心贡献：** 在原始 RGB-D 离开摄像端之前，跨标定视角传播 public/private 决策，并同时屏蔽颜色和深度，避免敏感几何在融合后重新出现。
- **Coding 接口：** capture-side detector、跨相机 identity propagation、depth-aware mask 和 sanitized point-cloud stream。
- **场景：** 3D 远程会议、教育、沉浸直播和空间数字人。
- **成熟度 / 证据：** 论文系统原型；真实场景 Recall 0.908、超过 30 FPS，但未公开实现及端到端网络延迟。
- **重要性 / 阅读：** 中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** Avatar-Forever 把实时、长时和高保真放入同一音频驱动模型，关键不是单纯增加上下文，而是让模型在训练时经历自身误差传播并学习恢复。它仍以音频和首帧为主要控制，尚无明确的可打断对话、视线事件或 function-calling 接口。

**3D/4D 表示。** TGRHuman提供带宽松服装的显式 3D 人体生成路径；相较视频人物，它更容易进入引擎资产管线，但没有证明生产所需的稳定拓扑、骨骼、蒙皮、LOD 和碰撞体。Seed2GS则使既有 3DGS 场景更可编辑，但不是角色生成器。

**实时交互。** Avatar-Forever 的 27.2 FPS 包括 DiT 与 VAE 解码，是可信度较高的吞吐数据；不过首帧延迟、音频缓冲长度、取消粒度和多路并发均未披露。当前只能称“实时生成研究原型”，不能等同于 RTC 数字人服务。

**音视频与情感表达。** 本批次没有新的情感标签、主动倾听或多语言 benchmark。Avatar-Forever展示说话、唱歌、情绪和身体动作，但仍属于作者定性 demo。

**工程部署。** ForeverCache 的 bounded history 和 per-chunk reset 适合流式服务；LoSA可进一步减少视频 DiT attention 成本。二者结合的产品路径清楚，但没有公开兼容实现。

**安全与身份治理。** 本周期没有新的肖像授权、水印或数字人溯源方案。InViStream解决的是空间采集隐私，而非合成身份授权；声音和肖像同意、输出水印及审计记录仍需外部系统完成。

## 5. 世界模型进展

- **架构与训练：** RIFT、StateFlow、D3D-GEN分别把未来、世界和领域规则表示成 K/V cache、持久 3D state 和约束数据库，继续削弱 RGB 视频作为唯一中间表示的地位。
- **可控/可玩生成：** StateFlow支持对象和相机的局部修改；D3D-GEN生成可碰撞、可导航场景；本周期没有新的通用实时 playable video 模型。
- **长时与物理一致性：** Avatar-Forever处理生成误差累积，StateFlow通过不重生成权威状态避免布局漂移。两者解决的是不同层面：前者稳定表现，后者稳定事实。
- **空间表示：** 对象中心 3D state、scene graph、`world.yaml`、3DGS foreground logit 和 HD map 正成为可查询、可编辑的运行时协议。
- **机器人/仿真：** RIFT降低控制延迟；D3D-GEN降低环境制作成本；Video2Track把公开视频变成封闭场测试。三者分别覆盖 policy、world 和 evaluation。
- **评测：** 反事实驾驶研究证明 world model 必须区分“生成一个可能发生的未来”和“回答这个已观察事件在另一动作下如何演化”。
- **补充观察：** Better Slots 的受控实验发现，世界模型规划成功率随 slot 质量提升但最终饱和，冻结预训练特征可能同样是 OOD 鲁棒性的主要来源，因此“对象中心”本身不能自动解释全部收益。[论文](https://arxiv.org/abs/2608.12078)

## 6. Coding × 新型互动作品

### 链路一：持续在线的数字角色

**业务 agent/LLM → 文本与语音流 → Avatar-Forever chunk worker → RTC 播放器 → 数字员工或虚拟主播。**

代码负责工具调用、事实核验、会话状态、音频切片、取消和降级；模型负责音频对应的表情、口型、身体动作及画面；RTC 保证时间戳和播放。已有实时吞吐及长时 demo，缺公开 API、插话和并发证据。

### 链路二：可编辑而非一次性生成的互动电影

**剧本 agent → 持久 scene graph/3D state → StateFlow 局部转移与相机规划 → 视频模型增强 → 互动预演。**

代码维护角色 ID、对象位置、剧情分支和镜头版本；生成模型负责局部资产及最终画面；传统引擎负责确定性空间关系和交互。已有项目 demo，代码与引擎插件待开放。

### 链路三：自然语言生成机器人训练世界

**领域描述 → grounded research agent → typed constraints → scene graph → `world.yaml` → Isaac Sim/Gazebo。**

LLM负责提取领域知识和提出布局；代码负责 schema 校验、几何、碰撞、可通行性和导出；模拟器负责物理与传感器。D3D-GEN已生成 450 个世界，是本批次最接近可执行软件工程管线的成果，但仓库未开放。

### 链路四：具有未来意识但不生成未来视频的机器人

**相机观测 → RIFT anticipation tokens → future K/V cache → action flow → 控制器。**

模型负责压缩未来分布；代码负责 cache 生命周期、action chunk、置信监控和安全边界；传统控制器负责执行、限速与急停。闭环 benchmark 证据充分，实机和开源证据不足。

### 链路五：自动调试生成内容的创作 agent

**创作 brief → 视频 API → DSG/CMQ 自动测试 → prompt 修订 → seed/CFG 贝叶斯搜索 → 最优镜头。**

代码负责预算、异步任务、指标和版本；视频模型负责候选内容；mLLM 负责语义和缺陷检查。已有 Veo 2 实验，但 10–100 次调用的成本与时延使其更适合高价值镜头，而非实时互动。

### 链路六：从用户视频到可控工业测试

**道路视频 → VLM 语义抽取 → 地图拓扑检索 → 多主体轨迹生成 → 封闭场车辆执行。**

模型提取情境并生成轨迹变化；代码完成坐标映射、风险参数、控制器接口和数据记录；真实测试场提供确定性执行。Video2Track已有作者实验，工具链未开放。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | StateFlow＋scene graph＋传统引擎 | 对象编辑、相机切换、可探索状态 | 项目 demo | 未披露 | 代码、物理、多人同步 | [项目页](https://yuyangyin.github.io/StateFlow/) |
| 影视/虚拟制作 | StateFlow＋agentic I2V | 镜头版本、局部重生成、自动挑选 | 论文/demo | I2V 最多测试 100 次调用 | API 成本、精修与版权 | [I2V 论文](https://arxiv.org/abs/2608.12290) |
| 直播与电商 | Avatar-Forever＋RTC | 连续语音驱动人物表演 | 作者 demo | 27.2 FPS，单 H100 | 首帧、插话、并发、审核 | [项目页](https://leeruibin.github.io/avatarforever-project-page/) |
| 品牌互动 | TGRHuman＋角色资产管线 | 文本定制角色及空间展示 | 论文原型 | 未披露 | rigging、品牌一致性、肖像权 | [论文](https://arxiv.org/abs/2608.12175) |
| 教育培训 | Avatar-Forever＋StateFlow | 数字讲师与可编辑空间演示 | 机会判断 | 未披露 | 事实性、课程控制、成本 | [Avatar-Forever](https://arxiv.org/abs/2608.12107) |
| 企业数字员工 | Avatar-Forever＋业务 agent | 工具结果转化为持续可见回复 | 研究原型 | 单 H100 27.2 FPS | SLA、取消、权限和审计 | [论文](https://arxiv.org/abs/2608.12107) |
| 陪伴与社交 | Avatar-Forever＋长期记忆 | 长时语音和视觉反馈 | 作者 demo | 11 分钟连续样例；并发未披露 | 心理安全、记忆隐私 | [项目页](https://leeruibin.github.io/avatarforever-project-page/) |
| 空间计算 | InViStream＋RGB-D 融合 | 多视角远程临场、隐私过滤 | 系统原型 | 超过 30 FPS | 误删、跨视角身份、网络成本 | [论文](https://arxiv.org/abs/2608.11645) |
| 机器人/仿真 | D3D-GEN＋RIFT＋Isaac/Gazebo | 生成世界、闭环动作、结果回放 | 论文原型 | RIFT 247.9 ms/chunk；D3D 数据库 184–221 秒 | 代码、实机、安全认证 | [D3D-GEN](https://arxiv.org/abs/2608.11876)、[RIFT](https://arxiv.org/abs/2608.11521) |
| 自动驾驶 | Video2Track＋CARLA/封闭场 | 反事实、风险调节和可重复测试 | 作者测试 | 未披露 | 场景真实性、坐标误差、责任认定 | [Video2Track](https://arxiv.org/abs/2608.11592) |

## 8. 可复现资源与开发者入口

| 资源 | 开放状态 | 许可证/要求 | 是否值得复现与最小路径 |
|---|---|---|---|
| [Avatar-Forever GitHub](https://github.com/leeruibin/avatarforever) | 仅 README 和素材；推理代码、checkpoint、demo 均待发布 | 代码许可证未披露；目标硬件为单 H100 | 暂不可复现。先实现音频 chunk、首帧锚点、cache reset 和 RTC 缓冲协议 |
| [StateFlow 项目页](https://yuyangyin.github.io/StateFlow/) | 视频与交互案例；代码标记 “soon” | 未披露 | 可先复刻对象中心 state schema、局部 diff 和相机可行性检查 |
| [D3D-GEN 论文](https://arxiv.org/abs/2608.11876) | 无公开代码 | 未披露；依赖 LLM/search、Shapely、3D 资产库、Isaac Sim/Gazebo | 很值得做小型复刻：限定一个办公室域，生成约束 JSON、scene graph 和单房间 `world.yaml` |
| [RIFT 论文](https://arxiv.org/abs/2608.11521) | 无代码/权重 | 单 A800 评测 | 等待开放；当前可在小型 WAM 中比较 rollout cache 与一次 prefill cache |
| [Agentic I2V](https://arxiv.org/abs/2608.12290) | 无代码；使用商业 Veo、Gemini、Vizier | API 成本未披露 | 可用任意 I2V API＋VLM＋Optuna 实现 5–10 次低预算版本 |
| [Seed2GS](https://arxiv.org/abs/2608.11928) | 无代码 | 依赖预建 3DGS 与分割/跟踪模型 | 等待实现；先验证单视图 mask 经虚拟相机轨道传播的对象覆盖率 |
| [反事实驾驶 benchmark](https://arxiv.org/abs/2608.11601) | 方法和数据构建描述，未见下载入口 | CARLA；186 个案例 | 可先构建单个加速/刹车 matched replay，验证“事实续篇证据”是否保留 |
| [LoSA](https://arxiv.org/abs/2608.12032) | 无实现 | 需修改 DiT attention kernel | 值得底层团队复刻：固定一个 early step 统计 99% attention mass，再与 feature cache 组合 |

本周期已报道的 Wan-Animate-2、FACT 和 UA-NWM 仍是当前窗口内开放度最高的资源，但截至本次检索未发现新的实质更新，因此不重复计为今日新增。

## 9. 系统架构与技术趋势判断

明显升温的方向：

1. **状态与表现分离。** StateFlow、D3D-GEN和上期 PBD-AG共同指向：世界事实应由结构化状态维护，视频模型负责表现，而非让每一帧重新决定世界是什么。
2. **未来表征接口化。** RIFT证明未来可以是 attention cache，而不是必须展示给人的视频；这会推动 world model 成为控制系统内部的低延迟服务。
3. **生成流程软件化。** Prompt、seed、CFG、评测问题、预算和最佳候选开始由 agent 自动管理，生成内容逐渐具有测试和优化循环。
4. **缓存成为实时生成的核心组件。** ForeverCache、LoSA以及此前 Wan-Animate-2 的缓存节点说明，高性能运行时的竞争已进入 token、block 和 chunk 生命周期管理。
5. **仿真世界从视觉资产转向约束编译。** D3D-GEN和Video2Track强调法规、拓扑、风险及执行器，而不仅是场景是否逼真。

正在形成的复用架构是：

`用户/业务事件 → agent 生成结构化计划 → 权威世界状态或未来 cache → 生成模型负责外观与概率性动态 → 引擎/控制器执行规则与物理 → 自动评测、回放和审计`

尚未解决的问题包括：数字人首帧和打断延迟、多会话并发、长时事实记忆与视频表现同步、生成 3D 人体的 rigging、反事实中的真实因果干扰、世界模型置信度校准、引擎级资产许可证及开放实现。

需要降级看待的表述包括：“无限生成”目前只由一个超过 11 分钟的样例支持；“实时”不代表消费者硬件可用；“simulation-ready”不等于通过实机 sim-to-real；项目页中的可玩原型也不等于生产级多人游戏。

## 10. 论文精读候选

1. **[Avatar-Forever](https://arxiv.org/abs/2608.12107)**  
   值得读：把效率训练和长时鲁棒性训练拆开，部署时再组合。重点看 §3.2 RRT、§3.3 ForeverCache、§5 的 30 秒和 11 分钟评测。复现风险是 22B 模型、合成数据和全参数 DMD 成本。

2. **[RIFT](https://arxiv.org/abs/2608.11521)**  
   值得读：用因果干预证明 action expert 真正消费什么，再据此重写推理架构。重点看 §3 cache intervention、§4 anticipation token、§5 延迟统计。风险是结果集中于特定 WAM 家族和模拟 benchmark。

3. **[StateFlow](https://arxiv.org/abs/2608.12314)**  
   值得读：把生成式预演重新定义为 persistent state authoring。重点看 world construction、state evolution 和 render-feedback camera planning。风险是代码未开放，且最终高保真仍依赖外部视频模型。

4. **[D3D-GEN](https://arxiv.org/abs/2608.11876)**  
   值得读：完整展示自然语言、检索、严格 schema、几何校验与模拟器导出的连接。重点看 §III-A 约束数据库和 §III-B 编译管线。风险是领域知识正确性和资产数据库许可。

5. **[How Can Driving World Models Do Counterfactual Prediction?](https://arxiv.org/abs/2608.11601)**  
   值得读：清晰区分预测与反事实。重点看因果定义、186-case benchmark 和 evidence transport。风险是周围车辆被假设不受替代 ego action 影响，离复杂真实交通仍远。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. Avatar-Forever 是否发布推理代码、权重、chunk 大小、首帧延迟和显存数据。
2. StateFlow 的 3D 状态格式能否导出 Unity、Unreal、USD 或 glTF。
3. RIFT 是否开放 LIBERO/RoboTwin checkpoint，并验证实机控制延迟。
4. D3D-GEN 是否发布 `world.yaml` schema、资产处理脚本和 Isaac/Gazebo 示例。
5. Agentic I2V 在固定美元预算下是否仍优于 best-of-N；VLM evaluator 偏差如何校准。
6. 反事实 world model 能否处理替代动作会改变其他主体行为的长时场景。
7. TGRHuman是否支持可动画拓扑、骨骼、服装分层和商用资产导出。
8. LoSA 与 ForeverCache、量化及 tensor parallel 组合时是否仍保持近无损质量。

### 本周可动手验证的小实验

1. **低预算 agentic 视频调试器**  
   目标：用 5–10 次生成替代人工盲调。组件：任意 I2V API、VLM、结构化 DSG/CMQ、Optuna。难点：评分噪声和 API 成本。成功判据：固定预算下，人类偏好显著高于随机 seed。

2. **持久世界状态最小原型**  
   目标：验证局部编辑是否避免场景漂移。组件：JSON scene graph、Three.js、对象 CRUD、相机轨迹和一个 I2V 模型。难点：3D 状态到高保真视频的条件映射。成功判据：连续五次局部修改后，未修改对象的位置和身份保持不变。

3. **Prompt-to-Gazebo 单房间编译器**  
   目标：复刻 D3D-GEN 最小链路。组件：领域约束 JSON、资产元数据、Shapely、SDF/URDF 或 `world.yaml`。难点：门宽、碰撞、通行区域和资产尺度。成功判据：自动生成的场景可加载，机器人能从入口无碰撞导航至目标点。

4. **未来 cache 替代 rollout 的消融实验**  
   目标：验证控制器需要的是未来表示还是生成过程。组件：小型 action-conditioned model、K/V hook、三种模式——完整 rollout、固定最终 cache、一次 anticipation prefill。难点：保持随机种子和初始状态严格一致。成功判据：一次 prefill 的任务成功率接近 rollout，同时显著降低 action-chunk 延迟。
