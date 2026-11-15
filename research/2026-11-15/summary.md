# 数字人、世界模型与互动作品研究日报：2026-11-15

检索窗口：2026-11-09 至 2026-11-15。已对照 11 月 9–14 日日报去重；日期采用官方公告、文档更新时间或 arXiv 实际提交时间，不采用抓取时间。

## 1. 本日摘要

本日最强信号来自产品化而非新论文：World Labs 将此前处于早期访问阶段的 Atlas 正式开放为网页工具、Developer API、CLI、MCP、Blender 扩展和 coding-agent skills，使“图片或灰盒场景 → RGBD/新视角 → Gaussian Splat → mesh → 游戏、影视或仿真”首次形成较完整的商业接口链。  
Seedance 2.5 则于今日开放 ModelArk API，将最长 30 秒、同步音频、最多 50 个参考资产、编辑和续写纳入统一异步任务接口，适合由代码生成分镜、管理角色参考并批量渲染，但它仍是离线内容生成器，不是逐帧可玩的世界模型。  
数字人方向出现两类增量：ERNIE-Wizshot-ME 将人物、气质、表演、情绪、声音和镜头拆成结构化 API 字段；MetaPerson 2.0 则增加文本/图片生成 humanoid 和 generic 3D 模型，并能继续进入 Unity、Unreal、WebGL 和 GLB 管线。  
这意味着数字人的可编程接口开始同时覆盖“视频化人物表演”和“可运行的骨骼角色资产”，但两者都没有提供全双工对话、实时口型延迟或并发 SLA。  
世界模型研究仍沿着昨日已观察到的路线推进：PixVid追求下一帧响应，vid2vid-long外置历史检索，WorldDraft检验代码世界能否作为权威状态，而 Atlas 则把显式空间资产、模型生成和 agent 工具链真正装进同一产品。  
Unreal Engine 5.9 Preview补上另一端：MCP、Python、Blueprint、StateTree、性能分析和玩法测试让 coding agent 能观察并修改传统引擎中的真实状态。  
因此，近期最可落地的架构并非“端到端模型取代引擎”，而是“代码/引擎维护规则与状态，世界模型或视频模型生成外观，agent负责规划与资产编排”。  
产业证据仍有明显边界：Atlas、Seedance、千帆和 MetaPerson 已有正式可调用入口；PixVid、vid2vid-long、AURA 2.0 等仍以论文和作者 demo 为主，规模化部署、P95 延迟和客户 ROI 大多未披露。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对 11 月 14 日 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中高 | Wizshot-ME结构化人物表演API；MetaPerson 2.0文本/图片生成3D角色 | 从论文组件推进到按秒计费API和跨引擎资产 | 上期主要是AURA“角色大脑”；本期补上可调用表演层与3D身体层 |
| 世界模型 | 高 | Atlas公开Developer API、MCP、CLI、Blender和splats-to-mesh | 从模型/demo跨入正式开发平台 | 昨日报告未收录Atlas 11月12日公开发布，今日补录其完整开发者证据 |
| Coding × 互动作品 | 高 | Atlas skills与Unreal MCP分别打通生成世界和权威运行时 | agent可直接构建空间资产、镜头、关卡并执行回归测试 | 从研究型Blender harness推进到供应商官方API、MCP和引擎工具集 |
| 工业应用 | 中高 | Seedance API今日开放；Atlas、千帆已有计费和权限体系 | 批量生产可用，实时生成世界仍是Preview | 新增明确API端点、成本控制、项目隔离与异步任务契约 |

## 3. 最值得关注的 10 个进展

### 1. Atlas：世界模型正式成为可被 coding agent 调用的空间开发平台

- **类型／日期／主线：**模型、Developer API、MCP、CLI、Blender插件、agent skills；11月12日；世界模型＋Coding。
- **官方链接：**[发布页](https://worldlabs.ai/atlas)、[API参考](https://docs.atlas.worldlabs.ai/api-reference)、[Apache-2.0 skills](https://github.com/worldlabsai/skills)。
- **相对上期：**Atlas模型早于窗口发布，但本次因11月12日从早期访问转为公开工具和API而纳入；11月14日日报漏收，今日补录。
- **核心贡献：**统一提供文本生图、Chisel灰盒构造、多图转posed RGBD、RGBD转Gaussian Splat及splats-to-mesh。显式3D资产可以离开模型进入游戏、Blender和仿真，而非只输出一段不可编辑视频。
- **Coding接口：**REST任务API、TypeScript client、CLI、远程MCP、`atlas-flythrough`、`atlas-splats`、`atlas-blender` skills；operation支持等待、取消、trace、webhook和幂等重试。
- **作品／场景：**可编辑互动电影场景、虚拟摄影棚、游戏关卡草模、建筑漫游、机器人real-to-sim环境。
- **成熟度／证据：**正式开发平台；API和工具可用，但并发额度仍未公开。模型能力和效果为官方评测，尚缺大规模第三方生产验证。
- **重要性／阅读：**高／必读。

### 2. Seedance 2.5：30秒原生音视频生成今日进入正式API

- **类型／日期／主线：**视频模型、API、SDK；11月15日14:00 UTC+8开放；支撑基础设施＋Coding。
- **官方链接：**[模型教程](https://docs.byteplus.com/en/docs/ModelArk/seedance-2-5)、[创建任务API](https://docs.byteplus.com/en/docs/ModelArk/create-video-generation-task-api)。
- **相对上期：**上期仅记录Runway聚合调用；今日新增BytePlus官方ModelArk端点和完整参数契约。
- **核心贡献：**最长30秒、同步人声/音效/音乐，支持新生成、编辑和前后续写；单请求最多30张图、10段视频、10段音频，共50项参考资产。
- **Coding接口：**`POST /api/v3/contents/generations/tasks`、异步查询、callback、优先级、draft-to-final、任务类型预校验及安全用户标识。
- **作品／场景：**多角色互动短剧、分支剧情片段、商品演示、数字人过场、低清草稿—正式成片工作流。
- **成熟度／证据：**正式商业API；能力来自官方文档，未见独立一致性评测或P95生成时长。
- **重要性／阅读：**高／必读。

### 3. Unreal Engine 5.9 Preview：传统引擎成为agent可观察、可测试的权威运行时

- **类型／日期／主线：**引擎Preview、MCP、Python；11月9日；Coding × 互动作品。
- **官方链接：**[发布说明](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-9-release-notes)、[Unreal MCP](https://dev.epicgames.com/documentation/unreal-engine/unreal-mcp-in-unreal-editor)、[外部agent配置](https://dev.epicgames.com/documentation/unreal-engine/run-external-ai-agents-in-the-unreal-assistant-panel-in-unreal-engine)。
- **相对上期：**已于11月13日报道；本期无新的正式版变化，因其与Atlas公开接口构成直接互补而保留。
- **核心贡献：**MCP工具覆盖场景、渲染、源码管理、调试、性能、玩法测试、音频和UI；Python可编辑Blueprint、StateTree、PCG及动画；Canvas可连接图像、视频、mesh和语音服务。
- **Coding接口：**Codex等外部agent可直接驻留编辑器；Toolset Registry和Ask/Read/Edit/Auto/Full权限限制可作为治理边界。
- **作品／场景：**自动搭建并测试生成式NPC关卡、数字人舞台、浏览器品牌体验和互动电影分支。
- **成熟度／证据：**可下载Preview；Epic明确说明QA周期短、WebGPU与agent功能仍有粗糙部分，不适合直接转换生产项目。
- **重要性／阅读：**高／必读。

### 4. ERNIE-Wizshot-ME：微表情数字人表演被拆成结构化API

- **类型／日期／主线：**数字人视频模型、正式API；API文档11月11日更新；数字人。
- **官方链接：**[产品与价格](https://cloud.baidu.com/product/qianfan_home/model_3.html)、[创建任务API](https://cloud.baidu.com/doc/qianfan-api/s/8mwbylac7)、[任务查询](https://cloud.baidu.com/doc/qianfan-api/s/Imwc2p4go)。
- **相对上期：**首次收录。
- **核心贡献：**单图、参考声音、文本和分镜生成4–15秒1080p真人风格视频；API显式描述人物身份、气质、台词、情绪、肢体表演、语速、机位和出场人物，支持最多10个人物和多镜头结构。
- **Coding接口：**创建＋轮询两步异步API，返回任务状态、视频URL、时长和分辨率；视频URL有效24小时，内容最多保存30天。
- **作品／场景：**播客访谈、对话短剧、讲师特写、产品口播和故事角色镜头。
- **成熟度／证据：**正式产品，0.7元/秒；效果优势仅有官方声称，未披露生成时间、身份保持率及并发默认值。
- **重要性／阅读：**高／必读。

### 5. MetaPerson 2.0：文本或图片直接生成可进引擎的3D humanoid

- **类型／日期／主线：**3D avatar产品、SDK；11月13日；数字人＋Coding。
- **官方链接：**[2.0发布记录](https://docs.metaperson.avatarsdk.com/business-integration/release_notes/desktop/)、[Unity插件](https://github.com/avatarsdk/metaperson-unity-plugin)、[官方GitHub](https://github.com/avatarsdk)。
- **相对上期：**首次收录。
- **核心贡献：**Creator 2.0新增从文本或图片生成Humanoid与Generic模型，并提供Avatar SDK Move示例动画。输出可进入GLB、骨骼、viseme和传统动画管线。
- **Coding接口：**JS/REST、Unity、Unreal、Android、iOS和Web集成；Unity插件可在运行时创建角色、换装并加载GLB。
- **作品／场景：**游戏NPC、XR化身、多人社交、虚拟展员和可换装角色。
- **成熟度／证据：**正式工具和BSD-3-Clause示例代码；生成服务本身为商业闭源产品，2.0更新未给角色质量benchmark或生成耗时。
- **重要性／阅读：**中高／可读。

### 6. WorldDraft：检验“代码建世界、视频模型渲染”何时有效

- **类型／日期／主线：**论文、benchmark、GitHub；11月12日；世界模型＋Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2611.16217)、[代码](https://github.com/KevinHuge/WorldDraft)。
- **相对上期：**已于11月14日报道；暂无新变化。
- **核心贡献：**系统比较代码/Blender草模的运动和几何是否被最终视频保留，并揭示高细节代理画面可能造成source leakage，使目标外观被草模“污染”。
- **Coding接口：**程序世界、相机轨迹、代理渲染和视频模型构成可自动评测管线。
- **作品／场景：**互动电影、物理镜头、广告分镜和游戏视觉重渲染。
- **成熟度／证据：**已开源benchmark；并非完整可玩运行时。
- **重要性／阅读：**高／必读。

### 7. PixVid：世界模型控制粒度缩小到下一帧

- **类型／日期／主线：**论文、demo；11月12日；世界模型。
- **官方链接：**[论文](https://arxiv.org/abs/2611.17336)、[项目页](https://chenshuo20.github.io/PixVid/)。
- **相对上期：**已报道，无代码或权重开放的新变化。
- **核心贡献：**保留预训练DiT，将时空VAE替换为逐帧像素输出头，使动作条件能影响下一帧。作者报告B200上蒸馏模型约29.4 fps、控制到画面中位约34 ms。
- **Coding接口：**相机、深度或动作事件逐帧送入，可接手柄和引擎输入。
- **作品／场景：**可玩视频、机器人视觉仿真和实时生成式镜头。
- **成熟度／证据：**论文实测＋demo；硬件口径高，代码和许可证待核验。
- **重要性／阅读：**高／必读。

### 8. vid2vid-long：用显式对应关系为实时角色保留长时外观

- **类型／日期／主线：**论文；11月12日；实时视频基础设施。
- **官方链接：**[论文](https://arxiv.org/abs/2611.17298)。
- **相对上期：**无新变化。
- **核心贡献：**在输入视频中寻找patch对应关系，用对应位置检索历史输出，再以cross-attention注入当前生成；记忆仅在出现新视觉内容时增长。
- **Coding接口：**可作为Self-Forcing、LongLive或Rolling Forcing等因果生成器的外接记忆模块。
- **作品／场景：**长时数字人风格化、直播换装、AR镜像和持续角色滤镜。
- **成熟度／证据：**作者实验；未见官方代码、插件或生产延迟数据。
- **重要性／阅读：**高／必读。

### 9. AURA 2.0：数字人的“角色大脑”获得主动观察和流式工具调用

- **类型／日期／主线：**论文、流式视频理解模型；11月12日；数字人＋Coding。
- **官方链接：**[论文](https://arxiv.org/abs/2611.17048)。
- **相对上期：**无实质新变化；权重和推理代码的完整可下载状态仍待核验。
- **核心贡献：**主动判断何时开口，基于当前视频和对话调用工具，支持用户视觉标注和帧对齐区域定位，并维护长时交互记忆。
- **Coding接口：**视频流事件→主动性策略→function call→工具结果→语音/头像输出。
- **作品／场景：**装配教练、主动导览、视觉客服和直播助理。
- **成熟度／证据：**研究模型，不是头像渲染器；完整数字人链路和误触发率未验证。
- **重要性／阅读：**高／必读。

### 10. ACE-CaP-0与BWM-Forge：代码分别承担生产执行和合成世界构造

- **类型／日期／主线：**论文、GitHub、真实机器人实验；11月12日；Coding × 工业仿真。
- **官方链接：**[ACE-CaP论文](https://arxiv.org/abs/2611.16872)、[ACE项目页](https://acerobotics2025.github.io/ACE-CaP/)、[BWM-Forge代码](https://github.com/Lcixs/BWM-Forge)。
- **相对上期：**均已报道，暂无新结果。
- **核心贡献：**ACE-CaP让coding agent在开发期索取演示、搭建仿真并修改策略，最终冻结为无需agent在线参与的程序；BWM-Forge把机器人、轨迹、场景和相机编译成可编辑Blender工程，再交给视频模型渲染。
- **Coding接口：**机器人观察/控制API、Blender Python、URDF、轨迹文件和视频服务适配器。
- **作品／场景：**工业工站技能、机器人合成数据、多机位产品演示和可审计自动化。
- **成熟度／证据：**真实机器人研究验证＋开源工具；尚无安全认证或规模化客户部署。
- **重要性／阅读：**高／必读。

## 4. 数字人 / 虚拟形象能力进展

- **生成与驱动：**Wizshot-ME新增结构化表演控制，MetaPerson 2.0新增文本/图片到3D角色；前者输出视频，后者输出可继续编程的角色资产。
- **3D/4D表示：**MetaPerson走传统骨骼mesh与GLB路线，优势是可进入游戏运行时；本周期没有新的高质量4D Gaussian人体或可自由视角真人数字人论文。
- **实时交互：**AURA 2.0解决“何时主动、调用什么工具”，但头像渲染仍需Nuplex、Muse、MetaPerson LiveSpeak或其他RTC表演层。Wizshot-ME的异步任务不适合逐句实时对话。
- **音视频与情感：**Wizshot-ME把情绪、动作、语气和视线纳入请求结构，是本周期最具体的情感表演接口；相关自然度仍只有厂商材料。
- **工程部署：**批量口播和离线短剧已经具备明确成本；实时数字员工仍缺端到端P95、打断恢复、弱网和并发证据。
- **安全与身份治理：**本周期没有新的肖像授权凭证、C2PA、水印或深伪检测发布。单图人物和声音接口进入批量API后，调用方必须自行建立授权、审计和输出留存。

## 5. 世界模型进展

Atlas的产品化使世界模型首次较完整地覆盖“生成、重建、空间编辑、实时预览和显式资产导出”。其技术价值不只是画面质量，而是RGBD、相机、splat和mesh均能被代码查询和加工。

架构上出现三层分工：PixVid负责低延迟逐帧视觉响应；vid2vid-long负责历史检索；Atlas、WorldDraft和Unreal负责显式空间或权威状态。长时一致性因而越来越依赖外置状态，而非无限扩大隐式上下文。

可控/可玩方面，Frameboy官方展示最高15 fps云端相机探索，但厂商也明确承认需要强力云GPU、成本高于本地渲染；它应定级为研究预览，不应与60 fps确定性游戏运行时等同。

机器人与仿真方面，Atlas可输出mesh并接Blender，BWM-Forge可构建任务代理世界，ACE-CaP可将agent经验冻结为程序。当前缺口是统一物理语义：视觉上合理的空间资产仍需传统碰撞体、关节和模拟器验证。

评测方面，WorldDraft、GridWM-Judge、PhysCapBench和FuGIBench共同说明，画面质量、物理描述、因果敏感性、行为时机必须分层验收；单一VLM总分不足以证明世界模型可用于工业闭环。

## 6. Coding × 新型互动作品

### 链路一：agent直接构建可编辑的生成世界

**图片/文字 → Atlas skill或MCP → posed RGBD与新视角 → Gaussian Splat → mesh → Blender/Unreal → 交互关卡**

agent负责探索缺失视角、布置相机和选择任务；Atlas负责补全外观与空间；引擎负责碰撞、玩法和发布。Atlas API与skills已可运行，商业游戏质量仍需人工资产修整。

### 链路二：代码世界上的生成式互动电影

**剧情状态机 → Unreal Blueprint/StateTree → 代理场景和相机 → WorldDraft式结构验收 → Seedance 2.5生成30秒音视频分支 → 玩家选择下一状态**

代码负责角色身份、分支和事件；视频模型负责演员表演和最终画面；引擎负责选择界面、存档和回放。接口均已存在，但完整组合属于本报告推断。

### 链路三：可编程数字人短剧工厂

**角色设定/授权素材 → 脚本agent拆镜头 → Wizshot-ME填写人物、情绪、动作和机位 → 异步任务队列 → 人工审核 → 发布**

已有正式API和明确0.7元/秒成本，适合播客、培训和营销；不适合实时客服，因为生成是4–15秒异步任务。

### 链路四：可运行的3D NPC生成

**文本/角色图 → MetaPerson 2.0生成humanoid → GLB/骨骼/viseme → Unity或Unreal → LLM角色脑、状态机和动作库**

生成服务负责身体初稿；游戏代码负责行为、导航、权限和网络同步；引擎负责动画和渲染。资产链可运行，但生成角色质量、拓扑稳定性和大规模并发创建尚无公开评测。

### 链路五：主动观察的数字员工

**摄像头/屏幕流 → AURA 2.0判断是否介入 → 业务状态机校验 → function call → TTS/头像表演 → 可撤销操作日志**

模型负责理解和建议，代码负责授权、幂等、撤销与审计，数字人层负责社会性反馈。主动观察和工具调用有论文证据，端到端产品仍是组合推断。

### 链路六：实时生成视觉与传统引擎协同

**手柄事件 → 引擎更新权威坐标和碰撞 → PixVid生成下一帧 → vid2vid-long检索历史外观 → 超时或漂移时回退传统渲染**

这种架构能构建生成式镜头或梦境关卡，但不应让视频模型保存任务真值。B200研究结果证明低延迟可能性，消费级硬件和多人确定性尚未解决。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展／技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Atlas＋Unreal MCP＋传统状态机 | agent建场景，玩家在引擎内互动 | API正式／引擎Preview | Atlas并发未公布 | 碰撞、拓扑、多人确定性 | [Atlas](https://worldlabs.ai/atlas) |
| 影视/虚拟制作 | Atlas Viewfinder/Blender＋Seedance | 选机位、生成镜头、终稿渲染 | 正式API | 未披露 | 资产修整、角色连续性 | [Blender接口](https://docs.atlas.worldlabs.ai/blender) |
| 直播与电商 | Wizshot-ME＋异步任务队列 | 批量人物口播 | 正式产品 | 0.7元/秒；延迟未披露 | 非实时、授权与审核 | [千帆](https://cloud.baidu.com/product/qianfan_home/model_3.html) |
| 品牌互动 | Unreal WebGPU＋Atlas/Seedance素材 | 浏览器关卡、分支视频 | Preview＋正式API | 未披露 | Web性能、加载体积 | [UE 5.9](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-9-release-notes) |
| 教育培训 | Wizshot-ME＋AURA＋工具调用 | 讲师表演、主动指物和问答 | 视频层正式／交互层研究 | 未披露 | 事实性、未成年人隐私 | [AURA 2.0](https://arxiv.org/abs/2611.17048) |
| 企业数字员工 | AURA＋业务状态机＋头像层 | 观察、建议、执行和撤销 | 研究组合 | 未披露 | 权限、误触发、SLA | [FuGIBench](https://arxiv.org/abs/2611.16400) |
| 陪伴与社交 | MetaPerson＋LLM＋动作/口型 | 可运行3D化身和长期对话 | 角色资产正式 | 未披露 | 情感依赖、记忆治理 | [MetaPerson](https://docs.metaperson.avatarsdk.com/business-integration/release_notes/desktop/) |
| 空间计算 | Atlas splat＋mesh＋Web/Blender | 自由视角探索 | 正式工具／实时Preview | Frameboy $0.50/分钟会话 | 云GPU、眩晕、空间漂移 | [计费文档](https://docs.atlas.worldlabs.ai/billing) |
| 机器人/仿真 | Atlas/BWM-Forge＋MuJoCo/Isaac/Blender | real-to-sim、轨迹和合成数据 | 工具可用／研究验证 | 未披露 | 物理真值、sim-to-real | [Atlas机器人说明](https://worldlabs.ai/blog/atlas) |

## 8. 可复现资源与开发者入口

| 资源 | 开放与许可证 | 硬件／成本 | 是否值得复现 | 最小验证路径 |
|---|---|---|---:|---|
| [World Labs skills](https://github.com/worldlabsai/skills) | Apache-2.0；API和Blender扩展另受商业条款约束 | Node.js 22＋按量API | 高 | 单图生成posed RGBD，再生成splat并导入Blender |
| [Atlas MCP](https://docs.atlas.worldlabs.ai/mcp) | 托管OAuth MCP | 按项目credits | 高 | 连接后仅运行`images2PosedRGBD`，检查operation trace和资产隔离 |
| [Seedance 2.5 API](https://docs.byteplus.com/en/docs/ModelArk/create-video-generation-task-api) | 商业API | 价格及额度依控制台 | 高 | 用1图＋1音频生成5秒片，记录排队、生成时长和失败码 |
| [Wizshot-ME API](https://cloud.baidu.com/doc/qianfan-api/s/8mwbylac7) | 商业API | 0.7元/秒 | 高 | 单人物、单镜头、两种情绪做盲评A/B |
| [MetaPerson Unity插件](https://github.com/avatarsdk/metaperson-unity-plugin) | BSD-3-Clause示例；服务商业 | Unity 6000.0.59f2+ | 中高 | 生成一个角色，导出GLB并验证骨骼、viseme和换装 |
| [WorldDraft](https://github.com/KevinHuge/WorldDraft) | 代码和数据开放，资产许可需逐项检查 | 需外部视频模型 | 高 | 比较低细节/高细节代理世界的运动保持与外观泄漏 |
| [BWM-Forge](https://github.com/Lcixs/BWM-Forge) | Apache-2.0 | Blender＋外部模型API | 高 | 导入URDF和短轨迹，先只生成代理视频 |
| [PixVid](https://chenshuo20.github.io/PixVid/) | demo；代码/权重待开放 | 论文实时结果使用B200 | 仅跟踪 | 等开放后首先测输入事件到下一帧时间戳 |
| [vid2vid-long](https://arxiv.org/abs/2611.17298) | 暂无代码入口 | 未披露 | 中高 | 测试人物离开画面后重返的服装和身份一致性 |

## 9. 系统架构与技术趋势判断

明显升温的是“显式世界资产＋生成式渲染＋agent工具”。Atlas把RGBD、相机、splat和mesh变成API对象；Unreal把Blueprint、StateTree和玩法测试暴露为MCP工具；WorldDraft则给两层之间提供验收方法。

第二条主流路线是分层时间尺度：传统引擎每帧维护物理与状态，世界模型生成高熵视觉，长时记忆使用对应关系、事件表或空间数据库，coding agent只在规划、异常和资产生成时调用。让昂贵模型持续控制所有帧的路线正在降温。

数字人正在分成三个独立层：Wizshot-ME/Seedance负责离线表演，MetaPerson负责可运行3D身体，AURA/Nuplex类模型负责持续感知和对话。真正的产品竞争点将转向这三层之间的RTC、状态同步、工具权限和身份治理。

关键未解决问题包括：世界模型输出的碰撞与物理真实性、可玩视频长时漂移、人物跨镜头身份、数字人实时P95、API并发与数据保留、agent误操作、肖像和声音授权、生成资产许可证及可观测性。

需要降级看待的说法包括：把Atlas的实时相机漫游等同于完整游戏；把Seedance 30秒视频称为长时世界；把Wizshot-ME称为全双工数字员工；把PixVid的B200结果视为端侧能力；把论文机器人成功率视为规模化工业部署。

## 10. 论文精读候选

1. [WorldDraft](https://arxiv.org/abs/2611.16217)：重点读3×3实验设计、world preservation、visual realization及source leakage；它直接定义代码世界和视频渲染器之间的接口风险。
2. [Back to Pixels / PixVid](https://arxiv.org/abs/2611.17336)：重点读pixel lifting、clean-video prediction、延迟口径和自回归漂移；复现风险是B200和未开放代码。
3. [vid2vid-long](https://arxiv.org/abs/2611.17298)：重点读patch对应、历史检索和动态内存增长；风险是遮挡、大形变和错误对应。
4. [AURA 2.0](https://arxiv.org/abs/2611.17048)：重点读主动开口策略、流式tool calling、视觉grounding和长时记忆；与普通数字人的差异是补充“脑和工具”，不负责头像渲染。
5. [ACE-CaP-0](https://arxiv.org/abs/2611.16872)：重点读Demonstrations on Request、Simulation on Demand和冻结程序；风险是任务规模小、缺安全认证和异常恢复数据。

## 11. 下周跟踪与可行动建议

持续跟踪：

1. Atlas各任务的正式价格、P50/P95、并发额度和商用输出许可。
2. Atlas生成mesh在碰撞、导航、可破坏环境和机器人接触任务中的几何误差。
3. Seedance 2.5 API的真实排队时间、角色一致性及50参考资产时的稳定性。
4. Wizshot-ME多人镜头中的说话人、视线、动作和声音身份是否会串扰。
5. MetaPerson 2.0生成模型的拓扑、骨骼、viseme及Unity/Unreal运行性能。
6. PixVid和vid2vid-long何时开放代码、权重与消费级GPU结果。
7. AURA 2.0开放资源及主动介入的误触发、撤销和隐私机制。
8. Unreal 5.9 agent生成项目的缺陷率、性能回归和源码审查成本。

本周可执行实验：

1. **Atlas到Unreal的最小世界管线。**目标：验证生成空间能否变成可玩场景。组件：Atlas API、Blender、Unreal 5.9。难点：坐标系、碰撞mesh和尺度。成功判据：角色可在场景内导航，无明显穿墙，并可重复生成同一相机路径。
2. **双数字人API表演对照。**目标：比较结构化表演参数的实际作用。组件：Wizshot-ME、同一人物图和声音。难点：控制台词时长与随机性。成功判据：至少20组盲评中，情绪、凝视和手势条件能被稳定识别。
3. **30秒分支互动电影。**目标：测试代码状态与Seedance镜头的身份同步。组件：剧情状态机、Seedance 2.5、三个固定角色参考。难点：分支间服装、空间和声音延续。成功判据：四个分支均保持角色身份，且用户选择后10秒内获得草稿反馈。
4. **agent生成世界回归测试。**目标：防止生成资产只“看起来正确”。组件：Unreal MCP、WorldDraft式指标、自动Play Mode。难点：把视觉评分与引擎真值分开。成功判据：每次资产更新同时通过碰撞、导航、相机和视觉一致性测试，并保存完整操作trace。
