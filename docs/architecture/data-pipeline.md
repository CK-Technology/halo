# Data Pipeline

> **Status:** Design intent.

This traces a single event from a source to its destinations.

```
 source            Eye                 Halo Core                    Output
┌────────┐     ┌──────────┐     ┌──────────────────────┐     ┌────────────┐
│ raw    │ ──► │ collect  │ ──► │ normalize → enrich → │ ──► │ format +   │
│ data   │     │ + parse  │     │ fan-out              │     │ ship       │
└────────┘     └──────────┘     └──────────────────────┘     └────────────┘
```

## Stages

### 1. Collect

The Eye pulls or tails its source (LAPI stream, REST poll, log tail, metrics scrape) and
parses raw payloads into structured records. Transport and auth details are the Eye's
concern; everything downstream sees structured data.

### 2. Normalize

Core maps the Eye's structured record onto the
[common event shape](core.md#normalized-event-proposed). This is where source-specific
severities, field names, and timestamps are reconciled.

### 3. Enrich *(optional)*

Add derived context — origin host, normalized severity, tags — without mutating `raw`.

### 4. Fan-out

The normalized event is pushed onto the internal stream and delivered to:

- the `/events` API (live consumers),
- the `/metrics` counters (aggregates),
- every enabled [Output](../integrations/outputs.md).

## Backpressure & reliability

- Channels between stages are **bounded**; a slow Output applies backpressure rather than
  consuming unbounded memory.
- A failing Eye is **isolated and restarted**; other Eyes keep flowing.
- Delivery semantics per Output (at-least-once vs. best-effort) are documented per sink as
  they are implemented.
