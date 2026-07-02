# 过去 7 天视频生成研究进展日报：2026-07-02

检索窗口：2026-06-25 至 2026-07-02。注：我可以生成本次日报，但当前对话本身不能自动每天定时运行；如果需要，我可以后续帮你做成脚本或定时任务。

## 1. 本日摘要

过去 7 天最明显的主线不是单纯更大 T2V 模型，而是“可控 world model 化”：多篇工作把视频生成从像素采样转向可编辑结构、动作条件、相机控制和长时序 rollout。AR + diffusion hybrid 继续升温，Vega 用 AR 预测语义关键帧 token、再用 diffusion 渲染高分辨率视频，是统一理解与生成的一条代表路线。视频 tokenizer 方向出现音视频统一表征 AVTok，重点从单纯视频压缩扩展到 audio-video joint latent。部署侧 EcoVideo 把 DiT 视频生成拆成云端关键帧 denoise + 边端插帧重建，说明低延迟和低带宽推理正在成为独立研究问题。机器人 / embodied world model 方面，DVG-WM 强调将 dynamics learning 与 visual synthesis 解耦，以提升规划场景中的效率。评测、安全方向本周期没有严格落在窗口内的高质量新增，但物理一致性仍是所有 world-model 工作共同指向的未解问题。

## 2. 最值得关注的 6 个进展

1. **World Narrative Model**
   类型：论文 / 项目页  
   链接：[arXiv](https://arxiv.org/abs/2606.31946)、[项目页](https://glassroom.sjtu.edu.cn/WNM/)  
   时间：2026-06-30  
   方向：长视频、多镜头与叙事一致性；Video World Models  
   贡献：提出用结构化 4D physical narrative 作为“要渲染什么”的蓝图，再驱动视频底座作为渲染器，目标是降低随机抽卡式生成。官方声称可编辑几何、角色骨架、轨迹、相机与光照。重要性：高；优先级：必读。来源：arXiv 摘要明确描述 4D 预可视化与可编辑世界表示。([arxiv.org](https://arxiv.org/abs/2606.31946))

2. **DVG-WM**
   类型：论文  
   链接：[arXiv](https://arxiv.org/abs/2606.32028)  
   时间：2026-06-30  
   方向：交互式世界模型 / robotics  
   贡献：把 embodied video world model 分成 dynamics preview 与高保真视觉合成两层，并用 flow matching 映射到 video latent；官方实验在 LIBERO 与真实平台上报告最高 3.97x 加速。重要性：高；优先级：必读。([arxiv.org](https://arxiv.org/abs/2606.32028))

3. **Vega: Bridging Video Understanding and Generation**
   类型：论文 / technical blog  
   链接：[arXiv](https://arxiv.org/abs/2606.31326)  
   时间：2026-06-30  
   方向：基础视频底座；生成范式  
   贡献：用共享 vocabulary 统一视频理解与生成，AR 负责关键帧语义 token，diffusion 负责 dense high-res rendering；在 VBench 与 VideoMME 上同时验证。重要性：高；优先级：必读。([arxiv.org](https://arxiv.org/abs/2606.31326))

4. **AVTok**
   类型：论文  
   链接：[arXiv](https://arxiv.org/abs/2606.30811)  
   时间：2026-06-29  
   方向：视频表征层；原生音视频一体化  
   贡献：提出 1D audio-video unified tokenizer，用共享 encoder-decoder、模态特定 query 和 unified codebook 表征音视频；面向 audio-to-video、video-to-audio 与 joint generation。重要性：高；优先级：必读。([arxiv.org](https://arxiv.org/abs/2606.30811))

5. **EcoVideo**
   类型：论文 / GitHub  
   链接：[arXiv](https://arxiv.org/abs/2606.30557)、[GitHub](https://github.com/IF-LAB-PKU/EcoVideo)  
   时间：2026-06-29  
   方向：推理加速与部署工程  
   贡献：用 early self-attention entropy 选高信息密度关键帧，云端大模型处理关键帧，边端轻模型插帧与细化；官方报告最高 2.9x 端到端加速。代码已开源，支持 Wan2.1/Wan2.2/CogVideoX 后端。重要性：中高；优先级：可读 / 可复现。([arxiv.org](https://arxiv.org/abs/2606.30557)) ([github.com](https://github.com/IF-LAB-PKU/EcoVideo))

6. **Directing the World**
   类型：论文 / 项目页  
   链接：[arXiv](https://arxiv.org/abs/2606.27964)、[项目页](https://whydahuzi.github.io/Directing-the-World.github.io/)  
   时间：2026-06-26  
   方向：控制、相机与 Video World Models  
   贡献：fast autoregressive controllable video generation，组合人类 motion 与 camera trajectory control，并提出 Fast-Slow Memory 稳定长时 rollout。重要性：中高；优先级：可读。([arxiv.org](https://arxiv.org/abs/2606.27964)) ([whydahuzi.github.io](https://whydahuzi.github.io/Directing-the-World.github.io/))

## 3. 按方向分类总结

1. **基础视频底座 / Video Foundation Models**  
   Vega 是本周期最相关进展，代表“理解 + 生成统一视频底座”的路线：共享 token vocabulary，AR 负责结构，diffusion 负责渲染。([arxiv.org](https://arxiv.org/abs/2606.31326))

2. **视频表征层：VAE / tokenizer / latent representation**  
   AVTok 值得重点看，它把 tokenizer 从视频单模态推进到 audio-video joint 1D latent，并尝试统一 codebook。([arxiv.org](https://arxiv.org/abs/2606.30811))

3. **训练数据与训练 recipe**  
   Directing the World 构建同步 video/text/human-motion/camera-trajectory 数据，并分 motion-centric 与 camera-centric 子集训练；这是本周期最明确的数据 recipe 新增。([arxiv.org](https://arxiv.org/abs/2606.27964))

4. **生成范式**  
   AR + diffusion hybrid 明显升温：Vega 用 AR 做语义关键帧规划、diffusion 做像素级渲染；Directing the World 则强调 autoregressive long-horizon world rollout。([arxiv.org](https://arxiv.org/abs/2606.31326)) ([arxiv.org](https://arxiv.org/abs/2606.27964))

5. **原生音视频一体化**  
   AVTok 是核心新增，贡献在 joint tokenization，而不是又一个级联系统。([arxiv.org](https://arxiv.org/abs/2606.30811))

6. **长视频、多镜头与叙事一致性**  
   WNM 最相关：把脚本、场景、轨迹、相机、光照变成可编辑世界蓝图，再驱动视频模型渲染。([arxiv.org](https://arxiv.org/abs/2606.31946))

7. **交互式世界模型 / Video World Models**  
   DVG-WM 和 Directing the World 都很相关，前者面向机器人操作规划，后者面向人和相机组合控制的长时生成。([arxiv.org](https://arxiv.org/abs/2606.32028)) ([arxiv.org](https://arxiv.org/abs/2606.27964))

8. **控制、编辑、相机与实时 V2V**  
   Directing the World 是本周期主项，尤其是 human-motion control 与 camera-trajectory control 的组合。([arxiv.org](https://arxiv.org/abs/2606.27964))

9. **推理加速与部署工程**  
   EcoVideo 是明确新增，重点是 cloud-edge 动态分工、entropy keyframe selection、EDEN interpolation 与 VBench 复现脚本。([github.com](https://github.com/IF-LAB-PKU/EcoVideo))

10. **评测、安全与可靠性**  
   本周期暂无严格落在 2026-06-25 至 2026-07-02 的高质量新增。物理一致性评测仍值得继续跟踪，但本次未把 2026-06-24 的 PQSG 纳入主表。

## 4. 技术趋势判断

明显升温：world model 化、相机/动作可控生成、AR + diffusion hybrid、音视频统一 tokenization、低延迟部署。  
正在成为主流：用显式结构或语义 token 做规划，再交给 diffusion/flow 渲染；用 latent 或 keyframe 层减少视频 DiT 的全帧计算。  
仍未解决：长时一致性、物理可验证性、角色/物体身份保持、动作条件与视觉质量的冲突、开放模型的可复现评测。  
可能研究增量有限的方向：只把现有视频模型包装成 director UI、只做 prompt expansion 或 pipeline glue、没有开放权重/代码/评测细节的“world model”宣称。

## 5. 论文精读候选

- [World Narrative Model](https://arxiv.org/abs/2606.31946)：读 system architecture、world representation、control agent 与 evaluation。新意在于把视频底座降格为 neural shader，研究重点转向 4D 可编辑中间表示。
- [AVTok](https://arxiv.org/abs/2606.30811)：读 tokenizer architecture、hierarchical training、downstream audio-video generation。新意是统一音视频 codebook 和 1D latent，而非双分支后融合。
- [Vega](https://arxiv.org/abs/2606.31326)：读 shared vocabulary、AR token prediction、diffusion rendering。新意是把视频理解和生成放入同一框架验证。
- [DVG-WM](https://arxiv.org/abs/2606.32028)：读 dynamics/synthesis decomposition、flow matching latent mapping、robot evaluation。新意是面向 embodied planning 的效率拆解。
- [EcoVideo](https://arxiv.org/abs/2606.30557)：读 entropy keyframe selection、cloud-edge scheduling、VBench evaluation。新意偏工程，但可复现价值较高。

## 6. 开源与复现实用资源

- [IF-LAB-PKU/EcoVideo](https://github.com/IF-LAB-PKU/EcoVideo)：值得复现。Apache-2.0，包含推理 pipeline、Wan/CogVideoX 后端、VBench 评测脚本；注意完整复现实验仍依赖对应 checkpoint、EDEN/RAFT 等组件。([github.com](https://github.com/IF-LAB-PKU/EcoVideo))
- [Directing the World 项目页](https://whydahuzi.github.io/Directing-the-World.github.io/)：适合看 demo 与方法说明，目前未确认代码/权重开源。([whydahuzi.github.io](https://whydahuzi.github.io/Directing-the-World.github.io/))
- [WNM 项目页](https://glassroom.sjtu.edu.cn/WNM/)：适合跟踪系统形态，但页面显示 invitation access，复现价值暂时有限。([glassroom.sjtu.edu.cn](https://glassroom.sjtu.edu.cn/WNM/))
- AVTok / Vega / DVG-WM：本轮只确认 arXiv，未确认官方代码仓库；建议下周继续跟踪代码释放。

## 7. 与长期研究地图的关系

- 视频底座平台化：Vega 强化统一理解与生成。
- tokenizer / VAE 基础设施：AVTok 是本周期重点。
- 原生音视频：AVTok 直接相关。
- 长视频与叙事一致性：WNM 代表结构化导演系统路线。
- world model 化：DVG-WM、Directing the World、WNM 都在推动。
- 实时与低成本部署：EcoVideo 是明确工程进展。
- 可靠评测与安全：本周期空缺，但物理一致性评测仍是下周重点。

## 8. 下周值得继续跟踪的问题

1. AVTok 是否发布 tokenizer 代码、checkpoint 或 downstream demo？
2. Vega 是否给出可复现训练/推理代码，尤其是 shared vocabulary 与 AR-diffusion 接口？
3. DVG-WM 是否开放机器人 benchmark 结果、LIBERO 配置和视频预测代码？
4. EcoVideo 是否被社区复现到 Wan2.2 / CogVideoX 的真实低显存场景？
5. WNM 是否会开放中间 4D representation schema、agent pipeline 或可编辑控制接口？
