# 数字人、世界模型与互动作品研究日报：2026-09-28

检索窗口：2026-09-22 至 2026-09-28。  
信息截点：2026-09-28 10:02（Asia/Shanghai）。已与 9 月 22—24 日日报去重；“相对上期”指相对 2026-09-24 日报。发布日期以 arXiv 提交记录、GitHub 合并记录及软件包元数据为准。

## 1. 本日摘要

本周期最重要的变化不是数字人画质再次跃迁，而是数字人服务开始披露真正的生产调度问题：DHSched 报告近 5 万路并发会话，并解决 GPU 会话迁移时多个旧、新 Worker 同时推进同一角色状态的问题，这是少见的规模化部署证据，但所有生产数据仍来自作者自报。

世界模型侧出现了清晰的“代码管规则、模型管视觉”收敛。GameDirector 用显式生命值、技能和终止规则约束生成式游戏；CoDeR 更进一步，让 coding agent 生成 Three.js 白盒世界，再由视频模型负责视觉实现。两者都说明，持续运行的生成世界不能仅靠像素历史保存权威状态。

WROP 将对象永久性和实体性拆成 150 个可程序化 Blender 任务，并一次性开放 150 万样本、固定考试、模型权重及训练栈，使“遮挡后物体是否仍存在”第一次成为可训练、可回归的工程指标。

实时运行时方面，vLLM-Omni 在 9 月 24—25 日补齐 LingBot World 的 VAE、FP8、KV gather 优化与实测数据：两张 H200 已达到低于 1 秒的平均 chunk cadence，但仍有播放缓冲耗尽，说明“平均实时”还不等于稳定实时。

数字人开发入口出现一个较小但值得跟踪的产品信号：Realtime Avatar 同期更新 TypeScript SDK 和 MCP，让 coding agent 能读取真实 avatar ID、余额和会话，而不是根据文档猜测接口；其工程接口完整，但用户规模、视觉质量和 SLA 缺乏独立验证。

数字人研究本身以垂直场景为主。PHOSA 聚焦手语中最难的手指、面部和全身联合表达；ARS-Avatar 聚焦可动画、可重打光的高保真表面表示。两者都还不是实时对话系统。

工业世界模型则继续向多传感器和低成本部署推进：HelloWorld 联合生成七路相机和条件 LiDAR，QuantWM 将因果视频模型的 KV cache 压至 2-bit，同时专门约束注意力与时间一致性。

总体上，本周期最可信的产业增量来自数字人生产调度和世界模型 serving，而不是新的通用可玩世界产品；代码化状态、确定性验证、结构化接口和可观测性正在成为生成式互动作品的核心骨架。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | DHSched 披露近 5 万并发生产调度；PHOSA 改善手语手部与面部表达 | 从单会话 SDK 推进到集群级会话迁移；生成研究仍以原型为主 | 新增生产调度证据、手语 avatar 与可重打光 surfel avatar |
| 世界模型 | 高 | 显式代码世界、对象永久性、多传感器生成和 2-bit KV 同时推进 | 评测与运行时明显增强；通用游戏产品仍未成熟 | 新增 GameDirector、CoDeR、WROP、HelloWorld、QuantWM |
| Coding × 互动作品 | 高 | 代码开始持有规则、状态、权限和资产接口，视频模型退居感知渲染层 | 从“prompt 控制视频”推进到 agent 生成并维护可执行世界 | 新增 Three.js 白盒世界、MCP 数字人入口、游戏状态 Director |
| 实时运行时 | 高 | 世界模型 serving 开始报告卡数、chunk cadence、首 chunk 和 underrun | 两张 H200 达到平均目标，但稳定性仍未过关 | vLLM-Omni 9 月 24—25 日出现实质更新 |
| 安全与治理 | 中 | 会话所有权 fencing 和 MCP 写权限默认关闭形成具体安全机制 | 运行安全有所提升；肖像授权、水印无同等级新增 | 新增执行权隔离与付费写操作门禁 |

## 3. 最值得关注的 10 个进展

### 1. DHSched：近 5 万并发数字人会话的生产调度

- **类型 / 日期 / 主线：**论文、生产系统；2026-09-22；数字人、实时运行时。
- **官方链接：**[论文与完整系统说明](https://arxiv.org/abs/2609.26363)。
- **相对上期：**首次收录；此前日报覆盖单会话数字人和 RTC SDK，本项首次提供大规模 GPU 会话迁移数据。
- **核心贡献：**以 `(WorkerID, epoch)` 标识会话执行代际，Dispatch 只负责条件提交，Infer-Controller 在接收会改变状态的输入前重新验证代际，从而阻止仍存活的旧 Worker 继续输出。
- **Coding 接口：**对应 `placement store → generation commit → worker claim → input fence → state reconstruction` 控制面；可复用于直播角色、多人 NPC shard 和长会话 agent。
- **规模与性能：**作者报告生产栈达到 49,987 路并发；单日完成 9,927 次迁移且未观察到双 owner；峰值 CreateLive P99 98.3 ms、PushText P99 16.5 ms。
- **场景：**数字员工、直播角色、客服、陪伴应用的弹性扩缩容和故障迁移。
- **成熟度 / 证据：**规模化部署；论文中的生产观测，尚无第三方审计或开源实现。
- **重要性 / 阅读：**高 / 必读。

### 2. GameDirector：游戏规则与生成渲染正式解耦

- **类型 / 日期 / 主线：**论文、交互 demo；2026-09-22；世界模型、Coding × 游戏。
- **官方链接：**[论文](https://arxiv.org/abs/2609.25652)、[项目页](https://jimntu.github.io/gamedirector/)。
- **相对上期：**补录；未出现在 9 月 22—24 日日报。
- **核心贡献：**Director 从视频观察中更新生命值、攻击力、技能槽和结束条件，控制 NPC 战术，再把决策翻译为提示词交给视频世界模型渲染。项目页报告玩法机制正确率 99.6%，端到端基线仅约 21%—34%，Boss 决策质量提升超过 39.9%。
- **Coding 接口：**玩家配置、显式状态表、规则函数、NPC planner、prompt compiler 和视频生成器。
- **场景：**玩家自定义 Boss、互动电影、直播观众改规则、品牌小游戏。
- **成熟度 / 证据：**研究 demo；论文实测，无公开生产运行时或完整代码。
- **重要性 / 阅读：**高 / 必读。

### 3. CoDeR：coding agent 成为世界的“状态与规则大脑”

- **类型 / 日期 / 主线：**论文、项目 demo；2026-09-22；世界模型、Coding × 互动作品。
- **官方链接：**[论文](https://arxiv.org/abs/2609.26458)、[项目页](https://becauseimbatman0.github.io/CoDeR)。
- **相对上期：**首次收录；与此前 Code World Model 方向相似，但新增多角色 agent 分工和显式逻辑空间组合。
- **核心贡献：**Creator 将概念拆成带接口契约的 Logical Spaces；多个 Executor 生成 Three.js 模块，白盒场景维护实体、规则和隐藏状态；Artist 视频模型负责画面；Traveler 决定可见与可交互区域。
- **Coding 接口：**模块依赖图、Three.js、实体状态、行为脚本、深度/Canny 视觉代理和视频 renderer。
- **场景：**开放式故事世界、持续演化的多人展览、AI 游戏导演、生成式数字孪生。
- **成熟度 / 证据：**可展示研究原型；论文实测。代码和权重仅承诺未来开放。
- **重要性 / 阅读：**高 / 必读。

### 4. WROP：把对象永久性变成可训练、可回归的世界模型能力

- **类型 / 日期 / 主线：**论文、dataset、benchmark、GitHub、权重；2026-09-23；世界模型评测。
- **官方链接：**[论文](https://arxiv.org/abs/2609.28654)、[代码](https://github.com/hokindeng/object-permanence)、[项目与 leaderboard](https://object-permanence.world/)、[150 万样本数据](https://huggingface.co/datasets/Hokin/object-permanence)。
- **相对上期：**首次收录。
- **核心贡献：**150 个 Blender 生成器覆盖遮挡、容器、障碍、支撑和碰撞等六类认知任务；固定考试含 300 个问题。作者评测 14 个视频模型，PWM-WROP 在 continuation 模型中排名第一。
- **Coding 接口：**CLI 可生成指定任务与随机种子；每个样本带输入视频、目标视频、prompt、逐帧物体轨迹和 provenance metadata，可直接进入训练及 CI。
- **场景：**可玩视频回归测试、NPC 视野外状态验证、机器人合成数据和物理错误定位。
- **成熟度 / 证据：**已开源；论文、人类盲评和公开资源。数据 214 GB，CC BY-NC 4.0。
- **重要性 / 阅读：**高 / 必读。

### 5. vLLM-Omni 世界模型运行时：平均实时已达标，稳定实时仍未完成

- **类型 / 日期 / 主线：**GitHub、推理运行时、benchmark；主要更新 2026-09-24—25；世界模型基础设施。
- **官方链接：**[实时推理路线图与逐 PR 状态](https://github.com/vllm-project/vllm-omni/issues/7074)。
- **相对上期：**实质更新：VAE decode、FP8 输入量化和默认 KV gather 于 9 月 24 日合并，9 月 25 日公开新的双卡及四卡实测。
- **核心贡献：**目标配置为 480×832、四步 DMD、每 chunk 12 帧。双 H200 测得约 892—896 ms 平均 cadence，低于 1 秒目标；四卡最新对照约 549 ms，仍未达到 500 ms 目标。
- **Coding 接口：**stepwise session、camera interaction、流式 chunk、bounded KV、Ulysses 并行和实时 benchmark。
- **工程边界：**双卡测试仍有 9—11 次 one-chunk-buffer underrun；多会话负载工具和 ComfyUI backpressure 工作流仍为 draft。
- **成熟度 / 证据：**开源运行时、可测工程路径；官方基准，无独立压力测试。
- **重要性 / 阅读：**高 / 必读。

### 6. Realtime Avatar SDK 与 MCP：coding agent 可查询并配置真实数字人资源

- **类型 / 日期 / 主线：**SDK/API、MCP、产品；SDK 0.23.0 于约 2026-09-24 发布，MCP 0.17.1 于约 9 月 27 发布；数字人、Coding。
- **官方链接：**[GitHub](https://github.com/theinfluencecompany/realtime-avatar-sdk)、[MCP 文档](https://realtimeavatar.ai/docs/mcp)、[API reference](https://realtimeavatar.ai/docs/api-reference)、[npm SDK](https://www.npmjs.com/package/realtime-avatar)。
- **相对上期：**新增；不同于上期 LiveKit–Synthesia 插件，本项将 avatar 账户、资产、费用和会话直接暴露给 coding agent。
- **核心贡献：**TypeScript SDK 覆盖 Next.js、Express、Hono、React 与 React Native；MCP 默认只读，可查询 avatar、余额、clip 和账单，写操作需显式开启。
- **Coding 接口：**`list_avatars`、`get_avatar`、`credit_balance`、`start_call`、图片/视频创建角色、clip library 和浏览器内工具调用。
- **场景：**coding agent 自动搭建数字人页面、实时教学角色、带业务工具的虚拟客服。
- **成熟度 / 证据：**正式 API/SDK、MIT 客户端；产品方文档。项目较新，视觉质量、并发和 SLA 尚无独立验证。
- **重要性 / 阅读：**中高 / 必读。

### 7. HelloWorld：七相机 RGB 与 LiDAR 的驾驶世界模型

- **类型 / 日期 / 主线：**论文、模型系统；2026-09-24；世界模型、自动驾驶。
- **官方链接：**[论文](https://arxiv.org/abs/2609.28931)。
- **相对上期：**首次收录。
- **核心贡献：**2B 模型以 ego pose、HD map 和 3D boxes 为控制条件，通过 block-causal 接口生成连续世界；支持七路同步相机和条件 LiDAR，并以 few-step distillation 降低推理成本。
- **Coding 接口：**结构化 ego pose、地图、3D box 序列和多传感器输出，适合接仿真编排、场景数据库和自动评测器。
- **场景：**自动驾驶反事实数据、传感器补全、封闭场景回放和策略回归测试。
- **成熟度 / 证据：**研究原型；论文实测。未核验到公开代码、权重或生产部署。
- **重要性 / 阅读：**高 / 必读。

### 8. PHOSA 与 MVSign：面向手语沟通的高保真 3D Gaussian Avatar

- **类型 / 日期 / 主线：**论文、benchmark、dataset 方法；2026-09-24；数字人。
- **官方链接：**[论文](https://arxiv.org/abs/2609.29292)、[项目页](https://naaapi.github.io/PHOSA/)。
- **相对上期：**新增；相较上期 SignGPT 的语言—动作生成，本项解决动作落到高保真人物后的手部和面部质量。
- **核心贡献：**MVSign 使用 16 路同步相机，覆盖五位手语者约 23,000 帧；混合 SMPL-X fitting 联合身体、MANO 手部和面部估计；渲染侧将 body、head、hands 分开建模。
- **关键证据：**20 位聋人参与者的作者用户研究中，PHOSA 在可理解性选择上获得 56.8%，对比三种基线最高为 20.8%；82.5% 更偏好其可理解性而非原始 SMPL-X。
- **Coding 接口：**SMPL-X 参数序列可连接手语生成器，Gaussian renderer 负责人物外观。
- **成熟度 / 证据：**研究原型、论文实测；论文明确承认三套 StyleUNet 限制实时性，代码及数据开放状态待核验。
- **重要性 / 阅读：**中高 / 必读。

### 9. QuantWM：面向连续视频的 2-bit KV cache

- **类型 / 日期 / 主线：**论文、推理加速；2026-09-22；世界模型基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2609.26425)。
- **相对上期：**首次收录。
- **核心贡献：**作者发现普通 2-bit KV 在 VBench 上可能看似无损，却产生严重闪烁。QSAC 根据历史 Query 敏感度选择 Key 聚类中心，PSAC 用低秩投影补偿关键注意力方向。
- **Coding 接口：**严格因果、免训练，可作为流式 AR-diffusion KV backend；已在 LingBot-World-v2、HY-World、Matrix-Game 和 LongCat-Video 等模型上评估。
- **关键结果：**作者报告 KV cache 最高压缩 6.20 倍且额外开销有限。
- **成熟度 / 证据：**论文实测；尚未核验到公开实现或独立复现。
- **重要性 / 阅读：**中高 / 可读。

### 10. ARS-Avatar：可动画、可重打光的 surfel 数字人

- **类型 / 日期 / 主线：**论文、3D avatar；2026-09-23；数字人、虚拟制作。
- **官方链接：**[论文](https://arxiv.org/abs/2609.27600)。
- **相对上期：**首次收录；相较上期 SAMIRA 的面部语义分区，本项面向全身、多视图和未知拍摄光照。
- **核心贡献：**用模板 mesh 形变先验约束 surfel，采用 deferred shading 估计 BRDF，并以可微 screen-space ambient occlusion 学习不同身体部位的遮蔽半径。
- **Coding 接口：**姿态驱动参数、材质、光照和 surfel 属性可分别进入 DCC 或实时渲染管线。
- **场景：**虚拟制作、服装展示、游戏角色光照适配和数字展演。
- **成熟度 / 证据：**研究原型、论文实测；未见代码、实时帧率或商用资产导出证据。
- **重要性 / 阅读：**中 / 可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**本周期没有超过 Muse、AVTR-1 的通用实时对话模型。高质量新增转向手语垂直场景：PHOSA 将身体、脸和手分开建模，解决一般 avatar 系统最容易模糊的手指细节。
- **3D/4D 表示：**PHOSA 使用 SMPL-X 驱动的分区 Gaussian；ARS-Avatar 使用可形变 surfel、BRDF 和可微环境遮蔽。二者都比纯神经视频更适合重定向、重打光和引擎合成，但资产建立依赖多视图采集或离线优化。
- **实时交互：**研究模型没有新的可靠实时数字。可进入产品的增量来自 Realtime Avatar 的 WebRTC/SDK/MCP，以及 DHSched 的集群控制面。
- **音视频与情感表达：**暂无新的高质量情绪、视线、语音韵律与全身手势联合控制模型。PHOSA 的面部和手部清晰度不等同于语音驱动或情感生成。
- **工程部署：**DHSched 说明数字人集群的难点已包括 owner fencing、故障迁移和状态恢复；Realtime Avatar 则把会话、工具、角色资产和付费权限变成普通 Web 开发接口。
- **安全与身份治理：**本周期没有新的肖像授权、撤回、水印或深伪检测进展。较有价值的安全增量是 MCP 写操作默认关闭，以及会话迁移中的单 owner 执行保证。
- **产品判断：**DHSched 所在生产栈和 Realtime Avatar API 已进入产品层；PHOSA、ARS-Avatar 仍是离线研究管线。

## 5. 世界模型进展

- **架构与训练：**CoDeR、GameDirector 把状态和规则移出视频模型；WROP 用程序生成认知任务反向训练视频模型；HelloWorld 用 block-causal 训练适应自生成历史。
- **可控 / 可玩生成：**控制信号正在从键盘和文本扩展为生命值、技能、实体关系、Three.js 模块、地图、ego pose 和 3D boxes。可控性开始具有软件接口，而不只是 prompt。
- **长时与物理一致性：**WROP专门处理遮挡后的对象存在、数量和位置；CoDeR 让离开视野的状态继续在代码世界中演化。两者都比扩大视频上下文更直接。
- **空间表示：**CoDeR 使用白盒 3D 场景和深度/Canny 代理；HelloWorld 使用七相机与 LiDAR；AR world model 仍需处理视角间状态一致性。
- **机器人 / 游戏 / 仿真：**游戏方向由 GameDirector、CoDeR 主导；自动驾驶由 HelloWorld 代表。本周期机器人方向没有超过上期 ME-U0、OpenDM 的新开放栈。
- **实时部署：**vLLM-Omni 与 QuantWM 分别从 kernel/runtime 和 KV memory 两侧降低成本；平均 cadence 已接近可交互，但 p95、buffer underrun 和多人并发仍是实际门槛。
- **评测：**WROP 表明画质评测无法发现对象凭空消失；未来 benchmark 必须验证状态、规则、条件服从和重观察一致性。

## 6. Coding × 新型互动作品

### 链路一：代码持有规则的生成式游戏

**玩家配置 → GameDirector 状态表与规则函数 → NPC planner → prompt compiler → 视频世界模型 → 游戏画面**

代码维护生命值、技能冷却、胜负和不变量；agent 负责战术和剧情；视频模型负责视觉。可形成玩家现场修改 Boss 规则的生成式游戏。GameDirector 已有 demo 和实验，但尚非完整游戏服务器。

### 链路二：coding agent 创建持续演化的世界

**自然语言概念 → Creator 拆分 Logical Spaces → Executor 生成 Three.js → 白盒世界持续运行 → Artist 生成视觉**

代码保存实体身份、物品归属、空间和离屏事件，生成模型只将确定状态转为高保真观察。可用于持续数小时的互动电影、展览和多人故事世界。CoDeR 有论文 demo，代码尚未开放。

### 链路三：数字人应用由 coding agent 直接搭建

**coding agent → Realtime Avatar MCP 查询账户 → 生成 Next.js/React 服务端路由 → API mint session → WebRTC 数字人**

MCP 提供真实 avatar ID、余额和素材状态；应用代码持有 API key、授权和工具；数字人服务负责语音、角色渲染与媒体传输。SDK 和 MCP 已可运行，但供应商规模和 SLA 待验证。

### 链路四：可迁移的数字人集群

**会话事件 → Dispatch 提交新 generation → Worker claim → fencing 校验 → RTC/生成状态恢复**

DHSched 使负载均衡器可以迁移长会话，同时避免两个角色实例对同一用户同时说话。适合大规模客服和虚拟主播，已有生产观测；实现细节未开源。

### 链路五：世界模型 CI 与认知回归测试

**Blender 任务 DSL → WROP 合成样本 → 模型 continuation → 轨迹/人类评测 → CI 阻断**

生成模型负责预测被遮挡后的场景；程序生成器和轨迹真值负责验收。可在发布新量化、蒸馏或模型版本前检测“物体消失、穿透、数量变化”等回归，已有完整资源。

### 链路六：实时可玩世界 serving

**键盘/相机事件 → vLLM-Omni stepwise session → AR-diffusion → 流式 VAE → 带 backpressure 的播放器**

代码负责输入事件、chunk 排队、session affinity、超时和缓存；模型负责视觉预测。当前双 H200 平均 cadence 达标，但 underrun 说明前端仍必须有自适应缓冲与降质策略。

### 链路七：手语数字员工

**文本/语音 → 手语动作生成器 → SMPL-X 序列 → PHOSA renderer → RTC/网页**

语言模型和规则词典负责语义，动作模型负责手语序列，PHOSA 负责高保真人物。PHOSA 已验证渲染与用户偏好，但完整实时链路属于**本报告推断**，且当前 renderer 速度不足。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展与所需技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | GameDirector、CoDeR、视频世界模型、规则服务 | 玩家配置规则，agent 控 NPC，模型渲染 | 研究 demo | 未披露 | 延迟、状态识别误差、多人同步 | [GameDirector](https://jimntu.github.io/gamedirector/) |
| 影视/虚拟制作 | ARS-Avatar、DCC、PBR renderer | 姿态驱动、动态重打光 | 研究原型 | 未披露 | 多视图采集、资产导出、实时性 | [论文](https://arxiv.org/abs/2609.27600) |
| 直播与电商 | Realtime Avatar SDK、WebRTC、业务工具 | 可打断对话、工具调用 | 正式 API，规模待验证 | 厂商报价与 SLA 需单独核验 | 形象授权、供应商锁定、审核 | [文档](https://realtimeavatar.ai/docs/) |
| 品牌互动 | CoDeR、Three.js、视频 renderer | 观众改变世界规则和实体 | 研究 demo | 未披露 | 生成延迟、资产与品牌一致性 | [论文](https://arxiv.org/abs/2609.26458) |
| 教育培训 | PHOSA、手语动作生成、RTC | 文本或课程内容转手语角色 | 组件研究原型 | 未披露 | 翻译准确性、非实时渲染 | [PHOSA](https://arxiv.org/abs/2609.29292) |
| 企业数字员工 | DHSched、avatar worker、RTC、业务 API | 长会话、迁移、故障恢复 | 作者报告规模化部署 | CreateLive P99 98.3 ms；PushText P99 16.5 ms | 未披露端到端视听延迟和成本 | [DHSched](https://arxiv.org/abs/2609.26363) |
| 陪伴与社交 | Realtime Avatar、长期记忆、内容安全 | 全双工对话、工具和摄像头观察 | 正式 API | 未披露 | 情绪安全、隐私、长期状态 | [SDK](https://github.com/theinfluencecompany/realtime-avatar-sdk) |
| 空间计算 | CoDeR、Three.js、WebXR | 代码世界与生成视觉结合 | 研究 demo | 未披露 | 视角延迟、几何漂移、设备算力 | [项目](https://becauseimbatman0.github.io/CoDeR) |
| 自动驾驶/仿真 | HelloWorld、七相机、LiDAR、地图与 3D boxes | 结构化条件生成反事实场景 | 研究原型 | few-step，具体时延未披露 | 传感器一致性、闭环车辆动力学 | [论文](https://arxiv.org/abs/2609.28931) |

## 8. 可复现资源与开发者入口

| 资源 | 许可证 / 开放情况 | 硬件与成本 | 最小验证路径 | 建议 |
|---|---|---|---|---|
| [WROP 代码](https://github.com/hokindeng/object-permanence) | 数据生成器 CC BY-NC 4.0；模型栈另受 Cosmos 条款约束 | 小规模 Blender 可在桌面运行；完整训练需 trn2.48xlarge、64 NeuronCore | 选择一个“遮挡后再出现”任务生成 20 个样本，用现有 V2V 模型续写并比较轨迹 | **优先复现** |
| [WROP 数据](https://huggingface.co/datasets/Hokin/object-permanence) | 214 GB、150 万样本、非商业 | 下载与存储成本较高 | 先取单个任务分片，不必下载全集 | 适合评测团队 |
| [vLLM-Omni 路线图](https://github.com/vllm-project/vllm-omni/issues/7074) | 开源；具体依赖按仓库许可证 | 实时目标需 2—4 张 H200 | 固定 40 chunks，记录首 chunk、均值、p95 和 underrun，不能只报 FPS | **优先复现** |
| [Realtime Avatar SDK](https://github.com/theinfluencecompany/realtime-avatar-sdk) | MIT 客户端；云端按量付费 | 普通 Web 开发环境，无本地 GPU | 用示例 avatar 建单按钮页面，验证中断、工具超时和异常降级 | 值得小规模验证 |
| [Realtime Avatar MCP](https://realtimeavatar.ai/docs/mcp) | npm 包；默认只读 | 需要服务 API key | 先只开启 `list_avatars` 与 `credit_balance`，确认 agent 不会自动产生费用 | **优先验证权限边界** |
| GameDirector / CoDeR | 项目页和 demo；完整代码未开放 | 未披露 | 只能审阅状态协议与 demo，暂不能复现端到端结果 | 仅跟踪 |
| PHOSA / ARS-Avatar | 论文与项目展示；开放状态待核验 | PHOSA 论文实验可用单张 RTX 3090 训练部分组件，但实时性不足 | 等待代码后先验证 SMPL-X 驱动的手部清晰度 | 条件性值得 |
| QuantWM / HelloWorld / DHSched | 当前主要为论文 | 未披露或依赖内部集群 | 只能实现简化概念验证，不能声称复现论文系统 | 暂不作为本周主实验 |

## 9. 系统架构与技术趋势判断

1. **显式状态明显升温。** GameDirector、CoDeR 和 WROP 分别从游戏规则、白盒世界和认知测试三个角度证明，视频历史不是可靠数据库。
2. **世界模型正在被重新定义为系统组件。** 它可以是 renderer、未来预测器、评测器或数据生成器，不必独自负责规则、记忆和控制。
3. **可复用架构趋于稳定：**`用户/传感器事件 → agent 或代码规划 → 权威状态与规则 → 生成模型渲染/预测 → verifier → RTC、引擎或控制器`。
4. **实时指标开始工程化。** 平均 FPS 正让位于首 chunk、chunk cadence、P95/P99、buffer underrun、迁移冲突和单位会话成本。
5. **coding agent 的接口从文件系统扩展到媒体运行时。** Three.js 模块、MCP、session API、camera event 和 streaming chunk 都开始成为可编程对象。
6. **数字人研究向垂直表达分化。** 手语、重打光、全身材质等问题单独优化，通用 talking head 指标已不足以描述产业价值。
7. **仍未解决：**跨小时状态校验、多人世界同步、生成视觉与权威状态错位、身份授权、实时审核、端到端延迟和生产成本。
8. **需降级看待：**CoDeR、GameDirector 的核心架构重要，但在代码未开放前仍是 demo；HelloWorld 的“实用”来自论文设计目标，不等于道路部署；QuantWM 的 6.20 倍为作者实验，不能直接换算为同等吞吐提升。

## 10. 论文精读候选

1. **[DHSched](https://arxiv.org/abs/2609.26363)**  
   值得读：少见的生产数字人控制面论文。重点看 generation ownership、conditional commit、claim/revalidate、迁移恢复及随机故障实验。复现风险是系统未开源，生产拓扑和 GPU worker 实现不完整。

2. **[GameDirector](https://arxiv.org/abs/2609.25652)**  
   值得读：直接回答传统游戏规则与生成式视频如何分工。重点看状态抽取、规则执行、NPC 决策、prompt 转译和机制正确率评测。风险是只覆盖三款游戏，尚未证明开放世界泛化。

3. **[CoDeR](https://arxiv.org/abs/2609.26458)**  
   值得读：coding agent 与视频模型结合最完整的架构之一。重点看 Logical Space 接口、模块组合、状态注册和白盒到生成视觉的条件桥接。风险是代码未开放、推理速度不透明。

4. **[Training Object Permanence in World Models](https://arxiv.org/abs/2609.28654)**  
   值得读：将认知科学任务转化为程序化训练和验收。重点看六类任务、数据随机化、固定考试、人类 Elo 与自动指标差异。风险是合成域偏差、214 GB 数据和非商业许可证。

5. **[PHOSA](https://arxiv.org/abs/2609.29292)**  
   值得读：数字人垂直场景中，数据、拟合、表示和目标用户研究形成完整闭环。重点看混合 SMPL-X fitting、partial hand kinematics、motion-aware sampling 和 Deaf-community study。风险是数据规模较小且 renderer 非实时。

## 11. 下周跟踪与可行动建议

### 继续跟踪

1. CoDeR 和 GameDirector 是否发布代码、模型权重、逐帧状态协议及可运行 demo。
2. DHSched 是否公开调度器、故障注入工具，以及迁移期间的音视频中断时长。
3. vLLM-Omni 的多会话压力测试、p95/p99、buffer underrun 和 ComfyUI backpressure PR 是否合并。
4. WROP 自动指标与人类对对象永久性判断的一致性，以及训练是否迁移到真实视频。
5. PHOSA 的 MVSign 数据、代码和许可证是否真正开放，能否将渲染压到 RTC 所需帧率。
6. Realtime Avatar SDK/MCP 的版本稳定性、实际首帧延迟、并发限制、费用和服务区域。
7. QuantWM 是否开放 kernel，并在两分钟以上 rollout 中保持身份和物体状态。
8. HelloWorld 是否提供代码、权重、七相机同步误差和实际闭环仿真速度。

### 本周可做的小实验

1. **规则外置的生成式 Boss**
   - 目标：验证显式状态是否比纯 prompt 更能保持生命值和技能冷却。
   - 组件：简单 TypeScript 状态机、任一可控视频模型、VLM 状态观察器。
   - 难点：生成画面可能与权威状态矛盾。
   - 成功判据：连续 30 个回合中生命值、冷却和终止条件零逻辑错误；视觉冲突率低于 10%。

2. **WROP 对象永久性回归**
   - 目标：建立世界模型升级前后的固定认知测试。
   - 组件：WROP 单任务分片、现有 V2V/continuation 模型、轨迹和人工盲评。
   - 难点：自动指标可能奖励画面相似而忽略物体是否合理存在。
   - 成功判据：至少覆盖三类、30 个样本；报告对象数量、位置、穿透和人工偏好，而非只报 VBench。

3. **coding agent 自动搭建数字人页面**
   - 目标：验证 MCP 是否减少虚构 ID、错误接口和密钥泄露。
   - 组件：Realtime Avatar MCP、TypeScript SDK、Next.js 测试项目、只读 API key。
   - 难点：区分构建期 MCP 与运行期应用 API，避免 agent 自动产生付费会话。
   - 成功判据：agent 使用真实 ready avatar 完成页面；密钥不进入浏览器 bundle；所有写操作均需人工确认。

4. **世界模型实时稳定性测试**
   - 目标：比较平均 cadence 与真实播放稳定性的差异。
   - 组件：vLLM-Omni、LingBot World、两张可用高端 GPU、带 backpressure 的 Web 播放器。
   - 难点：显存、host load 和解码/拷贝抖动。
   - 成功判据：40-chunk 会话中 p95 小于播放器缓冲预算、零 underrun，并分别记录首 chunk、均值、p95、p99 与显存峰值。
