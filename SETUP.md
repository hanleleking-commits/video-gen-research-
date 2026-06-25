# Environment Setup — Network Egress for the Research Routine

This repo is maintained by a scheduled **Claude Code on the web** routine that tracks
video-generation research. The routine's primary sources are **arXiv**, **Hugging Face**,
the **aha-time AIGC tracker**, and project/blog pages. Several of these are blocked by the
default network policy, which is why early digests carry an "environment caveat" note and
could not download PDF artifacts.

## Why sources get blocked

Cloud environments default to the **Trusted** network access level, which only allows a fixed
allowlist (package registries, GitHub, cloud SDKs). `arxiv.org`, `huggingface.co`,
`aha-time.com`, and `x.ai` are **not** on that list, so requests to them are denied at the
egress proxy (observed as `curl` exit `000` and `WebFetch` HTTP 403).

## Fix: switch the environment to Custom network access

1. Open the environment / routine for editing via the **cloud icon** (shown wherever you start
   a cloud session or configure a routine — there is no separate "Environments" page). For a
   routine, it is under **Environments and network access**.
2. Change the **Network access** selector from **Trusted** to **Custom**.
3. In the **Allowed domains** field, add one domain per line:

   ```text
   arxiv.org
   *.arxiv.org
   huggingface.co
   *.hf.co
   cdn-lfs.huggingface.co
   aha-time.com
   www.aha-time.com
   x.ai
   ```

4. **Check "Also include default list of common package managers"** — otherwise GitHub / npm /
   pip access is removed and the routine's `git push` and setup steps break. You want the custom
   domains **plus** the defaults.
5. Save.

### What each domain enables
- `arxiv.org` + `*.arxiv.org` — abstract pages and `arxiv.org/pdf/<id>` PDF downloads (and
  `export.arxiv.org` API). Enables saving `<id>.pdf` artifacts and browsing the cs.CV/cs.LG
  daily listings directly.
- `huggingface.co` + `*.hf.co` + `cdn-lfs.huggingface.co` — the site plus the LFS/CDN hosts that
  serve large files (page loads can work while file downloads still fail without the CDN host).
- `aha-time.com` + `www.aha-time.com` — the daily AIGC arXiv tracker.
- `x.ai` — removes the egress block on the Grok announcement pages. Note: `WebFetch` may still
  return 403 from sites that block automated fetchers regardless of egress policy, so this is
  not a guaranteed fix for `x.ai` specifically — but it clears the part under your control.

### Alternative
Setting the level to **Full** (any domain) also works and is simpler, but removes egress
allowlisting entirely. **Custom** is recommended for a routine that only needs a handful of
extra sites.

## Notes
- **GitHub uses a separate proxy** independent of the network-access setting, so this change does
  not affect `git` operations either way.
- **MCP connector traffic** is routed through Anthropic's servers and does not require allowlist
  entries.
- Reference: <https://code.claude.com/docs/en/claude-code-on-the-web> (Network access) and
  <https://code.claude.com/docs/en/routines> (Environments and network access).

## Branch note
The repo briefly had two diverged branches (`main` and `master`). They have been re-aligned to
the same commit. Keep a single default branch (delete the other from the GitHub Branches page)
so future digests don't split across branches. Branch deletion cannot be done from inside the
cloud environment (the git relay permits pushes but blocks ref deletion), so do it in the GitHub UI.
