# 数字人、世界模型与互动作品研究日报：2026-07-25

检索窗口：2026-07-19 至 2026-07-25。  
日期说明：当前为周六，7 月 25 日尚无新的 arXiv 工作日批次。本报告重点补充 7 月 24 日完整公开、但未进入 7 月 24 日日报的条目；论文实际提交时间多为 7 月 23 日，不将 arXiv 列表展示日期误作提交日期。

## 1. 本日摘要

本周期最重要的新信号是：coding agent 不再只为生成模型编写调用胶水，而开始直接在物理引擎中生成、运行、观察并迭代修改可执行世界。GS-Agent通过多agent编写Genesis仿真代码，将自然语言转换为包含刚体、流体、软体、材质、相机和灯光的可查询4D世界；其输出是模拟状态而非仅有像素的视频。Lumera从单图恢复可导入标准引擎的对象实例和参数化灯光，补足“生成场景无法继续编辑”的资产管线缺口。GraphVid则把多对象视频控制提升为有向交互图编辑，使代码可表达“谁推动谁、谁跟随谁”，但当前约200秒的生成时间仍不支持实时运行。世界模型运行时继续加速：Ms. Forcing在单张H200上报告22.84 FPS，SANA-Video 2.0通过混合线性—softmax注意力将720p、5秒视频压至13.06秒，但后者仍不是交互帧率。数字人方向出现FA-LAM单图4D Gaussian全头重建和GroupVideo多人身份视频生成，不过均未证明语音驱动、turn-taking或商用引擎实时部署。HyWorldVLA显示驾驶世界模型正在从“生成可看的未来”转向“以未来latent直接驱动动作专家”。安全侧新增ResponseGuard和XPlainVerse评测，但前者声称“完整发布”的仓库实际上仍以占位接口为主，不能按可部署组件计算。总体成熟度仍以论文、项目demo和部分权重为主，本周期没有新的客户规模化部署、SLA、并发成本或商业收入证据。

## 2. 今日变化雷达

| 主线 | 新增强度 | 最重要信号 | 成熟度变化 | 相对上期变化 |
|---|---:|---|---|---|
| 数字人 / 虚拟形象 | 中 | FA-LAM将单图头像扩展到静态3D与流式4D Gaussian全头；GroupVideo处理多人身份分离 | 有模型能力展示，无公开实时SDK、权重或产品部署 | 上期数字人增量偏弱；本期恢复到3D/4D头像与多人身份一致性，但仍非对话数字人 |
| 世界模型 | 高 | GS-Agent输出可执行仿真而非像素；Lumera恢复引擎对象与参数灯光；HyWorldVLA连接未来预测和动作 | 从视频demo向可查询物理状态、可编辑资产和决策接口推进 | 相比上期“状态寄存器＋生成渲染”，新增了agent自动编写仿真世界和引擎原生资产管线 |
| Coding × 互动作品 | 很高 | 多agent生成仿真代码、交互图成为视频控制接口、结构化世界可反复编辑 | GS-Agent有代码级项目演示；GraphVid有权重但代码链接失效；尚无端到端产品 | coding从部署优化进一步进入世界构建、物理参数搜索、镜头与灯光迭代 |

## 3. 最值得关注的 12 个进展

### 1. GS-Agent：多agent通过代码创建可模拟4D物理世界

- **类型 / 时间 / 主线：** 论文、agent系统、项目demo；2026-07-23；世界模型、Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21522)、[项目页与生成代码示例](https://umass-embodied-agi.github.io/gs-agent/)。
- **相对上期的新变化：** 全新条目；相比上期AlayaRenderer“引擎状态→生成画面”，GS-Agent处理的是“自然语言→可执行引擎世界”。
- **核心贡献与技术：** 将任务拆为资产选择、材质调整、放置、运动、相机和灯光agent；agent编写Genesis物理引擎代码，根据RGB、法线、分割和粒子状态反馈迭代。覆盖刚体、MPM流体/沙、软体及粒子交互。
- **Coding接口 / 场景：** 输出包含`scene.add_entity`、材料、碰撞、时间步、相机和控制函数的Python程序，可继续修改、重放和查询。适合物理演示、广告特效预演、机器人合成数据和交互式科学教育。
- **成熟度 / 证据 / 重要性：** 项目demo与论文作者实验；未见完整GitHub、agent编排代码或成本数据；高、必读。

### 2. Lumera：单图恢复引擎原生对象和参数化灯光

- **类型 / 时间 / 主线：** 论文、benchmark、参考管线；2026-07-23；世界模型、3D资产基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.20889)、[项目页](https://haidilao0328.github.io/Lumera/)。
- **相对上期的新变化：** 全新条目；把单图3D重建目标从“视觉上像”推进到“对象、灯光可检查和编辑”。
- **核心贡献：** Lumera-2K来自2,513个UE5工程，包含3.73M组件、63M对象实例、102.6K参数灯和95.1K视角；VLM预测对象框及灯光的坐标、RGB和强度，再结合单对象mesh、HDR环境估计与有界agent修正。
- **限制：** 灯光场景召回率虽为0.998，但0.5米阈值下单灯定位F1仅0.209，跨引擎泛化尚未证明。
- **Coding接口 / 场景：** 结构化对象框和灯光tuple可转换为UE、Unity或Blender场景命令；适合虚拟制片复景、数字孪生和可编辑互动空间。
- **成熟度 / 证据 / 重要性：** benchmark与作者管线，代码、数据下载未确认；高、必读。

### 3. FA-LAM：单图静态3D与流式4D Gaussian全头

- **类型 / 时间 / 主线：** 论文、头像模型；2026-07-23；数字人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.20922)。
- **相对上期的新变化：** 全新条目；本期最直接的3D/4D数字人增量。
- **核心贡献：** 用对称及语义注意力正则减少头部错误关注，并以双阶段训练分离大视角外观补全与动画学习；自回归、可见性感知token融合支持多视图及流式4D重建。
- **Coding接口 / 场景：** 理论上可将表情或多视图帧流转换为持续更新的Gaussian头像资产，用于虚拟会议、数字员工和游戏头像；当前未公开导出格式、blendshape/骨骼映射或Unity/UE插件。
- **成熟度 / 证据 / 重要性：** 研究原型、论文作者实验，无确认代码和速度；高、必读。

### 4. Ms. Forcing：单张H200上的22.84 FPS流式视频生成

- **类型 / 时间 / 主线：** 论文、流式运行时；2026-07-23；世界模型基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.20940)。
- **核心贡献：** 对高噪声状态使用更粗patch，并按查询尺度调整可见KV密度，活动窗口token减少45%；固定窗口位置使计算图保持静态，便于编译和硬件优化。
- **关键结果：** 作者报告单张H200达到22.84 FPS，比Rolling Forcing快39.6%，同时改善长短视频VBench。
- **Coding接口 / 场景：** 可作为动作条件世界模型、实时视频编辑或数字人渲染层的滚动生成内核；尚无公开API、代码和首帧延迟。
- **成熟度 / 证据 / 重要性：** 论文实测；高、必读。

### 5. SANA-Video 2.0：混合注意力与Sol-Engine长视频加速

- **类型 / 时间 / 主线：** 论文、项目、代码库；2026-07-23；视频基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21553)、[项目页](https://nvlabs.github.io/Sana/Video2/)、[Apache-2.0代码库](https://github.com/NVlabs/Sana/)。
- **核心贡献：** 以3:1比例混合线性和softmax注意力，用Attention Residual在深层复用全秩锚点；Sol-Engine进一步组合kernel fusion、cache和稀疏注意力。
- **关键结果：** 作者报告单张H100、40步下，5B模型生成720p/5秒视频耗时13.06秒；60秒设置的DiT前向比匹配softmax基线快3.2倍。
- **工程判断：** 主仓库已有视频训练/推理目录，但项目页仍标记“VIDEO SOURCE COMING SOON”，2.0专属权重和完整复现入口尚不清晰。
- **成熟度 / 证据 / 重要性：** 部分开源、作者实测；高、必读。

### 6. GraphVid：以有向交互图控制多对象视频

- **类型 / 时间 / 主线：** 论文、dataset、权重、交互demo；2026-07-23；Coding × 互动作品。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21580)、[项目页](https://plan-lab.github.io/projects/graphvid/)、[Apache-2.0权重](https://huggingface.co/PLAN-Lab/GraphVid)。
- **核心贡献：** 节点表示对象，边表示push、pull、lift、follow等关系；GINEConv把有向关系转换为LTX-Video 2B的条件token。GraphVid-Bench含27,504段交互视频。
- **Coding接口：** 场景图JSON天然适合由UI、剧情agent或规则系统编辑，优于逐对象手绘轨迹。
- **限制：** 标准121帧/24 FPS视频推理约200秒，不实时；项目页的GitHub代码链接返回404。Hugging Face上有约64.3 GB权重，但空模型卡中的通用Diffusers示例不足以证明可运行。
- **成熟度 / 证据 / 重要性：** 权重已上传、代码不完整；高、必读。

### 7. HyWorldVLA：混合像素与latent世界模型驱动驾驶动作

- **类型 / 时间 / 主线：** 论文、驾驶VLA；2026-07-23；世界模型、工业仿真。
- **官方链接：** [论文](https://arxiv.org/abs/2607.20988)。
- **核心贡献：** 预训练阶段同时预测视频latent并重建像素，协同微调阶段则让未来latent直接进入action expert生成轨迹，兼顾像素监督和latent抗噪性。
- **Coding接口 / 场景：** 可作为自动驾驶栈中的“传感器历史→未来表征→规划轨迹”模块，接入NAVSIM回放和闭环评测。
- **成熟度 / 证据 / 重要性：** NAVSIM v1/v2作者实验，无代码、实车或实时延迟；中高、必读。

### 8. Structured Dynamics Model：分离相机运动与对象运动

- **类型 / 时间 / 主线：** 论文、benchmark；2026-07-23；世界模型表示。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21576)、[项目页](https://lukasknobel.github.io/projects/StructuredDynamics/)、[GitHub占位仓库](https://github.com/lukasknobel/StructuredDynamics)。
- **核心贡献：** 在冻结图像ViT特征上学习主运动token和残差运动token，显式区分相机引起的变化与对象动力学；新增ProbeMotion评测。
- **Coding接口 / 场景：** 可用于世界模型控制信号解析、自动镜头与对象运动分离、视频资产分析。
- **成熟度 / 证据 / 重要性：** 论文与项目页；GitHub明确写有“Code coming soon”；中、可读。

### 9. GroupVideo：多人身份定制视频中的身份分离

- **类型 / 时间 / 主线：** 论文、dataset；2026-07-23；数字人、视频创作。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21027)。
- **核心贡献：** 用多模态身份对齐、语义perceiver、ID定位、边界框和mask正则缓解多人场景的身份混淆与“贴脸”现象；作者构建20,000段多人视频数据。
- **Coding接口 / 场景：** 脚本可将人物ID、空间区域和场景描述绑定，用于多人广告、虚拟组合或互动电影镜头。
- **成熟度 / 证据 / 重要性：** 离线论文原型，无代码、数据许可或实时证据；中、可读。

### 10. ResponseGuard：67.6毫秒的流式多模态安全判定

- **类型 / 时间 / 主线：** 论文、GitHub；2026-07-23；实时安全基础设施。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21401)、[GitHub](https://github.com/ndb796/ResponseGuard)。
- **核心贡献：** 不生成推理链，直接从请求、图像和当前响应的池化表示输出风险概率；作者报告A6000上67.6毫秒，对比推理式guard约10.12秒。
- **Coding接口：** 设计为每个句子边界重新评分并触发流式中断，适合数字人对话和生成世界聊天入口。
- **证据审计：** 论文称完整发布，但仓库中的checkpoint、训练代码、拦截harness和Colab仍全部标记“coming soon”；0.5阈值还会误中断28.5%的良性回答。
- **成熟度 / 证据 / 重要性：** 论文与接口预览，非可复现发布；中、可读。

### 11. XPlainVerse可解释深伪检测挑战

- **类型 / 时间 / 主线：** benchmark、竞赛、评测代码；2026-07-23；数字人安全。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21007)、[评测仓库](https://github.com/Abhijeet8901/XPlainVerse-ACMChallenge)。
- **核心贡献：** 同时要求真假分类、面向专业用户的复杂解释和面向普通用户的简明解释；基于百万级XPlainVerse并加入实体、视觉证据与解释简洁度指标。
- **Coding接口：** 提供JSONL提交schema、Python evaluator、vLLM/OpenAI-compatible judge入口及CodaBench。
- **成熟度 / 证据 / 重要性：** 评测代码可运行，但仓库未见明确许可证，完整数据条款需核验；中、可读。

### 12. GLAM-SLAM：长序列户外实时Gaussian建图

- **类型 / 时间 / 主线：** 论文、GitHub；2026-07-23；空间表示、机器人。
- **官方链接：** [论文](https://arxiv.org/abs/2607.21416)、[GitHub](https://github.com/pmermigkas/GLAM-SLAM/)。
- **核心贡献：** 以特征SLAM负责轻量跟踪，稀疏anchor grid和场景分区负责可扩展Gaussian建图，并用极线约束进行flow densification。
- **场景：** 机器人和空间计算设备可持续构建可渲染地图，为数字角色定位、遮挡和持久空间记忆提供底层表示。
- **证据审计：** 论文称代码公开，但仓库当前仅README、一个commit，无实现和许可证。
- **成熟度 / 证据 / 重要性：** 论文原型、代码未实际开放；中、仅跟踪。

## 4. 数字人 / 虚拟形象能力进展

**生成与驱动。** FA-LAM解决单图头像的大视角头部补全和动画训练冲突，GroupVideo解决多人身份混合，但两者均未覆盖语音到口型、视线、手势及可打断对话。当前不能将它们外推为数字员工系统。

**3D/4D表示。** FA-LAM的Gaussian全头比纯2D视频更适合自由视角和重复驱动，但尚未证明可导出为标准mesh、骨骼或blendshape。数字人资产进入Unity、UE或WebGL仍需格式转换、LOD、碰撞代理和表情控制层。

**实时交互。** 本周期的实时突破主要来自Ms. Forcing，而非头像模型。它证明生成内核可以达到视频帧率，但没有给出音频条件、首帧延迟和身份漂移结果。

**音视频与情感表达。** 暂无新的原生音视频数字人、active listening、情感手势或turn-taking高质量新增。

**工程部署。** 短期可行路线是：FA-LAM离线生成头像资产，传统运行时处理RTC、ASR、LLM、TTS和打断，Gaussian渲染器或预训练动画模块执行表情；不应在每次对话时重建头像。

**安全与身份治理。** GroupVideo扩大多人身份生成能力，XPlainVerse提供可解释检测评测。生产系统仍需人物授权、参考资产版本、生成日志、水印和下架机制；检测器不能替代授权管理。

## 5. 世界模型进展

**架构与训练。** Ms. Forcing和SANA-Video 2.0分别从流式窗口的噪声冗余、长序列注意力复杂度下手；Structured Dynamics则尝试在latent中分离相机和对象运动。

**可控/可玩生成。** GraphVid将控制接口从像素轨迹提升为交互图；GS-Agent直接生成仿真代码；Lumera输出对象和灯光参数。结构化控制明显升温。

**长时与物理一致性。** GS-Agent的物理一致性来自传统求解器，不依赖视频模型“猜测”守恒关系。Ms. Forcing支持持续生成，但视觉连续仍不等于库存、碰撞或因果状态正确。

**空间表示。** Lumera的对象/灯光tuple、GLAM-SLAM的Gaussian地图及GS-Agent的实体/粒子状态分别对应可编辑场景、在线空间记忆和物理执行层。

**机器人、游戏与仿真。** HyWorldVLA让未来预测直接服务规划；GS-Agent可生成可查询合成环境；二者分别代表“模型内世界表征”和“代码外显仿真世界”。

**实时部署与评测。** Ms. Forcing的22.84 FPS具有实时意义；SANA-Video 2.0的13.06秒生成5秒视频仍为离线或准交互；GraphVid约200秒只能用于创作预演。

## 6. Coding × 新型互动作品

1. **自然语言 → 多agent生成Genesis代码 → 物理求解器 → 可反复编辑的科学/品牌体验**  
   agent负责资产、参数、镜头和代码修正；物理引擎负责确定性状态；生成模型负责理解需求与视觉判断。已有项目demo，完整agent代码未公开。

2. **单张场景图 → Lumera对象/灯光tuple → UE/Unity场景实例 → 可编辑虚拟制片复景**  
   模型恢复对象、mesh候选和灯光；导入脚本映射资产ID、坐标与光照；引擎承担碰撞、导航和实时渲染。论文有量化证据，引擎插件仍属本报告推断。

3. **用户编辑交互图 → GraphVid条件token → 多对象视频 → 互动电影分支预演**  
   剧情agent生成节点与有向边，用户调整关系，模型生成镜头结果。权重存在，但200秒推理和代码404使其目前更适合离线预演。

4. **头像照片 → FA-LAM Gaussian全头 → 表情驱动器/RTC → 自由视角数字角色**  
   FA-LAM负责身份资产，代码负责音频事件、表情参数、LOD和网络同步，传统渲染器负责显示。头像能力有论文证据，实时对话链路属于本报告推断。

5. **传感器历史 → HyWorldVLA未来latent → action expert → 驾驶轨迹**  
   世界模型负责未来表征，动作专家负责轨迹，传统安全栈负责碰撞检查和最终仲裁。已有NAVSIM实验，无实车和实时部署证据。

6. **流式生成内容 → ResponseGuard句边界评分 → 中断/降级状态机 → 安全数字人或NPC**  
   guard输出风险概率，代码决定停止、改写或切换模板回答。架构合理且有论文延迟数据，但公开实现仍未完成。

## 7. 工业应用与成熟度矩阵

| 场景 | 代表进展 / 技术栈 | 互动机制 | 当前成熟度 | 关键成本/延迟 | 主要阻碍 | 证据 |
|---|---|---|---|---|---|---|
| 游戏 | GS-Agent＋物理引擎＋状态服务器 | 语言修改实体、材质和运动后重模拟 | 项目demo | 未披露 | agent成本、资产许可、生成代码安全 | [GS-Agent](https://umass-embodied-agi.github.io/gs-agent/) |
| 影视/虚拟制作 | Lumera＋UE5＋mesh/HDR管线 | 单图复景后人工编辑对象和灯光 | 研究管线 | 未披露 | 灯光定位、跨引擎导出、人工精修 | [Lumera](https://arxiv.org/abs/2607.20889) |
| 直播与电商 | FA-LAM＋RTC＋TTS＋Gaussian渲染 | 语音事件驱动头像表情 | 机会判断 | 头像推理未披露 | 口型、视线、并发和审核 | [FA-LAM](https://arxiv.org/abs/2607.20922) |
| 品牌互动 | GraphVid＋场景图UI＋视频模型 | 用户修改人物/商品交互关系 | 离线demo | 121帧约200秒 | 非实时、代码缺失、品牌一致性 | [GraphVid](https://plan-lab.github.io/projects/graphvid/) |
| 教育培训 | GS-Agent＋仿真脚本＋教学状态机 | 修改物理条件并观察结果 | 项目demo | 未披露 | 参数正确性、课程审核 | [GS-Agent](https://arxiv.org/abs/2607.21522) |
| 企业数字员工 | FA-LAM＋LLM/TTS＋ResponseGuard | 对话、工具调用和流式中断 | 组件级研究 | guard 67.6 ms；头像未披露 | 完整代码、权限和SLA | [ResponseGuard](https://github.com/ndb796/ResponseGuard) |
| 陪伴与社交 | FA-LAM＋长期记忆＋表情状态机 | 对话状态持续驱动头像 | 机会判断 | 未披露 | 情感安全、身份滥用、长期一致性 | [FA-LAM](https://arxiv.org/abs/2607.20922) |
| 空间计算 | GLAM-SLAM＋Gaussian地图＋角色定位 | 在线建图、持久遮挡和空间锚点 | 论文原型 | 论文称实时，具体FPS未披露 | 仓库为空、移动端预算 | [GLAM-SLAM](https://arxiv.org/abs/2607.21416) |
| 机器人/仿真 | GS-Agent＋Genesis＋VLA | 生成环境、rollout和策略评测 | 研究demo | 未披露 | sim-to-real、参数可信度 | [GS-Agent](https://umass-embodied-agi.github.io/gs-agent/) |
| 自动驾驶 | HyWorldVLA＋NAVSIM＋action expert | 未来latent驱动轨迹规划 | benchmark原型 | 未披露 | 闭环安全、实时性、实车泛化 | [HyWorldVLA](https://arxiv.org/abs/2607.20988) |

## 8. 可复现资源与开发者入口

- [SANA代码库](https://github.com/NVlabs/Sana/)：Apache-2.0，已有视频训练、推理、SANA-WM和streaming目录；2.0项目页仍显示视频源待发布。最小验证应先确认5B 2.0配置和权重是否实际存在，再复测单H100的720p/5秒延迟。
- [GraphVid权重](https://huggingface.co/PLAN-Lab/GraphVid)：Apache-2.0，约64.3 GB并含118 MB的`graph.pth`；代码链接404、模型卡为空，暂不建议直接下载。最小验证路径是等待官方仓库恢复并核对图schema。
- [GS-Agent项目页](https://umass-embodied-agi.github.io/gs-agent/)：公开多段Genesis Python代码，可手工复现单个物理场景；没有完整多agent系统、依赖锁定和许可证。
- [XPlainVerse评测仓库](https://github.com/Abhijeet8901/XPlainVerse-ACMChallenge)：已有conda环境、JSONL schema、Python evaluator、vLLM/OpenAI-compatible后端与CodaBench入口；许可证未明确。适合先跑`--skip-qwen`快速验证，再启动Qwen judge完成正式指标。
- [ResponseGuard仓库](https://github.com/ndb796/ResponseGuard)：当前只有结果、接口预览和设计说明，checkpoint、训练代码及streaming harness仍待发布，不可复现论文结果。
- [Structured Dynamics](https://github.com/lukasknobel/StructuredDynamics)：仅README，明确标注代码待发布。
- [GLAM-SLAM](https://github.com/pmermigkas/GLAM-SLAM/)：仅README和一个commit，与论文“代码公开”表述不一致，暂不可复现。
- FA-LAM、Lumera、HyWorldVLA和GroupVideo：未确认公开代码、权重或数据许可，不宜成为当前生产依赖。

## 9. 系统架构与技术趋势判断

明显升温的方向是：

- 以对象、灯光、场景图、物理实体和动作latent作为模型与代码之间的结构化协议；
- 让coding agent直接操作模拟器并通过视觉/状态反馈修正代码；
- 将视频世界模型定位为观察生成器或未来表征器，而非规则、物理和权威状态的唯一载体；
- 用混合线性注意力、多尺度patch和固定静态计算图降低流式运行成本。

正在形成的复用架构是：

`自然语言/玩家输入 → agent规划 → JSON/图/仿真代码 → 权威状态或物理引擎 → 生成渲染/未来预测 → 评测与安全guard → 用户观察`

其中代码负责规则、状态、权限、回退与可观测性；生成模型负责意图理解、外观合成、未来候选和资产补全；传统引擎负责碰撞、网络同步和确定性执行。

关键未解问题包括：数字人脸—手—身体—语音统一实时控制；从生成像素可靠反写对象状态；模型首次响应延迟和并发成本；agent生成仿真代码的沙箱与资源限制；跨引擎资产schema；人物、数据和3D资产许可。

需要降级看待的材料包括：ResponseGuard“完整发布”与实际占位仓库不一致；GLAM-SLAM论文称公开但仓库只有README；GraphVid虽有权重但代码404；SANA-Video 2.0项目展示充分，但专属权重和视频源入口仍不明确。这些不等于造假，但工程成熟度低于发布文案给人的直观印象。

## 10. 论文精读候选

1. [GS-Agent](https://arxiv.org/abs/2607.21522)：重点读多agent角色分工、代码执行反馈、物理引擎接口和与SWE-Agent的比较。差异是生成可执行世界而非视频；风险是完整系统未开源。
2. [Lumera](https://arxiv.org/abs/2607.20889)：重点读Lumera-2K构建、对象/灯光schema、有界agent修正和跨基线评测。风险是UE数据许可及跨Unity/Blender泛化。
3. [FA-LAM](https://arxiv.org/abs/2607.20922)：重点读注意力错误分析、双阶段训练、流式4D token融合。风险是无代码、无明确运行速度和引擎导出。
4. [Ms. Forcing](https://arxiv.org/abs/2607.20940)：重点读多尺度patch调度、MSSA计算量和H-DMD训练—推理匹配。风险是22.84 FPS依赖H200且未披露首帧延迟。
5. [GraphVid](https://arxiv.org/abs/2607.21580)：重点读场景图schema、GINEConv条件注入和MoveBench指标。差异是语义关系控制而非密集轨迹；风险是200秒推理与代码缺失。

## 11. 下周跟踪与可行动建议

继续追踪：

1. GS-Agent是否发布完整agent编排、Genesis依赖、prompt和评测代码。
2. SANA-Video 2.0的5B/14B权重、Sol-Engine实现及可复现延迟。
3. GraphVid的GitHub、数据集和正确推理脚本是否恢复。
4. FA-LAM是否公布头像生成速度、Gaussian格式和表情驱动接口。
5. Lumera-2K是否开放，以及输出能否真实导入UE或其他引擎。
6. ResponseGuard是否按承诺发布checkpoint和流式拦截harness。
7. Ms. Forcing在H100/消费级GPU上的首帧延迟、显存和动作条件表现。
8. HyWorldVLA是否出现闭环NAVSIM、代码或实车验证。

本周可动手验证：

1. **物理世界代码生成实验**  
   目标：复现GS-Agent项目页的一段刚体或粒子场景，再让coding agent只修改材质、速度和相机。组件：Genesis、Python、视觉评分器。难点：资产路径、求解器稳定性。成功判据：三次自然语言修改均生成可执行代码，且对象状态可查询、可重放。

2. **场景图驱动的互动电影控制层**  
   目标：先不运行GraphVid，用JSON定义人物、物体及`push/follow/lift`边，并由剧情agent修改。组件：React Flow或NetworkX、JSON Schema、任意预览渲染器。难点：将自然语言稳定映射为无冲突有向图。成功判据：五类剧情指令均产生合法图diff，可回滚并生成确定性时间线。

3. **生成式数字人流式安全门**  
   目标：搭建句边界审核与中断状态机，先用现有分类器替代尚未发布的ResponseGuard。组件：流式LLM/TTS、风险分类器、WebSocket。难点：误拦截和语音已播放内容回收。成功判据：危险句在TTS播放前阻断，良性测试误拦截率低于团队设定阈值。

4. **引擎原生场景schema验证**  
   目标：手工模拟Lumera输出，为对象和灯光定义统一JSON，再导入Blender或UE。组件：JSON Schema、引擎Python API、五个测试场景。难点：坐标系、单位、光强和资产ID映射。成功判据：同一schema可重复生成场景，物体和灯光均可单独移动、删除和重新渲染。
