# Scene plan — derived from measured narration (durations.json)

Gap between narrations: 0.5 s. Scene visual starts 0.3 s before its
narration (scene 1 starts at 0). TOTAL = last narration end 75.97 + 0.93
fade = **76.90 s**.

| Scene | n_start | n_dur | n_end | scene_start | scene_dur |
|-------|---------|-------|-------|-------------|-----------|
| s1_hook | 0.50 | 7.95 | 8.45 | 0.00 | 8.65 |
| s2_context | 8.95 | 10.05 | 19.00 | 8.65 | 10.55 |
| s3_step1 | 19.50 | 10.85 | 30.35 | 19.20 | 11.35 |
| s4_step2 | 30.85 | 10.05 | 40.90 | 30.55 | 10.55 |
| s5_step3 | 41.40 | 11.12 | 52.52 | 41.10 | 11.62 |
| s6_meaning | 53.02 | 12.65 | 65.67 | 52.72 | 13.15 |
| s7_recap | 66.17 | 9.80 | 75.97 | 65.87 | 11.03 |

Music: 76.9 s pad, fade-out at 73.4, track 8, volume 0.11.

## Visual notes (one idea per scene)

1. **s1_hook** — centered question "A flow packet arrives. Which service
   processes it?"; at ~6.3 s (narration beat "it isn't telemetry d") the
   answer chip "not telemetryd" slides in with a strike-through accent.
2. **s2_context** — telemetryd panel (grayed, mono label
   `daemon-boot-telemetryd`) with 8 queue chips (JTI, OpenConfig,
   Netflow-5/9, IPFIX, SFlow, NXOS, BMP, Graphite) each stamped
   DISABLED as narration says "all eight queues"; an accent bypass arrow
   draws underneath the panel ("the real pipeline bypasses it").
3. **s3_step1** — step chip 01, title "Minion — one UDP port". Four
   protocol badges (Netflow-5 `0x0005`, Netflow-9 `0x0009`, IPFIX
   `0x000A`, sFlow `5`) drop toward a port box `UDP 4729`; caption
   "FlowProtocol.detect() — 4-byte sniff, payload stays opaque".
4. **s4_step2** — step chip 02, title "minion-gateway". Four lanes, each
   `publish<Proto>` gRPC method → drawn edge → Kafka topic chip
   `DeltaV.Sink.Telemetry-<proto>`; footer "key = location@minion-id".
5. **s5_step3** — step chip 03, title "flow-enricher". Four topic chips
   converge via drawn edges into an enricher panel ("one Spring Cloud
   Stream binding", replicas badge ×N), node-context side input, edge →
   ClickHouse cylinder (`deltav-flows`).
6. **s6_meaning** — two panels. Left "Content-blind": mini pipeline with
   Minion/gateway marked "no parsing", enricher marked "all protocol
   semantics". Right "Scale = replicas": 4×4 partition grid cells light
   up sequentially, counter rises to "16 assignable partitions".
7. **s7_recap** — full pipeline drawn left→right (Exporters → Minion →
   gateway → Kafka → flow-enricher → ClickHouse), dashed gray spur to
   telemetryd ("extension seat"); closing title "Delta-V telemetry,
   explained."
