# Video Generation Research Digest — 2026-06-24

**Coverage window:** ~last 7 days requested (≈June 17–24, 2026), extended back to **early June 2026** for the reasons noted below.

> **Honesty note on freshness & access.** This is the first digest in this repo, so there is no prior-day overlap to deduplicate against. Two environment constraints shaped what could be captured:
> 1. **arXiv and Hugging Face are blocked by this environment's egress policy** (HTTP 403 from the proxy on `arxiv.org` and `huggingface.co`). I could therefore not browse the arXiv `cs.CV`/`cs.LG` daily listings, the aha-time AIGC tracker (403), or download PDFs directly. GitHub (incl. `raw.githubusercontent.com`) is reachable.
> 2. **Web-search indexing lags ~10 days** for this niche. Searches for the literal last 7 days (arXiv IDs `2606.17xxx`–`2606.22xxx`) returned no indexed video-generation papers yet, even though non-video late-June IDs (e.g. `2606.20061`, `2606.22343`) are visible. The freshest *verifiable* video-generation work is therefore from **the first half of June 2026**, listed below with confirmed arXiv IDs and dates.
>
> Every item below was cross-checked to a real arXiv ID / project page. Nothing here is fabricated. The curated awesome-list (ziqihuangg/Awesome-From-Video-Generation-to-World-Model) was also checked but its newest entries only reach **March 2026**, so it added nothing new for this window.

---

## Top 3 reads today

### 1. MilliVid: Hierarchical Latents for Long-Range Consistency in Video Generation
- **Authors / org:** P. Chandratreya, D. Charatan, B. Van Hoorick, S. Zakharov, V. Guizilini, P. Isola, V. Sitzmann (MIT CSAIL + Toyota Research Institute — Scene Representation Group)
- **Link:** https://arxiv.org/abs/2606.09056 · project: https://scenerepresentations.org/publications/millivid/ · HF: https://huggingface.co/papers/2606.09056
- **Date:** 2026-06-08 · **Subfield:** Long-video generation / consistency
- **Summary:** Long-range consistency is hard because even a few dozen frames blow up transformer sequence length. MilliVid pretrains an autoencoder that compresses each frame into a *hierarchy* of tokens — from full latent resolution down to a handful of tokens per frame — where coarse levels carry scene layout/semantics and fine levels add appearance/texture. A video diffusion model then generates via **coarse-to-fine rollout** in this multi-scale token space, mitigating the sequence-length explosion while preserving long-range coherence. A clean, architecturally interesting take on the perennial long-video consistency problem.

### 2. ViMax: Agentic Video Generation
- **Authors / org:** HKUDS (University of Hong Kong Data Intelligence Lab)
- **Link:** https://arxiv.org/abs/2606.07649 · HTML: https://arxiv.org/html/2606.07649
- **Date:** ≈2026-06-02 · **Subfield:** Long-form / agentic video generation
- **Summary:** Targets long-form video that needs narrative planning and visual consistency beyond what single short-clip models provide. ViMax frames generation as a **multi-agent pipeline** (director / screenwriter / producer / generator) whose specialized agents negotiate narrative decisions, visual continuity, and production quality before and during synthesis. Representative of the shift from "one model, one clip" toward orchestrated, agentic video production systems.

### 3. Making Time Editable in Video Diffusion Transformers
- **Authors / org:** (arXiv 2606.10183 — see abstract page)
- **Link:** https://arxiv.org/abs/2606.10183 · PDF: https://arxiv.org/pdf/2606.10183
- **Date:** 2026-06-08 · **Subfield:** Controllable video / DiT
- **Summary:** Proposes a temporal-control method that extends a pretrained video Diffusion Transformer with **explicit "time editing"** — direct control over motion speed and temporal structure of a generated clip. Addresses a gap in DiT-based video models, where spatial/semantic control has matured faster than fine-grained control of *time* itself (slow-mo, speed ramps, re-timing) without retraining the base model.

---

## More items by subfield

### Long-video / autoregressive generation
- **LongLive-RAG: A General Retrieval-Augmented Framework for Long Video Generation** — arXiv:2606.02553 (2026-06-01) · https://arxiv.org/abs/2606.02553
  Autoregressive video diffusion enables variable-length synthesis but suffers accumulated errors and identity drift over long horizons. LongLive-RAG brings **retrieval-augmented generation** to long video: it retrieves and conditions on previously generated context to anchor identity and reduce drift, presenting RAG as a general framework rather than a single architecture. Complements the global-state / memory line of work (VRAG, RELIC).

### Streaming / real-time & efficiency
- **Real-Time Generation of Streamable Talking Portrait Video with Reference-Guided Deep Compression VAEs** — arXiv:2606.01620 (2026-06-01) · https://arxiv.org/abs/2606.01620 · https://arxiv.org/html/2606.01620v1
  Speech-audio- and reference-image-conditioned talking-portrait generation built for streaming. Uses a **causal video VAE** for deep latent compression plus an **autoregressive latent denoising** model so frames can be produced and streamed with low latency. Sits at the intersection of efficiency, streaming/autoregressive video, and audio-driven generation.

### Multi-modal generation (adjacent — image, not video)
- **MMDiff: Extending Diffusion Transformers for Multi-Modal Generation** — arXiv:2606.16673 (2026-06-15, Oxford VGG) · https://arxiv.org/abs/2606.16673
  *Listed for completeness and explicitly flagged as out-of-scope:* MMDiff turns a frozen image DiT into a multi-modal generator producing images jointly with dense perceptual modalities (e.g. segmentation), reporting up to +28.7% mIoU from multi-timestep feature fusion. It is **image + perception, not video generation** — included only so a future digest doesn't mistake it for new video work.

---

## Industry / model-release context (no new open research artifacts this window)
No *new* major video-model release landed in the last 7 days that I could verify; the spring 2026 wave (Kling 3.0 — Feb; Seedance 2.0 — Feb; Wan 2.6/2.7 — Apr; Veo 3.1) is the current commercial baseline. Treat blog "best-of-2026" roundups (Pinggy, WaveSpeed, TeamDay) as secondary/marketing context, not primary research.

## Items that could NOT be fetched (and why)
- **All arXiv PDFs** (`arxiv.org/pdf/<id>`) — blocked by environment egress policy (proxy 403). Could not save `<id>.pdf` artifacts this run. Metadata + abstracts captured instead as `*.md` files in this folder.
- **aha-time AIGC tracker** (`aha-time.com/arxiv_daily_aigc`) — HTTP 403.
- **Hugging Face papers/blog** (`huggingface.co`) — blocked by egress policy (proxy 403).
- **arXiv `cs.CV`/`cs.LG` daily "recent" listings** — 403; prevented direct enumeration of the literal last-7-day submissions.

*If PDF capture is important, allow-listing `arxiv.org` (and optionally `huggingface.co`) in the environment's network egress settings would let a future run download the actual artifacts.*
