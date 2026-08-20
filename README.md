# 야식 — issue-shorts channel

Shorts-only channel. Issue/어그로/실시간 트렌드 콘텐츠. Launch platforms: YouTube
Shorts + TikTok. Planned expansion (not this phase): Instagram Reels, Naver,
Kakao. Target cadence: as many uploads/day as the pipeline and the human
publish-confirmation step allow, aimed at roughly one per hour — see
`docs/decisions/0020-yasik-issue-shorts-channel.md` for why that number is a
ceiling on production throughput, not on publish automation (REQ-0025/REQ-0031
require a human to confirm every externally visible publish; no exception is
made for this channel).

Read before touching this project:

- [`docs/decisions/0020-yasik-issue-shorts-channel.md`](../../docs/decisions/0020-yasik-issue-shorts-channel.md) — the design doc: architecture, package reuse map, sourcing/rights strategy, automation ceiling, dashboard registration.
- [`inputs/reference-library/`](inputs/reference-library/) — frame-and-cut corpus of 6 of the 7 reference channels the owner named, top 5 videos by view count each. **Analysis-only** (REQ-0006): informs format design, is never reproduced, composited, or shipped as 야식 content. `CHANNEL-STYLE-ANALYSIS.md` there is the completed cross-channel synthesis and three draft `ShortsEditTemplateContract` records (pacing/hook design proposals).
- [`LAYOUT-TEMPLATES.md`](LAYOUT-TEMPLATES.md) — two more `ShortsEditTemplateContract` records, but for a different axis: real, implemented *visual layout* (text zone placement), not pacing. `tpl_yasik_sandwich_small_video` is the currently active render structure; `tpl_yasik_full_video_centered_caption` is the earlier structure it replaced, kept named and documented rather than only recoverable from git history.
- [`brief/content-sourcing-research-2026-08-20.md`](brief/content-sourcing-research-2026-08-20.md) and [`brief/content-sourcing-research-round2-2026-08-20.md`](brief/content-sourcing-research-round2-2026-08-20.md) — the robots.txt/ToS research behind every REQ-0037 provider adapter; read before adding a new source rather than assuming a plausible-looking site is safe to add.
- [`config/brands/yasik.json`](../../config/brands/yasik.json) — brand/channel profile.
- `packages/shorts-runtime` — the domain package this project's pipeline runs on: topic ledger/dedup, edit-template contracts. Everything else (collection, rights gate, rendering, QA, multi-platform publish) is reused from the packages already in this repo — see the ADR's reuse map before adding a new package.
- [`scripts/issue-sourcing-cycle.mjs`](scripts/issue-sourcing-cycle.mjs) — runs one real sourcing cycle: fetches all 13 REQ-0037 sources live (12 web + 1 public Telegram channel), dedups through the topic ledger, persists to `work/topic-ledger.json`. `node projects/yasik-shorts/scripts/issue-sourcing-cycle.mjs` from the repo root. Proven live 2026-08-20 across three runs, most recently 199 signals → 112 new + 87 merged into a 297-topic ledger. Deliberately NOT REQ-0011's durable SQLite queue or a scheduler — a lightweight JSON-file bridge so repeated runs actually deduplicate; promoting it to durable storage is separate, undone work.

## Reference channels (owner-supplied, 2026-08-19)

| Channel | Handle | Status |
| --- | --- | --- |
| 뇌전구 | `@뇌전구` | collected |
| 콩만이 | `@Kongman2` | collected |
| 노빠꾸소개맨 | `@노빠꾸소개맨` | collected |
| 잡동사니 | `@잡동사니` | **handle resolves to an unrelated small livestream/gaming channel (1,320 subs) — not an issue-shorts channel. Needs the correct handle from the owner before collection.** |
| 썰스피커 | `@썰스피커` | collected |
| 만렙백수 | `@lv999_backsoo` | collected |
| 탁탐정 | `@탁탐정` | collected |

## Why this is not `projects/youtube-longform`

시드/SEED (`projects/youtube-longform`) is a single long-form documentary
channel with a persona narrator and a 20+ minute per-episode pipeline. 야식 is
the opposite production shape: many short, disposable, high-cadence pieces
across multiple platform accounts, sourced from real-time issues rather than
researched long-form scripts. They share the rendering/QA/publish substrate
but are different projects with different cadence, sourcing, and approval
economics — do not merge their briefs or reuse one channel's claims/consent
scope for the other.
