# 数字人、世界模型与互动作品研究日报：2026-07-31

检索窗口：2026-07-25 至 2026-07-31。  
日期说明：当前日期已核验为 2026-07-31；论文日期采用 arXiv submission history，产品日期采用官方发布页。已与 7 月 24—30 日日报去重，仅重复收录出现正式上线、代码/数据开放或完整技术细节的项目。

## 1. 本日摘要

本日最重要的新信号是，生成视频开始被定义为一种“持续运行、可接收事件、保存状态并渐进输出”的服务，而非一次性离线推理：Visko Orbis 公开了低于一秒的提示更新可见延迟、24 FPS 的 4K 输出以及分版本会话状态，但尚未开放模型、API和硬件配置。世界模型的数据层也明显结构化，CG-World 将骨骼、控制器、相机、灯光、物理缓存、接触事件和反事实分支组织为约 85 万段对齐样本，不过数据尚未实际下载。ContactFlow则把机器人动作抽象成3D接触点轨迹，使人类示范和不同机器人可以共享同一种视频世界模型条件，在10项实机任务中正确预测8项结果，但推理仍远非实时。

Coding × 互动作品方向出现了最强产品化证据：Fortnite LLM Conversations在美国时间7月30日跨过实验状态，Verse API暴露会话历史、工具能力、插话、限流、审核和失败事件，成为本周期最完整的“生成角色＋游戏状态机＋发布平台”闭环。影视创作方面，CineWeaver将镜头边界、逐镜头参考和外观记忆变成可编排推理结构；FilmBench v2进一步开放双语数据和FilmOps代码，使镜头尺度、构图、机位和运镜可以进入自动化测试。

数字人基座相对昨日没有新的高质量talking-head或原生音视频模型发布；新增主要是角色动作工具、影视评价和身份安全。MoSAIC提供身体分区级动作风格路由，Two2Four把普通人体动作转为四足角色表演，但二者目前都缺少确认的公开运行时。总体上，证据仍呈两极分化：Fortnite和FilmOps已有开发接口或代码；多数世界模型与角色生成工作仍停留在作者实验，成本、并发、SLA和第三方复现基本空白。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 低—中 | 身体分区动作编辑、跨物种角色驱动及解释型防伪 | 仍以离线角色动画和研究评测为主 | 未出现强于TaoMate、OmniMate、AptAvatar、Ripple的新基座 |
| 世界模型 | 高 | 持续运行的视频服务、全状态数据协议、接触条件与rollout崩溃诊断同时出现 | Visko有完整系统报告；CG-World和ContactFlow仍不可复现 | 从“显式状态模型”进一步下沉到数据schema、接触协议和运行时会话 |
| Coding × 互动作品 | 很高 | Fortnite NPC正式进入可发布平台；FilmOps开放可编程影视评价 | 从论文demo进入正式产品和Apache代码 | 上期“即将上线”变为可发布节点，并补齐Verse事件与会话API |

## 3. 最值得关注的 11 个进展

### 1. Visko Orbis 1.0：持续运行的实时视频模型

- **类型 / 时间 / 主线：** 模型技术报告、系统demo；2026-07-29；世界模型、Coding × 互动视频。
- **官方链接：** [论文与完整技术细节](https://arxiv.org/abs/2607.26694)。
- **相对上期的新变化：** 全新条目；7月30日日报未收录。
- **核心贡献：** 将视频生成定义为带有持久会话状态的chunk式latent-flow服务。近期历史保留高分辨率latent，远期历史逐级压缩并写入固定容量memory；提示异步编码，在下一未提交chunk生效，并以session/prompt version避免旧任务覆盖新状态。
- **关键结果：** 作者报告原生生成为832×480，经流式超分输出4K/24 FPS；提示变化平均不到1秒可在输出中看到；展示一小时rollout。执行层包含W8A8、编译融合算子、全序列多GPU并行、FIFO媒体流水线和渐进解码。[完整系统说明](https://arxiv.org/html/2607.26694v1)
- **Coding接口 / 场景：** 论文给出“时间戳事件＋会话状态＋持续视频流”的逻辑接口，可由直播控制器、剧情agent或观众事件驱动；但没有公共API、SDK或代码。
- **成熟度 / 证据 / 重要性：** 作者系统demo；官方论文声称，硬件数量与成本未披露；高、必读。

### 2. Fortnite LLM Conversations正式进入可发布节点

- **类型 / 时间 / 主线：** 平台产品、Verse API、角色运行时；正式节点2026-07-30；Coding × 互动作品、数字角色。
- **官方链接：** [上线说明](https://www.fortnite.com/news/publish-islands-with-llm-conversations-starting-july-30)、[开发文档](https://dev.epicgames.com/documentation/fortnite/llm-conversations-in-unreal-editor-for-fortnite)、[Verse Conversations模块](https://dev.epicgames.com/documentation/fortnite/verse-api/unrealenginedotcom/conversations)。
- **相对上期的新变化：** 上期仍需等待美国时间上线；本期已跨过官方承诺日期，且开发文档补齐`ai_session`、工具能力、历史压缩和异常类型。
- **核心贡献：** `persona_component`将语音NPC连接到Scene Graph；`ai_session`维护历史、固定条目和可注册capability。事件包括开始/停止收听、开始/停止说话、插话、审核后的最终消息、prompt失败和限流错误。
- **Coding接口 / 场景：** Verse代码可监听`CommitSayEvent`、`PromptFailureEvent`和`StopSayEvent`，把对话结果连接到任务、奖励、动画或世界事件；传统Conversation Device可继续承担确定性剧情。
- **产品与治理：** 底层采用Gemini 3.1 Flash-Lite和ElevenLabs Eleven v3；36个角色带平台定义声音/persona。官方称普通语音与转录不保存，举报时提交文本日志，并可能根据容量引入限流。
- **成熟度 / 证据 / 重要性：** 正式产品平台；官方产品与API文档；高、必读。

### 3. CG-World：从工业CG工程提取完整世界状态

- **类型 / 时间 / 主线：** dataset、数据协议、benchmark；2026-07-29；世界模型基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26452)、[完整数据协议](https://arxiv.org/html/2607.26452v1)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 约85万段1—5秒的生产级CG片段同时记录语义、空间结构、骨骼与控制器状态、运动曲线、相机、灯光、物理缓存、接触事件及多pass渲染；其中约43.35万段具完整多模态对齐，51万段可重算物理。
- **反事实设计：** 提供观察、动作、机制和严格反事实分支；公开计划包含10万条观察干预、5000条动作反事实和5000条机制反事实。
- **Coding接口 / 场景：** DCC或引擎导出器可把scene graph、rig、solver和render pass序列化成统一样本；适合训练状态条件视频模型、动作预测器、虚拟制作检查器和VLA。
- **限制：** 数据尚未发布；作者承诺在论文接收或camera-ready前开放。L1/L2及部分L3仅限非商业研究，完整工程文件需审核。
- **成熟度 / 证据 / 重要性：** 数据协议与作者实验，待发布；高、必读。

### 4. ContactFlow：以接触点轨迹统一人类与机器人动作

- **类型 / 时间 / 主线：** 论文、机器人实机demo；2026-07-29；世界模型、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26579)、[方法与实机结果](https://arxiv.org/html/2607.26579v1)。
- **相对上期的新变化：** 全新条目；与RoFacto的完整机器人渲染不同，它只编码物体侧3D接触点轨迹。
- **核心贡献：** 将接触点轨迹投影到图像空间形成7通道条件，使Wan 2.1/2.2视频模型可以混合学习DROID、Taste-ROB、TACO、OakInk及LIBERO中的人手和机器人操作。
- **Coding接口 / 场景：** VLA先提出轨迹；URDF、RGB-D、对象mesh和相机标定生成Contact Flow；世界模型想象结果；Gemini判断是否成功；通过后才在Franka Panda上执行。
- **关键结果：** 作者报告在10项未见桌面任务中正确预测8项真实结果；直接运行π0.5未成功完成任何测试。但样本很小，且作者明确承认推理远非实时。
- **成熟度 / 证据 / 重要性：** 实机作者实验，无公共代码；高、必读。

### 5. Video Representation Regularization：用有效秩诊断长rollout崩溃

- **类型 / 时间 / 主线：** 论文、训练方法、评测指标；2026-07-29；世界模型基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.27036)。
- **相对上期的新变化：** 全新条目；补充上期FreqForcing的频域解释。
- **核心贡献：** 发现滑窗式自回归视频开始漂移时，模型hidden representation的有效秩会显著下降；单纯扩大训练数据并未提高抗漂移性，作者据此加入轻量representation regularization。
- **关键结果：** 相对Diffusion Forcing，作者报告VBench Aesthetic Quality由38.65升至55.56，Imaging Quality由44.37升至72.08。
- **Coding接口 / 场景：** 训练框架可把effective rank作为rollout健康指标；在线服务也可据此触发memory刷新、回滚、切换渲染器或终止不可信分支。
- **成熟度 / 证据 / 重要性：** 论文实测、无确认代码；中高、必读。

### 6. CineWeaver：把多镜头边界与参考资产路由写入推理图

- **类型 / 时间 / 主线：** 论文、项目demo；2026-07-29；Coding × 影视、视频基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26529)、[项目页](https://cineweaver.github.io/)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 不重新训练视频模型，通过gap frame、transition-aware masked attention、逐镜头条件、隔离式VAE解码制造明确切镜；shot-routed reference把人物、背景或构图参考绑定到指定镜头；anchor token维持全片色调和外观。
- **Coding接口 / 场景：** 分镜JSON可以显式列出镜头、参考资产和切换点，运行时据此生成attention mask和reference routing；适合分支电影、广告版本化与虚拟制片预演。
- **成熟度 / 证据 / 重要性：** 项目demo；代码标注“soon”；中高、可读。

### 7. FilmBench v2＋FilmOps：影视生成获得可编程质量门

- **类型 / 时间 / 主线：** benchmark、dataset、GitHub；v1为2026-07-27，v2及资源更新为2026-07-29；Coding × 影视评价。
- **官方链接：** [论文v2](https://arxiv.org/abs/2607.24241)、[FilmBench数据](https://huggingface.co/datasets/skylenage/FilmBench)、[FilmOps GitHub](https://github.com/Neo-yk/FilmOps)。
- **相对上期的新变化：** 本周期新增v2、Apache-2.0数据入口及可运行FilmOps代码。
- **核心贡献：** 1169个导演审核prompt覆盖20类电影，其中1056个为多镜头；评价分为3个一级、12个二级和35＋3个细项。FilmOps实现景别、构图、机位、色调、人物布局和运镜六类算子。
- **关键结果：** 自动评价的模型级排序与行业专家达到作者报告的Spearman 0.95/0.96；多镜头相对单镜头平均下降7.9分，最弱模型下降22.8分。
- **Coding接口 / 场景：** Python pipeline、YAML配置、测试和安装验证脚本可直接加入CI，对生成镜头做自动验收与失败重试。
- **成熟度 / 证据 / 重要性：** 已开源代码和数据；权重需单独下载，DINOv3等组件需另核许可证；高、必读。

### 8. MoSAIC：身体分区级角色动作风格路由

- **类型 / 时间 / 主线：** 论文、角色动作编辑；2026-07-28；数字角色、虚拟制作。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26304)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 将内容动作与参考动作按身体区域分解，根轨迹采用独立条件通路，允许头、手臂、躯干或腿分别绑定不同参考。
- **关键结果：** 作者在128段动作、896种路由条件上报告：未选择区域误差由70.64降至66.45毫米，非目标区域泄漏由18.08降至9.88毫米。
- **Coding接口 / 场景：** 动画图或脚本可用`body_region → reference_clip`映射生成局部动作；可服务NPC手势、虚拟讲师和角色表演编辑。
- **成熟度 / 证据 / 重要性：** 研究原型，未确认代码/引擎插件；中、可读。

### 9. Two2Four：普通人体动作驱动四足虚拟角色

- **类型 / 时间 / 主线：** 论文、角色动画；2026-07-28；虚拟形象、虚拟制作。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26108)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 两阶段扩散模型只用四足动作训练，却可接收普通人体动作作为结构化条件；支持走、跑、跳、坐卧，以及头部或单肢局部操控。
- **Coding接口 / 场景：** 摄像头或动捕数据经过人体骨架提取后进入动作生成器，再输出到四足rig；可构建虚拟宠物、实时木偶和低成本动物预演。
- **成熟度 / 证据 / 重要性：** 作者实验，无确认代码和实时指标；中、可读。

### 10. AtlasLC：面向XR资产库的3DGS压缩

- **类型 / 时间 / 主线：** 论文、3DGS部署基础设施；2026-07-29；数字资产、空间计算。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26525)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 对已经发布的对象级Gaussian资产进行source-free、training-free压缩，不需要原始图片、相机或逐资产优化；通过局部竞争剪枝和确定性atlas packing减少预处理。
- **关键结果：** 作者报告atlas准备最高加速25倍，端到端压缩最高5倍；在近似体积下节省约6%—8%码率。
- **Coding接口 / 场景：** 适合作为资产服务器的离线构建步骤，压缩商品、道具或数字角色配件后按需传输至XR客户端。
- **成熟度 / 证据 / 重要性：** ISMAR 2026论文实测，未确认代码；中、可读。

### 11. FAS-R1：解释型人脸防伪进入多任务MLLM

- **类型 / 时间 / 主线：** 论文、dataset、身份安全；2026-07-29；数字人安全。
- **官方链接：** [论文](https://arxiv.org/abs/2607.26432)。
- **相对上期的新变化：** 全新条目。
- **核心贡献：** 3B模型同时输出真实性、攻击类型、伪造区域及文字解释；先用FAS-R1-23K长推理数据监督微调，再用难度感知GRPO处理化妆、面具等困难攻击。
- **关键结果：** 98.75%真实性准确率、93.33%攻击类型准确率均为作者域内结果，不能直接推断真实产品误报率。
- **Coding接口 / 场景：** 可作为数字员工登录、真人授权采集和直播开播前检查的辅助模块；最终决策仍应结合活体传感器和风险规则。
- **成熟度 / 证据 / 重要性：** 论文实测；代码“即将发布”；中、仅跟踪。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** 相对昨日暂无新的高质量speech-driven talking head、唇形或原生音视频数字人。MoSAIC和Two2Four扩展的是可编辑角色动作，不应误报为对话数字人升级。

**3D/4D表示。** AtlasLC改善对象级3DGS的传输和实例化成本，但没有解决头像表情、毛发、服装与rig统一。此前FA-LAM、DynHair等仍是本窗口内更直接的人头/毛发表示进展，本日无实质更新。

**实时交互。** 最强新增来自Fortnite运行时而非神经头像：语音、persona、会话历史、插话事件、审核和游戏状态已经进入同一发布平台。Visko可持续生成视频，但还不能证明人物身份、口型或对话轮次稳定。

**音视频与情感表达。** 本日没有超过TaoMate、OmniMate、STEER和Ripple的新模型。尤其需要继续区分“24/28/30 FPS吞吐量”与“低首帧延迟、可插话、全双工感知”。

**工程部署。** 当前最可产品化的是传统3D角色＋LLM/TTS＋事件API；神经视频头像仍缺少并发、失败回退、内容审核和SLA。角色动作生成更适合作为离线资产管线或预演工具。

**安全与身份治理。** Fortnite提供声音授权、IP persona锁定、家长控制和举报日志。FAS-R1增加可解释防伪，但尚无跨设备、压缩视频和对抗攻击下的第三方验证。

## 5. 世界模型进展

**架构与训练。** Visko把流式生成、memory、few-step distillation、GRPO与多GPU serving合为一体；Representation Regularization则把有效秩下降识别为rollout失稳前兆。

**可控/可玩生成。** 本日新增控制以“提示事件”和“接触点轨迹”为主。前者适合实时叙事和直播，后者适合机器人操作；它们都比自然语言提示更容易由程序生成和验证。

**长时与物理一致性。** Visko的一小时展示仍是作者单方诊断，不能等同于对象状态持续一小时正确。CG-World的反事实机制数据和ContactFlow的实机结果更接近因果验证，但覆盖范围有限。

**空间表示。** 接触点、骨骼状态、物理缓存、相机、灯光及多pass渲染正在成为视频模型与引擎之间的公共协议。像素不再是唯一世界状态。

**机器人、游戏与仿真。** ContactFlow展示“策略提出—视频想象—VLM核验—实机执行”；CG-World提供训练这种闭环所需的中间状态和干预数据；Fortnite展示同一分层思想在游戏NPC中的产品版本。

**实时部署与评测。** Visko报告24 FPS和低于一秒提示响应，但多GPU数量、显存及成本未披露。ContactFlow明确非实时。有效秩可以成为新的长时服务观测指标，但需要跨模型验证。

## 6. Coding × 新型互动作品

1. **观众/导演事件 → Visko会话状态 → 持续生成视频 → 互动直播与分支电影**  
   代码负责事件时间戳、prompt version、队列、回滚和内容审核；模型负责连续画面与风格变化；播放器负责渐进交付。已有作者系统证据，公共API仍缺失。

2. **玩家语音 → Fortnite `persona_component` → Verse事件/能力 → 可发布生成式NPC**  
   LLM负责自由语言，Verse负责任务、奖励、权限和失败处理，Unreal负责角色、物理和网络同步。已有正式平台与API，是本周期最强落地链路。

3. **scene graph/solver状态 → CG-World schema → 状态条件模型 → 规则可验证的生成世界**  
   DCC或引擎导出状态与反事实分支，模型学习动作后果，规则检查器比较预测和真实状态。数据尚未开放，因此当前属于有实验支撑的工程方向。

4. **VLA候选轨迹 → URDF/接触点渲染 → ContactFlow世界模型 → 机器人预执行核验**  
   代码维护标定、运动学和安全边界；模型预测接触结果；VLM只做高层成功判断；低层控制器保留最终执行权。已有10项实机小样本证据。

5. **分镜JSON → CineWeaver路由与attention mask → 多镜头生成 → 自动广告/互动电影**  
   代码指定镜头、参考资产和切点；生成模型承担人物、场景与镜头画面；传统剪辑系统负责音轨、版本与人工精修。项目demo存在，代码待发布。

6. **生成镜头 → FilmOps Python pipeline → 自动质量门 → 虚拟制作CI**  
   流水线检查景别、构图、机位、色调、人物布局和运镜；失败镜头可自动重试或送人工。代码与数据均已开放，适合立即验证。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | Fortnite Persona＋Verse＋Scene Graph | 语音、插话、工具与任务事件 | 正式平台 | 延迟、限流未披露 | 容量、英语限定、内容治理 | [官方文档](https://dev.epicgames.com/documentation/fortnite/llm-conversations-in-unreal-editor-for-fortnite) |
| 互动直播 | Visko＋多GPU流式服务 | 运行中切换prompt | 作者demo | <1秒提示可见；4K/24 FPS | 硬件、API、并发成本 | [论文](https://arxiv.org/abs/2607.26694) |
| 影视/虚拟制作 | CineWeaver＋FilmOps＋分镜系统 | 镜头级参考、切换和自动验收 | demo＋开源评测 | 生成成本未披露 | 生成代码缺失、人工精修 | [FilmOps](https://github.com/Neo-yk/FilmOps) |
| 直播与电商 | Visko/数字人＋商品状态服务 | 用户事件改变展示内容 | 机会判断 | 未披露 | 商品真实性、延迟、审核 | [Visko](https://arxiv.org/abs/2607.26694) |
| 品牌互动 | CineWeaver＋参考资产库 | 按用户选择生成不同镜头 | 研究demo | 未披露 | 品牌一致性和版权 | [项目页](https://cineweaver.github.io/) |
| 教育培训 | Fortnite NPC＋确定性课程状态机 | 问答触发教学事件 | 平台可构建 | 未披露 | 事实可靠性、未成年人治理 | [官方说明](https://www.fortnite.com/news/publish-islands-with-llm-conversations-starting-july-30) |
| 企业数字员工 | Persona/RTC＋业务工具 | 语音问答和function/tool调用 | 组件级可用 | 未披露 | 权限、审计、SLA | [Verse模块](https://dev.epicgames.com/documentation/fortnite/verse-api/unrealenginedotcom/conversations) |
| 陪伴与社交 | 生成NPC＋长期会话 | 多轮对话与插话 | 平台产品早期 | 未披露 | 隐私、角色越界、成本 | [Persona API](https://dev.epicgames.com/documentation/fortnite/verse-api/unrealenginedotcom/conversations/persona_component) |
| 空间计算 | AtlasLC＋3DGS资产服务 | 对象资产按需加载 | 论文原型 | 最高25×准备加速 | 代码、客户端兼容 | [论文](https://arxiv.org/abs/2607.26525) |
| 机器人/仿真 | ContactFlow＋VLA＋RGB-D＋URDF | 候选动作想象与核验 | 实机作者实验 | 非实时；成本未披露 | 标定、深度误差、安全 | [论文](https://arxiv.org/html/2607.26579v1) |

## 8. 可复现资源与开发者入口

- [FilmOps](https://github.com/Neo-yk/FilmOps)：Apache-2.0代码，包含Python package、YAML配置、示例、测试与安装检查。需要Python 3.9＋、PyTorch、FFmpeg；人物布局和运镜算子依赖InternVL3-14B。最小路径是先只启用shot scale与color tone，分析10段内部镜头。
- [FilmBench数据](https://huggingface.co/datasets/skylenage/FilmBench)：Apache-2.0数据卡，包含中英文prompt、参考类型及多模型视频结果。适合建立内部模型回归集；源电影及衍生内容的商业使用仍应单独核验。
- [Fortnite Conversations文档](https://dev.epicgames.com/documentation/fortnite/llm-conversations-in-unreal-editor-for-fortnite)：无需自部署模型，最小路径是创建一个persona，监听开始说话、停止说话、插话和失败事件，再将审核后的最终消息连接到一个任务状态。
- [Visko Orbis](https://arxiv.org/html/2607.26694v1)：只有论文和系统结构，无模型、代码或API；适合复现会话状态机，不适合投入模型复现。
- [CineWeaver](https://cineweaver.github.io/)：项目页有完整demo和路由设计，代码仍为“soon”。
- CG-World、ContactFlow、MoSAIC、Two2Four、AtlasLC和Video Representation Regularization均未确认公共代码或权重；暂不建议投入完整环境搭建。
- FAS-R1代码仍为“即将发布”，当前只能复查论文协议，不能验证其跨域防伪能力。

## 9. 系统架构与技术趋势判断

本周期明显升温的是四种可编程中间层：

`事件与会话状态`、`显式世界状态`、`几何/接触协议`、`自动评价算子`

正在形成的可复用架构为：

`用户或agent事件 → 权威状态/分镜/动作规划 → 结构化模型条件 → 生成模型持续rollout → 自动评价与安全门 → 引擎/播放器/机器人执行`

代码负责权威状态、权限、时间、工具调用、网络、回滚和日志；生成模型负责开放语言、表演、视觉外观和难以手写的环境响应；传统引擎负责碰撞、物理、资产、动画和确定性结果；评价模型负责筛选而不应拥有最终安全权限。

长时世界模型的研究重点正从“增加历史帧”转向三件事：固定预算memory、可观测的状态协议，以及对rollout失稳的内部诊断。数字人则尚未完成同等程度的接口标准化：音频chunk、表情、视线、插话和角色状态仍由不同模型分别处理。

需要降级看待的内容包括：Visko的“一小时无明显漂移”和4K实时性，因为硬件与第三方复现缺失；CG-World的“dataset”尚未真正开放；ContactFlow的8/10实机结果样本过小；FAS-R1的高准确率属于域内作者实验。相较之下，Fortnite API与FilmOps具有更强工程证据。

## 10. 论文精读候选

1. [Visko Orbis 1.0](https://arxiv.org/html/2607.26694v1)：重点读3.3 Memory、4.1 Live State、4.4 Progressive Delivery和5 Evaluation。价值在于完整描述持久视频服务；风险是模型与硬件不可得。
2. [CG-World](https://arxiv.org/html/2607.26452v1)：重点读3.3反事实分支、4.2规模和4.5许可证。它比普通视频dataset更接近引擎运行日志；风险是数据尚未发布。
3. [ContactFlow](https://arxiv.org/html/2607.26579v1)：重点读3.1接触编码、3.3推理数据处理和4.1实机流程。与RoFacto相比条件更紧凑、更物体中心；风险是非实时且依赖标定。
4. [Video Representation Regularization](https://arxiv.org/abs/2607.27036)：重点读有效秩与漂移时刻的相关实验、数据规模消融和正则项。风险是需要验证有效秩是原因还是伴随现象。
5. [FilmBench v2](https://arxiv.org/html/2607.24241v2)：重点读4.1 FilmOps、5.2人类一致性和5.9多镜头难度。适合建立生成式影视CI；风险是自动评价可能固化特定电影语言偏好。

## 11. 下周跟踪与可行动建议

继续跟踪：

1. Visko是否开放API、模型、GPU数量、单会话显存和并发成本。
2. Fortnite首批公开LLM NPC岛屿的响应延迟、限流、插话可靠性和Creator Portal分析。
3. CG-World是否给出真实下载日期、文件格式、样例schema及非商业许可证全文。
4. ContactFlow是否开放接触点提取、symbolic twin和机器人执行代码。
5. FilmOps各算子权重是否完整可下载，DINOv3和InternVL组件能否用于商业制作。
6. CineWeaver是否开放代码，并支持通用Wan、LTX或其他视频模型。
7. effective-rank指标能否预测ABot、Wonder、Visko等不同架构的失稳。
8. 新一代数字人是否同时报告首帧延迟、插话、视觉反馈、并发和身份安全，而非只报告FPS。

本周适合动手验证：

1. **Fortnite可打断任务NPC**  
   目标：验证生成对话与确定性游戏规则能否安全结合。组件：UEFN、`persona_component`、Verse任务状态机。难点：插话、审核替换和超时。成功判据：任何模型回复都不能直接修改奖励；只有通过白名单capability和规则检查的事件可以推进任务。

2. **FilmOps生成镜头CI**  
   目标：把影视语言评价加入现有视频生成流水线。组件：FilmOps、FFmpeg、10—30段内部镜头。难点：权重许可和InternVL显存。成功判据：景别、色调和运镜三个算子能输出结构化JSON，且对人工标注达到至少80%一致。

3. **世界状态最小schema**  
   目标：复现CG-World的核心思想。组件：Unity/Godot、scene graph、物理缓存、相机和事件日志。难点：状态—帧时间对齐。成功判据：同一初始状态可重放事实分支和至少三个动作/机制反事实分支，所有结果具有可追踪父分支。

4. **ContactFlow轻量验证器**  
   目标：验证接触条件是否优于完整机器人mask。组件：RGB-D、URDF、对象mesh、两种动作渲染条件和小型视频模型。难点：相机标定与3D接触估计。成功判据：在20条候选轨迹上，Contact Flow对成功/失败结果的排序优于无条件和完整actor-mask基线。
