# Alethic-ISM — Research Program Handoff

**As of 2026-08-29.** This document records Alethic-ISM's current scientific role in the wider **Doing Ethics with AI (DEWA)** research program and its independent systems-paper trajectory.

For cross-program current state, first read:

- `xnuxi/sacre-prototype/docs/CENTRAL_COORDINATOR_CONTINUITY.md`
- `xnuxi/sacre-prototype/docs/CURRENT_COORDINATOR_STATUS.md`

## Where Alethic-ISM sits

Part I, *Doing Ethics with AI: Practical Ethics Engineering, Product-Led Philosophy, & Computer-Aided Ethics* (Ghose, Rasaee, Singer, & Savulescu, 2026a), introduces **Doing Ethics with AI (DEWA)** as a general paradigm in which normative procedures can be specified, constructed as computational systems, investigated, validated, and ultimately studied in use.

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

The high-level DEWA editorial/formatting pass is complete enough to return to the principal substantive task: **P2 maximal integrated master**, built from current P2 v47 before submission compression.

Current high-level Drive documents include Program Overview v3 and Publication Program v18. The current Alethic-ISM research/publication plan is **v3**:

https://docs.google.com/document/d/1ghrO0Ngk0nWiZylpQmZD0yoyORiJ51DJ/edit

Overall DEWA Drive root:

https://drive.google.com/drive/folders/1cYrCfxRhIwsO5Uo-5nIAxJUPTL5Z0cDe

## What P2 should use Alethic-ISM to show

P2 should use Alethic-ISM for three bounded but substantive purposes.

### 1. Construction history as philosophical evidence

Early normative-computation graphs help document the product-led-philosophy claim that construction exposes conceptual incompleteness. Making a verbal or diagrammatic procedure executable forces decisions about inputs, state, ordering, boundaries, persistence, iteration, and outputs. Those construction pressures contributed to the specification work that became SACRE.

The historical figures therefore matter when they show how building changed the philosophical object, not merely because they prove that software existed.

### 2. General technical capability

Alethic-ISM demonstrates technical infrastructure deeper than a prompt wrapped in an interface. Relevant capabilities include:

- explicit instruction/state abstraction;
- immutable/persistent state;
- graph-structured execution history;
- provenance and replay/reconstruction;
- controlled model/configuration variation;
- branching/comparison of workflows;
- publication/sharing of research states;
- distributed execution and larger-scale research workflows.

P2 should use only the capabilities needed to make its construction argument. The standalone systems paper owns the deeper architecture and evaluation.

### 3. A possible scaling architecture for normative computation

The instruction-state-machine abstraction provides a plausible architecture for highly traceable normative computation at larger scale. Earlier SACRE/ISM modes or integration paths may be described as developed/prototyped where supported.

Do not upgrade historical/prototype integration into a claim that current REai or Bioethics Bench production execution requires ISM.

## Immediate P2 evidence/figure inventory

The maximal P2 master should review the June 15 / earlier DEWA-ISM materials and current repo state, then select approximately four high-value figures if they genuinely advance the argument:

1. an instruction/state-machine graph or Studio view that makes the abstraction legible;
2. a WTP/synthetic-data graph or multi-model execution view demonstrating cross-domain use/scale;
3. an early collective-reflective-equilibrium/SACRE precursor graph showing construction feeding back into theory;
4. a later SACRE execution architecture in ISM, explicitly labeled as a scalable/auditable architecture or prototype rather than the mandatory current REai backend.

Detailed processor, template, publishing, deployment, and operational diagrams usually belong in the standalone paper or supplement unless one is essential to P2's argument.

For each candidate figure preserve:
- source/original date;
- workflow/application shown;
- whether the state is historical, public-current, advanced-current, or conceptual/prototype;
- what claim the figure supports;
- whether it belongs INLINE / SUPPLEMENT / ALETHIC-ISM PAPER / DUPLICATIVE.

## Standalone systems paper

Provisional title:

> **Alethic-ISM: An Instruction-State-Machine Architecture for Auditable AI Research Workflows**

The scientific object is the architecture itself across multiple workflows, with SACRE as one historically important case rather than the organizing center.

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

## Cross-workflow evidence

Build a paper-facing evidence inventory across at least several mature workflows:

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

1. Inventory/reconcile June 15 and earlier ISM figures against current terminology and technical truth.
2. Build the cross-workflow evidence table needed for both P2 and the systems paper.
3. Supply the P2 maximal master with bounded, reader-led ISM material and correctly captioned figures.
4. Develop the standalone systems paper around the instruction-state-machine contribution and measurable cross-workflow evidence.
5. Keep P2 self-contained: enough ISM to show what construction taught and what infrastructure was built, without turning P2 into the full systems paper.

## Handoff format

Every substantive Alethic-ISM research handoff should state:

**Ref/branch; Changed; Verified; Workflow(s) inspected; Product/paper impact; Evidence status; Current-vs-historical status; Cross-repo dependency; Next dependency.**

For figures, include **source/date; claim supported; truth-status; intended home**.

## Reference

Ghose, S., Rasaee, K., Singer, P., & Savulescu, J. (2026a). *Doing ethics with AI: Practical ethics engineering, product-led philosophy, & computer-aided ethics.* Manuscript in preparation.
