# Aegis Red Agents

Operator-grade autonomous red-team platform.

All platform agents operate under the standing protocols in
`PROTOCOL.md` — they are binding, not advisory.

## Architecture

The platform lives in **this repo**: Ops Console evolution, the provider
gateway/fallback layer, the skills-as-data registry, memory tiering, and —
later — tenancy and audit.

The SecScan RedTeam desktop fork is the **pinned runtime foundation**. It
lives under `runtime/` as a git submodule
(`runtime/secscan-redteam-desktop`) and ships the runner, the harness, and
the desktop app the platform operates through.

The pin is deliberate. Bump it only as its own reviewed change — never
casually, never as a side effect of other work.

## Build order

From the approved plan, in order:

1. **Console** — done; ships in the pinned runtime (Phase 5 operator
   console + scope guard).
2. **Provider fallback / breadth** — gateway-style provider routing so a
   single provider outage or refusal never stalls an engagement.
3. **Skills-as-data** — capabilities as versioned data in the registry,
   not hard-coded behavior.
4. **Memory tiering** — hot/warm/cold recall for agents and engagements.
5. **Tenancy** — after audit formalization.
6. **Swarm** — last, and only on evidence it earns its complexity.

## Gate discipline

Adoption gates govern every layer change. Every significant change is
proposed, run through a benchmark gate, and written up in an editor
review. Carlos adopts; nothing self-adopts. See `PLATFORM.md` for the
layer map and the full discipline, including the worked example.
