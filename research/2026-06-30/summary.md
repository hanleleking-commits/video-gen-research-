# 过去 7 天视频生成研究进展日报：2026-06-30

检索窗口：2026-06-24 至 2026-06-30。说明：我可以按这个模板生成日报，但当前环境不能在会话外自动定时推送。

## 1. 本日摘要

过去 7 天主线不是“大模型发布”，而是围绕 **AR/causal video diffusion、world model 控制、评测与可复现 benchmark** 的方法型进展。最值得关注的是 Causal-rCM，把 teacher-forcing consistency model 和 self-forcing DMD 放到统一框架里，目标是 1-2 step 的流式/交互式视频生成。World model 方向明显升温，Directing the World、MemoBench、SimFoundry 都在强调长时记忆、可控 rollout、物理/机器人场景闭环。评测方向也变得更细：PQSG/FinePhyEval 和 MemoBench 都不是泛泛打分，而是定位物理违背、遮挡后状态记忆等具体失败模式。个性化/推荐与视频生成开始耦合，RaG 把“生成视频”纳入工业推荐闭环。Tokenizer/VAE 和原生音视频一体化本周期没有看到高质量新增。整体看，研究重心从单次 T2V 质量，继续转向低延迟、长时一致性、可控交互和可诊断评测。

## 2. 最值得关注的 7 个进展

1. **Causal-rCM**
   - 类型：论文 / 技术报告
   - 链接：[arXiv](https://arxiv.org/abs/2606.25473)
   - 时间：2026-06-24
   - 方向：1 / 4 / 7 / 9
   - 贡献：提出面向 AR video diffusion 的 teacher-forcing CM + self-forcing DMD 统一蒸馏 recipe；官方声称 1-2 step causal Wan2.1-1.3B 达到 VBench-T2V 84.63，并用于 Cosmos 3 world model。来源见 arXiv 摘要与提交记录。([arxiv.org](https://arxiv.org/abs/2606.25473))
   - 重要性：高；优先级：必读

2. **Directing the World**
   - 类型：论文 / 项目页
   - 链接：[arXiv](https://arxiv.org/abs/2606.27964)，[项目页](https://whydahuzi.github.io/Directing-the-World.github.io/)
   - 时间：2026-06-26
   - 方向：6 / 7 / 8
   - 贡献：AR world model 中组合 SMPL 人体运动与 Plücker-ray 相机轨迹控制；项目页声称支持 15-20 秒 long-horizon rollout 和 50K curated clips。([arxiv.org](https://arxiv.org/abs/2606.27964)) ([whydahuzi.github.io](https://whydahuzi.github.io/Directing-the-World.github.io/))
   - 重要性：高；优先级：必读

3. **MemoBench**
   - 类型：benchmark / 论文
   - 链接：[arXiv](https://arxiv.org/abs/2606.27537)
   - 时间：2026-06-25
   - 方向：7 / 10
   - 贡献：评测动态环境中“物体消失再出现”后的状态记忆，包含 360 个 synthetic/real clips，并评估 8 个模型。([arxiv.org](https://arxiv.org/abs/2606.27537))
   - 重要性：高；优先级：必读

4. **PQSG / FinePhyEval**
   - 类型：benchmark / dataset / GitHub
   - 链接：[arXiv](https://arxiv.org/abs/2606.25306)，[GitHub](https://github.com/atinpothiraj/pqsg)
   - 时间：2026-06-24
   - 方向：10
   - 贡献：用 Physics Question Scene Graph 对 T2V 物理合理性做细粒度问答式评测；开源 repo 含代码、cached data、FinePhyEval manifest 和复现实验脚本。([arxiv.org](https://arxiv.org/abs/2606.25306)) ([github.com](https://github.com/atinpothiraj/pqsg))
   - 重要性：高；优先级：必读

5. **DomainShuttle**
   - 类型：论文
   - 链接：[arXiv](https://arxiv.org/abs/2606.26058)
   - 时间：2026-06-24
   - 方向：1 / 8
   - 贡献：面向 open-domain subject-driven T2V，区分 in-domain 保真与 cross-domain 可编辑性；提出 Domain-MoT、Video-Reference DualRoPE 和 Cross-Pair Consistent Loss。([arxiv.org](https://arxiv.org/abs/2606.26058))
   - 重要性：中；优先级：可读

6. **Recommendation as Generation**
   - 类型：论文 / 项目页
   - 链接：[arXiv](https://arxiv.org/abs/2606.25496)，[项目页](https://recommendation-as-generation.github.io/)
   - 时间：2026-06-24
   - 方向：3 / 6
   - 贡献：把短视频推荐从“检索已有视频”扩展到“按用户兴趣生成视频”；通过 disentangled semantic IDs、Video Generation Agents 和 cross-domain reward learning 做工业闭环。([arxiv.org](https://arxiv.org/abs/2606.25496)) ([recommendation-as-generation.github.io](https://recommendation-as-generation.github.io/))
   - 重要性：中；优先级：可读

7. **SimFoundry**
   - 类型：论文 / 官方项目页
   - 链接：[arXiv](https://arxiv.org/abs/2606.28276)，[NVIDIA Research 项目页](https://research.nvidia.com/labs/gear/simfoundry/)
   - 时间：2026-06-26
   - 方向：7 / 3
   - 贡献：从单个真实视频自动构建可交互仿真场景，用于机器人 policy 训练与评测；官方报告 sim-real performance 相关性 Pearson 0.911。([arxiv.org](https://arxiv.org/abs/2606.28276)) ([research.nvidia.com](https://research.nvidia.com/labs/gear/simfoundry/))
   - 重要性：中；优先级：可读

## 3. 按方向分类总结

1. **基础视频底座 / Video Foundation Models**  
   Causal-rCM 是本周期最接近底座训练/蒸馏 recipe 的工作，直接围绕 causal DiT、AR video diffusion、Wan/Cosmos 类底座展开。DomainShuttle 属于 subject-driven T2V 能力扩展，不是新底座。

2. **视频表征层：VAE / tokenizer / latent representation**  
   本周期暂无高质量新增。

3. **训练数据与训练 recipe**  
   Causal-rCM 的 synthetic-data AR distillation recipe 值得跟踪。Directing the World 的 50K controllable clips 和 RaG 的 cross-domain reward learning 也有 recipe 价值。

4. **生成范式**  
   明显升温的是 **autoregressive video diffusion + causal attention + few-step distillation**。Causal-rCM 和 Directing the World 都指向长时/流式生成，而不是一次性全片段 denoising。

5. **原生音视频一体化**  
   本周期暂无高质量新增。

6. **长视频、多镜头与叙事一致性**  
   Directing the World 重点覆盖 15-20 秒 rollout、人体运动与相机轨迹组合控制。RaG 则从推荐/广告场景切入多阶段视频生成 agent，但叙事一致性仍偏工程系统。

7. **交互式世界模型 / Video World Models**  
   本周期最热方向。Causal-rCM、Directing the World、MemoBench、SimFoundry 都围绕 world model 的低延迟、可控 rollout、状态记忆或机器人仿真。

8. **控制、编辑、相机与实时 V2V**  
   Directing the World 强调 human-camera compositional control；DomainShuttle 关注 reference subject 控制；Holo-World 虽论文 v1 是 2026-06-18，早于本窗口，但项目页/GitHub/model 入口显示其围绕 camera/object/weather unified control，建议作为延伸跟踪而非本周期主条目。([xiangchenyin.github.io](https://xiangchenyin.github.io/Holo-World/))

9. **推理加速与部署工程**  
   Causal-rCM 是核心新增，重点在 1-2 step AR diffusion distillation、自定义 FlashAttention-2 JVP kernel 和 streaming 生成。

10. **评测、安全与可靠性**  
   PQSG/FinePhyEval 和 MemoBench 是本周期最实用的评测新增：前者评物理 plausibility，后者评动态状态记忆。两者都比泛 VBench 更适合定位具体失败原因。

## 4. 技术趋势判断

- 明显升温：AR video diffusion、causal video model、world model memory、camera/action-conditioned rollout、物理一致性评测。
- 正在成为主流：teacher-forcing / self-forcing 结合的 post-training，few-step distillation，VLM-assisted video evaluation，显式相机/人体/物体控制。
- 仍未解决：长时误差累积、遮挡后状态更新、角色/物体身份稳定、物理因果一致性、benchmark 与真实用户偏好的相关性。
- 研究增量有限风险：部分“agentic video generation for industry”可能更偏系统集成；如果没有开放模型、数据或可复现实验，研究价值要打折。

## 5. 论文精读候选

1. **Causal-rCM**  
   重点读：Introduction、Causal-rCM algorithm、Infrastructure、Experiments。新意在把 TF-CM 和 SF-DMD 统一为 forward/reverse objective complementarity，并落到 AR video diffusion distillation。

2. **Directing the World**  
   重点读：Fast-Slow Memory training、human motion control、camera control、dataset construction。新意在异构控制解耦后再组合，目标是稳定长时 world rollout。

3. **MemoBench**  
   重点读：benchmark design、disappear-and-reappear protocol、model analysis。新意在评测“看不见期间状态变化”的记忆能力，而不是只看可见帧一致性。

4. **PQSG / FinePhyEval**  
   重点读：question graph construction、tree scoring、human correlation、released repo scripts。新意在把物理失败拆成 object/action/physics 层级诊断。

5. **SimFoundry**  
   重点读：real-to-sim pipeline、digital cousins、sim-real correlation evaluation。新意在把视频输入转成可交互仿真资产，用于 policy 学习与评测。

## 6. 开源与复现实用资源

- **PQSG GitHub**：值得复现。repo 提供 `scripts/reproduce.py`、cached data、FinePhyEval manifest，且 quick start 声称无需 API key 可复现主表。([github.com](https://github.com/atinpothiraj/pqsg))
- **Directing the World 项目页**：值得跟踪。页面有 “Code & Models” 入口和大量 demo，但我本轮未确认到可下载代码/权重。
- **Holo-World GitHub / HF 入口**：作为延伸跟踪。GitHub 当前显示 1 commit、Apache-2.0、无 release；HF 页面访问受限。([github.com](https://github.com/XiangchenYin/Holo-World))
- **SimFoundry 项目页**：值得看 demo，不适合立即复现；未看到代码发布。
- **RaG 项目页**：偏工业系统展示，暂不适合复现。

## 7. 与长期研究地图的关系

- 视频底座平台化：Causal-rCM 强相关。
- tokenizer / VAE 基础设施：本周期空缺。
- 原生音视频：本周期空缺。
- 长视频与叙事一致性：Directing the World、RaG。
- world model 化：Causal-rCM、Directing the World、MemoBench、SimFoundry。
- 实时与低成本部署：Causal-rCM。
- 可靠评测与安全：PQSG/FinePhyEval、MemoBench。

## 8. 下周值得继续跟踪的问题

1. Causal-rCM 是否会放出训练代码、distilled Wan 权重或 FlashAttention-2 JVP kernel？
2. Directing the World 的 Code & Models 是否真正开放，数据集是否可下载？
3. PQSG/FinePhyEval 是否会被 Sora/Veo/Wan/Cosmos 新版本快速刷新？
4. MemoBench 是否发布数据与 leaderboard，是否能成为 world model memory 的标准评测？
5. Holo-World 的 GitHub/HF 是否补齐代码、模型权重和 HoloStateData 下载入口？
