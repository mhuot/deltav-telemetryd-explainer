# Delta-V flows — explainer video

An 83-second narrated explainer of how flow telemetry ingestion works in
Delta-V: Minion → minion-gateway → Kafka → flow-enricher → Kafka →
ClickHouse. The entire video is source code: every scene, word, color, and
timing is a one-line edit in `video/index.html` followed by a ~30-second
re-render.

**Watch:** [`production/renders/explainer-v3.mp4`](production/renders/explainer-v3.mp4)

## How it was made

Produced entirely locally (no cloud generation) with:

| Tool | Role |
|------|------|
| [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M) | Narration TTS (voice `af_heart`, one WAV per scene) |
| [HyperFrames](https://github.com/heygen-com/hyperframes) | HTML/CSS/GSAP composition → deterministic MP4 via headless Chrome |
| FFmpeg | Music synthesis (`aevalsrc` sine pad), encode, QA probes |
| GSAP 3 | Scene animation (vendored, no CDN) |

The production method — script locks first, every downstream number is
*derived* from measured narration audio, validation gates before render —
follows the explainer-video workflow originated by
[Idan Shimon](https://github.com/idanshimon).

## Repo layout

```
production/
  research/research-brief.md      # every scripted claim, source-mapped; SME corrections
  proposal.md                     # narrative arc, scene list, theme
  script/script.md                # locked narration (v1→v3 revision history)
  scene_plan/scene-plan.md        # derived timing table + per-scene visual notes
  checkpoints/decision-log.json   # append-only production decisions
  checkpoints/self-review.md      # post-render QA evidence (all versions)
  checkpoints/snapshots|frames/   # reviewed stills
  assets/audio/                   # Kokoro WAVs + durations.json
  renders/explainer-v3.mp4        # the deliverable
tools/tts_generate.py             # narration synthesis (edit SCENE_NARRATIONS)
video/index.html                  # THE VIDEO — 7 scenes, one GSAP timeline
video/assets/                     # vendored gsap + scene audio
```

## Re-rendering

```bash
# prerequisites: node >= 22, bun, ffmpeg with libx264, a HyperFrames build
cd video
node "$HYPERFRAMES_DIR/packages/cli/dist/cli.js" lint
node "$HYPERFRAMES_DIR/packages/cli/dist/cli.js" check
node "$HYPERFRAMES_DIR/packages/cli/dist/cli.js" render --quality high \
  --output ../production/renders/explainer.mp4
```

To change narration: edit `SCENE_NARRATIONS` in `tools/tts_generate.py`,
run it in a Python 3.12 venv with `kokoro`, `numpy`, `soundfile`, then
re-derive the timing table in `production/scene_plan/scene-plan.md` from
the new `durations.json` — scene `data-start/duration`, `<audio
data-start>`, and the JS scene constants in `video/index.html` must agree.

## Version history

- **v3** — reframed as "Delta-V flows, explained" per SME direction: the
  vestigial telemetryd daemon is out of scope for the Flows conversation
  and no longer appears; scaling is attributed to pure Kafka (4 partitions
  per topic; consumer groups spread them — enricher replicas and
  ClickHouse's consumer threads alike).
- **v2** — corrected per SME review: flow-enricher is a Spring Cloud Stream
  *processor* (consumes raw-flow topics, produces enriched flows to the
  `deltav-flows` Kafka topic; ClickHouse ingests it with its own Kafka
  consumer).
- **v1** — superseded; not included in this repo.
