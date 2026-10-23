# Platform layer map

## What belongs where

### This repo — the platform

- **Ops Console evolution** — the operator surface: engagement oversight,
  layer/gate queues, benchmark results, scope & safety reporting. (v1
  ships inside the pinned runtime's UI; its evolution is directed from
  here.)
- **Provider gateway / fallback** — provider routing, fallback chains,
  refusal handling, cost/quality policy.
- **Skills-as-data registry** (`skills/`) — agent capabilities as
  versioned, reviewable data rather than code changes.
- **Platform services** (`platform/`) — services the platform adds on top
  of the runtime.
- **Memory tiering** — how agents and engagements recall prior work:
  what stays hot, what cools, what is archived.
- **Tenancy & audit** (later) — multi-operator boundaries and the audit
  formalization that must land before tenancy does.
- **Docs** (`docs/`) — proposals, gate packages, editor reviews, design
  notes.

### The pinned runtime — `runtime/secscan-redteam-desktop`

- The red-team runner and harness (layer composition, engagement
  execution, proof receipts, safety brakes).
- The desktop app and its installer.
- The UI server the console v1 ships in.

The runtime tracks upstream on its own branches in its own repo. This
repo consumes it only through the submodule pin.

## Data-store note

The rc.5 runtime build keeps engagement data in the Electron userData
directory:

```
AppData\Roaming\com.secscan.redteam\engagements\
```

Earlier builds kept engagements inside the install directory
(`...\Programs\SecScan RedTeam\resources\redteam-runner\engagements\live`),
and the rc.5 upgrade did **not** migrate that store — the new build
started empty and the old store was left behind with the replaced install.

Standing rule for every future installer: **preserve or migrate the
engagement store across upgrades, explicitly.** An upgrade that silently
strands engagement history is a defect, not a side effect.

## Gate discipline

Every layer change goes through the same pipeline:

1. **Propose** — a written package: what changes, why, the risks named
   up front.
2. **Benchmark gate** — the change runs against known targets with
   ground truth; metrics are computed against that ground truth, never
   against the runner's own claims alone.
3. **Editor review** — an independent write-up: completeness checklist,
   evidence tables, mechanism findings, disclosures, verdict with
   rationale.
4. **Carlos adopts** — adoption is a human decision, every time.
   Nothing self-adopts, and a rejected package stays rejected until a
   new version earns a new gate.

### Worked example: doctrine v1 — REJECTED 2026-10-23

Package `pkg-adversary_doctrine-001` ("black hat brain, white hat hands")
proposed adding an adversary-doctrine layer to the harness. It ran a
20-run gate matrix (10 ground-truth targets × baseline/candidate arms)
and was scored by manifest cross-check:

- Proven findings: **2 vs 2** — no capability gain.
- Claims: 14 → 27, false-positive rate **0.857 → 0.926** — the §8
  improvement rule and the FP ceiling both failed.
- Cost rules passed (tokens +2%, steps +12%); it was rejected for noise,
  not expense.

Verdict: **REJECT**. The gate also surfaced two findings worth more than
the verdict — chain-reach was 0/3 in *both* arms (a coverage failure
upstream of doctrine), and the candidate retrieved more canaries than the
baseline but converted the same number (a reporting/conversion failure).
A future v2 has to solve those problems, not add aggression.

That is the process working as designed: the doctrine was plausible,
well-argued, and still rejected on evidence.
