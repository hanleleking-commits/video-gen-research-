# 过去 7 天视频生成研究进展日报：2026-07-08

时间窗口：2026-07-01 至 2026-07-08。重点依据 arXiv 新提交/更新列表、论文页和官方 GitHub/项目页；对论文中的性能结论均按“作者报告/官方声称”处理。

## 1. 本日摘要

过去 7 天的主线不是又一个通用 T2V 基座发布，而是围绕“可控、长程、实时、世界模型评测”在做补短板。最明显升温的是交互式 world model：MoWorld、Point as Skeleton、Multiplayer Interactive World Models、CrashTwin 都在把视频生成推向可交互、可闭环、可物理诊断的模拟器方向。部署侧也有明显进展，MobileWan 和 ISPA 都在处理长视频/视频扩散的内存瓶颈，只是一个偏移动端扩散压缩，一个偏 AR KV cache 压缩。控制与编辑方面，Aura、SparseCtrl-HOI、Anti-Prompt 分别对应多主体一致性、稀疏关键帧控制和 I2V 版权/隐私防护。长视频叙事方面 PACR-Video 代表了“冻结基座 + 轻量 adapter + prompt memory/routing”的路线。VAE/tokenizer 方向本周期没有明显独立新 tokenizer，但 V-LITE 的 HDR-aware VAE、MobileWan 的 VAE 解码优化、GRN 的 HBQ tokenization 都值得跟踪。

## 2. 最值得关注的 10 个进展

1. **MoWorld: A Flash World Model**  
   类型：论文 / 项目页  
   链接：https://arxiv.org/abs/2607.06216  
   时间：2026-07-07  
   方向：7 / 9  
   贡献：作者提出面向实时交互的 world model，包含 3D-native 数据引擎、课程式 cross-frame pretraining、denoising-step distillation 和混合精度并行推理；官方声称可到 50 FPS、成本为现有 world model 的 30%-50%。重要性：高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.06216))

2. **MobileWan: Closing the Quality Gap for Mobile Video Diffusion**  
   类型：论文 / 项目页  
   链接：https://arxiv.org/abs/2607.06173  
   时间：2026-07-07  
   方向：1 / 4 / 9  
   贡献：从 Wan2.2-5B 出发，用 recurrent reformulation、causal linear attention、attention head pruning、sampling-step distillation 和 VAE 解码优化，把 5B 视频扩散模型压到移动设备运行；作者报告 5 秒 480x832、16 FPS、20 秒端到端延迟、VBench 83.79。重要性：高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.06173))

3. **Point as Skeleton: Accumulated Point Cloud Enhanced AR Generation for Closed-Loop Autonomous Driving Simulation**  
   类型：论文 / GitHub  
   链接：https://arxiv.org/abs/2607.06516  
   时间：2026-07-07  
   方向：7 / 4  
   贡献：面向自动驾驶闭环仿真，用 ego/actor state、地图和点云 skeleton 条件驱动 AR 视频生成，并提出 Reset-and-Roll 减少未来信息污染。代码已标注可用，值得跟踪复现。重要性：高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.06516))

4. **Aura: Consistent Multi-Subject Video Generation via VLM-Grounded Semantic Alignment**  
   类型：论文 / 项目页 / GitHub  
   链接：https://arxiv.org/abs/2607.04311  
   时间：2026-07-05；7 月 8 日 arXiv replacement 列表显示有更新  
   方向：6 / 8 / 3  
   贡献：面向多主体/多元素一致性，结合 VLM learnable queries、DiT 两阶段对齐、subject-aware RoPE-Shift、learnable subject tokens 和 Memory Tokens；还强调“AI director-level captions”和数据构建流程。重要性：高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.04311))

5. **SparseCtrl-HOI: Sparse Temporal Control for Human-Object Interaction Video Generation**  
   类型：论文 / dataset / GitHub  
   链接：https://arxiv.org/abs/2607.05994  
   时间：2026-07-07  
   方向：8 / 3  
   贡献：只用少量关键帧控制人-物交互视频，提出 TiRoPE 和 MLLM motion prior injection，并发布 SparseHOI-5K 数据集与代码。重要性：中-高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.05994))

6. **Prompt-Adapter Context Routing for Parameter-Efficient Multi-Shot Long Video Extrapolation**  
   类型：论文  
   链接：https://arxiv.org/abs/2607.06481  
   时间：2026-07-07  
   方向：6 / 4  
   贡献：冻结 T2V DiT，通过低秩 temporal adapters、shot-role prompt tokens、recursive prompt bank 和 narrative dependency routing 做多镜头长视频外推。重要性：中-高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.06481))

7. **Towards Memory-Efficient Autoregressive Video Generation via ISPA**  
   类型：论文  
   链接：https://arxiv.org/abs/2607.00712  
   时间：2026-07-01  
   方向：4 / 9  
   贡献：把 AR 视频模型的 KV cache 压缩从“丢 token”改为“把历史上下文吸收到实例特定权重调制中”；作者报告 1.3B-14B 架构上可去掉最多 50% KV cache，视觉质量近似无损。重要性：高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2607.00712))

8. **Video Generation Models Are Inherent Lighting Estimators / V-LITE**  
   类型：论文 / 项目页  
   链接：https://arxiv.org/abs/2607.04674  
   时间：2026-07-06  
   方向：2 / 8  
   贡献：把动态环境光估计重写成视频 inpainting，引入 synthetic chrome ball、HDR-aware VAE 和 LoRA 微调，说明视频生成模型内部可能已学到可用于物理光照估计的时空先验。重要性：中。优先级：可读。([arxiv.org](https://arxiv.org/abs/2607.04674))

9. **Anti-Prompt: Image Protection against Text-Guided Image-to-Video Generation**  
   类型：论文 / 安全  
   链接：https://arxiv.org/abs/2607.01499  
   时间：2026-07-01；v2 2026-07-06  
   方向：10 / 8  
   贡献：针对文本引导 I2V 的版权和隐私风险，在输入图像中加入不可感知扰动，使生成视频出现结构和时间一致性失败；还提出 Video-LLM 辅助评测协议。重要性：中。优先级：可读。([arxiv.org](https://arxiv.org/abs/2607.01499))

10. **A Physics-Grounded Benchmark for Multi-Agent Dynamics in World Models / CrashTwin**  
   类型：benchmark  
   链接：https://arxiv.org/abs/2606.28757  
   时间：2026-06-27；2026-07-08 arXiv new list 显示 replacement，纳入原因是本周期更新/重新进入列表  
   方向：7 / 10  
   贡献：面向多智能体碰撞动态的 world model 物理评测，包含 25K 可控合成和 12K 真实碰撞序列，评估时空一致性、动量/动能守恒和 world-dynamics integrity。重要性：高。优先级：必读。([arxiv.org](https://arxiv.org/abs/2606.28757))

## 3. 按方向分类总结

1. **基础视频底座 / Video Foundation Models**  
   本周期没有看到新的通用闭源/开源 T2V 大基座正式发布。更值得关注的是 MobileWan 这类基于 Wan2.2-5B 的压缩部署工作，以及 Aura/SparseCtrl-HOI 等在现有 DiT/视频扩散基座上扩展控制能力的工作。

2. **视频表征层：VAE / tokenizer / latent representation**  
   没有独立 video tokenizer 大论文成为主线。相关信号包括 V-LITE 的 HDR-aware VAE、MobileWan 的 memory-optimized VAE decoding、GRN 的 Hierarchical Binary Quantization，但都不是专门的视频 tokenizer 基础设施发布。([arxiv.org](https://arxiv.org/abs/2607.04674))

3. **训练数据与训练 recipe**  
   SparseCtrl-HOI 发布 SparseHOI-5K；Aura 强调 AIGC grounding-augmenting-verification 数据管线和 director-level captions；MoWorld 强调 3D-native data engine 与 curriculum cross-frame pretraining。趋势是从“泛视频语料堆规模”转向“任务结构化数据 + 自动标注/验证”。

4. **生成范式**  
   diffusion/DiT 仍是主流，但 AR 和 hybrid 化明显升温。ISPA 直接针对 AR streaming video 的 KV cache；Point as Skeleton 用 AR rollouts 做闭环驾驶仿真；MobileWan 把扩散过程改写成 chunk-wise recurrent 推理。

5. **原生音视频一体化**  
   本周期暂无高质量新增。没有看到新的 joint audio-video foundation model 或 lip-sync/foley 方向的强更新。

6. **长视频、多镜头与叙事一致性**  
   PACR-Video 是本周期最直接相关工作：冻结基座，通过 prompt bank 和 adapter routing 保持实体、地点、风格和因果进展。Aura 的多主体一致性也可服务多镜头角色保持，但它更偏 reference-conditioned 控制。

7. **交互式世界模型 / Video World Models**  
   本周期最热方向。MoWorld 主打实时 world model；Point as Skeleton 主打自动驾驶闭环生成仿真；Multiplayer Interactive World Models 把多玩家 action streams 纳入条件，并报告 Rocket League 四玩家实时生成；CrashTwin 则补物理评测。([arxiv.org](https://arxiv.org/list/cs.CV/new))

8. **控制、编辑、相机与实时 V2V**  
   Aura 解决多主体 reference binding；SparseCtrl-HOI 用少量关键帧控制 HOI；PanoGaussian 处理大视角变化下的 4D scene synthesis；Anti-Prompt 从防护角度反向利用 I2V 的文本依赖。([arxiv.org](https://arxiv.org/abs/2607.01663))

9. **推理加速与部署工程**  
   MobileWan 和 ISPA 是重点。前者面向移动端视频扩散部署，后者面向 AR 视频模型长程 KV cache 压缩。MoWorld 也把 real-time/NPU/低成本作为核心卖点，但需要等待代码、demo 或第三方复现验证。

10. **评测、安全与可靠性**  
   CrashTwin 和 Anti-Prompt 最相关。CrashTwin指出高感知质量可能掩盖严重物理违规；Anti-Prompt则把 I2V 版权/隐私防护具体化为输入图像防生成扰动。EgoDyn-Bench 也值得旁观，虽然更偏 foundation model 物理/自车运动理解。([arxiv.org](https://arxiv.org/list/cs.CV/new))

## 4. 技术趋势判断

明显升温：world model、闭环仿真、长程记忆/一致性、低成本部署。  
正在成为主流：冻结大视频基座 + 轻量 adapter/control 模块；结构化条件如点云、关键帧、prompt memory、action streams；用 VLM/MLLM 做语义对齐或 motion prior。  
仍未解决：长时间物理一致性、跨镜头身份绑定、复杂人-物交互中的接触动力学、公开可复现的实时 world model 评测。  
研究增量可能有限的方向：只在现有视频扩散模型上叠加 UI/工作流、只报告主观 demo、没有公开代码/数据/评测的“实时”或“world model”宣称。

## 5. 论文精读候选

1. **MoWorld**：重点读 data engine、distillation、inference pipeline、evaluation。新意在把 world model 能力和 NPU/实时部署作为一体化目标。  
2. **MobileWan**：重点读 recurrent reformulation、causal linear attention、head pruning、VAE decoding。新意在移动端部署 5B 级视频扩散，而不是训练小模型。  
3. **ISPA**：重点读 parametric absorption 的 closed-form weight modulation。新意在把历史上下文从外部 KV cache 转为实例特定参数补偿。  
4. **Aura**：重点读 VLM-DiT alignment、subject-aware RoPE-Shift、Memory Tokens、数据构建。新意在多主体 reference binding 的语义和位置双重解耦。  
5. **CrashTwin**：重点读物理指标、3D 属性恢复和 benchmark 设计。新意在把 world model 从“看起来像”推向“动力学可信”。

## 6. 开源与复现实用资源

- **SparseCtrl-HOI**：论文页声明代码和 SparseHOI-5K 数据集公开，值得复现，尤其适合做 sparse keyframe control baseline。([arxiv.org](https://arxiv.org/abs/2607.05994))  
- **Aura**：arXiv replacement 列表标注 Project page 和 Code，适合跟踪多主体一致性实验，但需检查权重是否完整释放。([arxiv.org](https://arxiv.org/list/cs.CV/new))  
- **Point as Skeleton**：论文页声明代码可用，值得用于自动驾驶生成仿真和 closed-loop rollout 对比。([arxiv.org](https://arxiv.org/abs/2607.06516))  
- **MobileWan / MoWorld**：目前重点看项目页和论文，是否放出可运行代码、模型或设备 demo 是下周关键。  
- **CrashTwin**：作为 benchmark 价值高，尤其适合评估 world model 物理一致性；需确认数据和工具链开放状态。

## 7. 与长期研究地图的关系

- **视频底座平台化**：本周期没有新通用基座，更多是在 Wan/DiT 等基座上做模块化扩展。  
- **tokenizer / VAE 基础设施**：VAE 解码、HDR VAE、HBQ 是弱信号，尚未形成新共识。  
- **原生音视频**：本周期冷。  
- **长视频与叙事一致性**：PACR-Video 和 Aura 是直接增量。  
- **world model 化**：本周期最强主线，MoWorld、Point as Skeleton、Multiplayer WM、CrashTwin 都指向这里。  
- **实时与低成本部署**：MobileWan、ISPA、MoWorld 是核心。  
- **可靠评测与安全**：CrashTwin、Anti-Prompt 是核心。

## 8. 下周值得继续跟踪的问题

1. MoWorld 是否会放出可运行 demo、代码、模型权重或 NPU 端复现细节？  
2. MobileWan 的移动端结果是否能被第三方复现，尤其是 20 秒端到端延迟和 VBench 83.79？  
3. Aura 是否释放完整训练/推理代码和多主体评测集，还是只放项目页 demo？  
4. CrashTwin 是否公开 25K 合成 + 12K 真实碰撞数据和物理诊断工具？  
5. Point as Skeleton 的闭环 nuPlan/nuScenes 接口能否成为驾驶 world model 的可复现 baseline？
