# Video Generation Research Tracker — Index

Daily digests of new work in video generation: text-to-video, image-to-video, video diffusion,
autoregressive/streaming video, video world models, and efficiency/distillation.

Each dated folder under `research/` contains a `summary.md` (ranked, grouped by subfield) plus
per-paper metadata files.

| Date | Top headlines | Digest |
|------|---------------|--------|
| 2026-06-26 | Causal-rCM (unified TF+SF autoregressive diffusion distillation; 2-step Wan2.1-1.3B → VBench-T2V 84.63, applied to Cosmos 3) · Wan-Streamer v0.1 (Alibaba end-to-end real-time interactive foundation model, sub-second full-duplex A/V) · Towards Error-Free Long Video Generation (infinite single-shot, T-RFlow) — **busy arXiv week** (21 in-window papers); arxiv/HF/aha-time now reachable, PDFs captured | [research/2026-06-26/summary.md](research/2026-06-26/summary.md) |
| 2026-06-25 | Grok Imagine Video 1.5 GA (xAI; #1 Image-to-Video Arena, native single-pass audio, ~6s 720p in ~25s, $4.20/min) — **light day**, no new in-window arXiv video papers verifiable | [research/2026-06-25/summary.md](research/2026-06-25/summary.md) |
| 2026-06-24 | MilliVid (hierarchical coarse-to-fine latents for long-range consistency) · ViMax (agentic multi-agent long-form video) · Making Time Editable in Video DiTs (explicit temporal control) · LongLive-RAG (retrieval-augmented long video) | [research/2026-06-24/summary.md](research/2026-06-24/summary.md) |

---

### Notes
- **Coverage:** primary sources are arXiv (cs.CV / cs.LG), official project/blog pages, the
  [Awesome-From-Video-Generation-to-World-Model](https://github.com/ziqihuangg/Awesome-From-Video-Generation-to-World-Model)
  list, and the [arXiv daily AIGC tracker](https://www.aha-time.com/arxiv_daily_aigc/).
- **Environment caveat — RESOLVED as of 2026-06-26:** in earlier runs (2026-06-24/25) `arxiv.org`,
  `huggingface.co`, and `aha-time.com` were unreachable (curl `000` / `WebFetch` 403). As of
  2026-06-26 all three are reachable from this environment: the arXiv API (`export.arxiv.org/api/query`)
  was used to browse the literal last-7-day listing and PDFs were downloaded directly from
  `https://arxiv.org/pdf/<id>`. The `aha-time.com` tracker page loads but renders its report list
  client-side, so the arXiv API remains the more reliable primary source. See [SETUP.md](SETUP.md)
  for network-config background.
