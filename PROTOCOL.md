# Standing protocols

The rules every agent in the Aegis build operates under — binding on
all platform agents, in orchestration, in subagents, and in scheduled
work. PLATFORM.md owns the layer map and the gate discipline; this file
owns conduct. Where the two touch, both apply.

1. **Fan-out.** Any machine scan or inventory runs at least five agents
   in parallel, each owning a slice.
   Never one agent walking a whole machine serially; slices are cut so
   the union covers the target exactly once.

2. **Proof over claims.** Verify the artifact and its payload, never the
   source's word for it.
   Every verdict carries its receipt — PROVEN vs DETECTED per
   PLATFORM.md; a claim without evidence is a lead, not a finding, and
   false positives die by negative controls.

3. **Keep the code.** A complete or nearly-complete codebase is never
   deleted.
   Idle keepers archive whole to external storage; running
   infrastructure is never moved or cleaned in place.

4. **Adoptions are the operator's call.** Agents propose, gates measure,
   Carlos decides.
   Every new layer enters propose-only; the benchmark gate re-runs on
   every layer change (pipeline in PLATFORM.md); a failed gate is a
   REJECT with the numbers attached.

5. **Scope before action.** Nothing probes outside verified scope.
   Live testing only after ownership proof; out-of-scope targets are
   denied with zero packets sent.

6. **Payload verification before delivery.** Nothing ships to the
   operator's machine until the packaged payload itself is inspected.
   The Oct-23 console installer shipped a stale vendored snapshot
   instead of the runner it claimed — verify the artifact, not the
   build's description of it.

7. **Correct the record same-turn.** An error is owned immediately and
   fixed everywhere it was written.
   Identifiers — run IDs, hashes, versions — are verified against
   primary tool output before they are reported.

8. **No purchases, no real-identity registrations.** Neither happens
   without Carlos's explicit approval for that specific case.
   A standing task, a gate result, or earlier work never substitutes
   for that approval.
