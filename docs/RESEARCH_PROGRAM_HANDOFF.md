# Alethic-ISM — Research Program Handoff

**As of 2026-08-29.** This document records the relationship between Alethic-ISM and the wider research program without treating the system as merely a component of one application.

## Where Alethic-ISM sits

Part I, *Doing Ethics with AI: Practical Ethics Engineering, Product-Led Philosophy, & Computer-Aided Ethics* (Ghose, Rasaee, Singer, & Savulescu, 2026a), introduces **Doing Ethics with AI (DEWA)** as a general paradigm in which normative procedures are specified, constructed as computational systems, made interactive, and investigated empirically and in use.

Alethic-ISM intersects with that program because it was used in early computational investigations that helped make normative workflows executable and inspectable. Its scientific identity is nevertheless broader. Alethic-ISM should be treated as a **domain-neutral instruction-state-machine infrastructure for auditable AI research workflows**, not as a SACRE-specific backend and not as a technology whose value depends on DEWA.

The core idea is that instructions transform immutable states. The resulting graph can function simultaneously as a program, a record of execution, a provenance structure, and a research dataset. This creates a technical substrate for workflows in which the history of computation matters as much as the terminal output.

## Relationship among DEWA, Alethic-ISM, and specific applications

```text
                         Alethic-ISM
              domain-neutral technical substrate
          instructions • immutable states • graph history
          provenance • replay • multi-model execution • scale
                    /            |             \
                   /             |              \
        DEWA / normative      synthetic CV      other research
           workflows             / WTP             workflows
              |
        early SACRE graphs
              |
      later SACRE / REai work
      (current REai does NOT require ISM)
```

This distinction matters. Alethic-ISM can support DEWA research, but DEWA is not defined by Alethic-ISM; likewise, current ReflectiveEquilibrium.AI and Bioethics Bench should not be described as depending on Alethic-ISM unless a present dependency is actually implemented and evidenced.

## What P2 should use Alethic-ISM to show

P2 should use Alethic-ISM for three bounded but substantive purposes.

### 1. Construction history as philosophical evidence

Early normative-computation graphs are not merely historical screenshots. They help document the central product-led-philosophy claim that building a procedure can expose conceptual incompleteness. When an initially verbal or diagrammatic method is forced into executable state transitions, decisions must be made about inputs, boundaries, ordering, persistence, iteration, and outputs. Those construction pressures contributed to the specification work that became SACRE. P2 should therefore use the early ISM work to show how engineering fed back into philosophy.

### 2. General technical capability

Alethic-ISM demonstrates that the research program developed engineering infrastructure deeper than a prompt wrapped in a user interface. Relevant capabilities include persistent and inspectable state, graph-structured execution history, provenance, replay/reconstruction, parallel or comparative model execution, workflow publication, and distributed processing. These features should be presented accurately and with evidence rather than as a catalogue of aspirational architecture.

### 3. A possible scaling architecture for normative computation

The instruction-state-machine abstraction provides a plausible architecture for highly traceable normative computation at larger scale. Earlier SACRE/ISM modes or integration paths can be described as developed or prototyped where supported. They must not be silently upgraded into a claim that current REai or Bioethics Bench production execution depends on ISM.

## Core technical contribution for the standalone paper

The standalone systems paper should make the architecture itself the object of study. Candidate technical contributions include:

- **instruction abstraction:** explicit executable units that transform state;
- **immutable state:** each transition preserves rather than overwrites prior research state;
- **graph as computation and evidence:** execution topology doubles as program history and inspectable dataset;
- **provenance and replay:** outputs remain connected to the sequence of instructions, models, inputs, and prior states that produced them;
- **research control:** workflows can be repeated, compared, branched, and inspected under explicit configurations;
- **multi-model execution:** model choice can become a controlled axis of the workflow rather than hidden implementation detail;
- **publishing and collaboration:** research states and workflows can be made persistent and shareable;
- **distributed scale:** instruction/state processing can be separated from a single interactive session and scaled across larger research programs.

The paper should show which of these are implemented in the public repository baseline, which are demonstrated in mature applications, and which belong to more advanced active development. The open-source baseline and more advanced closed-source/current development must not be conflated.

## Cross-workflow evidence

The strongest argument for domain generality is empirical use beyond SACRE. Synthetic contingent valuation / willingness-to-pay work is particularly important because its project documentation states that all simulations were run on Alethic-ISM. That provides an independent research workflow in which the infrastructure is central rather than incidental.

The standalone paper should therefore seek evidence across at least several mature workflows:

```text
Alethic-ISM architecture
    ├── normative computation / early SACRE investigations
    ├── synthetic contingent valuation / WTP
    └── additional mature workflow(s)
         ↓
compare provenance • replay • model control • scale • operational behavior
```

Repository-reported scale figures, including the current >500M-data-point statement, should be described as project-reported unless independently reconstructed or documented in a paper-facing evidence package.

## Evaluation questions for the systems paper

The paper should move beyond an architecture description by asking measurable questions. Candidate evaluations include reconstruction fidelity, completeness of provenance, repeat/replay behavior, branching and comparison cost, model/configuration control, throughput under distributed execution, storage/data growth, failure recovery, publication/retrieval behavior, and portability of the instruction-state abstraction across distinct research workflows.

The relevant comparator is not necessarily a competing commercial orchestration platform. The scientific question is whether ISM's explicit instruction/state model creates inspectability, research control, and scalable evidentiary structure that are otherwise difficult to preserve in ad hoc AI pipelines.

## P2 figure restoration direction

For the maximal integrated P2 master, restore approximately four high-value Alethic-ISM figures from the June 15 and related historical materials, then reconcile them with current repository terminology:

1. an instruction/state-machine graph or Studio view that makes the abstraction immediately visible;
2. a WTP/synthetic-data graph or multi-model execution view demonstrating domain generality and scale;
3. an early collective-reflective-equilibrium/SACRE precursor graph showing construction influencing theory;
4. a later SACRE execution architecture in ISM, explicitly labeled as a scalable/auditable architecture or prototype rather than the mandatory current REai backend.

Detailed processor, state-template, publishing, deployment, and operational diagrams should usually move to the standalone paper or supplement unless one is essential to the P2 argument.

## Standalone publication

Provisional title:

> **Alethic-ISM: An Instruction-State-Machine Architecture for Auditable AI Research Workflows**

The paper should establish the general system across multiple mature workflows, with SACRE as one historically important case rather than the organizing center.

Current Drive prospectus v2: https://docs.google.com/document/d/1-Umbhhfod6_6Y9p5jS-O8QtomK7g2dD2/edit

Overall DEWA Drive root: https://drive.google.com/drive/folders/1cYrCfxRhIwsO5Uo-5nIAxJUPTL5Z0cDe

## Truthfulness boundaries

- Do not claim that current REai or Bioethics Bench requires Alethic-ISM.
- Do not describe partially completed or historical SACRE/ISM integration as a current production dependency.
- Distinguish public/open-source repository state from more advanced active development where relevant.
- Treat repository-reported scale as project-reported until independently evidenced.
- Use cross-workflow evidence, particularly synthetic-CV/WTP, to establish generality rather than assuming it from architecture alone.
- Preserve the historical fact that ISM contributed to the construction lineage without making it the sole origin of SACRE or the only possible DEWA execution architecture.

## Immediate next steps

1. Inventory the June 15 and October 2025 DEWA/ISM figures against current repo concepts and decide which four best advance the P2 narrative.
2. Build a paper-facing evidence inventory for cross-workflow execution, provenance, replay, and scale.
3. Draft the standalone technical paper around the instruction-state-machine abstraction rather than around SACRE alone.
4. Keep P2 self-contained but bounded: enough ISM to explain what construction taught us and what infrastructure was built, without turning P2 into the entire systems paper.
5. Use the maximal P2 master to classify ISM material as INLINE / SUPPLEMENT / ALETHIC PAPER / DUPLICATIVE before submission compression.

## Reference

Ghose, S., Rasaee, K., Singer, P., & Savulescu, J. (2026a). *Doing ethics with AI: Practical ethics engineering, product-led philosophy, & computer-aided ethics.* Manuscript in preparation.
