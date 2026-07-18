# Script — LOCKED v2 (2026-07-18)

Revision history: v1 locked 2026-07-17. v2 changes ONLY s5_step3, per SME
feedback (SME): the enricher does not write to ClickHouse directly — it is
an SCS processor that produces enriched flows to the `deltav-flows` Kafka
topic, which ClickHouse ingests with its own Kafka consumer.

v2 s5_step3 (32 words, measured 14.38 s): "Step three, the flow enricher.
One Spring Cloud Stream binding consumes all four topics, parses each
protocol, adds node context, and publishes enriched flows back to Kafka —
ClickHouse's own consumer ingests them."

Original v1 script below for the record; all other scenes unchanged.

---


Voice: Kokoro `af_heart`, speed 1.1, lang `a`. Budget ≈2.55 w/s.
TTS column uses spaced acronyms ("U D P", "g R P C", "telemetry d");
on-screen text uses real spellings (UDP, gRPC, telemetryd).
186 words → ≈73 s narration → ≈78 s total with breathing room.

| Scene | Narration (TTS text, verbatim) | Words |
|-------|-------------------------------|-------|
| s1_hook | When a router sends a flow packet into Delta V, which service actually processes it? Here's the surprise: it isn't telemetry d. | 22 |
| s2_context | Delta V still ships telemetry d, but as a hollowed out shell: all eight of its queues are disabled. The real pipeline bypasses it entirely, in three steps. | 28 |
| s3_step1 | Step one, the Minion. One U D P port, four seven two nine, receives all four flow protocols. A four byte version sniff tells them apart; nothing else is parsed. | 30 |
| s4_step2 | Step two, the gateway. Each protocol streams over its own g R P C method, and lands on its own Kafka topic, keyed by location and Minion I D. | 29 |
| s5_step3 | Step three, the flow enricher. One Spring Cloud Stream binding consumes all four topics, parses each protocol, adds node context, and writes flow documents to ClickHouse. | 26 |
| s6_meaning | The payoff: the coupling is content blind — only the enricher understands flows. And scaling is just replicas: Kafka spreads sixteen partitions across the group, with no code or config changes. | 31 |
| s7_recap | So: Minion, gateway, Kafka, enricher, ClickHouse. Telemetry d keeps its seat for future protocols — but today, it just watches the traffic go by. | 24 |

Claim mapping: s1→facts 1–2, s2→facts 1,2,9, s3→facts 3–5, s4→facts 6–7,
s5→fact 8, s6→facts 11–13, s7→facts 2,10 (see research-brief.md).
