# Video Generation Research Digest — 2026-06-26

Window covered: ~2026-06-19 → 2026-06-26 (last 7 days, UTC). Primary source this run was the
arXiv API (`export.arxiv.org`), cross-checked against web search, the
[Awesome-From-Video-Generation-to-World-Model](https://github.com/ziqihuangg/Awesome-From-Video-Generation-to-World-Model)
list, and the [arXiv daily AIGC tracker](https://www.aha-time.com/arxiv_daily_aigc/).

**Environment note (changed from prior runs):** `arxiv.org`, `huggingface.co`, and `aha-time.com`
are now reachable from this environment, so PDF artifacts were downloaded directly and the in-window
arXiv listing was browsed via the arXiv API. All 21 items below are real, in-window arXiv papers with
verified IDs; PDFs are saved in this folder.

It was a **busy week on arXiv** — autoregressive/streaming distillation and interactive foundation
models dominated, alongside a cluster of efficiency and controllability work. No major *new* closed
model launched in-window (Grok Imagine Video 1.5, GA'd 2026-06-25, was already captured under
`research/2026-06-25/`).

---

## Top 3 reads today

1. **Causal-rCM: A Unified Teacher-Forcing and Self-Forcing Open Recipe for Autoregressive Diffusion
   Distillation** — Kaiwen Zheng, Guande He, Min Zhao, Jintao Zhang, Jianfei Chen et al. (Tsinghua
   et al.). [arXiv:2606.25473](https://arxiv.org/abs/2606.25473) · PDF `2606.25473.pdf`.
   Extends the rCM distillation framework to autoregressive video diffusion, casting teacher-forcing
   (offline, forward-divergence consistency models) and self-forcing (on-policy, reverse-divergence
   DMD) as complementary halves. Ships the first teacher-forcing continuous-time CMs (sCM/MeanFlow)
   for AR video via a custom-mask FlashAttention-2 JVP kernel (10× faster convergence than discrete
   CMs). A distilled 2-step causal Wan2.1-1.3B hits VBench-T2V 84.63 with 1–2 steps, and the recipe
   is applied to Cosmos 3 to build an action-conditioned interactive world model. *Subfield:
   Efficiency/Distillation × Streaming/World models.*

2. **Wan-Streamer v0.1: End-to-end Real-time Interactive Foundation Models** — Lianghua Huang, Zhifan
   Wu, Wei Wang et al. (Alibaba / Wan team).
   [arXiv:2606.25041](https://arxiv.org/abs/2606.25041) · PDF `2606.25041.pdf`.
   A native-streaming, single-Transformer model that handles language, audio, and video as both input
   and output via interleaved tokens under block-causal attention, replacing the usual
   VAD→ASR→LLM→TTS→avatar→video cascade. The full stack (causal encoders/decoders, low-latency token
   scheduling) supports 160 ms streaming units at 25 fps, ~200 ms model-side latency and ~550 ms total
   interaction latency — sub-second full-duplex audio-visual conversation. *Subfield: Streaming /
   Real-time interactive.*

3. **Towards Error-Free Long Video Generation** — Shuning Chang, Weihua Chen, Jiasheng Tang, Hangjie
   Yuan et al. (Alibaba). [arXiv:2606.22370](https://arxiv.org/abs/2606.22370) · PDF `2606.22370.pdf`.
   An infinite-length single-shot framework targeting error accumulation and attribute drift. A
   diffusion model is finetuned as an autoregressive video-extension model, then refined with
   LLM-style causal attention *between* clips (bidirectional within a clip, unidirectional across
   clips). Constant KV-cache memory at inference plus a "truncation-rectified flow" (T-RFlow)
   technique suppress drift, setting a new bar for coherent minute-level synthesis. *Subfield: Long
   video.*

---

## Efficiency / Distillation / Acceleration

- **CoDMD: Copula-aware Distribution Matching Distillation for Fast Video Generation** — Wenhu Zhang,
  Kun Cheng, Changyuan Wang et al. [arXiv:2606.21982](https://arxiv.org/abs/2606.21982) · PDF
  `2606.21982.pdf`. Diagnoses few-step DMD failures (layout instability, oversaturation, broken
  motion) as an *unregulated copula*: standard DMD has no constraint on relational geometry across
  batch elements/frames. Adds a lightweight relational regularizer built from existing teacher/fake
  score estimates (no new nets or data). Distills 50-step Wan-2.1-T2V (1.3B & 14B) into 4-step
  students at ~25× speed-up with VBench 84.46 / 84.87, beating rCM and vanilla DMD. *Distillation.*

- **ScalingAttention: Discovering Intrinsic Sparse Attention Topology for Video DiTs** — Ruiliang
  Zhou, Xuecheng Wu et al. [arXiv:2606.23019](https://arxiv.org/abs/2606.23019) · PDF
  `2606.23019.pdf`. Training-free sparse attention exploiting the observation that high-mass attention
  regions per head converge to a stable, prompt-agnostic "intrinsic sparse topology" that is
  weight-encoded and scale-invariant. WEST extracts a block-sparse prior mask offline (no runtime
  search); FAST tunes head-wise sparsity by diffusion fidelity. With a co-designed bit-wise
  block-sparse kernel, gets up to 1.90× end-to-end speedup on Wan2.1. *Sparse attention / inference.*

- **Chorus II: Cross-Request Sparsity Reuse for Efficient Image-to-Video Generation** — Hao Liu,
  Chenghuan Huang et al. [arXiv:2606.25040](https://arxiv.org/abs/2606.25040) · PDF `2606.25040.pdf`.
  Serving-side acceleration: similar I2V requests (repeated effect templates, recurring shot layouts)
  share consistent sparse attention patterns, so historical sparse masks act as request-conditioned
  priors with near-zero online mask-prediction overhead. Optional feature reuse plus a guidance
  enhancement step mitigate drift; default config gives 2.16× speedup at preserved quality. *Serving.*

- **Sol Video Inference Engine: Agent-Native Full-Stack Acceleration** — Yitong Li, Junsong Chen,
  Haopeng Li et al. [arXiv:2606.23743](https://arxiv.org/abs/2606.23743) · PDF `2606.23743.pdf`.
  Argues the best acceleration recipe is highly instance-specific (model × hardware × config), then
  organizes five training-free techniques — cache, sparse attention, token pruning, quantization,
  kernel fusion — into an *agentic* stack: parallel skill agents optimize each technique, an
  integrator composes them, a human validator checks quality. >2× end-to-end on 64B Cosmos3-Super,
  22B LTX-2.3, and 2B SANA-Video at near-lossless VBench. *Inference framework.*

## Streaming / Autoregressive / Real-time avatars

- **InteractiveAvatar: Real-Time Streaming Video Generation for Consistent and Intent-Aware Avatars**
  — Quanyue Song, Yishan He et al. [arXiv:2606.22905](https://arxiv.org/abs/2606.22905) · PDF
  `2606.22905.pdf`. Real-time infinite-streaming audio-driven avatars via autoregressive distillation.
  A Long-Short Visual Memory (LSVM) compresses history into compact tokens for both short-range and
  long-term consistency, and a Reasoning-Reaction Module (state-cycling + cache-switching) aligns
  speech/actions with user intent in interactive settings. SOTA visual consistency over long
  durations. *Streaming avatars.*

## Long / Multi-shot video

- **UnityShots: Memory-Driven Multi-Shot Audio-Video Generation with Boundary-Aware Gating** — Jiehui
  Huang, Yuechen Zhang, Bin Xia et al. [arXiv:2606.21661](https://arxiv.org/abs/2606.21661) · PDF
  `2606.21661.pdf`. Built on LTX-2.3, maintains two fixed-size memory slots (long-term anchored to the
  opening shot, short-term holding the previous tail) updated at each cut by a boundary-conditioned
  gate fusing visual-cut probability and beat-tracker signals; a reference speaker token preserves
  vocal timbre across shots. Releases a 200-sequence multi-cultural multi-shot benchmark; leads
  open-source baselines on cross-shot coherence across I2V/T2V/R2V. *Multi-shot audio-video.*

## Controllability & Editing

- **TriMotion: Modality-Agnostic Camera Control for Video Generation** — Seunghyun Shin, Jifei Song,
  Hae-Gon Jeon, Jiankang Deng et al. [arXiv:2606.20774](https://arxiv.org/abs/2606.20774) · PDF
  `2606.20774.pdf`. Maps video, pose, and text descriptions of the *same* camera trajectory into a
  shared motion embedding space, trained with a new geometry-grounded Motion Triplet Dataset and a
  latent motion-consistency objective (avoiding pixel-space decoding). Enables sequential motion
  composition and cross-modal motion interpolation. *Camera control.*

- **OrthoMotion: Disentangling Camera and Subject Motion via Geometry-Semantics Orthogonal Attention**
  — Zijie Meng. [arXiv:2606.22835](https://arxiv.org/abs/2606.22835) · PDF `2606.22835.pdf`. Proves
  that camera/subject entanglement in 2D conditioning is *representational, not architectural* (a
  non-identifiable inverse problem), then routes camera motion into a norm-preserving RoPE-phase
  rotation and subject motion into a gated cross-attention value injection. A decoupling regularizer
  drives the two response subspaces to orthogonality, cutting a new Cross-Talk Error metric by >2.4×
  with no fidelity loss. *Motion disentanglement.*

- **Vera: A Layered Diffusion Model for Content-Preserving Video Editing** — Hongkai Zheng, Ta-Ying
  Cheng, Yisong Yue, Zhuoning Yuan. [arXiv:2606.23610](https://arxiv.org/abs/2606.23610) · PDF
  `2606.23610.pdf`. Instead of regenerating every pixel, Vera generates an *edit layer* plus an alpha
  matte for compositing over the source video, separating creative editing from content preservation
  by design. Extends a T2V DiT into a Mixture-of-Transformers with per-layer DiTs interacting via
  joint self-attention; trained on 486K frames of layered data with accurate mattes. *Video editing.*

- **DomainShuttle: Freeform Open-Domain Subject-driven Text-to-Video Generation** — Nan Chen, Yiyang
  Cai, Rongchang Xie et al. [arXiv:2606.26058](https://arxiv.org/abs/2606.26058) · PDF
  `2606.26058.pdf`. Subject-driven (S2V) generation that "shuttles" between in-domain (max subject
  fidelity) and cross-domain (preserve intrinsic features, vary style/attributes per prompt) regimes.
  Uses a Domain-MoT with domain-aware AdaLN, a Video-Reference DualRoPE that places reference and
  video tokens in separate RoPE spaces, and a Cross-Pair Consistent Loss for intrinsic-feature
  extraction. *Subject-driven T2V.*

- **DramaDirector: Geometry-Guided Short Drama Generation** — Hengji Zhou, Sijie Liu, Lianghao Xia,
  Liqiang Nie et al. [arXiv:2606.24107](https://arxiv.org/abs/2606.24107) · PDF `2606.24107.pdf`.
  Plot-to-short-drama generation: a planner borrows cinematographic geometry from a gallery of real
  short-drama shots indexed by depth and pose, decouples static-visual vs dynamic-narrative
  conditions, and is trained with schema-constrained SFT + GRPO under a text-visual alignment reward.
  Introduces the DramaBoard benchmark (35 dramas, 2.8K episodes, 81K shots).
  [Code](https://github.com/iLearn-Lab/DramaDirector). *Story-to-video.*

## 4D / Novel-view video

- **MVTrack4Gen: Multi-View Point Tracking as Geometric Supervision for 4D Video Generation** —
  JoungBin Lee, Jaewoo Jung, Takuya Narihira et al.
  [arXiv:2606.26087](https://arxiv.org/abs/2606.26087) · PDF `2606.26087.pdf`. Novel-view video
  synthesis from a monocular reference along a target trajectory. Finds that specific attention layers
  encode strong cross-view/temporal correspondence cues; routes them into an auxiliary multi-view
  point-tracking head and jointly trains the diffusion model with a point-tracking objective,
  improving geometric and motion consistency for camera-conditioning-only models. *4D / novel-view.*

## Video world models

- **Compression and Retrieval (CaR): Implicit Memory Retrieval for Video World Models** — Zhan Peng,
  Jie Ma, Huiqiang Sun et al. [arXiv:2606.23105](https://arxiv.org/abs/2606.23105) · PDF
  `2606.23105.pdf`. Attention-driven implicit memory retrieval for long-horizon consistency: viewpoint
  info is injected via positional encoding so memory is retrieved through attention, plus a
  lightweight context-compression network for cheap long contexts. Introduces SceneFly, a large
  synthetic dataset with realistic camera trajectories and frame-level annotations. *World models.*

- **Autonomous Video Generation with Counterfactual Controllability for Self-Evolving World Models** —
  Xin Wang, Wenxuan Liu, Tongtong Feng, Wenwu Zhu (Tsinghua).
  [arXiv:2606.24152](https://arxiv.org/abs/2606.24152) · PDF `2606.24152.pdf`. Position paper pushing
  back on "video generation *is* world modelling": argues current models learn only a partial,
  implicit spatiotemporal world model. Proposes *counterfactual controllability* — asking what happens
  under an action, testing whether the generated future survives embodiment constraints, and feeding
  action knowledge back into imagination — as the criterion for self-evolving world models.
  *World models (position).*

## Evaluation & Benchmarks

- **Physics Question Scene Graph (PQSG): Fine-grained Evaluation of Physical Plausibility in T2V** —
  Atin Pothiraj, Jaemin Cho, Elias Stengel-Eskin, Mohit Bansal (UNC).
  [arXiv:2606.25306](https://arxiv.org/abs/2606.25306) · PDF `2606.25306.pdf`. A hierarchical,
  graph-structured VLM question pipeline that localizes physical-law violations across objects,
  actions, and physics, with logical dependencies ensuring each query is contextually valid.
  Introduces FinePhyEval (physics prompts + human-annotated videos from Sora 2, Veo 3, Wan 2.1);
  reports higher correlation with human judgment than prior work and ranks closed-source models above
  Wan 2.1 on physical realism. *Evaluation.*

- **GeoT2V-Bench: Benchmarking 3D Consistency in Text-to-Video Models via 3D Reconstruction** —
  Chenrui Fan, Paolo Favaro (Bern). [arXiv:2606.24829](https://arxiv.org/abs/2606.24829) · PDF
  `2606.24829.pdf`. Reconstruction-based diagnostic for camera-prompted T2V: estimates per-frame
  intrinsics/poses (VGGT-style), fits DeformableGS, derives a static MedianGS proxy and renders it
  along the estimated path. Reports a continuous reconstruction *profile* (image motion, trajectory
  behavior, static rendering error, flow agreement) rather than a single score; 3,840 reconstructions
  across 12 open-weight configs reveal complementary failure modes. *Evaluation / 3D consistency.*

## Safety & Protection

- **VPA-Guard: Defending and Benchmarking I2V Generation Against Visual Prompt Attacks** — Yining Sun,
  Haoyu Kang, Jiajun Wu et al. [arXiv:2606.25592](https://arxiv.org/abs/2606.25592) · PDF
  `2606.25592.pdf`. Shows that innocuous static visual cues (arrows, sketches, emojis) in I2V inputs
  can be read as executable temporal instructions and unfold into harmful actions. Introduces
  VVA-Bench (first vision-centric prompt-attack benchmark for video safety) — ASR reaches 100% on
  Wan 2.7 and 74.8% on Veo 3.1 — and VPA-Guard, a retrieval-augmented self-evolving defense that cuts
  ASR by 44.2% and harmfulness by 73.4% while preserving utility. *Safety.*

- **ChronoLock: Protecting Videos from Unauthorized Text-to-Video Personalization** — Jiaming He,
  Jiashu Zhang, Hanwei Zhu, Yi Yu et al. [arXiv:2606.21146](https://arxiv.org/abs/2606.21146) · PDF
  `2606.21146.pdf`. First proactive perturbation framework targeting the *temporal* denoising dynamics
  that make T2V personalization possible (prior protections target static appearance). Disrupts
  intra-chunk temporal adaptation and enlarges inter-chunk boundary mismatch, with transformation-
  sampled updates for robustness to preprocessing. Reduces motion imitation on UCF Sports / HMDB51
  across popular T2V backbones. *Protection / data misuse.*

---

### Items noted but not expanded (in-window, video-adjacent)
A number of related in-window papers were surfaced but kept out of the main list to avoid padding —
e.g. *TIGER* (face video restoration, 2606.24336), *Recommendation as Generation* (industrial
personalized video, 2606.25496), *CineCap* (cinematographic captioning, 2606.24636), and several
robotics/embodied "world model" papers that fall outside generative video proper.

### Coverage notes
- All 21 highlighted items are in-window arXiv submissions (2026-06-19 → 2026-06-26) with verified IDs
  and downloaded PDFs.
- No genuinely new closed/foundation **model launch** was verifiable in-window beyond Grok Imagine
  Video 1.5 (GA 2026-06-25), already recorded under `research/2026-06-25/`.
- Deduplicated against 2026-06-24 and 2026-06-25 digests (different paper IDs; no overlap).
