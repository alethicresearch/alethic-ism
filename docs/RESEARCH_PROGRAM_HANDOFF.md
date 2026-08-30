# Alethic-ISM — Research Program Handoff

**As of 2026-08-29.** This document records Alethic-ISM's current scientific role in the wider **Doing Ethics with AI (DEWA)** research program and its independent systems-paper trajectory.

For cross-program current state, first read:

- `xnuxi/sacre-prototype/docs/CENTRAL_COORDINATOR_CONTINUITY.md`
- `xnuxi/sacre-prototype/docs/CURRENT_COORDINATOR_STATUS.md`

## Where Alethic-ISM sits

Part I, *Doing Ethics with AI: Practical Ethics Engineering, Product-Led Philosophy, & Computer-Aided Ethics* (Ghose, Singer, & Savulescu, 2026a), introduces **Doing Ethics with AI (DEWA)** as a general paradigm in which normative procedures can be specified, constructed as computational systems, investigated, validated, and ultimately studied in use.

Alethic-ISM intersects with DEWA because it was used in early computational investigations that helped make normative workflows executable and inspectable. Its scientific identity is broader: **a domain-neutral instruction-state-machine infrastructure for auditable AI research workflows**.

The core abstraction is that instructions transform immutable states. The resulting graph can function simultaneously as executable workflow, execution history, provenance structure, and research dataset. This makes the lineage of computation inspectable rather than reducing a research workflow to its terminal output.

## Relationship to DEWA and other workflows

```text
                         Alethic-ISM
              domain-neutral technical substrate
     instructions • immutable states • graph history • provenance
         replay • model/configuration control • distributed scale
                    /            |             \
                   /             |              \
        DEWA / normative      synthetic CV      other research
           workflows             / WTP             workflows
              |
        early normative / SACRE graphs
              |
      later SACRE / REai work
      (current REai does NOT require ISM)
```

Alethic-ISM can support DEWA research, but DEWA is not defined by Alethic-ISM. Current ReflectiveEquilibrium.AI and Bioethics Bench must not be described as depending on ISM unless a present dependency is actually implemented and evidenced.

Synthetic contingent valuation / willingness-to-pay work is important independent evidence because its project documentation states that the simulations were run on Alethic-ISM. This helps establish that the architecture is not SACRE-specific.

## Current program state

The P2 maximal-master and submission-composition stages are complete. Current P2 artifacts are:

- **P2 v49 main** — https://docs.google.com/document/d/1HpMtZSNbrLr_g37pypSQ5thHmnhZVQah/edit
- **P2 v49 Supplementary Information** — https://docs.google.com/document/d/16uDrozERPVusO0GcQj2Qj9q5HF_lw38y/edit
- **P2 v48 preserved maximal authorial master** — https://docs.google.com/document/d/12W2-SHZ1CSdbTfQWO1i0nXcCViVR_AiI/edit

P2 now uses Alethic-ISM in the bounded role intended by the program: construction history, evidence that executable building can generate philosophical pressure, selected technical depth, independent cross-workflow context, and a possible auditable/scalable architecture. It does not present current REai or Bioethics Bench as requiring ISM.

The current Alethic-ISM research/publication plan remains **v3**:

https://docs.google.com/document/d/1ghrO0Ngk0nWiZylpQmZD0yoyORiJ51DJ/edit

Overall DEWA Drive root:

https://drive.google.com/drive/folders/1cYrCfxRhIwsO5Uo-5nIAxJUPTL5Z0cDe

## What P2 now preserves

The composed P2 submission set keeps four especially important ISM contributions:

1. **construction history as philosophical evidence** — early executable normative graphs exposed the mismatch between `Policy Strength` and reflective-equilibrium coherence;
2. **general technical capability** — explicit instruction/state structure, persistence, provenance/replay, model/configuration control, and graph-based execution;
3. **independent cross-workflow evidence** — synthetic contingent valuation/WTP demonstrates use outside SACRE/DEWA;
4. **possible scale architecture** — later SACRE/ISM material is framed as historical/prototype/scaling architecture rather than a required present backend.

Deeper architecture and application detail is routed to the P2 supplement and, more importantly, to the standalone systems-paper track.

## Standalone systems paper — current priority

Provisional title:

> **Alethic-ISM: An Instruction-State-Machine Architecture for Auditable AI Research Workflows**

The scientific object is the architecture itself across multiple workflows, with SACRE as one historically important case rather than the organizing center.

The next substantive Alethic-ISM work should therefore move beyond supplying P2 and build the systems paper around measurable cross-workflow evidence.

Candidate contribution/evaluation areas include:

- reconstruction fidelity and provenance completeness;
- replay/repeat behavior;
- branching/comparison and configuration control;
- portability of the instruction/state abstraction across workflows;
- multi-model execution;
- throughput and distributed execution behavior;
- storage/data growth;
- failure recovery;
- publication/retrieval of research state.

The strongest systems paper will distinguish architecture claims from measurable evidence rather than presenting a catalogue of features.

## Cross-workflow evidence program

Build a paper-facing evidence inventory across several mature workflows:

```text
Alethic-ISM architecture
    ├── normative computation / early SACRE investigations
    ├── synthetic contingent valuation / WTP
    └── additional mature workflow(s)
         ↓
compare provenance • replay • model control • scale • operational behavior
```

For every workflow, identify:
- repository/ref or preserved evidence source;
- exact role of Alethic-ISM;
- implemented capabilities actually exercised;
- available run/scale/provenance evidence;
- limitations on what can be claimed.

The four high-value figure families already identified for P2 remain useful source material for the standalone paper:

1. instruction/state-machine graph or Studio view;
2. WTP/synthetic-data or multi-model execution view;
3. early collective-reflective-equilibrium/SACRE precursor graph;
4. later SACRE execution architecture in ISM, explicitly labeled by historical/current/prototype status.

For each figure preserve **source/date; workflow; current-vs-historical/prototype status; claim supported; evidence source; intended paper home**.

## Open-source vs active development

The public/open-source repository baseline and more advanced active/closed development must not be conflated. When a capability appears only in more advanced development, say so explicitly and support it with a paper-facing artifact before making it a central result.

Repository-reported scale figures, including the current >500M-data-point statement, should be described as **project-reported** unless independently reconstructed or documented in a stronger evidence package.

## Truthfulness boundaries

- Current REai and Bioethics Bench do not require Alethic-ISM.
- Partially completed/historical SACRE-ISM integration is not current production dependency.
- Architecture plausibility is not the same as demonstrated scale/performance.
- Public OSS state and advanced active development are distinct evidence states.
- Repository-reported scale remains project-reported unless independently supported.
- Synthetic-CV/WTP is important independent workflow evidence; do not imply domain generality solely from architecture.
- Preserve ISM's genuine role in the construction lineage without making it the sole origin of SACRE or the only possible DEWA execution architecture.

## Current next actions

1. Convert the P2-era figure/evidence inventory into the standalone systems-paper evidence table.
2. Reconcile public-current vs advanced-current implementation claims across each mature workflow.
3. Identify at least one additional mature non-SACRE workflow beyond synthetic-CV/WTP if the evidence is strong enough.
4. Draft the standalone systems paper around architecture + measured cross-workflow behavior, not a feature catalogue.
5. Feed P2 only targeted factual corrections if new systems evidence materially changes a statement already in v49.

## Handoff format

Every substantive Alethic-ISM research handoff should state:

**Ref/branch; Changed; Verified; Workflow(s) inspected; Product/paper impact; Evidence status; Current-vs-historical status; Cross-repo dependency; Next dependency.**

For figures, include **source/date; claim supported; truth-status; intended home**.

## Reference

Ghose, S., Singer, P., & Savulescu, J. (2026a). *Doing ethics with AI: Practical ethics engineering, product-led philosophy, & computer-aided ethics.* Manuscript in preparation.
