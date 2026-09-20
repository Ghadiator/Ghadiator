# Governance as Code

A reference implementation that makes governance controls observable in a regulated submission-review workflow.

## The engineering problem

An AI-generated recommendation needs a defined route through validation, evidence checks and human review. The implementation records that route so a reviewer can inspect what happened and which controls intervened.

I built the validation workflow and a static, interactive showcase. The frontend connects four views: architecture, compliance mappings, assurance controls and recorded evidence.

## Inspect two recorded cases

### 1. A case pauses for human review

The trace reaches a human decision gate after a non-clean validation result. It then records a reviewer decision and the transition to approval.

[Read or download the human-review trace](evidence/hitl_pause_resume.json)

Inspect `events` for the pause and subsequent decision, `outcome` for the terminal state, and `provenance` for the originating fixture. This case does not invoke a model.

### 2. An unsupported finding is blocked

A controlled fixture supplies a Judge finding without verified support. The verification layer strips the unsupported tag and the case is flagged for human review.

[Read or download the unsupported-claim trace](evidence/judge_bluff.json)

This is a hermetic test scenario using a function-backed model stand-in. It demonstrates the control under the fixture’s conditions; it is not a measurement of a live model’s reliability.

## What I built

- **Workflow controls:** typed contracts, explicit states and configurable rules.
- **Verification:** a procedural layer that checks evidence behind proposed findings.
- **Human oversight:** review gates with recorded decisions and transitions.
- **Traceability:** append-only events and versioned architecture decisions.
- **Frontend:** a recorded-run player, architecture drill-downs, control mappings and an evidence ledger with JSON downloads.
- **Assurance:** hermetic scenarios and tests that exercise failure paths.

## Evidence scope

The JSON files are copies of the showcase’s exported fixture traces. Each carries its source reference and provenance. Event timestamps are ordinal positions, not measured elapsed time. Exported traces may contain fewer events than the raw capture; both counts are stated in the provenance.

The source implementation is maintained in a private repository. The traces here make two specific behaviors inspectable without implying that the full source is publicly available. This is a reference implementation, with no claim of production deployment or compliance certification.

[Back to my profile](README.md) · [Discuss the work](mailto:agh961235@gmail.com)
