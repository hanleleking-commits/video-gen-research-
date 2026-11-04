# 数字人、世界模型与互动作品研究日报：2026-11-04

检索窗口：2026-10-29 至 2026-11-04。发布日期以论文、仓库和官方页面为准；已与 10 月 29 日至 11 月 3 日日报去重。

## 1. 本日摘要

本周期最强的数字人增量不是“更像真人”，而是长时生成、音画联合建模和可部署推理同时前进：TaoMate-HD 已开放推理代码与预览权重，可生成连续一小时的原生音视频，但目前仍依赖 8 张 H20。  
VidDub 则把视频配音压缩到两步扩散，并一次性开放训练、推理、权重和固定 benchmark；其 11.3 FPS 是离线 prepared-input 吞吐，不代表实时交互。  
单图全身角色方面，新工作开始用模型自产多姿态监督训练个体 LoRA，目标是解决大动作下脸部和服装身份漂移，但对应代码尚未实际发布。  
世界模型最重要的变化，是 AgiWorld 和 GSExplorer 都把长期一致性从“扩大视频上下文”转向“显式空间记忆＋生成—重建闭环”。  
AgiWorld 更进一步，用 agent 将自然语言意图编译成结构化世界状态、相机轨迹与分段提示词，代表世界模型开始具备类似游戏导演系统的控制平面。  
Coding 侧最值得关注的是 Three-Live：它把 LLM 生成的 Three.js 程序沙箱化编译为 GLB/USDZ，并接入 WebXR、Filament 和 RealityKit，使“代码生成空间内容”具备跨端运行时。  
Isekai3D SDG 和 Minimal v0.1 则显示工业世界模型的另一条务实路线：先由 Unreal 或测绘数据维护可验证状态，再让生成模型负责外观和合成数据，而不是让视频模型承担全部物理与规则。  
总体上，本周期已有代码和权重的数字人项目明显多于可运行世界模型；世界模型的核心结果仍主要来自作者自测，尚缺公开权重、独立复现和生产并发证据。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 高 | 原生音视频长时生成、两步视频配音、音频引导人脸修复集中发布 | 从论文演示推进到代码＋权重；但算力和离线约束仍重 | 上期以 HairDeck 等结构化资产为主，本期转向流式表演与后期配音 |
| 世界模型 | 中高 | agent 规划、显式 3DGS/点云记忆成为长期一致性的共同解法 | 仍以研究原型为主；Minimal 提供有限工业验证 | 相对 Wizard、GROME、L3G-World，本期进一步形成“agent／状态层—空间记忆—生成渲染”闭环 |
| Coding × 互动作品 | 高 | Three.js 程序可安全编译到 WebXR、Android 和 Apple 原生运行时 | 出现 Apache-2.0 SDK、CLI、移动端包和浏览器 playground | 从“代码控制视频模型”扩展到“代码本身成为可移植空间资产” |

## 3. 最值得关注的 10 个进展

### 1. TaoMate-HD：长时原生音视频数字人开放推理

- **类型／主线：** 模型、GitHub、权重；数字人＋支撑基础设施  
- **官方链接：** [论文](https://arxiv.org/abs/2611.03394)、[代码](https://github.com/TaoLiveAIGC/TaoMate-HD)、[预览权重](https://huggingface.co/TaoLiveAIGC/TaoMate-HD-preview)
- **发布时间：** 论文 2026-11-02 UTC；代码与 LoRA 2026-11-03。
- **相对上期的新变化：** 首次进入日报；已具备真实下载与推理入口。
- **核心贡献：** 每个 chunk 用两次 480p 去噪确定结构，再用一次 768p 去噪细化；音频沿同一轨迹生成，并将各阶段音画预测传给下一 chunk。作者报告相对直接 768p 生成加速 1.6 倍，流水并行再加速 1.92 倍，CER 从 30.75% 降至 19.57%。
- **Coding 接口：** Python CLI＋JSON prompt；支持连续 prompt window、serial/accelerated 两种模式。
- **场景：** 长时虚拟主播、数字讲师、连续角色表演、预生成互动叙事片段。
- **成熟度／证据：** 已开源推理与预览 LoRA；论文实测＋官方 demo。10 秒样例需 8×H20 96GB，24 FPS 输出但生成吞吐最高报告为 8.99 FPS，尚非实时。
- **重要性／阅读：** 高／必读。

### 2. VidDub：两步视频配音及完整复现链

- **类型／主线：** 论文、模型、GitHub、benchmark；数字人  
- **官方链接：** [论文](https://arxiv.org/abs/2611.03404)、[代码与复现说明](https://github.com/LAW1223/VidDub)
- **发布时间：** 论文 2026-11-02；训练代码、权重 2026-11-03。
- **相对上期的新变化：** 全新；代码、2.81B 完整推理栈、VidDub-Bench 与训练 recipe 同步开放。
- **核心贡献：** 替换音频时屏蔽原嘴部重建监督，避免模型被旧口型拉回；再通过轨迹回归和分布匹配蒸馏得到两步学生。公开实现以 81 帧窗口、64 帧步长处理长视频。
- **Coding 接口：** `scripts/infer.py --video --audio --output`；提供批量 benchmark、训练、哈希审计和确定性验证工具。
- **场景：** 影视本地化、数字员工多语言版本、营销视频批量改口型。
- **成熟度／证据：** 已发布代码、权重和固定测试集；作者报告单张 H20 上 prepared-input 吞吐 11.3 FPS。仅适合单个、较正面的可检测人脸，仍是离线窗口式处理。
- **许可证：** source-available；个人商业项目可用，但公司使用、SaaS、API 或平台集成需另行授权。
- **重要性／阅读：** 高／必读。

### 3. AgiWorld：agent 成为持久世界的控制平面

- **类型／主线：** 论文、demo；世界模型＋Coding  
- **官方链接：** [论文](https://arxiv.org/abs/2611.02341)
- **发布时间：** 2026-11-02 UTC。
- **相对上期的新变化：** 相比 GROME、L3G-World 侧重记忆表示，AgiWorld 新增高层 agent 的世界设计与路线规划闭环。
- **核心贡献：** AgiWorld-Agent 维护已确认/待探索世界状态，把意图转成度量相机轨迹和结构化 prompt；AgiWorld-WM 分块生成视频并把几何写回空间记忆。可见性检索与可靠性排序用于减轻遮挡和累计深度误差。
- **Coding 接口：** 论文定义了结构化世界状态、相机轨迹、prompt 和反馈接口，但没有公开 SDK。
- **场景：** 自动预演、可探索虚拟片场、agent 导演的互动电影和生成式漫游。
- **成熟度／证据：** 研究原型；作者报告分钟级旅程和 revisiting benchmark SOTA，暂无代码、权重或公开运行 demo。
- **重要性／阅读：** 高／必读。

### 4. GSExplorer：3D Gaussian 世界状态驱动生成—重建闭环

- **类型／主线：** 论文；世界模型  
- **官方链接：** [论文](https://arxiv.org/abs/2611.01543)
- **发布时间：** 2026-11-01。
- **相对上期的新变化：** 与上期 GROME 的免训练分解记忆相比，GSExplorer 明确把生成视频持续反馈给 SLAM/3DGS 后端。
- **核心贡献：** 3DGS 渲染为视频生成提供多模态条件，生成结果再经不一致过滤和跨 chunk bundle adjustment 更新世界状态；训练数据包含 29.3 万组退化 3DGS 渲染与真实目标视频。
- **Coding 接口：** 理论上暴露相机轨迹、3DGS 状态和重建后端接口；未发布代码。
- **场景：** 可重访虚拟空间、虚拟勘景、空间数字孪生补全。
- **成熟度／证据：** 研究原型；作者报告单步 Camera Control 99.07，并展示 481 帧离开—返回 rollout。缺独立验证。
- **重要性／阅读：** 高／必读。

### 5. One Image Is Enough：单图角色的测试时个性化

- **类型／主线：** 论文、项目页；数字人  
- **官方链接：** [论文](https://arxiv.org/abs/2611.02964)、[项目仓库](https://github.com/SadmanPinon/one-image-is-enough)
- **发布时间：** 2026-11-02。
- **核心贡献：** 先合成目标人物的多姿态伪真值，再筛选身份最一致样本，为每个角色训练 LoRA；无需真人多姿态视频。
- **Coding 接口：** 计划基于 UniAnimate-DiT、Wan2.1、FLUX.1 Kontext 和 DiffSynth-Studio。
- **场景：** 用户上传一张图生成全身动作角色、虚拟试衣和个性化短剧。
- **成熟度／证据：** 论文实测、尚不可复现。arXiv 摘要称代码和权重可用，但仓库明确写明预计 2026 年 12 月发布；应以后者为准。生成器衍生权重计划采用非商业许可证。
- **重要性／阅读：** 高／可读。

### 6. Three-Live：LLM 生成 3D 代码的跨端安全运行时

- **类型／主线：** SDK、GitHub、运行时；Coding × 互动作品  
- **官方链接：** [官方公告](https://opensource.googleblog.com/2026/11/?m=0)、[GitHub](https://github.com/google/three-live)
- **发布时间：** Google Open Source Blog 于 2026-11-03 宣布；仓库 v1.0.0 实际发布于 2026-10-06，本次因窗口内官方发布与开发者入口完善纳入。
- **核心贡献：** 将受限 Three.js 程序在 Worker/iframe 或嵌入式 JS 引擎中沙箱执行，再编译为 GLB/USDZ；提供执行超时、几何配额和网络隔离。
- **Coding 接口：** Web Component、npm、Rust CLI、Kotlin/JNI、Swift Package；对接 WebGL2/WebXR、Filament、ARCore、RealityKit。
- **场景：** 语音生成 AR 展品、可编程品牌空间、空间教学工具、动态生成的 3D UI。
- **成熟度／证据：** Apache-2.0 Developer Preview，已有 playground 与预编译二进制；官方称典型编译耗时 2–5 ms，但尚无独立移动端压力测试。
- **重要性／阅读：** 高／必读。

### 7. TaoMate-FVSR：音频约束的人脸视频修复

- **类型／主线：** 论文、GitHub、权重；数字人  
- **官方链接：** [论文](https://arxiv.org/abs/2611.03393)、[推理代码](https://github.com/TaoLiveAIGC/TaoMate-FVSR)
- **发布时间：** 论文 2026-11-02；推理代码 2026-11-03。
- **核心贡献：** 不只锐化脸部，还用音频约束唇齿恢复；同时支持原音频修复和替换音频的视觉配音。
- **Coding 接口：** Python CLI，包含 DWPose 裁剪、512/1024 推理、颜色/蒙版融合和贴回完整画面。
- **场景：** 低清数字人修复、场景级 AIGC 人脸补强、配音后处理。
- **成熟度／证据：** Apache-2.0 推理运行时＋独立权重；在 H20 完成 smoke test。评测包含后验挑选的 30 段替换音频子集，泛化结论需谨慎。
- **重要性／阅读：** 中高／可读。

### 8. Isekai3D SDG：可重放的 Unreal 合成游戏数据管线

- **类型／主线：** 平台、Unreal 插件、技术报告；支撑基础设施＋Coding  
- **官方链接：** [NVIDIA Research 页面](https://research.nvidia.com/publication/2026-10_isekai3d-sdg-platform-diverse-synthetic-gameplay-visual-data)、[arXiv](https://arxiv.org/abs/2611.03626)
- **发布时间：** 官方页面 2026-10-26；arXiv 于 2026-11-02 提交，因窗口内公开论文版本纳入。
- **核心贡献：** 把世界模拟和画面采集分开：先记录世界状态、角色动作和相机，再重复回放生成 RGB、深度、法线、实例 ID 及对齐元数据。
- **Coding 接口：** Unreal Engine 插件、DLL 扩展和无需安装编辑器的打包应用。
- **场景：** 世界模型训练、游戏 agent、神经渲染、视觉回归测试。
- **成熟度／证据：** 平台技术报告；公开页面未给出可下载仓库和许可证，复现入口不足。
- **重要性／阅读：** 中高／可读。

### 9. Minimal World Model v0.1：生成视频进入真实机器人验证闭环

- **类型／主线：** 产品技术展示、工业案例；世界模型  
- **官方链接：** [Minimal 官方页面](https://www.minimal.tech/blog/world-model)
- **发布时间：** 2026-11-02。
- **核心贡献：** 由航空激光扫描构建米级、地理配准和语义标注的可驾驶城市；模拟器保持几何和标签，LTX-2.3 将其转成真实感视频。系统还在闭环交通中测试配送机器人。
- **Coding 接口：** 模拟器状态、检测真值和规划轨迹形成可评分接口；具体 SDK/API 未披露。
- **场景：** 自动驾驶/配送机器人训练、回归测试与合成数据。
- **成熟度／证据：** 公司 v0.1 原型；已有多个城市片段和机器人演示，但准确率、成本、规模及外部客户未披露。
- **重要性／阅读：** 中高／可读。

### 10. TailorRig：可控的服装辅助骨骼生成

- **类型／主线：** 论文、dataset；数字人＋角色资产管线  
- **官方链接：** [论文](https://arxiv.org/abs/2611.02902)
- **发布时间：** 2026-11-02；SIGGRAPH Asia 2026 论文。
- **核心贡献：** 根据既有身体骨架模板和艺术家语义标签，自回归生成裙摆、外套等辅助骨链；Human-Mix 包含超过 3.8 万个 humanoid 模型。
- **Coding 接口：** 输出结构化骨骼与 skinning weight，适合导入 DCC 或游戏引擎；代码与数据尚未开放。
- **场景：** 游戏角色换装、数字人服装动画、影视资产批量绑定。
- **成熟度／证据：** 论文实测，非可运行发布。
- **重要性／阅读：** 中／可读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：** TaoMate-HD 将文本、语音和视频放进同一自回归轨迹；One Image Is Enough 用合成多姿态监督解决单图角色身份漂移。
- **3D/4D 与资产表示：** 本周期没有新的高质量 4D 头像重建发布；TailorRig 的价值在于补齐服装骨骼这一传统资产管线环节。
- **实时交互：** TaoMate-HD 的输出是 24 FPS 视频，但推理吞吐最高仅 8.99 FPS 且需 8×H20，不能视为实时数字人。VidDub 的 11.3 FPS 同样是离线 prepared-input 指标。
- **音视频与情感：** 原生联合音视频和音频约束唇齿恢复明显升温；情感可控性、多说话人 turn-taking 和用户打断仍缺强证据。
- **工程部署：** VidDub 和 TaoMate-FVSR 已具备脚本化入口；TaoMate-HD 的硬件门槛过高，更适合实验室或云端批处理。
- **安全与身份治理：** 本周期暂无新的水印、授权或深伪检测方案。VidDub 仅提供使用规范，未内置可验证水印或身份授权协议，不能等同于治理能力。

## 5. 世界模型进展

- **架构与训练：** 主流结构正从单一视频自回归器变为“agent/规则层＋空间记忆＋条件视频生成＋重建反馈”。
- **可控与可玩：** AgiWorld 已能把自然语言目标转换成结构化 prompt 和相机轨迹，但不是低延迟玩家控制；真正可玩性仍弱于上期 Wizard、Fight World Model。
- **长时与物理一致性：** GSExplorer 用 3DGS/SLAM 保持可重访空间；AgiWorld 用点云记忆和可靠性排序维护分钟级旅程。两者改善的是空间持久性，不等于正确动力学。
- **空间表示：** 显式 3DGS、点云和度量相机轨迹正取代“纯历史帧缓存”，成为长期一致性的关键中间层。
- **机器人/游戏/仿真：** Minimal 和 Isekai3D 都把规则、真值与视频外观分开，分别服务机器人验证和世界模型训练。
- **实时部署与评测：** 本周期没有新的公开实时世界模型运行时；AgiWorld、GSExplorer 均无代码，论文指标仍需独立复现。

## 6. Coding × 新型互动作品

### 链路一：agent 导演的持续生成世界

**能力：** AgiWorld 的 agent 规划＋空间记忆  
→ **代码/运行时：** 自然语言目标编译为世界状态、结构化 prompt 和相机轨迹  
→ **互动体验：** 用户只描述“探索废弃车站并返回入口”，系统自动设计路线和未见区域  
→ **工业场景：** 虚拟勘景、互动电影预演、主题空间概念验证。  
**证据：** 论文 demo；暂无公开运行时。

### 链路二：LLM 即时生成空间作品

**能力：** LLM 生成 Three.js 场景程序  
→ **代码/运行时：** Three-Live 沙箱、配额校验、GLB/USDZ 编译、WebXR/RealityKit/Filament  
→ **互动体验：** 语音要求“生成可抓取的太阳系模型”，场景在网页、手机 AR 或头显中运行  
→ **工业场景：** 教育、品牌 AR、空间电商、展览导览。  
**证据：** 已开源、有 playground；Developer Preview。

### 链路三：可持续更新的虚拟主播

**能力：** TaoMate-HD 原生音视频长时生成  
→ **代码/运行时：** 服务端维护 prompt window 和音画历史，事件系统追加台词或节目段落  
→ **互动体验：** 角色可连续讲解并跨段保持声音和画面  
→ **工业场景：** 数字讲师、栏目式主播、长时内容生成。  
**证据：** 代码和权重可运行；当前 8×H20 且低于实时，本周期尚不能支持真正可打断直播。

### 链路四：多语言数字员工内容流水线

**能力：** ASR/翻译/TTS＋VidDub 两步口型迁移  
→ **代码/运行时：** 工作流引擎提交原视频与替换音频，批量 runner 生成并执行 Sync-C、身份和画质检查  
→ **互动体验：** 同一数字员工快速生成多语言培训或客服视频  
→ **工业场景：** 企业培训、产品说明、影视本地化。  
**证据：** 可运行；离线生产，不是实时对话数字人。

### 链路五：可验证的世界模型训练工厂

**能力：** Isekai3D 的世界状态重放与多通道采集  
→ **代码/运行时：** Unreal 插件记录动作、角色、相机和状态，再重放采集 RGB/深度/法线/实例 ID  
→ **互动体验：** 同一玩法自动生成不同相机、材质和渲染条件的数据  
→ **工业场景：** 世界模型训练、游戏 bot、视觉回归测试。  
**证据：** 技术报告与打包平台描述；公开下载入口待核验。

### 链路六：机器人数字孪生中的生成式外观层

**能力：** 米级语义城市＋视频生成器  
→ **代码/运行时：** 模拟器负责道路、交通、真值和闭环响应，LTX-2.3 负责真实感渲染  
→ **互动体验：** 机器人可在同一可验证路线中反复训练，同时看到更接近真实摄像头的画面  
→ **工业场景：** 配送机器人和自动驾驶测试。  
**证据：** Minimal 官方 v0.1；外部验证和规模数据未披露。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 | 所需技术栈／互动机制 | 当前成熟度 | 成本或延迟 | 主要阻碍 |
|---|---|---|---|---|---|
| 游戏 | Isekai3D、GSExplorer | Unreal 状态记录＋3DGS/视频生成 | 平台报告／研究原型 | 未披露 | 数据和世界模型代码未开放 |
| 影视/虚拟制作 | VidDub、AgiWorld | 配音流水线＋agent 相机规划 | 可离线试用／概念预演 | VidDub 11.3 FPS，1×H20 | 肖像授权、镜头级人工审核 |
| 直播与电商 | TaoMate-HD | 音视频生成＋prompt 事件流 | 可运行研究代码 | 8×H20，最高 8.99 FPS | 低于实时、成本和打断能力 |
| 品牌互动 | Three-Live | LLM→Three.js→WebXR/AR | Developer Preview | 官方称编译 2–5 ms | 复杂资产质量、脚本安全 |
| 教育培训 | TaoMate-HD＋Three-Live | 数字讲师＋交互 3D 教具 | 原型可组合 | 未披露 | 并发、内容审核、学习效果 |
| 企业数字员工 | VidDub | TTS＋两步口型迁移 | 离线生产可用 | 11.3 FPS prepared-input | 许可证限制、非实时 |
| 陪伴与社交 | TaoMate-HD | 对话 agent＋音视频历史 | 研究原型 | 8×H20 | turn-taking、延迟、安全 |
| 空间计算 | Three-Live | WebXR/RealityKit/Filament | 开发者预览 | bundle 约 45–55KB | 原生端兼容与生产稳定性 |
| 机器人/仿真 | Minimal、Isekai3D | 可验证模拟状态＋生成渲染 | 企业 v0.1／平台报告 | 未披露 | sim-to-real、覆盖率、闭环验证 |

证据入口：[TaoMate-HD](https://github.com/TaoLiveAIGC/TaoMate-HD)、[VidDub](https://github.com/LAW1223/VidDub)、[Three-Live](https://github.com/google/three-live)、[Isekai3D](https://research.nvidia.com/publication/2026-10_isekai3d-sdg-platform-diverse-synthetic-gameplay-visual-data)、[Minimal](https://www.minimal.tech/blog/world-model)。

## 8. 可复现资源与开发者入口

| 资源 | 许可证/硬件 | 是否值得复现 | 最小验证路径 |
|---|---|---|---|
| [TaoMate-HD](https://github.com/TaoLiveAIGC/TaoMate-HD) | MiniMax H3 Community License；官方样例 8×H20 96GB | 有机房资源时值得 | 跑 10 秒双 prompt 样例，测 CER、首帧延迟和跨窗口重复 |
| [VidDub](https://github.com/LAW1223/VidDub) | source-available；企业/API 需授权；1×H20 报告 | 高 | 用 81 帧正面单人视频替换音频，再跑固定 benchmark |
| [TaoMate-FVSR](https://github.com/TaoLiveAIGC/TaoMate-FVSR) | Apache-2.0；BF16 CUDA，H20 已测试 | 中高 | 对低清 talking-head 分别用原音频与替换音频修复 |
| [One Image Is Enough](https://github.com/SadmanPinon/one-image-is-enough) | 代码计划 Apache-2.0；部分权重非商业 | 暂不复现 | 等待 12 月代码和权重；当前只能审阅方法 |
| [Three-Live](https://github.com/google/three-live) | Apache-2.0；浏览器或移动端即可 | 高 | 让 LLM 生成一个受限 Three.js 场景，在 playground 验证沙箱和 GLB 导出 |
| [VidDub-Bench](https://github.com/LAW1223/VidDub) | 随项目条款；256 个合成源片段 | 高 | 校验 manifest/hash，跑同音频与跨音频两组条件 |
| AgiWorld / GSExplorer | 无公开代码或权重 | 暂不值得投入复现 | 先复核论文中的 memory ablation 与 revisiting 协议 |
| Isekai3D SDG | 公开下载与许可证未确认 | 仅跟踪 | 等待插件、打包应用或 dataset 正式入口 |

## 9. 系统架构与技术趋势判断

1. **显著升温：** 原生音视频、少步蒸馏、空间记忆、agent 控制平面和 LLM 生成 3D 程序。
2. **正在成为主流的分层：**  
   `agent/规则与状态层 → 结构化中间表示 → 生成渲染层 → 重建/评测反馈`。  
   AgiWorld、GSExplorer、Minimal 和 Three-Live 分别验证了这一架构的不同切面。
3. **代码的新角色：** 不再只是调用一次模型 API，而是持续维护世界状态、事件、相机、权限、预算、回滚和质量门禁；模型负责补全外观、动作和未见内容。
4. **仍未解决：** 真正低延迟的可打断音视频数字人、多人稳定交互、长期物理一致性、公开世界模型运行时、并发成本、跨平台资产质量和端到端身份授权。
5. **需降级看待：** “一小时生成”不等于一小时无漂移；“24 FPS 输出”不等于实时推理；“世界模型”若只是自动生成静态仿真城市，也不等同于学习型动力学模型。
6. **营销风险：** Minimal 的案例有明确工程链路，但性能、客户和规模均未披露；AgiWorld、GSExplorer 的 SOTA 仍是作者自测；Three-Live 是非官方支持的 Google 项目，应按 Developer Preview 而非生产级 SDK 评估。

## 10. 论文精读候选

1. [AgiWorld](https://arxiv.org/abs/2611.02341)  
   **原因：** 最清晰地把 agent harness 引入视频世界模型。重点读 world-state、agent/WM 接口、memory maintenance 和 ablation。复现风险高：未开源且依赖多组件。

2. [TaoMate-HD](https://arxiv.org/abs/2611.03394)  
   **原因：** 长时音视频生成与流水并行结合紧密。重点读 progressive-resolution、stage-local history、continuation training 和部署优化。复现风险是 8×H20 与 MiniMax H3 许可证。

3. [VidDub](https://arxiv.org/abs/2611.03404)  
   **原因：** 研究问题、监督冲突和两步蒸馏都定义得较完整，并提供训练代码。重点读 replacement-aware loss、matched trajectory regression、DMD 和 Long-20。风险在许可证及正面单人脸假设。

4. [GSExplorer](https://arxiv.org/abs/2611.01543)  
   **原因：** 展示视频生成器和 SLAM/3DGS 后端如何闭环。重点读退化数据构造、不一致过滤、跨 chunk BA 和 481 帧 revisit。风险是无代码、重建误差可能被 benchmark 掩盖。

5. [One Image Is Enough](https://arxiv.org/abs/2611.02964)  
   **原因：** 用自产监督替代真人多姿态采集，对个性化角色生产有直接价值。重点读伪真值筛选、静态 clip LoRA 和身份—运动权衡。风险是代码尚未发布且部分权重限非商业使用。

## 11. 下周跟踪与可行动建议

### 持续跟踪

1. TaoMate-HD 是否提供单机、低显存或量化版本，以及真实首帧和文本更新延迟。
2. VidDub 在侧脸、多人、遮挡和非 25 FPS 视频上的失败率。
3. One Image Is Enough 是否按计划开放代码、权重及单角色训练时间。
4. AgiWorld、GSExplorer 是否发布代码、模型或可交互 demo。
5. Three-Live 的沙箱能否抵御资源耗尽、恶意材质和复杂脚本攻击。
6. Isekai3D 是否开放 Unreal 插件、打包应用、示例数据和许可证。
7. Minimal 是否披露仿真规模、生成成本、sim-to-real 指标与真实道路结果。
8. 数字人发布是否开始默认集成授权凭证、C2PA/水印和可验证溯源。

### 本周可动手的小实验

1. **VidDub 多语言回归测试**  
   目标：验证两步模型在中文、英文及快语速替换音频上的同步性。  
   组件：VidDub、Whisper、3 段授权正面视频。  
   难点：显存、音频时长匹配、窗口接缝。  
   成功判据：无明显身份漂移，窗口边界无跳变，Sync-C 不低于原仓库样例的 90%。

2. **Three-Live 生成式 AR 原型**  
   目标：从自然语言生成可抓取、缩放的教学模型。  
   组件：任意 coding LLM、Three-Live/XR-Live、手机浏览器。  
   难点：限制生成代码只使用允许的 Three.js 子集。  
   成功判据：生成代码通过沙箱、2 秒内展示、能导出 GLB，并在至少两个平台正确运行。

3. **空间记忆最小模拟**  
   目标：验证显式记忆是否比单纯历史帧更能维持“离开后返回”的一致性。  
   组件：现有 I2V 模型、单目深度、简易点云/3DGS 缓存、固定相机回环轨迹。  
   难点：深度尺度和遮挡过滤。  
   成功判据：返回视角的特征匹配、深度误差和对象计数均优于仅最近帧条件基线。

4. **数字人生产门禁**  
   目标：为音画生成建立可自动执行的质量与合规检查。  
   组件：SyncNet、ASR CER、身份相似度、帧间闪烁指标、授权清单和水印步骤。  
   难点：自动指标与人工观感相关性有限。  
   成功判据：能自动拦截口型错位、身份漂移、重复台词和未授权身份四类故障，并保留完整审计记录。
