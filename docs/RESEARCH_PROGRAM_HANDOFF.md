# Alethic-ISM — Research Program Handoff

**As of 2026-08-29.** This document records the relationship between Alethic-ISM and the broader Doing Ethics with AI (DEWA) research program.

## Governing identity

Alethic-ISM should be treated as a **domain-neutral instruction/state-machine infrastructure**, not as a SACRE-specific backend. Its technical contribution is the architecture itself: instructions transform immutable states, and the resulting graph can function simultaneously as program, execution history, provenance record, and research dataset.

The system participated in early normative-computation investigations and is relevant to provenance, traceability, replay/reconstruction, multi-model execution, publishing, and possible high-scale distributed normative workflows. It also supports non-DEWA research workflows.

## Relationship to DEWA / P2

P2 should use Alethic-ISM for three bounded purposes:

1. **intellectual/construction history:** early normative graphs and executions helped expose specification problems and materially contributed to the development of SACRE;
2. **technical capability:** demonstrates deeper engineering around persistent state, provenance, multi-model execution, and reproducible workflow graphs;
3. **possible scaling architecture:** illustrates how normative computation could be executed through a traceable distributed substrate at larger scale.

P2 must **not** imply that current ReflectiveEquilibrium.AI or Bioethics Bench execution requires Alethic-ISM unless that dependency is actually implemented and evidenced. Earlier SACRE/ISM integration paths should be described as developed/prototyped architectures where appropriate, not silently upgraded to current production dependencies.

## Standalone paper

Provisional title:

> **Alethic-ISM: An Instruction-State-Machine Architecture for Auditable AI Research Workflows**

The paper should establish the general system across multiple mature workflows rather than centering only SACRE.

Candidate demonstrations include:

- SACRE / early normative-computation graphs;
- synthetic contingent valuation / willingness-to-pay workflows;
- one or more additional mature workflows with reproducible evidence;
- provenance/replay, state inspection, multi-model execution, publishing, and distributed scaling measurements.

## Evidence boundaries

- Distinguish the open-source baseline from more advanced closed-source/active development where relevant.
- Treat repository-reported scale numbers as project-reported unless independently documented in a paper-facing evidence package.
- Use independent workflows such as synthetic-CV/WTP to establish domain generality.
- Do not treat incomplete integration experiments as production dependencies.

## P2 figure restoration direction

For the maximal integrated P2 master, prioritize approximately four high-value Alethic-ISM figures:

1. instruction/state-machine graph abstraction or Studio view;
2. WTP/synthetic-data graph or multi-model execution demonstrating domain generality/scale;
3. early collective reflective-equilibrium/SACRE precursor graph showing construction influencing theory;
4. later SACRE execution architecture in ISM, explicitly labeled as a scalable/auditable architecture or prototype rather than the mandatory current REai backend.

Detailed processor/state/template/publishing/deployment material should normally move to supplement or the standalone ISM paper.

Drive prospectus: https://docs.google.com/document/d/1wBRVgyZKpzYkT9aekHfPNaopyfd6uQX7/edit

## Immediate next steps

1. Inventory June 15 and October 2025 DEWA/ISM figures against current repo concepts.
2. Identify the best reproducible evidence for cross-workflow execution and scaling.
3. Draft the standalone technical contribution around the instruction-state-machine abstraction.
4. Keep P2 self-contained but bounded: enough ISM to explain what it contributed to DEWA, not the entire systems paper.
