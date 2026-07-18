# Proposal — "Where do Delta-V flows actually go?"

**Shape:** concept explainer (step diagrams). **Target:** ~65–70 s, 7 scenes,
1920×1080, 30 fps. **Audience:** engineers new to the Delta-V codebase.

## Arc

| # | Scene id | Role | Visual |
|---|----------|------|--------|
| 1 | s1_hook | Hook: which service processes a flow packet? Not telemetryd. | Question headline; "not telemetryd" accent reveal |
| 2 | s2_context | telemetryd is a hollowed-out shell; the pipeline bypasses it | telemetryd card grayed out, "all 8 queues disabled" stamp, bypass arrow |
| 3 | s3_step1 | Step 01 — Minion: one UDP port, 4-byte protocol sniff | Port funnel + four protocol badges with version-header bytes |
| 4 | s4_step2 | Step 02 — gateway: per-protocol gRPC methods → per-protocol Kafka topics | Four lanes: gRPC method → topic chip, key label |
| 5 | s5_step3 | Step 03 — flow-enricher: one SCS binding, parse + enrich → ClickHouse | Topics converge into enricher ×N → ClickHouse cylinder |
| 6 | s6_meaning | Why it's built this way: content-blind coupling + scale by replicas | 4×4 partition grid lighting up, replica counter to 16 |
| 7 | s7_recap | Recap: full pipeline drawn end-to-end; telemetryd stays as extension seat | Left-to-right flow diagram, edges drawn in sync; title card |

## Theme

Dark neutral ground (deep navy `#0d1520`), panels `#16212e`, ink
`#e8eef4`, ONE accent `#4cc2ff`, hairlines `#2a3947`. Prose in
"Segoe UI"/system-ui; identifiers (topics, ports, class names) in
ui-monospace. Calm pacing: 0.4–0.6 s scene padding, eased entrances,
SVG edge draws synced to narration beats.

## Audio

Kokoro `af_heart` @ 1.1, one WAV per scene (track 7). A quiet synthesized
pad on track 8 at volume 0.11 — the subject is abstract infrastructure and
a low bed keeps the 70 s from feeling clinical.

## Output

H.264 + AAC master 1920×1080 → `production/renders/explainer-v1.mp4`.
No publication packaging unless requested.
